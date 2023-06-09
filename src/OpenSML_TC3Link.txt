(* https://github.com/feecat/opensml *)
FUNCTION_BLOCK OpenSML_TC3Link
VAR_INPUT
	StatusWord AT %I*:UINT;
	Modes_of_operation_display AT %I*:SINT;
	Torque_Actual_Value AT %I*:INT;
	Position_Actual_Value AT %I*:DINT;
	Velocity_Actual_Value AT %I*:DINT;
	Current_Actual_Value AT %I*:INT;
	Following_Error_Actual_Value AT %I*:DINT;
END_VAR
VAR_OUTPUT
	ControlWord AT %Q*:UINT;
	Modes_of_operation AT %Q*:SINT;
	Target_Position AT %Q*:DINT;
	Profile_Velocity AT %Q*:UDINT;
	Target_Velocity AT %Q*:DINT;
	Target_torque AT %Q*:INT;
END_VAR
VAR_IN_OUT
	Axis:OpenSML_Axis;
END_VAR


(*******************************
For TwinCAT3, You can use TC3Link like below:

	VAR
		Axis_1: OpenSML_Axis;
		SyncPos: OpenSML_SyncPosition;
		link: OpenSML_TC3Link;
	END_VAR
	
	SyncPos(blablablabla);
	//Make sure link() call after other instance like OpenSML_SyncPosition.
	link(Axis:=Axis_1);

Then you can link variable to IO device like MAIN.link.Target_Position.
*******************************)
//Output							
	ControlWord        := Axis.ControlWord;
	Modes_of_operation := Axis.Modes_of_operation;
	Target_Position    := Axis.Target_Position;
	Profile_Velocity   := Axis.Profile_Velocity;
	Target_Velocity    := Axis.Target_Velocity;
	Target_torque      := Axis.Target_torque;
//Input							
	Axis.StatusWord                   := StatusWord;
	Axis.Modes_of_operation_display   := Modes_of_operation_display;
	Axis.Torque_Actual_Value          := Torque_Actual_Value;
	Axis.Position_Actual_Value        := Position_Actual_Value;
	Axis.Velocity_Actual_Value        := Velocity_Actual_Value;
	Axis.Current_Actual_Value         := Current_Actual_Value;
	Axis.Following_Error_Actual_Value := Following_Error_Actual_Value;
