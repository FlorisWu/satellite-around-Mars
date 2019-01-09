# Satellite trajectory around Mars

During my freshman year studying physics at Imperial College London, I did a Python project plotting the motion and energies of a satellite travelling around Mars.

## Loading packages and defining constants
The function called "odeint" from the package scipy is used to solve first order ordinary equations.

```python
import scipy as sp
import numpy as np
import pylab as pl
import scipy.integrate as spi
G=6.67e-11 #gravitational constant
MM=6.4e23 #mass of Mars
MS=260 #mass of satellite
R=3e6 #radius of Mars
times=100000
pi=3.141592654 #pi
```

## Trajectory when satellite crashes with Mars
When initial velocity of satellite is 500 m/s, it prints "Satellite crashes."

```python
def f1(x, t):
    xx=x[0] #distance x
    vx=x[1] #velocity in x direction
    xy=x[2] #distance y
    vy=x[3] #velocity in y direction
    at=-(G*MM*MS/(xx**2+xy**2)) #Newton's law of gravitation
    ax=-G*MM*xx/(xx**2+xy**2)**1.5 #acceleration in x direction
    ay=-G*MM*xy/(xx**2+xy**2)**1.5 #acceleration in y direction
    return [vx,ax,vy,ay]
    
t=sp.linspace(0.,1000000.,times) #time

xx0=[-5*R, 0, -2*R, 500] #starting position and velocity

MARS=sp.arange(0,360,0.01) #drawing Mars
soln=spi.odeint(f1,xx0,t) #solution of first order ordinary equation
dist=(soln[:,0]**2+soln[:,2]**2)**0.5 #final position

for p in range(times):
    if dist[p] < R:
        print("Satellite crashes.")
        break
    else: pass

xx=soln[:,0]
vx=soln[:,1]
xy=soln[:,2]
vy=soln[:,3]

fig=pl.figure(figsize=(20,10))
pl.plot(xx,xy)
pl.plot(R*sp.sin(MARS),R*sp.cos(MARS),'r')
pl.ylim((-0.5e8,1.8e8))
pl.xlim((-0.5e8,1.8e8))
pl.xlabel("x(m)", fontsize=25)
pl.ylabel("y(m)", fontsize=25)
pl.xticks(fontsize=16)
pl.yticks(fontsize=16)
```

<img src="https://github.com/FlorisWu/satellite-around-Mars/blob/master/trajectory_500.jpg?raw=true" width="900"/>

## Energies when satellite crashes with Mars

```python
PE=-G*MM*MS/(xx**2+xy**2)**0.5 #potential energy
KE=0.5*MS*(vx**2+vy**2) #kinetic energy

fig=pl.figure(figsize=(20,10))
pl.plot(t,PE,'r-') #potential energy in red
pl.plot(t,KE,'b-') #kinetic energy in blue
pl.plot(t,PE+KE,'g-') #total energy in green
pl.xlabel("t(s)")
pl.ylabel("energy(J)")
pl.legend(["potential energy-red", "kinetic energy-blue", "total energy-green"])
pl.show()
```

<img src="https://github.com/FlorisWu/satellite-around-Mars/blob/master/energy_500.jpg?raw=true" width="900"/>

## Trajectory when satellite travels in an ellipse (initial velocity is 2000 m/s)

<img src="https://github.com/FlorisWu/satellite-around-Mars/blob/master/trajectory_2000.jpg?raw=true" width="900"/>

## Energies when satellite travels in an ellipse

<img src="https://github.com/FlorisWu/satellite-around-Mars/blob/master/energy_2000.jpg?raw=true" width="900"/>
