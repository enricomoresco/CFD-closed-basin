# Closed Basin quasi-3D simulations of incompressible flow in a closed basin with idrostatic approx
## _PHISICAL FORMULATION_

In order to simulate the behavior of a fluid in a close basin we should start from the navier stokes equations:

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/1.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/2.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/3.gif)


Fx, Fy, Fz in general are the resultant of any external force acting on our system:
in our case we have the gravity force acting along the z-axis 

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/4.gif)

and the coriolis inertial force acting along x and y axes as result of earth rotation (the centrifugal force is neglectable)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/5.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/6.gif)

f is the coriolis parameter, is about 10E-4 rad/s mutiplied by the sine of the latitude.
For a wide shallow basin we can simplify the third equation with the hidrostatic approximation:

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/7.gif)

Considering a uniform water density we can write the spatial derivatives of p, for example ∂p∂x as

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/8.gif)

with n the elevation of the surface of the basin

Then we consider incompressibility

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/9.gif)

Integrating on all the water column we obtain

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/10.gif)

with U,V integrals of the velocity over all the water column 

The system of differential equations now looks like this

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/11.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/12.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/8.gif)

![NS_u](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/10.gif)

As boundary condition for a closed basin seems reasonable a no-slip condition and a null pressure gradient

![u_boundary](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/13.gif)

![v_boundary](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/14.gif)

![∂p/∂xi(boundary)](https://github.com/enricomoresco/Software-and-Computing-Repository/blob/main/15.gif)



the system is closed and ,given an initial condition, we are ready to numerically solve it.

## _NUMERICAL FORMULATION_

### _Definition of the grid_

The grids used are the easiest possible: uniform 3-D  solidal with our x,y,z axes.
The only complication is the using of two grids, one for the velocity and one staggered grid for the pressure, with each cell of the second one centered in the intesections of the cells of the first one.

For general purpose and definition of our algorithms, a Staggered CFD Grid shall be used. It ensures the presence of very real non-zero pressure gradient across the nodes in any condition, even in the case of a checkered grid. The staggering also ensures realistic behaviors of the descretized momentum equations for spatially oscillating pressures. Also, the direction of the velocity vectors are exact.

So the velocity grid will have n-horizontal per m-vertical points and the pressure grid will have n+1 per m+1.

DISCRETIZATION OF THE EQUATIONS

The Pressure equation can be easily discetized, calling i',j',k' the x,y,z indices in the pressure grid and i , j , k the x,y,z indices in the velocity grid 
we can calculate U(i,j) and V(i,j) as

U
V

Since U,V lie on a different grid than the p points, we shall calculate the discrete derivatives ΔU/Δx(i',j') , ΔV/Δy(i',j') by mediating on the direction ortogonal to the derivative itself.
du_dx
dv_dy

Finally we can write our nu equation discretized

nu

The discretization of the NS equations runs in the same staggering problem encountered in the discretization of the basin-level-equation but we should convert our nu derivative terms from the pressure grid to the velocity one so we can write the derivatives:
d_eta_dx
d_eta_dy

Furthermore we should find a way to discretize the advection term to let each cell communicate to all the adiacent and conservate locally the momentum.
Notice that if we lazy discretize the derivative as done before

udxu_1

each cell communicate only with the following one and not with the previous one, so the most obvious solution is the centered finite difference

udxu_2

so we can write the N.S. equations as

NSx
NSy

With the coriolis force 

Fco

And the wind stress, given the 3m wind field

Fwind

In order to find the stationary solutions we will calculate the relative variations of velocity and pressure and we will sum the square values 
then we will look for a solution with a small enough value of total relative variation in the last time step

TRV





















