# Simulating the Non-Linear Dynamics of the Tumbling Toy

####Overview: 
This was my final project for [Dr. Todd Murphey](http://www.mccormick.northwestern.edu/research-faculty/directory/profiles/murphey-todd.html)'s [Dynamics class](http://www.mccormick.northwestern.edu/mechanical/courses/descriptions/314-theory-of-machines-dynamics.html). It involved modeling and simulating (on Mathematica) the impact-driven nonlinear dynamics of the tumbling toy, which consists of   

+	a hollow wooden case with a pivot on each side, 
+	a metal ball which can roll along the long axis of the case,
+	an inclined saw-toothed rail on which the pivots step down.   

This multibody system has been of special interest to researchers due to its complex dynamical behaviour. The tumbling motion is characterized by the alternating rotation of the case around one of the two pivots with the rolling of the ball from end to end of the case. The system has 4 degrees of freedom, which are the configuration variables of the simulation: q = [xRC,  yRC,  θ,  xCB]<sup>T</sup> , where xRC and yRC are the tangential and normal displacement of the CoM of the case relative to the rail, and θ is the rotation of the of the case with respect to the rail. xCB is the displacement of the ball within the body. Below is a short animation of the simulation:   

<div style="max-width: 500px;" id="_giphy_26tPqPdmsbbc7zRxm"></div><script>var _giphy = _giphy || []; _giphy.push({id: "26tPqPdmsbbc7zRxm",w: 624, h: 421});var g = document.createElement("script"); g.type = "text/javascript"; g.async = true;g.src = ("https:" == document.location.protocol ? "https://" : "http://") + "giphy.com/static/js/widgets/embed.js";var s = document.getElementsByTagName("script")[0]; s.parentNode.insertBefore(g, s);</script>

Following impacts govern the motion of the system:   

*   Impacts of the ball with the ends of the case
*   Elastic impacts of the pivots on the rail    

The modeling assumptions are:   

+   Friction between the pivots and the rail in negligible, hence it is ignored
+   The ball is considered a point mass moving without friction within the case
+   All impacts are elastic
+   Pivots are triangular prisms, instead of cylinders (to make impacts less cumbersome to handle)


####Coordinate Frames of the Modeled System:   
![Frames](http://i.imgur.com/qZo6kcu.png?1)

####Problem Set-Up and Simulation:   
The inertial and geometric parameters were sourced from [this paper](http://www.inm.uni-stuttgart.de/mitarbeiter/leine/papers/journal_publications/Leine_x_van_Campen_x_Glocker_-_Nonlinear_Dynamics_and_Modeling_of_Various_Wooden_Toys_with_Impact_and_Friction.pdf) by Leine, Campen, and Glocker, which used the Newton-Euler approach to solve this system. I instead used the Variational Principle (Lagrangian Dynamics) to get the equations of motion.   

The kinetic energy of the case and the ball were written in terms of their body velocities. The potential energy of each was found using homogeneous transformations to represent height in world frame. With the Lagrangian now available, 4 Euler-Lagrange equations (one corresponding to each state variable) were written. The RHS of each eq. was 0, since the only constraint (ball’s translation confined to the case) is embedded within the transformation matrix from the case to the ball, and there are no external forces.   

Mathematica's *NDSolve* is used to numerically integrate the Euler-Lagrange equations, with arbitrarily chosen initial conditions. However, this integration is done piecewise, since it is stopped whenever an impact is detected (using *WhenEvent*). 8 impact conditions were checked at every instant (6 from the vertices of the two triangular pivots, and 2 from the ball hitting the case’s ends). After every impact, new initial conditions for integration are found by solving the impact laws of momentum change and Hamiltonian conservation (giving 5 eqs.). It is assumed that at any instant only one impact condition from the 8 is true.   
