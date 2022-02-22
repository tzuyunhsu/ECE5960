# Lab 3: Sensors

For the prelab, I planned out the connections between two time-of-flight sensors and one IMU unit as the sketch shown below. Vin of the two ToF sensors and IMU are connected to the same Artemis input, so do all the other SDA, SCL, and ground connections. The shutdown pins of the time of flight sensors are connected to GPIO pins of the Artemis board because they will run on the same address at the same time, which is not ideal to control any one of them individually.   

/// wire picture

## Part A: Time of Flight Sensors
1. I soldered the connections of VL53L1X by using the qwiic cables based on the sketch from the prelab. Since we have to hook up more than one sensor to this connection, I twisted two wires together on both ends to fit into the electrical contact hole of the ToF sensor. The extra wire is used to daisy-chain the second ToF sensor and the IMU.  
/// one sensor pic
2. After soldering one ToF sensor to the Artemis, I ran the example wire file to test the I2C connection. It showed 0x29 detected in the serial monitor, while it is also followed by 'No device detected' in the next output. The address matched with my expectation, and the 'No device detected' was due to the lack of extra sensors in the I2C connection.  
/// i2c picture
3. Discuss the pros and cons of three distance range modes: short, medium, and long.   
  According to the datasheet, distance mode is a parameter to optimize the settings and tuning for the best ranging performance. The performance is evaluated by the potential application distance range and the ambient light conditions. The short distance mode has less effect from the ambient while the long one got affected the most by it. However, the long one has the maximum distance range (4 m), which is more suitable for the final robot application if the robot needs to move around freely in a room.
4. I chose the long distance mode to test the ToF sensor by running the ReadDistance example from the library. I taped the ToF sensor at the cover of my computer and measured the distance against a wooden door and a white wall. The measured distance demonstrated a linear relationship within the distance range of 2500 mm in both cases. The distance measured against a white wall tends to deviate a bit more from the actual distance than the wooden door case, which might be due to the light reflection from the wall, but the overall difference is not obvious. The accuracy of both cases is mostly lower than 4 % in the measurement range, while the wood case gets the distance data more precise as the disance goes longer. This can be explained by the effect from the long distance range mode.  
/// read distance
/// accuracy
5. By using the wire schematics described in the prelab section, I hooked up two ToF sensors and ran the ReadDistance example to prove they work as expected. Since two ToF sensors would share the same address, the shutdown pins of the ToF sensors are connected to GPIO pin 6 and 8 respectively to control the access to data at a time. In the video, I taped both sensors at the cover of my computer, and I set GPIO 6 to HIGH, GPIO 8 to LOW. GPIO 6 controls the shutdown pin of the sensor in the left when viewing from the side facing the sensors. As I blocked the left sensor with a black object, the distance detected decreased. On the other hand, nothing happened to the distance reading when I blocked the right sensor. I tested and got the same outcome from testing the right sensor.  
/// shutdown pin code
/// 2 sensor video

### Additional Tasks
1. Infrared-based distance sensors comparison.  
  (1) Amplitude-based IR sensor, such as VCNL4040, includes an infrared LED to transmit signal to the environment. The proximity sensor receives the reflected signal from the object, and calculates the distance between the sensor and the object. It is mostly a short-range sensor. Also, it is cheap and only requires simple circuitry for applications, but it is sensitive to surface colors, textures, and ambient light.   
  (2) Triangulation-based IR sensor measures the distance by calculating the difference in angles between the light reflected and the sensor surface. It is less sensitive to colors, textures, and ambient light compared to the amplitude-based sensors, but the cost per each is higher.  
  (3) Time of flight sensor emits a pulse modulated signal and records the time until the signal returns. It is mostly insensitive to colors, textures, and ambient light, and it collects different outputs other than distance measurements. 
 
 2. Timing budget is the time required to perform one range measurement, and the intermeasurement period is the time delay between two ranging measurements. If the intermeasurement period is shorter than the timing budget, the next ranging operation runs immediately after the current ranging measurement completes. I tried the SetIntermesaurementPeriod example from the VL53L1X library to adjust different values for both timing budget and intermeasurement period. I first set both the timing budget and the intermeasurement period to 1 ms, which means the next measurment would start ranging right after current operation, and the ranging time period is 1 ms. The serial monitor failed to print out the intermeasurement period because it is lower shorter than the minimum timing budget. Then, I set it to 20 ms, the minimum of the timing budget, and the measurements run smoothly. The ranging is at 50 Hz under this setting, which is reasonable for the anticipated robot speed.  
/// timing code
 3.  In this section, I explored the limit checking settings from signal and sigma. The signal rate measurement represents the amplitude of the signal reflected from the target and detected by the device. As for the sigma, it represents the estimation of the standard deviation of the measurement. By running the StatusAndRate example, I checked the signal rate and range status while changing the distance in front of the sensor. Mostly, the sensor is in good status when I tried to decrease the sensing distance by covering it by my finger. However, the status failed frequently if I tried to rotate the sensor. This might affect the sensing when the robot is doing stunt in the future, and the need to secure the wiring and sensor in place is definitely necessary.  
/// status video

## Part B: IMU
1. I soldered the IMU (ICM20948) to the wires that are already connected with the ToF sensors in part a. Below shows the complete wiring between the Artemis board, two ToF sensors, and one IMU unit.  
/// full wire picture
2. I ran the Example1_Basics to test the functionality of the IMU. AD0_VAL represents the LSB of the 7-bit address of ICM-20948. By changing it from 1 (default) to 0, the address of the IMU unit switches from 0x69 to 0x68. I set this value to 0 since the IMU is hooked up with other sensor units. It needs the updated address to function with other sensors for communication. 
  From the serial plotter in arduino, the sensor data from accelerometer changed drastically as I rotated or flipped the sensor. The sign of the output changed from positive to negative as I rotated it 180 degrees. As I accelerated the board, the accelerometer data showed a couple spikes in the output. However, the data from gyroscope didn't change much during the whole testing. It only showed some huge change in the output right at the time I rotated or flipped the board.  
  /// acc, gyro video

### (a) Accelerometer
1. Refering to the material from the previous lecture, I found pitch by taking atan2 of the accX() and accZ() data and roll by taking atan2 of the accY() and accZ() data. As I changed the angle of the board, the changes in pitch and roll changed as accordingly to my movement. Pitch data goes from -90 degrees to 90 degrees. The step in the middle is supposed to be 0 degree. However, the board seemed to be held slightly tilted against the table when I measured the data, so the outcome showed -10 degrees instaed as shown in the figure.  
/// pitch and roll pic. 
//accuracy: do standard deviation when the sensor is stable!!!!  
2. /// increase the measuring freq. 

### (b) Gyroscope
1. 

### Additional Tasks
