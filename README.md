# Arduino Tensorflow Gesture Recognition
In this project I use the newly added [TensorFlow Lite](https://www.tensorflow.org/lite/microcontrollers) support for 32 bit Arduino boards (not all Arduino boards are 32 bit, in fact, most of them are 8 bit).

I made a quick neural network that recognised several fighting movements such as uppercut, punch, slash and stabbing using the [Arduino Nano 33 BLE Sense](https://store.arduino.cc/arduino-nano-33-ble-sense) which already includes a gyro and accelerometer sensor. 

In this [youtube video](https://www.youtube.com/watch?v=UlTW5wlSB3c) we can see it working in real time.

This project takes inspiration from the one found in the Arduino Blog about training your own [Gesture Recognition Classifier](https://blog.arduino.cc/2019/10/15/get-started-with-machine-learning-on-arduino/)

Each movement was first captured 10 times using the first script `IMU_Capture` with the Arduino IDE and then saving that gyro data into different `.csv` file.

Then the Neural Network was trained on my desktop computer using the `Tensorflow_to_TensorflowLite_Converter` which uses the `.csv` files we obtained before, as training examples for the Neural Network. When we have trained the Network, we convert it to a `model.h` file that can be compiled for an Arduino or any other Microcontroller, such as an ESP32.

Lastly we attach that `model.h` file into the `IMU_Classifier` which already has all the TensorFlow Lite instructions that will predict the movement.

Let's get a bit more in depth with each step of the process:

## Prerequisites

### Python

We will need to have a 3.5.x to 3.7.x version of Python installed on our computer (Those are the only Python versions with TensorFlow 2.0 support by now). We will also need to have a 2.x version of TensorFlow in our computer, we can install it using the following command in our console or virtual enviorenment if we are using one:

`pip3 install tensorflow`

We will also be using a Jupyter Notebook in our second step, therefore we also have to install an instance of it:

`pip install jupyterlab`

### Arduino IDE

Since we are using a non standard Arduino board, the [Arduino Nano 33 BLE Sense](https://store.arduino.cc/arduino-nano-33-ble-sense), we need to add it to the Board Manager in our Arduino IDE in **Tools > Board > Boards Manager**

![Arduino Board Manager](https://lh4.googleusercontent.com/k88wXiRDpbadmTW1EreSetBJwHgMN3IP4skuTRywedIgp2aAWvzg3mqyDPZ2_fafH7tFXK-GtFwPEcnMAM0fqfa8XeYCc7orh6LXg4pD2_fKu1JQqw8LALMHfv6lFIBA3a_9pYeg)

We also have to add the library that reads the LSM9DS1 library in order to read from the IMU (Inertial Measurement Unit) inside **Tools > Manage Libraries**

![LSM9DS1 Library](https://lh6.googleusercontent.com/70d7uyocSAzU5B6ytAwSNCtsuguE64yHl_fDjND__IW5K8N2qnmXels6cgOAiKRzRT_4rfr-JyC3lPy5XOz29700xKLLSFOCdN6UEJiUMCfwNmZOXUOye5s7j6lV6VN93gJDNE32)

Then we can plug the Arduino Nano 33 to our computer's USB, choose the right board in **Tools > Board > Arduino Nano 33 BLE** and choose the right port.

Now we are ready to get into the first script.

## 1 IMU Capture

This short script simply starts recording all the gyro and accelerometer values into a comma separated format once the device goes over a certain `accelerationThreshold` defined at the beginning. Several tests have determined that `1.55` is a good empirical value to start recording movements with a certain force.

Once the device has gone over that threshold it will start outputting gyro and accelerommeter data at 100Hz for 119 samples in the Serial Port, found in **Tools > Port > Arduino Nano 33 BLE**. Then it will stop outputting samples into the serial port until another sudden movement goes over that threshold.

We will record 10 instances of each of the movements. We will copy what we get in the Serial port and copy it into different `.csv` files, one file for each movement. Those files have already been created and copied into the second part of this tutorial.

## 2 Tensorflow to TensorflowLite Converter

In this part we can either train online the Neural Netwrok using a [Colab created by Sandeep Mistry](https://colab.research.google.com/github/arduino/ArduinoTensorFlowLiteTutorials/blob/master/GestureToEmoji/arduino_tinyml_workshop.ipynb) or use the slightly modified version I provide and train it in our machine.



