## Lab 3: Sensors

Before the lab, I planned out the connections between two time-of-flight sensors and one IMU unit as the messy sketch shown below. The wires are used based on the color coding, but I used green wires to connect to ground instead since I don't have black wires at the time.

/// wire picture

# Part A: Time of Flight Sensors
1. For the hardware, I soldered the connections on VL53L1X with the qwiic cables.....
2. Discuss the pros and cons of three distance modes: short, medium, and long.  
  According to the datasheet, distance mode is a parameter to optimize the settings and tuning for the best ranging performance. The performance is evaluated by the potential application distance range and the ambient light conditions. The short one has less effect from the ambient while the long one got affected the most by it. However, the long one has the maximum distance range, which is more suitable for the final robot application if the robot needs to move around freely in a room.

# Part B: IMU
1. //Setup
2. I ran the Example1_Basics to test the functionality of the IMU. AD0_VAL represents the LSB of the 7-bit address of ICM-20948. By changing it from 1 (default) to 0, the address of the IMU unit switches from 0x69 to 0x68. I set this value to 0 since the IMU is hooked up with other sensor units, it needs the updated address to function with other sensors in communication. 
  The values for x, y, and z axes of acclerometer change from time to time along with the signs as I flip or rotate the board. The values from gyroscope also change as the motion continues, but it is not as obvious as the acclerometer. 

# (a) Accelerometer
1. //What I got on arduino serial is pitch: 1.4, roll: 0; by calculation pitch: -0.7854, roll: 0
