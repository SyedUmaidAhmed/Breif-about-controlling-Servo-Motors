# Breif-about-controlling-Servo-Motors
The repository is an informative content to drive servo motors with Raspberry Pi's.

These are actually consist of a circuitry which receives the command and are responsible for their control at particular angle. The circuitry is placed inside the motor unit and it consists of a position able shaft which is fitted with a gear. The servo motor is rotated by an electrical signal which determines that at which angle it will rotate.

To know the working of the servo motor, we will have to take a look inside the servo unit. Inside the servo unit, there is a dc motor, control circuit and a potentiometer. The motor is attached to the control wheel with the help of gears. The potentiometer’s resistance changes with the rotation of motor. The control circuit determines that at which angle and in which direction it will rotate.

The actual and the desired position determine the speed of the servo motor. If the motor is near to its desired position, then it will move slowly and if the motor is far, then it will move fast. This is called proportional control.


import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BOARD)
GPIO.setup(11,GPIO.OUT)
pwm=GPIO.PWM(11,50)

#_The servos position is controlled by the pulsewidth of a 50 Hz PWM signal. Hence, we need to turn the PWM sequence on at 50 Hz. Note that for a 50 Hz signal, the Period of the signal is 1/50=.02 seconds, or 20 milliseconds. Keep this Period in mind as we will come back to it later. We start by creating a PWM object on Pin 11 with a 50 Hz signal with above command.

* Pulse Width, 1, 1.5 and 2 ---> Full Left, Middle and Full Right 

DutyCycle =PulseWidth/Period

Period = 1/frequency, 

so:
DutyCycle = PulseWidth/(1/frequency) = PulseWidth * frequency

For Example: The PulseWidth that will give us a full left position is 1 milllisecond. We now calculate the applied DutyCycle to give us the desired position:

DutyCycle = PulseWidth*frequency=.001 *50 = .05 = 5%


So, for a 50 Hz signal, if we set the DutyCycle to 5, then we should see the servo move to the full left position. Similarly, if we set DutyCycle to 7.5, we should get the middle position, and if we set it to 10 we should be in the full right position. You can get all the intermediate positions by linearly scaling between 5 and 10. Note that these values will vary between brands, and between individual servos, so play around with your servo to get it calibrated. We are now ready to apply a command to position the servo. If we want the servo in the full left position, we should set the DutyCycle to 5%. We do that with the command:

>>>pwm.start(5)

This will start the PWM signal, and will set it at 5%. Remember, we already specified the 50 Hz signal when we created the pwm object in our earlier commands. Now if we want to change the position, we can change the DutyCycle. For example, if we want to go to the middle position, we want a DutyCycle of 7.5, which we can get with the command:

>>>pwm.ChangeDutyCycle(7.5)

Now if we want the full right position, we want a duty cycle of 10, which we would get with the command:

>>>pwm.ChangeDutyCycle(10)

Remember, it is not DutyCycle that actually controls servo position, it is PulseWidth. We are creating DutyCycles to give us the desired PulseWidth.

To do the linear equation I need two points. Well, I know that for a desired angle of 0, I should apply a DutyCycle of 2. This would be the point (0,2). Now I also know that for a desired angle of 180, I should apply a DutyCycle of 12. This would be the point (180,12). We now have two points and can calculate the equation of the line. (Remember, play with your servo . . . your numbers might be slightly different than mine, but the methodology below will work if you use your two points)
Remember slope of a line will be:

m=(y2-y1)/(x2-x1)=(12-2)/180-0)=10/180 = 1/18

We can now get the equation of the line using the point slope formula.

y-y1=m(x-x1)
y-2=1/18*(x-0)
y = 1/18*x + 2

Putting in our actual variables, we get

DutyCycle = 1/18* (DesiredAngle) + 2

Now to change to that position, we simply use the command:

pwm.ChangeDutyCycle(DutyCycle)



