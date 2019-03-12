
# PID Control Project
This is a C++ project to create a **Proportional Integral Derivative** controller or **PID**, in order to drive a simulated car. A Proportional Integral Derivative controller is a control loop feedback mechanism widely used in industrial control systems and a variety of other applications requiring continuously modulated control. A PID controller continuously calculates an error value as the difference between a desired setpoint (SP) and a measured process variable (PV) and applies a correction based on proportional, integral, and derivative terms (denoted P, I, and D respectively).


The simulator provides cross-track error CTE, speed, and steering angle data via local websocket. With the continuous stream of data the PID controller responds with steering and throttle commands driving the car reliably around the simulator track. The project involves implementing the controller primarily for the steering angle of the car (although I used the value from this controller to also determine throttle), as well as tuning coefficients for each PID value in order to calculate a steering angle that keeps the car on the track. 

## Proportional control
**P** is the component that is proportional to the current track error. It has direct impact on the trajectory because it makes the car to “correct” in the same proportion of the error in the opposite direction. There’s a natural overshooting effect that will cause the car to swivel hard left and right eventually driving the car off-track. See a video below of PID controller with I and D = 0, using only the proportional control.
 
![Image of proportional control](images/proportionalcontroller.gif)

PID controller with **I** and **D = 0**, using only the proportional control, causes the car to oscillate or stir rapidly from left to right which is not desirable for controlling a self driving car.


## Integral control
**I** is the integral control, **I** considers all past values of the CTE and it’s measured by the integral or the sum of the crosstrack errors over time. The reason we need it is that there’s likely residual error after applying the proportional control. This ends up causing a bias over a long period of time that avoids the car to get in the exact trajectory. This integral term seeks to eliminate this residual error by adding a historic cumulative value of the error.

![Image of integral control](images/integralcontroller.gif)

PID controller with **P** and **D = 0**, using only the Integral control, causes the car to stir with the wrong trajectory which is not desirable for controlling a self driving car.


## Derivative control
**D** is the Derivative control, **D** is the best estimate of the future trend of the error, based on its current rate of change. A way to cancel the overshoot effect is to introduce a temporal derivative of the CTE. When the car has turned enough to reduce the crosstrack error, **D** will inform the controller that the error has already reduced. As the error becomes smaller over time the counter steering won’t be as sharp helping the converge the movement to the target trajectory.

![Image of Derivative](images/derivativecontroller.gif)

PID controller with **P** and **I** = 0, using only the Derivative control, the car drives reasonably well for a short distance and then drives off the track which is not desirable for controlling a self driving car.

## Choosing the Hyperparameters

How I chose the optimal coefficient values for our PID controller. Picking a coefficient that increases or decreases too much P, I or D can cause the car trajectory to go awfully wrong.

There are a few approaches to iterate over different values and get to optimal hyperparameters.

The twiddle algorithm, also known as “coordinate ascent” is a generic algorithm that tries to find a good choice of parameters for an algorithm that returns an error. Unfortunatly, the controller with resulted parameters was able to drive car around the track but with a lot of wobbling. Hence the need to tune the parameters manually by try-and-error process.

## Final simulation

I implemented the final PID controller with **P = 0.15, I = 0.0 and D = 2.5**. The full video of driving around the track is in the videos folder (pidcontroller.mp4)

![Image of PID Controller](images/pidcontroller.gif)


## Comments 
PID controller can also be achieved using **Deep Reinforcement Learning** what would very interesting is to see how this compares with this project approach and what is actually the best strategy for a self driving car.
