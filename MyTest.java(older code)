package org.firstinspires.ftc.teamcode;

import com.qualcomm.robotcore.eventloop.opmode.LinearOpMode;
import org.firstinspires.ftc.robotcore.external.Telemetry;
import com.qualcomm.robotcore.eventloop.opmode.TeleOp;
import com.qualcomm.robotcore.hardware.DcMotor;
import com.qualcomm.robotcore.hardware.DcMotorSimple;
import com.qualcomm.robotcore.hardware.Servo;

@TeleOp(name = "Driven", group = "")
public class MyTest extends LinearOpMode
{
		//All code here has been optimized for human readability, this means the following has been done...
		//
		//Comments added everywhere. If something doesn't make sense, read those
		//Anything that needs multiple lines has been split up very clearly to indicate differences
		//Anything that made no sense to have as multiple lines has been simplified to 1, but has an in-depth comment about how it works
		//
		//If you would like the shortened version, let me know, and I can cut all the empty space, and make it illegible

		public static final double motorStr = -1; //Default motor power
		public static float multiplier = 1; //Multiplier to apply to motors, can actually be changed during runtime
		public static float deadSpace = 0f; //Amount of deadspace on joystick (not counted for movement)
		private DcMotor spinner; //Thing
		private DcMotor frontLeft; //Front left
		private DcMotor backLeft; //Back left
		private DcMotor backRight; //Back right
		private DcMotor frontRight; //Front right
		private Servo grabber_servo; //hand servo
		private DcMotor arm; //Arm

		@Override
		public void runOpMode()
		{
				
				spinner = hardwareMap.dcMotor.get("spin");
				frontLeft = hardwareMap.dcMotor.get("front_left");
				backLeft = hardwareMap.dcMotor.get("back_left");
				backRight = hardwareMap.dcMotor.get("back_right");
				frontRight = hardwareMap.dcMotor.get("front_right");
				grabber_servo = hardwareMap.servo.get("grabber");
				arm = hardwareMap.dcMotor.get("arm");

				boolean changedSpd = false;
				//Init. goes here, above start
				waitForStart();
				while (opModeIsActive())
				{
						//Look, I didn't put this here, but if it works, who am I to say no?
						//I suspect it's to adjust for 2 being in the opposite direction as the other 2 (opposite orientations)
						spinner.setDirection(DcMotorSimple.Direction.REVERSE);
						frontLeft.setDirection(DcMotorSimple.Direction.FORWARD);
						backLeft.setDirection(DcMotorSimple.Direction.REVERSE);
						telemetry.update();

						//Controls for NOT driving controller...

						//Spin the spinner if the x or y button is pressed
						
						if(gamepad2.x)
							spinner.setPower(.75);
						else if(gamepad2.y)
							spinner.setPower(-.75);
						else
							spinner.setPower(0);
						//spinner.setPower(gamepad2.x ? (.75 : gamdpad2.y ? (-.75 : 0)));
						
						//Set one grab button, press once to close, once to open
						if(gamepad2.a)
						{
								grabber_servo.setPosition(0);
						}
						else if(gamepad2.b)
						{
								grabber_servo.setPosition(100);							
						}
						//if(gamepad2.a)
						//		grabber_servo.setPosition(grabber_servo.getPosition() < 0 ? (100 : -10));

						//#region Original Code
						/*if (gamepad2.a)
								grabber_servo.setPosition(-10);
						else if (gamepad2.b)
								grabber_servo.setPosition(100);*/
						//#endregion

						//If the left bumper is pressed or the joystick is up, set the power to .3,
						//else if the right bumper is pressed or the joystick is down, set the power to -.3,
						//else if the position is forward (not leaning towards bot) set the power to .05, to prevent it from falling to the ground.
						//=-=-=-=-=-=-=-=-=-=-=-=
						//SEE ARM.GETPOSITION()
						//arm.setPower((gamepad2.left_bumper || gamepad2.left_stick_y < 0) ?? (.3 : (gamepad2.right_bumper || gamepad2.left_stick_y > 0) ?? (-.3 : arm.getPosition() > 0 ? (.05 : 0)));

						//#region Origianl Code
						else if (gamepad2.left_bumper || gamepad2.left_stick_y < 0 || gamepad2.dpad_up)
						{
							if(grabber_servo.getPosition() < 75) //OPEN
								arm.setPower(0.57);
							else if(grabber_servo.getPosition() > 75) //CLOSED
								arm.setPower(0.775);
						}
						else if (gamepad2.right_bumper || gamepad2.left_stick_y > 0 || gamepad2.dpad_down)
						{
							arm.setPower(-.28);
						}
						else
						{
								arm.setPower(0.1);
						}



						//Controls for driving controller

						//Do we want logic for modifying the deadZone for control(s)?
						//I'd have to do a little more work to implement it, so decided not to actually do anything until idea is approved/disapproved

						//If the multiplier hasn't been changed...
						//If the dpad is down, decrease mult by .25
						//Else if the dpad is up, increase mult by .25
						//Else increase by 0
						//Else increase by 0 (duplicate intentional)
						if(!changedSpd)
						{
							if(gamepad1.dpad_down || gamepad1.left_bumper)
							{
								multiplier -= .25;
								changedSpd = true;
							}
							else if(gamepad1.dpad_up || gamepad1.right_bumper)
							{
								multiplier += .25;
								changedSpd = true;
							}
						}
						if(changedSpd && !gamepad1.dpad_down && !gamepad1.dpad_up)
						{
							changedSpd = false;
						}
						//multipler += !changedSpd ? (gamepad1.dpad_down ? (-.25f : gamepad1.dpad_up ? (.25f : 0)) : 0);

						//Get the distance from the center of the left joystick. Let me know if we want right one, also.
						float yDif = Math.abs(gamepad1.left_stick_y); //Distance from y = 0
						if(yDif < deadSpace) yDif = 0;
						//yDif = yDif > deadSpace ? (yDif : 0); //If within deadspace, set calculation distance to 0
						float xDif = Math.abs(gamepad1.left_stick_x); //Distance from x = 0
						if(xDif < deadSpace) xDif = 0;
						//xDif = xDif > deadSpace ? (xDif : 0); //If within deadspace, set calculation distance to 0

						//Y pos < 0 is back
						//Y pos > 0 is forward
						//X pos > 0 is right
						//X pos < 0 is left

						// + power == motor turning forward
						// - power == motor turning backwards
						double power = motorStr * multiplier;
						telemetry.addData("key1", power);

						if (gamepad1.left_stick_y < 0 && yDif > xDif)
						{
								//BACKWARD
								telemetry.addData("key1", power);
								backRight.setPower(-power);
								frontLeft.setPower(-power*.2); //Faulty motor
								frontRight.setPower(-power);
								backLeft.setPower(-power);
						}
						else if (gamepad1.left_stick_y > 0 && yDif > xDif)
						{
								//FORWARD
								telemetry.addData("key1", power);
								backRight.setPower(power);
								frontLeft.setPower(power*.2); //Faulty motor
								frontRight.setPower(power);
								backLeft.setPower(power);
						}
						else if (gamepad1.left_stick_x > 0 && xDif > yDif)
						{
								//STRAFE RIGHT
								backRight.setPower(power);
								frontLeft.setPower(power*.2); //Faulty motor
								frontRight.setPower(-power);
								backLeft.setPower(-power);
						}
						else if (gamepad1.left_stick_x < 0 && xDif > yDif)
						{
								//STRAFE LEFT
								backRight.setPower(-power);
								frontLeft.setPower(power*.2); //Faulty motor
								frontRight.setPower(power);
								backLeft.setPower(power);
						}
						else if (gamepad1.right_stick_x > 0 || gamepad1.right_trigger > 0) //Right joystick control (changed from trigger)
						{
								//TURN RIGHT
								frontRight.setPower(power); //Faulty motor
								backLeft.setPower(-power*.5);
								frontLeft.setPower(-power*.2);
								backRight.setPower(power*.5);
						}
						else if (gamepad1.right_stick_x < 0 || gamepad1.left_trigger > 0) //Right joystick control (changed from trigger)
						{
								//TURN LEFT
								frontRight.setPower(-power*.5); //Faulty motor
								backRight.setPower(-power*.5);
								backLeft.setPower(power*.5);
								frontLeft.setPower(power*.2);
						}
						else
						{
								//STOP
								backRight.setPower(0);
								frontRight.setPower(0);
								backLeft.setPower(0);
								frontLeft.setPower(0);
						}
					telemetry.update();
				}
		}
}
