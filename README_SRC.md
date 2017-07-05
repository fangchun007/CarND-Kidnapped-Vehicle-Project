This file explains how I implement the present version of particle filter algorithm. It also reveals my commends on the starter code of this project.

1. How do I set the number of particles.

Recall that the distribution of particles at any time represents the possiblility of location of the vehicle at the present time. With more detail, where there accumulates more particles, the higher possiblility for this place to be the vehicle's location. Therefore, the number of particles should depend on the following aspects.

a. the initial distribution of position of vehicle: Gaussian 
b. the dimension of the map: 2D
c. The accuracy of prediction
