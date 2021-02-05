# OpenSML

Open Sources SoftMotion Light For CiA402 Servo Drivers  

Tips:  
[Here you can found .library file](https://github.com/feecat/OpenSML/issues/2#issuecomment-771620326) for codesys and beckhoff, you can just import and test it, Thanks elconfa.  
There is suggest to read [keba servoone usermanual](https://www.lti-motion.com/fileadmin/lti-motion/downloads/03-Geraetedokumentationen/ServoOne/Option1-Kommunikation/SO_UserM_EtherCAT-CANopen_2020-04_EN.pdf) to get more information about cia402.  
And if you have any questions please feel free to push your issues.  

## Why

codesys is the best plc programming platform. With the Raspberry Pi, you can get an advanced plc programming environment at a very low price.

However, the softmotion license was canceled in the latest version of codesys for raspberry pi. (> 3.5.14.40, which means Raspberry Pi 4 must pay to get a softmotion license)

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

Example for Cyclic synchronous Velocity Mode:

```
Power_X(Axis:=Axis_X,xEnable:=Enable);
SyncVelocity_X(
	Axis:=Axis_X ,
	xEnable:=EnableMove ,
	diTargetVelocity:=diTargetVel ,
	rAcceleration:=10000 ,
	rDeceleration:=10000 ,
	diCycleTime:=1000 ,
	xError=>Error);
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
| Cyclic Sync Velocity Mode |:white_check_mark:|OpenSML_SyncVelocity|
