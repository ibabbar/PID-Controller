
# PID Control Project
This is a C++ project to create a **Proportional Integral Derivative** controller or **PID**, in order to drive a simulated car around a track. A Proportional Integral Derivative controller is a control loop feedback mechanism widely used in industrial control systems and a variety of other applications requiring continuously modulated control. A PID controller continuously calculates an error value as the difference between a desired setpoint (SP) and a measured process variable (PV) and applies a correction based on proportional, integral, and derivative terms (denoted P, I, and D respectively).


The simulator provides cross-track error CTE, speed, and steering angle data via local websocket. With the continuous stream of data the PID controller responds with steering and throttle commands driving the car reliably around the simulator track. The project involves implementing the controller primarily for the steering angle of the car (although I used the value from this controller to also determine throttle), as well as tuning coefficients for each PID value in order to calculate a steering angle that keeps the car on the track. 

## Proportional control
**P** is the component that is proportional to the current track error. It has direct impact on the trajectory because it makes the car to “correct” in the same proportion of the error in the opposite direction. There’s a natural overshooting effect that will cause the car to swivel hard left and right eventually driving the car off-track. See a video below of PID controller with I and D = 0, using only the proportional control.
 
![Image of proportional control](images/proportionalcontroller.gif)