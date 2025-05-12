# GPS-Position-Prediction-and-correction-using-Kalman-Filter
# Introduction
Various uncontrollable and unpredictable factors ( e.g., atmospheric disturbances, failure of the GPS antenna, electromagnetic interference, weather change, GPS signal attack, or solar activity) may cause GPS receivers to lose signal occasionally, even if their antennas are placed in a location with an unobstructed view of the satellites. Kalman Filter is an iterative mathematical process that quickly estimates one’s true position by using a set of equations and consecutive data inputs. In order to reduce errors made by a GPS in prediction of a vehicle’s position, linear Kalman Filter and non-linear Kalman Filter algorithms are used. In addition to this, u-Blox Neo6M GPS module is used to measure the vehicle’s position. Data from a triaxial accelerometer - ADXL345, triaxial magnetometer - QMC5833L and triaxial gyroscope - MPU6050 are used. With this data we were able to measure the heading of the object and make more accurate predictions.. Kalman FIlter is a three stage process and it provides low cost and accurate estimation of the object's position. The model was tested in real time on Raspberry Pi 3 and the predicted results were plotted on Google Maps.
## Proposed System
The proposed system consists of a triaxial Accelerometer, Gyroscope and Magnetometer along with position and velocity measured by the GPS module. Raspberry Pi 3 is used to receive the sensor readings and perform real time analysis. An adaptive filter called Kalman Filter is used to predict and correct the state vector. Initially, the diagonal elements in the covariance matrix used in the Kalman Filter are populated with the standard deviation values of the state variables. As the number of iterations increases, the filter becomes more confident about its’ prediction and the standard deviation is reduced.

![image1](https://github.com/user-attachments/assets/7edf2f33-d6fa-47c1-8b11-d2e6f007b531)
# Linear Kalman Filter
The Kalman Filtering process involves 3 major steps: Predict, Correct and Update.

![image2](https://github.com/user-attachments/assets/7fec7432-1bb5-41af-8d82-4895f7bdcf31)

Above is the flow diagram of a typical Kalman Filter
It’s a discrete-time process. In the predict stage, the algorithm makes the best possible prediction of the next state with the knowledge of physics. All the variables that are used in the Linear Kalman Filter exhibit linear relation. To increase the accuracy of the system, we take the orientation of the body into consideration. This requires modification in the algorithm to handle the nonlinearities.
Below is the flow diagram of the the equations involved in the Kalman Filter process.
![image3](https://github.com/user-attachments/assets/a49a9735-2e9c-4ecc-bab4-61e2a741d6a2)

# Non-Linear Kalman Filter 
The proposed non-linear Kalman Filter system consists of two main stages. In the first stage, the orientation of the object is estimated and in the second stage, the Kalman Filter uses the estimated angles from the first step to estimate the position.
![image44](https://github.com/user-attachments/assets/564c38a1-0574-44e1-9efa-bbe108b81701)
In the first stage, we estimate the Euler angles, roll(θ), pitch(φ) and yaw(Ψ), to estimate the orientation of the object. These angles are used to transform vectors from body frame to the navigation frame. The Gyroscope gives the angular speed which can be integrated to obtain Euler angles (g_r, g_p, g_y) roll(Θg), pitch(Φg) and yaw(Ψg). The Magnetometer and Accelerometer’s readings are fused together by sensor fusion [6] to obtain Euler angles (f_r, f_p, f_y) roll(Θf), pitch(Φf) and yaw(Ψf).

**Set 1 : Fusion Euler angles (Θf, Φf, Ψf) :**


