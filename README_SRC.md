This file explains how I implement the present version of particle filter algorithm. It also reveals my commends on the starter code of this project.

### 1. How do I set the number of particles?

Recall that the distribution of particles at any time represents the possiblility of location of the vehicle at the present time. With more detail, where there accumulates more particles, the higher possiblility for this place to be the vehicle's location. Therefore, the number of particles should depend on the following aspects.

a. the initial distribution of position of vehicle: Gaussian 

b. the dimension of the map: 2D

c. The accuracy of iteration

Here, we used a 2-D normal distribution (based on GPS measurement) as the initial distribution of the vehicle. On the other hand, since we later use measurement of distances from landmarks instead, which has not worse accuracy than GPS (in the code, they are the same), to predict the location of the vehicle, one can believe that the distribution will have higher and higher peak as time goes by. 

Sum up, we will set the number of particles num_particles = 8 * 17 = 136.

### 2. Explaination about the implementation of the function: void ParticleFilter::prediction(double delta_t, double std_pos[], double velocity, double yaw_rate).

a. In my opinion, the implemetation heavily depends on which value of std_pos[] are we going to use. Logically, the predicted location of vehicle is the present position plus the displacement in the time slot delta_t. The noises come in in the step of displacement as the noise of turn and noise of move. However, if we check the places where we used this function in main.cpp, the real parameter used here is sigma_pos-the GPS measurement uncertainty. This usage is misleading, in my opinion. GPS measurement uncertainty should has nothing to do with prediction step. I guess it is because of the measurement uncertainty has the same value as GPS measurement uncertainty in our simulator, Tiffany then used GPS measurement uncertainty directly without change to another name. I also checked the values of previous_velocity and previous_yawrate. They were mostly below 10 and 0.08, respectively. Notice that std_pos = [0.3, 0.3, 0.01]. So it is not good to use them as a noise of turn or move. Otherwise, we will get a bad prediction. For example, distance of the displacement is approximately previous_velocity * delta_t = 1, where std_pos[0] = 0.3, accounted for around 30%.

In the end, we decide to add the random Gaussian noises, if we still use sigma_pos as the input value of the parameter std_pos, after we finished the addition of present location and the displacement whose calculation is based only on previous_velocity and previous_yawrate. 

b. The prediction step will be carried out for every single particle. For a fixed particle, we will first decide whether yaw_rate should be considered as zero. Then split the process into two cases according to the judge result. Notice that we acctually use the same yaw_rate for every particle during one iteration. Therefore, if we can make clear the relation between yaw_rate and zero before one goes to the for-loop of particles, it will save some time.






b. Transformation
