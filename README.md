# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

---

## Rubric Points

### The Model

A Model Predictive Controller (MPC) attempts to guide a vehicle along a desired path by repeatedly fitting a polynomial curve to several waypoints that lie on the desired path.  At each timestep, a new curve is fit to a new set of waypoints.  (For this particular model, each new curve is fit to the 10 nearest waypoints).  This approach allow the controller to adjust its control actuators (steering angle and acceleration) every timestep while at the same time taking into account future changes in road curviture.  

The model used for this project was a kinematic model that included the vehicle's position in cartesian coordinates (x, y), its orientation (psi), its speed (v), current cross-track-error (cte) and error in the orientation angle (epsi).  Kinematic models are less accurate than dynamic models but they are easier to implement.  The update equations are shown below:

![Kinematic Equations](./img/equations_1.png)


### Timestep Length and Elapsed Duration (N & dt)

The final values chosen for the timestep length and elapsed duration were 12 steps and 0.08 seconds respectively.  (I started with suggested values of 10 and 0.1 and modified each value several times in both directions by small increments.) 

If the timestep length is increased too much, the polynomial curves do not fit well which causes the vehicle to understeer.  If the timestep length is decreased too much, the model can't update quickly enough. Decreasing the elapsed duration can lead to erradically-shaped (too "curvy") polynomial curves. Increasing elapsed duration too much results in slow reaction to changes in the road.


### Polynomial Fitting and MPC Preprocessing 

The curve used for this project was a 3rd-order polynomial which provides accurate-enough approximations for road curvatures.  The curve fitting involves minimizing the "cost function".  The cost function penalizes different behaiviors like driving at innapropriate speeds, driving too far from the center lane, and making drastic changes to steering angle and acceleration from timestep to timestep. 

As suggested in Udacity's video walkthrough for the project, the waypoints were pre-processed using a coordinate transformation. The transformation had the effect of moving the car to the origin of the "world coordinate" reference frame. (Waypoints along a perfectly straight road would be oriented along the x-axis).  This makes fitting the curve easier and helps prevent problems such as the polynomial function having multiple outputs for a single input.  


### Model Predictive Control with Latency

Latency between the simulator and the controller of 100ms was handled by using the state of the current time along with the amount of time (dt) from timestep to timestep to predict the future state of the vehicle.  Standard kinematic equations for position, velocity and change in time were used to forecst the vehicles position for the next timestep.  


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.

* **Ipopt and CppAD:** Please refer to [this document](https://github.com/udacity/CarND-MPC-Project/blob/master/install_Ipopt_CppAD.md) for installation instructions.
* [Eigen](http://eigen.tuxfamily.org/index.php?title=Main_Page). This is already part of the repo so you shouldn't have to worry about it.
* Simulator. You can download these from the [releases tab](https://github.com/udacity/self-driving-car-sim/releases).
* Not a dependency but read the [DATA.md](./DATA.md) for a description of the data sent back from the simulator.


## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./mpc`.

## Tips

1. It's recommended to test the MPC on basic examples to see if your implementation behaves as desired. One possible example
is the vehicle starting offset of a straight line (reference). If the MPC implementation is correct, after some number of timesteps
(not too many) it should find and track the reference line.
2. The `lake_track_waypoints.csv` file has the waypoints of the lake track. You could use this to fit polynomials and points and see of how well your model tracks curve. NOTE: This file might be not completely in sync with the simulator so your solution should NOT depend on it.
3. For visualization this C++ [matplotlib wrapper](https://github.com/lava/matplotlib-cpp) could be helpful.)
4.  Tips for setting up your environment are available [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)
5. **VM Latency:** Some students have reported differences in behavior using VM's ostensibly a result of latency.  Please let us know if issues arise as a result of a VM environment.

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/b1ff3be0-c904-438e-aad3-2b5379f0e0c3/concepts/1a2255a0-e23c-44cf-8d41-39b8a3c8264a)
for instructions and the project rubric.

