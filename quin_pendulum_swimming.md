```python
%matplotlib inline
```


```python
import pynamics
from pynamics.frame import Frame
from pynamics.variable_types import Differentiable,Constant
from pynamics.system import System
from pynamics.body import Body
from pynamics.dyadic import Dyadic
from pynamics.output import Output,PointsOutput
from pynamics.output_points_3d import PointsOutput3D
from pynamics.constraint import AccelerationConstraint,KinematicConstraint
from pynamics.particle import Particle
import pynamics.integration
import numpy
import matplotlib.pyplot as plt
plt.ion()
from math import pi,sin
import sympy
from sympy import sqrt
import math
import logging
import pynamics.integration
import pynamics.system
import numpy.random
import scipy.interpolate
import cma
import pandas as pd

system = System()
pynamics.set_system(__name__,system)

#import test data and convert to numPy arrays
data_spine = pd.read_excel(r'C:\Users\kwanf\Dropbox (ASU)\Thesis\Spines\Swimming\swimming_data_none.xlsx')
df_spine  = data_spine.to_numpy()
data_spine_narrow = pd.read_excel(r'C:\Users\kwanf\Dropbox (ASU)\Thesis\Spines\Swimming\swimming_data_narrow.xlsx')
df_spine_narrow  = data_spine_narrow.to_numpy()
data_spine_steep = pd.read_excel(r'C:\Users\kwanf\Dropbox (ASU)\Thesis\Spines\Swimming\swimming_data_steep.xlsx')
df_spine_steep  = data_spine_steep.to_numpy()

#change depending on which spine 0:No taper 1:Narrow 2:Steep
spine = 1
df = df_spine_narrow
length = df.shape[0]
 
pitch = numpy.array([0, 2.25, 4.5])
b = numpy.array([5694.51456258, 4858.41452731, 3133.57966076])
k = numpy.array([2691.80443292, 1874.77901593, 1539.635253])

plt.close('all')

# In[4]: Parameterization

#Constants
tpu_rho = 1220  #density of TPU A95 units: kg/m^3
rho = 998 #density of water @25C units: kg/m^3
l_full = .25
l = l_full/5
h = 0.04
th = .003     #thickness
p = Constant(pitch[spine]*pi/180,'p',system)   #pitch of taper

constants = system.constant_values.copy()
pv = constants[p]

hA = h - 2*(((l_full/10)*sin(pv))/sin(90-pv))
hB = h - 2*(((l_full*(3/10))*sin(pv))/sin(90-pv))
hC = h - 2*(((l_full*(5/10))*sin(pv))/sin(90-pv))
hD = h - 2*(((l_full*(7/10))*sin(pv))/sin(90-pv))
hE = h - 2*(((l_full*(9/10))*sin(pv))/sin(90-pv))


m_A = tpu_rho*l*th*hA
m_B = tpu_rho*l*th*hB
m_C = tpu_rho*l*th*hC
m_D = tpu_rho*l*th*hD
m_E = tpu_rho*l*th*hE

lA = Constant(l,'lA',system)
lB = Constant(l,'lB',system)
lC = Constant(l,'lC',system)
lD = Constant(l,'lD',system)
lE = Constant(l,'lE',system)

mA = Constant(m_A,'mA',system)
mB = Constant(m_B,'mB',system)
mC = Constant(m_C,'mC',system)
mD = Constant(m_D,'mD',system)
mE = Constant(m_E,'mE',system)
#mM = Constant(.2/1000,'mM',system)

g = Constant(9.81,'g',system)
b = Constant(b[spine],'b',system)
e = Constant(k[spine],'e',system) #Youngs Modulus k  = E*A/L
c_d_x = Constant(0.001599,'c_d_x',system) #drag coefficient in y

freq = Constant(1,'freq',system)
amp = Constant(44*pi/180,'amp',system)

Ixx_A = Constant(1/12*m_A*(hA*2 + th**2),'Ixx_A',system)
Iyy_A = Constant(1/12*m_A*(hA**2 + l**2),'Iyy_A',system)
Izz_A = Constant(1/12*m_A*(l**2 + th**2),'Izz_A',system)
Ixx_B = Constant(1/12*m_B*(hB*2 + th**2),'Ixx_B',system)
Iyy_B = Constant(1/12*m_B*(hB**2 + l**2),'Iyy_B',system)
Izz_B = Constant(1/12*m_B*(l**2 + th**2),'Izz_B',system)
Ixx_C = Constant(1/12*m_C*(hC**2 + th**2),'Ixx_C',system)
Iyy_C = Constant(1/12*m_C*(hC**2 + l**2),'Iyy_C',system)
Izz_C = Constant(1/12*m_C*(l**2 + th**2),'Izz_C',system)
Ixx_D = Constant(1/12*m_D*(hD**2 + th**2),'Ixx_D',system)
Iyy_D = Constant(1/12*m_D*(hD**2 + l**2),'Iyy_D',system)
Izz_D = Constant(1/12*m_D*(l**2 + th**2),'Izz_D',system)
Ixx_E = Constant(1/12*m_E*(hE**2 + th**2),'Ixx_E',system)
Iyy_E = Constant(1/12*m_E*(hE**2 + l**2),'Iyy_E',system)
Izz_E = Constant(1/12*m_E*(l**2 + th**2),'Izz_E',system)

#Differentiable State Variables
qA,qA_d,qA_dd = Differentiable('qA',system)
qB,qB_d,qB_dd = Differentiable('qB',system)
qC,qC_d,qC_dd = Differentiable('qC',system)
qD,qD_d,qD_dd = Differentiable('qD',system)
qE,qE_d,qE_dd = Differentiable('qE',system)

x,x_d,x_dd = Differentiable('x',system)
y,y_d,y_dd = Differentiable('y',system)

#Intial Values
initialvalues = {}
initialvalues[qA]=44*pi/180
initialvalues[qA_d]=0*pi/180
initialvalues[qB]=44*pi/180
initialvalues[qB_d]=0*pi/180
initialvalues[qC]=44*pi/180
initialvalues[qC_d]=0*pi/180
initialvalues[qD]=44*pi/180
initialvalues[qD_d]=0*pi/180
initialvalues[qE]=44*pi/180
initialvalues[qE_d]=0*pi/180

initialvalues[x]=0
initialvalues[y]=0
initialvalues[x_d]=0
initialvalues[y_d]=0

statevariables = system.get_state_variables()
ini = [initialvalues[item] for item in statevariables]

tinitial = 0
tfinal = df[-1,1]  #time of simulation to match time of experiment
fps = 30
tstep = 1/fps
t = numpy.r_[tinitial:tfinal:tstep]

# In[5]: Kinematics

#Frames
N = Frame('N',system)
A = Frame('A',system)
B = Frame('B',system)
C = Frame('C',system)
D = Frame('D',system)
E = Frame('E',system)

system.set_newtonian(N)

A.rotate_fixed_axis(N,[0,0,1],qA,system)
B.rotate_fixed_axis(N,[0,0,1],qB,system)
C.rotate_fixed_axis(N,[0,0,1],qC,system)
D.rotate_fixed_axis(N,[0,0,1],qD,system)
E.rotate_fixed_axis(N,[0,0,1],qE,system)

#Vectors
pNA= x*N.x + y*N.y
pAB = pNA + lA*A.x
pBC = pAB + lB*B.x
pCD = pBC + lC*C.x
pDE = pCD + lD*D.x
pEtip = pDE + lE*E.x

#Centers of Mass
pAcm=pNA+lA/2*A.x
pBcm=pAB+lB/2*B.x
pCcm=pBC+lC/2*C.x
pDcm=pCD+lD/2*D.x
pEcm=pDE+lE/2*E.x
pMcm = pDE+(lE-0.006)*E.x

#Angular Velocity
wNA = N.get_w_to(A)
wAB = A.get_w_to(B) 
wBC = B.get_w_to(C)
wCD = C.get_w_to(D) 
wDE = D.get_w_to(E) 

#Velocity of Center of Mass
vA=pAcm.time_derivative()
vB=pBcm.time_derivative()
vC=pCcm.time_derivative()
vD=pDcm.time_derivative()
vEtip=pEcm.time_derivative()

#Interia and Bodys
IA = Dyadic.build(A,Ixx_A,Iyy_A,Izz_A)
IB = Dyadic.build(B,Ixx_B,Iyy_B,Izz_B)
IC = Dyadic.build(C,Ixx_C,Iyy_C,Izz_C)
ID = Dyadic.build(D,Ixx_D,Iyy_D,Izz_D)
IE = Dyadic.build(E,Ixx_E,Iyy_E,Izz_E)

BodyA = Body('BodyA',A,pAcm,mA,IA,system)
BodyB = Body('BodyB',B,pBcm,mB,IB,system)
BodyC = Body('BodyC',C,pCcm,mC,IC,system)
BodyD = Body('BodyD',D,pDcm,mD,ID,system)
BodyE = Body('BodyE',E,pEcm,mE,IE,system)
#BodyM = Particle(pMcm,mM,'ParticleM',system) #Mass of tracker

# In[6]: Forces

#system.addforce(torque*sympy.sin(freq*2*sympy.pi*system.t)*A.z,wNA)

#Aerodynamic Forces
f_aero_Ay = rho * vA.length()*(vA.dot(A.y)) * (hA*l) * A.y
f_aero_By = rho * vB.length()*(vB.dot(B.y)) * (hB*l) * B.y
f_aero_Cy = rho * vC.length()*(vC.dot(C.y)) * (hC*l) * C.y
f_aero_Dy = rho * vD.length()*(vD.dot(D.y)) * (hD*l) * D.y
f_aero_Ey = rho * vEtip.length()*(vEtip.dot(E.y)) * (hE*l) * E.y

f_aero_Ax = rho * 0.5 * c_d_x * vA.length()*(vA.dot(A.x)) * A.x
# f_aero_Bx = rho * 0.5 * c_d_x * vB.length()*(vB.dot(B.x)) * (hB*th) * B.x
# f_aero_Cx = rho * 0.5 * c_d_x * vC.length()*(vC.dot(C.x)) * (hC*th) * C.x
# f_aero_Dx = rho * 0.5 * c_d_x * vD.length()*(vD.dot(D.x)) * (hD*th) * D.x
# f_aero_Ex = rho * 0.5 * c_d_x * vEtip.length()*(vEtip.dot(E.x)) * (hE*th) * E.x

system.addforce(-f_aero_Ay,vA)
system.addforce(-f_aero_By,vB)
system.addforce(-f_aero_Cy,vC)
system.addforce(-f_aero_Dy,vD)
system.addforce(-f_aero_Ey,vEtip)

system.addforce(-f_aero_Ax,vA)
# system.addforce(-f_aero_Bx,vB)
# system.addforce(-f_aero_Cx,vC)
# system.addforce(-f_aero_Dx,vD)
# system.addforce(-f_aero_Ex,vEtip)

#Spring Forces
kA = e*(hA*th)
system.add_spring_force1(kA, (qA)*N.z, wNA)
kB = e*(hB*th)
system.add_spring_force1(kB, (qB-qA)*N.z, wAB)
kC = e*(hC*th)
system.add_spring_force1(kC, (qC-(qB))*N.z, wBC)
kD = e*(hD*th)
system.add_spring_force1(kD, (qD-(qC))*N.z, wCD)
kE = e*(hE*th)
system.add_spring_force1(kE, (qE-(qD))*N.z, wDE)

#Damping Force
bA = b*(hA*th)*mA 
system.addforce(-bA*wNA, wNA)
bB = b*(hB*th)*mB
system.addforce(-bB*wAB, wAB)
bC = b*(hC*th)*mC
system.addforce(-bC*wBC, wBC)
bD = b*(hD*th)*mD
system.addforce(-bD*wCD, wCD)
bE = b*(hE*th)*mE
system.addforce(-bE*wDE, wDE)

# In[7]: Constraints and Plots

pos = amp*sympy.cos(freq*2*pi*system.t)
eq = pos*N.z-qA*N.z
eq_d = eq.time_derivative()
eq_dd = eq_d.time_derivative()
eq_dd_scalar = []
eq_dd_scalar.append(eq_dd.dot(N.z))

system.add_constraint(AccelerationConstraint(eq_dd_scalar))

f,ma = system.getdynamics();

tol = 1e-7
points = [pNA,pAB,pBC,pCD,pDE,pEtip]

statevariables = system.get_state_variables()
ini = [initialvalues[item] for item in statevariables]

func1 = system.state_space_post_invert(f,ma,return_lambda = False)

states=pynamics.integration.integrate(func1,ini,t,rtol=tol,atol=tol,hmin=tol, args=({'constants':system.constant_values},))

points_output = PointsOutput(points,system) 
y = points_output.calc(states,t)


fig = plt.figure()

ax1 = plt.subplot(2,1,1)
ax1.plot(t,y[:,0,0],df[:,1],(-1*df[:,3]))
ax1.legend(['Simulated','Experimental'])
ax1.set_title('Position vs Time of Head in X-Axis')
ax1.set_xlabel('Time (s)')
ax1.set_ylabel('Position (m)')

ax2 = plt.subplot(2,1,2)
ax2.plot(t,y[:,0,1],df[:,1],(df[:,4]-df[0,4]))
ax2.legend(['Simulated','Experimental'])
ax2.set_title('Position vs Time of Head in Y-Axis')
ax2.set_xlabel('Time (s)')
ax2.set_ylabel('Position (m)')

fig.tight_layout()

ve = (-1*df[-1,3])/df[-1,1]
vs = y[-1,0,0]/tfinal

points_output.plot_time(5)

if spine == 0:
    points_output.animate(fps = fps,movie_name = 'quin_swimming_2D_none.mp4',lw=2,marker='o',color=(1,0,0,1),linestyle='-')
    print("Taper: None | Simulated Velocity: %f | Experimental Velocity: %f" %(vs, ve))
elif spine == 1:
    points_output.animate(fps = fps,movie_name = 'quin_swimming_2D_narrow.mp4',lw=2,marker='o',color=(1,0,0,1),linestyle='-')
    print("Taper: Narrow | Simulated Velocity: %f | Experimental Velocity: %f" %(vs, ve))
elif spine == 2:
    points_output.animate(fps = fps,movie_name = 'quin_swimming_2D_steep.mp4',lw=2,marker='o',color=(1,0,0,1),linestyle='-')
    print("Taper: Steep | Simulated Velocity: %f | Experimental Velocity: %f" %(vs, ve))
else:
    points_output.animate(fps = fps,movie_name = 'quin_swimming_2D.mp4',lw=2,marker='o',color=(1,0,0,1),linestyle='-')
    print("Simulated Velocity: %f | Experimental Velocity: %f" %(vs, ve))


```

    2022-03-22 15:37:25,333 - pynamics.system - INFO - getting dynamic equations
    2022-03-22 15:37:26,132 - pynamics.system - INFO - solving a = f/m and creating function
    2022-03-22 15:37:30,144 - pynamics.system - INFO - substituting constrained in Ma-f.
    2022-03-22 15:37:30,856 - pynamics.system - INFO - done solving a = f/m and creating function
    2022-03-22 15:37:30,981 - pynamics.integration - INFO - beginning integration
    2022-03-22 15:37:30,981 - pynamics.system - INFO - integration at time 0000.00
    2022-03-22 15:37:34,607 - pynamics.system - INFO - integration at time 0000.50
    2022-03-22 15:37:38,027 - pynamics.system - INFO - integration at time 0001.60
    2022-03-22 15:37:41,527 - pynamics.system - INFO - integration at time 0002.57
    2022-03-22 15:37:44,971 - pynamics.system - INFO - integration at time 0003.58
    2022-03-22 15:37:48,520 - pynamics.integration - INFO - finished integration
    2022-03-22 15:37:48,544 - pynamics.output - INFO - calculating outputs
    2022-03-22 15:37:48,552 - pynamics.output - INFO - done calculating outputs
    

    Taper: Narrow | Simulated Velocity: -0.170973 | Experimental Velocity: -0.114535
    
    
![png](Images\output_1_3.png)
    



    
![png](Images\output_1_4.png)
    



    
![png](Images\output_1_5.png)
    


[Return](\index)
