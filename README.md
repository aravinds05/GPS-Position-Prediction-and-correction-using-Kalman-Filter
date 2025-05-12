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

![image5](https://github.com/user-attachments/assets/d74c2d41-487b-4d07-a639-9acae11d3c0e)

**Set 2 : Gyroscope Euler angles (Θg, Φg, Ψg) :**

![iamge6](https://github.com/user-attachments/assets/23bbe15c-ae83-453b-9166-7a2d0d9131b6)

These values are fed to a Kalman filter by considering fusion angles as the observed values and gyroscope angles as the measured values. The equations are modified as

![iamge7](https://github.com/user-attachments/assets/5ef7ef93-441a-44e6-92a1-7c646b448e4a)

The estimated Euler angels are used to construct rotation matrices

![image8](https://github.com/user-attachments/assets/1672424a-5de5-4ae2-a7fb-904a145ccfd9)

From theses matrices, we can find Direction Cosine Matrix (DCM) by matrix multiplication.

![iamge9](https://github.com/user-attachments/assets/43038557-d17c-4630-ac75-3442cec34679)

In the second stage, we estimate the position and velocity of the object. The prediction stage is modified as

![image10](https://github.com/user-attachments/assets/f517a4e0-5ccb-4c14-8d99-2531d112ea65)

And the correct and update stage is modified as

![iamge11](https://github.com/user-attachments/assets/681e2216-be5b-4d43-8564-f86f69f1cd87)

## Results
We first tested the algorithm on pre-recorded test data. After obtaining the prediction, we plotted it on Google Earth to compare results.

![image12](https://github.com/user-attachments/assets/b6625422-67ff-4302-8382-5a9899bc66b5)

The above figure is the position plot using the test data on Google earth. the red dots are the faulty GPS measurements, the white dots are the predictions made by the linear Kalman Filter and the yellow dots are the predictions made by our proposed system.

![image13](https://github.com/user-attachments/assets/acc9e43d-b3c5-41ee-a195-7d3f06018c0f)

![image14](https://github.com/user-attachments/assets/fa5973f0-bece-4dda-bf19-066e598bbc8d)

In the above plots, the Blue line indicates Kalman filter prediction, the Orange line indicates Measured values and the Green line indicates current Corrected values. Following are the Euler angle plots obtained while predicting the position using the above system.

![image15](https://github.com/user-attachments/assets/aa548450-f9e2-44d3-ac9a-346521cb044e)
![image16](https://github.com/user-attachments/assets/d1000833-2e05-402b-a8bf-d86e4dfbb443)
![iamge17](https://github.com/user-attachments/assets/8273343f-fff6-40f7-9606-b4d85d220a33)

In the above plots, the Blue line indicates Kalman filter prediction, the Orange line indicates Measured values and the Green line indicates current Corrected values. We can observe that the Kalman Filter predicted line closely follows the Fusion angles which is more gradual and smooth compared to the Gyroscope angles. After testing the algorithm on test data, we tested it in real time. The sensor readings and the GPS readings were read, processed and plotted on Google maps in real time.

![image18](https://github.com/user-attachments/assets/479e81f9-040b-43fd-b6b7-f9b567b84e7c)

In the above plot, the green line is the predicted position by our algorithm and the red line the measured position by the GPS module. After ending the program, the data was saved in a csv file for further analysis. Following is the position plot.

![image19](https://github.com/user-attachments/assets/a08bd9c0-b2cd-4f67-b473-c016f5fdb54f)

The Orange line indicates the GPS measured data and the Blue line indicates the Kalman Filter predicted data. We can observe some spikes in the GPS measured data. This is rectified by considering the previously predicted value as current value if the error is greater than 50 m. Following are the velocity plots obtained during real time prediction.

![image20](https://github.com/user-attachments/assets/f16a38ef-9df6-4b6e-ba1c-fd638b2bbea4)
![image21](https://github.com/user-attachments/assets/6da0ffb5-8de5-4f89-b1a8-d98495ea6b07)
![iamge22](https://github.com/user-attachments/assets/65e15506-2127-4172-b378-300024a8b0cd)

# Conclusion

Kalman filters is a very powerful tool and is used in a variety of state estimation systems. They’re able to make accurate predictions and is still computationally cheap. Methods such as lane tracking, and traffic sign localization together with map matching can be used along with sensor fusion to make more accurate prediction. The GPS devices used in systems are of high grade and expensive. It is possible to arrive at a low cost solution using a cheaper GPS module and IMU measurements with the help of the Kalman Filters.

















