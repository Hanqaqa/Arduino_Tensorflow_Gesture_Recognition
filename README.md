# Arduino Tensorflow Gesture Recognition
In this project I use the newly added **TensorFlow Lite** support for 32 bit Arduino boards (not all Arduino boards are 32 bit, in fact, most of them are 8 bit).

I made a quick neural network that recognised several fighting movements such as uppercut, punch, slash and stabbing using the **Arduino Nano 33 BLE Sense** which already includes a gyro and accelerometer sensor.

Each movement was first captured 10 times using the first script `IMU_Capture` with the Arduino IDE and then saving that gyro data into different `.csv` file.

Then the Neural Network was trained on my desktop computer using the `Tensorflow_to_TensorflowLite_Converter` which uses the `.csv` files we obtained before as training examples for the Neural Network. When we have trained the Network, we convert it to a `model.h` file that can be compiled for an Arduino.

Lastly we attach that `model.h` file into the `IMU_Classifier` which already has all the TensorFlow Lite instructions that will predict the movement

In this youtube video we can see it working in real time https://www.youtube.com/watch?v=UlTW5wlSB3c 

Let's get a bit more in depth with each 
