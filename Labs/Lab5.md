# Lab 5: Open Loop Control

## Objective
In this lab, we want to change from manual to open loop control of the car, which means the car motion will be pre-programmed to perform the required tasks. We learn how to program motor drivers to control the car movement and understand the effectiveness of the PWM singals.

## Prelab
We intend to parallel couple the two inputs and outputs on each motor driver, which means we use two channels to drive one single motor. This enables us to drive twice the average average current without overheating the chip. As for the circuit design, I soldered A, B channels with the same input and output numbers together. Since we need to drive pwm signal from the Artemis board to the motor drvier, I chose GPIO pin 9, 11, 12, 13 to connect to the inputs of two motor drivers respectively. It's also better to power the Artemis board and the motor drivers/motors from separate batteries since the range of the voltage required to power up the motors is higher than the voltage for Artemis board. If they use the same battery, the Artemis board may fail.

## Lab Procedure
1. First, I connect the power and signal input to a motor driver and use analogWrite to generate PWM signals from the Artemis board. For now, dc power supply is used to power up the motor driver for debugging feasibility. I use GPIO pin 9 and 11 to generate PWM signal from 0 to 255, and then ramps it down. As shown in the video, the duty cycle goes from 0 to 100% and decreases back to 0% gradually in the oscilloscope.  

/// pwm video

2. I connected one motor driver to a motor, and power it from an external power supply to see the outcome of driving a motor from the current code. At first, I only heard buzzing sound but the wheels did't rotate at all. I realized the voltage may not be high enough to drive the motor, so after I ramped the power supply up to 6 V, the wheels started to rotate. This proved that the wiring is all good, so the next step is to power the motor with the battery.  

/// wheel video

3. After checking that both motor drivers function as expected, I took the car apart to remove the unwanted parts and planned out the wire paths to hook up all the components. I removed the original PCB board and the wires connected to the LEDs and motors since I'll replace the PCB with the Artemis board. Motor drivers are connected to the Artemis board as shown in the prelab circuit design, and the VIN and GND pins for the power source are powered by a 850 mAh battery. The Artemis board is placed in the slot where the PCB board originally sat in and is powered a 650 mAh battery on its own. Also, I resoldered some parts to adjust the length of the wires to fit onto the car stably. Tapes are used to fasten all connections, including the previous sensors, in place while testing.  

///car pic

4. When the whole car can be powered by batteries, I wrote a simple move forward code to test the lower limit for which each motor still turns while on the ground. It turned out that the pwm input needs to be at least 42 or 43 to drive the car to move as expected. Sometimes 40 stills works, but the outcome is unstable and may only drive one side of the wheels.  

5. I recorded a short video to perform that the car can move at a fairly straight line at low speed. It drifted a little bit because the starting time of the two wheels differed at such low signal. The car can move straight easily at a higher speed, which will be shown in the next video.  

/// slow car

6. 


## Additional Tasks
1. According to the Arduino documentation, 
