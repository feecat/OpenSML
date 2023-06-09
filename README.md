# OpenSML

Open Sources SoftMotion Light For CiA402 Servo Drivers  

Tips:

Library file should worked on CODESYS and TwinCAT3, Quadratic velocity ramp calc we used Struckig Library.

There is suggest to read [keba servoone usermanual](https://support.keba.com/cds/online/#doc/01-SOCANOPETHCAT-bh-en/01-SOCANOPETHCAT-bh-en) to get more information about cia402.  
And if you have any questions please feel free to push your issues.  

## Why

Codesys is the best PLC programming platform. With the Raspberry Pi, you can get an advanced plc programming environment at a very low price. However, the softmotion license was canceled in the latest version of codesys for raspberry pi. (> 3.5.14.40, which means Raspberry Pi 4 must pay to get a softmotion license)

The motion control used by most customers is very simple and does not involve interpolation motion.

This project aims to provide the open source cia402 motion library for codesys or twincat users.

## Target

In theory, an interpolation procedure can be made, similar to CNC. I am still working hard for this, but time is not guaranteed.

## How
I only use ST (Struct text language) programming and add as many comments as possible.

Example for MC_Power:
```
Power_X(Axis:=Axis_X,xEnable:=Enable);
```

Example for Home And Absolute Position Move:

```
CASE iState OF
	0:
		Power_X(Axis:=Axis_X,xEnable:=xStart);
		IF Power_X.xDone THEN
			iState:=10;
		END_IF;
	10:
		Home_X(Axis:=Axis_X , xEnable:=xHomeEnable);
		IF Home_X.xDone THEN
			iState:=20;
		END_IF
	20:
		ProfilePosition_X(
			Axis:=Axis_X , 
			xEnable:=xMoveEnable , 
			xExecute:=xMoveExecute , 
			xContinueMove:=FALSE , 
			xRelativeMode:=FALSE , 
			diTargetPosition:=diTarget , 
			udiProfileVelocity:=udiVel);
END_CASE
```

Example for Cyclic Synchronous Position Mode:

```
Power_X(Axis:=Axis_X,xEnable:=Enable);
SyncPosition_X(
	Axis:=Axis_X , 
	MaxVelocity:=2000 , 
	MaxAcceleration:=20000 , 
	MaxJerk:=200000 , 
	CycleTime:=0.001 , 
	TargetPosition:=rTarget , 
	xEnable:=TRUE , 
	lrScale:=1.0 , 
	xSimulation:=TRUE,
	xError=> ;
```


## TODO
|FuncBlock| Status | Name |
| --- |---| --- |
|Struct|:white_check_mark:|OpenSML_Axis|
|Power|:white_check_mark:|OpenSML_Power|
|Reset|:white_check_mark:|(use OpenSML_Power)|
|Home|:white_check_mark:|OpenSML_Home|
|Halt|:white_check_mark:|OpenSML_Stop|
|MoveAbsolute|:white_check_mark:|OpenSML_ProfilePosition|
|MoveVelocity|:white_check_mark:|OpenSML_ProfileVelocity|
|MoveRelative|:white_check_mark:|OpenSML_ProfilePosition|
|Stop|:white_check_mark:|OpenSML_Stop|
|Jog|:construction:|-|
|TouchProbe|:construction:|-|
| Cyclic Synchronous Velocity Mode |:white_check_mark:|OpenSML_SyncVelocity|
| Cyclic Synchronous Position Mode |:white_check_mark:|OpenSML_SyncPosition|

## Velocity Ramp

Quadratic velocity ramp calculate is very, very, very difficulty. Its simple when calc a curve, but its complexable when break a move without speed jitter.  

I had create a 6th velocity ramp base g2 at [https://github.com/feecat/OpenSML/issues/4#issuecomment-1146995447](https://github.com/feecat/OpenSML/issues/4#issuecomment-1146995447) , But it cannot break a move. This is unacceptable for most applications.  

[Struckig](https://github.com/stefanbesler/struckig) and [Ruckig](https://github.com/pantor/ruckig) is for multi dof robot calc, Ruckig calculates a trajectory to a target waypoint (with position, velocity, and acceleration) starting from any initial state limited by velocity, acceleration, and jerk constraints. It already finish mostily job in CSP moveabsolute. Also..it have a little bug, when i use Struckig, sometime it get non-zero acceleration when finished, it wll cause unexception move. I had remove some funcition, reduce program size and make it work with OpenSML. For now its still in test, If you found some bug please push your issues. Thanks.

At this trace,  
1. `POU.otg.CurrentPosition` from struckig.
2. `POU.Axis1.fSetPosition` from SM3_Basic.
3. `POU.csp.otg.CurrentPosition` from OpenSML_SyncPosition.

They overlap perfectly, diff < 1e-10.

![](/img/1.png)  
