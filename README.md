# IoT-Project - Bacne Helper

## Introduction

It's an IoT project that can help people put medicine on their bacnes. By using the recognition and the robotic arm, the device can complete the whole process. And the user can watch the streaming by the user interface.

## Hardware Components

1. Raspberry Pi 3 *1
2. MG996R Servo Motor *6
3. 6-DOF Robotic Arm *1
4. Pi Camera *1
5. PCA9685 (I2C Interface) *1
6. Switching Power Supply *1 (with wires)
7. Dupont Lines *4 (at least)

## Preparation

### Raspberry Pi 3 

You need to install some packages to start this project.

`
pip3 install flask
`

`
pip3 install opencv-python
`

`
pip3 install adafruit-circuitpython-servokit
`

`
pip3 install azure-cognitiveservices-vision-customvision
`

`
pip install msrest
`

### MG996R Servo Motor

Before you put them onto the robotic arm, you can first test the way they operate by the code. It will first ask you enter a number between 0-180, and you will see the motor operating.
You can also use it to test a single motor after your arm is ready. I will explain it later.
The code is in "IoT_Project/servo_test.py")

### Switched Power Supply

It can provide the power that the motors need by transfer the AC to DC.
Because it connect to the socket directly so <font color="red"> please be careful</font>


![image](https://raw.githubusercontent.com/oohyuti/IoT-Project/main/Power%20Supply.JPG)

Tip : I stick the chopsticks on the wire and hold the chopsticks instead of the plug itself. So that if there is anything wrong, you won't get hurt.
![image](https://raw.githubusercontent.com/oohyuti/IoT-Project/main/plug.jpg)

### 6-DOF Robotic Arm

If you got the arm and the servo motors seperately, you can puzzle them up by [this link](https://www.taiwansensor.com.tw/6軸機械手臂組裝教學/).(Remember to double check the direction or you may need to reinstall them if you put them in the wrong directtion.

### Custom Vision

It is a service provided by Microsoft. You can use it to train your acne model and use the prediction key to call the API in your code. The thing you need to do is to look for all kinds of acne's picture and tag those acnes in the custom vision page.(https://blog.cavedu.com/2019/09/30/azure-custom-vision/).

## Connect the components - How it looks like

### STEP 1 : Put the pi camera on your raspberry pi 3

### STEP 2 : Connect the PCA9685 and your raspberry pi 3

You can check out the picture and you can put the 'vcc' to pinout 1(3v3 power), too.
<font color="red"> Please be careful! Don't insert the wrong pinout or it will easily burn out. </font>

![image](https://www.aranacorp.com/wp-content/uploads/16-channel-pwm-controller-pca9685-raspberry-pi_bb-1080x675.png)


### STEP 3 : Connect the servo motors and your PCA9685

There are sixteen positions that can provide the control of the servo motors. So you can insert the dupont lines one by one from the 0 position.

### STEP 4 : Connect the PCA9685 and the power supply

<font color="red"> Please be careful when you plug into the socket and don't touch the power supply when it's powered. </font>
![image](https://raw.githubusercontent.com/oohyuti/IoT-Project/main/PCA9685_Power%20Supply.jpg)

### You can watch the video [click here](https://www.youtube.com/watch?v=Q-PQdTYBZAw) to check if you have connected all the things right.

## Test the robotic arm

After connecting all the hardware components. You can now test your robotic arm by insert the init angles.
You can find the code in "IoT/init_servo_test.py"

You have to change the number on line 13 `INT_ANG  =[179, 0, 70, 0, 110, 90]`. Just insert the numbers you want to test and click run, and the motor will start to turn. Remember to keep your eyes on the devices when it is running. Because when the servo motors are installed onto the robotic arm, there will be some angle restrict to the motors so please be careful.

Remeber to record those angles that each motor can't reach. By this way, you can set up the minimum angles in your main program.

## Run the main program - server.py

Before you run the main program server.py. There is still something to set up.

### Custom Vision - Prediction Key

I've taken off my prediction key, endpoint, iteration name and project id in "server.py".
You can find all those data in the operation page of custom vision. (Click the right-top setting button and you will see the information.) Remember to publish the one you want to use as the recognition function.

### Revise the minimum and maximum restrict of robotic arm

In line30 and line31, I've set up my configuration, you should correct them with your record when your testing your robotic arm by the init_servo_test.py

### Now, you can run "server.py"

After you run the program, go to 127.0.0.1:5000 to see the user interface. If the console came up with some numbers or you see the green box, you can click the let's go button to see the arm moving.

## There's still something you should know

1. If the console comes up with ` [Errno 98] Address already in use` , it means that the program stopped accidently. 
You should then enter `netstat -tlnp|grep 5000` in the terminator. It will come up with the PID number that occupies the port number. And next you enter `kill <PID number>`.

2. If the program can't find the camera, you can change then number 0 to -1 in line81 `vc = cv2.VideoCapture(-1)`. It may help.

## [My Video](https://youtu.be/dkQ-8wHkPck)

## Reference Link

1. servo_test.py & init_servo_test.py : https://www.aranacorp.com/en/using-a-pca9685-module-with-raspberry-pi/
2. connect the PCA9685 and your raspberry pi 3 : https://www.aranacorp.com/wp-content/uploads/16-channel-pwm-controller-pca9685-raspberry-pi_bb-1080x675.png
3. server.py:
   https://dev.to/stratiteq/puffins-detection-with-azure-custom-vision-and-python-2ca5
   https://www.hackster.io/ruchir1674/video-streaming-on-flask-server-using-rpi-ef3d75
