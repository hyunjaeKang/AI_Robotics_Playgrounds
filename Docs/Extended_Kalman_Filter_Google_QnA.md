

## What is dead reckoning in extended kalman filter


In an Extended Kalman Filter (EKF), dead reckoning is a process of estimating a system's (like a robot's) position based on its previous position, velocity, and heading from onboard sensors like gyroscopes and odometers. The EKF uses dead reckoning as a prediction step, which provides an initial estimate of the next state. Dead reckoning alone accumulates errors, but the EKF corrects this by fusing the dead reckoning estimate with measurements from other sensors, such as GPS or laser range finders, to provide a more accurate and stable final state estimate. [1, 2, 3]  


### How it works 

• Prediction (Dead Reckoning): The process starts by using onboard sensors (like IMUs or wheel encoders) to predict the new position based on the last known position and movement data. This is the "dead reckoning" part. The equations are often based on a kinematic model of the robot. 
• Error Accumulation: Because dead reckoning relies only on onboard sensors, errors from each sensor reading accumulate over time. This causes the estimated position to drift away from the true position. 
• Correction (Kalman Filter): The EKF then uses measurements from a more absolute sensor (like GPS) or a landmark-based sensor to correct the error. 
• Sensor Fusion: The EKF mathematically combines the "predicted" state from dead reckoning with the "measured" state from the absolute sensor to produce an optimal, combined estimate of the true position. It weighs the confidence in each measurement based on their estimated uncertainty. [1, 2, 3, 4]  

### Why it's used in EKFs 

• To provide a prediction: The dead reckoning component is essential because it provides a continuous estimate of the system's state between absolute measurements. 
• To handle sensor imperfections: It allows the filter to work even when absolute positioning sensors (like GPS) are temporarily unavailable or inaccurate. 
• To incorporate non-linear models: The "Extended" part of the EKF is crucial because it allows the system to handle the non-linear equations that often arise from a robot's kinematic model, which is the mathematical description of how the robot moves. [1, 2, 5]  

AI responses may include mistakes.

[1] https://ieeexplore.ieee.org/document/1338645/
[2] https://www.youtube.com/watch?v=r5Mnxb6LnhI
[3] http://www.cs.rpi.edu/courses/spring01/mobile-robotics/handouts/assign2.pdf
[4] https://pointonenav.com/news/why-you-need-dead-reckoning-top-applications-for-ins/
[5] https://www.intermodalics.ai/blog/real-time-robotics-the-extended-kalman-filter

-----

## What is difference dead reckoning and estimated in extended kalman filter?

In an Extended Kalman Filter (EKF), dead reckoning is the "predict" step, using a dynamic model to estimate the system's next state based on its current state and known physics (e.g., motion). Estimated (or updated) position is the result of the "update" step, which corrects the dead reckoning prediction by fusing it with noisy sensor measurements, using the Kalman gain to weigh the two sources of information and reduce accumulated errors. [1, 2, 3]  

### Dead reckoning (Predict step) 

• What it is: A method of calculating a system's position based on its last known position, speed, and direction over a period of time. 
• Function within EKF: This is the prediction phase, where the EKF uses the system's motion model to project the state forward to the next time step. 
• Pros: Provides a smooth estimate of movement, especially when no external sensors are available. 
• Cons: Errors from sensor noise (like from an IMU) accumulate over time, leading to drift and an inaccurate final position. [1, 3, 4, 5, 6]  

### Estimated (Update step) 

• What it is: The final, corrected state of the system after the EKF has combined the dead reckoning prediction with an actual sensor measurement. 
• Function within EKF: This is the update phase. The EKF takes the predicted position (from dead reckoning) and a new measurement (e.g., from a GPS) and blends them together. 
• Pros: The final estimate is more accurate than dead reckoning alone because it corrects for the accumulated error by using the absolute or more reliable measurement. 
• Cons: The accuracy of the estimate depends on the quality of the measurement, the correctness of the motion model, and the Kalman gain, which is calculated based on the uncertainties (covariances) of the prediction and the measurement. [1, 2, 4, 7]  

AI responses may include mistakes.

[1] https://en.wikipedia.org/wiki/Kalman_filter
[2] https://www.sciencedirect.com/topics/computer-science/dead-reckoning
[3] https://www.cavliwireless.com/blog/not-mini/what-is-dead-reckoning
[4] https://ucb-ee106.github.io/106b-sp23site/assets/proj/proj3.pdf
[5] https://www.sciencedirect.com/topics/earth-and-planetary-sciences/dead-reckoning
[6] https://ietresearch.onlinelibrary.wiley.com/doi/10.1049/iet-rsn.2020.0155
[7] http://www.cs.rpi.edu/courses/spring01/mobile-robotics/handouts/assign2.pdf


----

## What is dead reckoning in extended kalman filter and Kalman filter

In both the Extended Kalman Filter (EKF) and the Kalman Filter, dead reckoning is the prediction step, where an object's next position is estimated by using its last known location, velocity, and a dynamic model of its movement. The Kalman filter then performs an update step by fusing this prediction with new, noisy sensor measurements, like GPS, to correct the accumulated errors inherent in dead reckoning and produce a more accurate estimate. [1, 2, 3]  
Dead reckoning 

• What it is: A classical navigation method that estimates a system's current state based on its previous state and a dynamic model of motion (e.g., velocity, acceleration, wheel rotation). 
• How it works: It projects the system's position forward in time, using a set of inputs and a motion model, without using external position updates. 
• Limitations: It is prone to accumulating errors over time, which can lead to a drift in position estimates. [2, 3]  

How it's used in filters 

• Kalman Filter: Dead reckoning is the "predict" step. It takes the previous best estimate of the state and uses the system's dynamics model to predict the next state. 
• Extended Kalman Filter (EKF): Dead reckoning is also the "predict" step. The EKF is used for nonlinear systems, which are common in robotics and navigation. 

	• Prediction: The EKF's prediction step is based on the dead reckoning model. 
	• Update: The EKF then incorporates an external measurement (e.g., GPS, IMU) in its "update" step. Since the external measurements may be noisy and the dead reckoning prediction is prone to drift, the EKF combines them optimally to correct the error and provide a more accurate, fused estimate. [1, 3, 4, 5, 6]  

Example 

• Scenario: A truck's location is being tracked. 
• Dead Reckoning: Estimates the truck's new position by using its previous position and integrating its wheel speed and steering angle over a small time interval. This provides a smooth but eventually inaccurate estimate. 
• EKF/Kalman Filter: After the dead reckoning prediction, the EKF takes this prediction and the noisy GPS data, weighs them according to their respective uncertainties, and produces a more accurate final position estimate. The process then repeats for the next step. [1, 3, 7, 8, 9]  

AI responses may include mistakes.

[1] https://ieeexplore.ieee.org/document/1338645/
[2] https://ucb-ee106.github.io/106b-sp23site/assets/proj/proj3.pdf
[3] https://en.wikipedia.org/wiki/Kalman_filter
[4] https://arxiv.org/abs/2508.11396
[5] https://robotics.stackexchange.com/questions/7645/how-do-i-choose-the-best-filter-for-dead-reckoning-with-an-imu
[6] https://eureka.patsnap.com/article/kalman-filter-vs-extended-kalman-filter-which-one-to-choose
[7] https://en.wikipedia.org/wiki/Kalman_filter
[8] https://www.sciencedirect.com/science/article/pii/S187704281101439X/pdf?md5=3a7464ab924d499e63ca358c0c582255&pid=1-s2.0-S187704281101439X-main.pdf&_valck=1
[9] https://link.springer.com/article/10.1007/s11831-022-09815-7

