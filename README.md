# Anomaly_Detection
This project is meant to demonstrate machine learning techniques for anomaly detection in vehicles
with the goal of acheiving robust predictive maintenance. The predictive maintenance will be useful 
in alerting vehicle repair technicians ahead of time of impending faults, and this could save lives. 
This project is useful for theme parks where several rides are operated multiple times in a day and 
safety is critical. Downtime is very low, and so a software doing some real time anomaly detection
would be the best maintenance strategy.

In this specific project, i have used a robot car to demonstrate anomaly detection in vehicles. This
robot car is the Pololu 3Pi+ 32U4 robot. 

![image](https://user-images.githubusercontent.com/31663476/144352296-b9932a4b-7978-4f8f-963b-809c3642a9be.png)

This robot contains 18 sensors on board, namely

- Line sensor1
- Line sensor 2
- Line sensor 3
- Line sensor 4
- Line sensor 5
- Crash sensor left
- Crash sensor right
- Gyroscope x-axis
- Gyroscope y-axis
- Gyroscope z-axis
- Accelerometer x-axis
- Accelerometer y-axis
- Accelerometer z-axis
- Magnetometer x-axis
- Magnetometer y-axis
- Magnetometer z-axis
- Speed sensor left
- Speed sensor right

The robot perfoms a line following function, which is essentially it has algorithm that uses
the line sensors to follow precisely a black colored line against a white background. Using 
some software written in Arduino IDE, as well as extra nrf52 microcontroller to add bluetooth
functionality to the device, i was able to collect data from the 18 sensors into a nice csv file
on a local computer, while the car was in motion. 

![image](https://user-images.githubusercontent.com/31663476/144353558-7d0b5e4a-b9ee-47e3-93de-326604ddb238.png)

The image above illustrates the setup that was used to collect the data. I used 3 different Pololu robots to
collect data. Each of them was fitted with an extra bluetooth capable chip which was relaying the data from 
the 18 sensors to a bluetooth gateway device connected to a local computer. The bluetooth gateway device would 
then send the values via Serial protocol to a C# application i designed to capture the values. The c# application 
then stores the value to a csv file.The sample rate was 20Hz, meaning i captures 20values per second from the 
moving car and these were streamed in real time via bluetooth to the local computer.

![image](https://user-images.githubusercontent.com/31663476/144354373-74fc990e-2bcf-4995-93b3-e9f0c70e7324.png)

The image above shows the format of a typical csv file obtained by using the described method.This data 
is then improted into Google Tensorflow which uses a python language and is available for free for any 
Google account holders. It is important to note that there were 2 sets of data obtained. Firstly, i 
obtained normal data which is when all parameters are nominal for a duration of 10minutes (training data).I then
obtained several sets of testing data by creating anomolous scenarios. Some of these included

- A blinded line sensor covered in duck tape
- Removed a rubber on one of the tyres leaving rim exposed
- Damaged a section of the track 

Once in Tensorflow, i then trained an autoencoder with 18 inputs corresponding to 
the 18 sensors. The autoencoder would then try to faithfully reproduce the 
input and measure some loss in the process. I set some thresholds for the loss, 
and those thresholds are used to categorize an anomaly from a normal data point. 
The images below show the appropriate training losses for the various sensors.

![image](https://user-images.githubusercontent.com/31663476/144355720-672883d2-d4e9-4245-a7db-a4a8a820b096.png)


