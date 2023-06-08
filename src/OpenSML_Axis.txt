TYPE OpenSML_Axis :
STRUCT
//Output
	ControlWord :UINT;
	Modes_of_operation :SINT;
	Target_Position :DINT;
	Profile_Velocity :UDINT;
	Target_Velocity :DINT;
	Target_torque :INT;
//Input
	StatusWord :UINT;
	Modes_of_operation_display :SINT;
	Torque_Actual_Value :INT;
	Position_Actual_Value :DINT;
	Velocity_Actual_Value :DINT;
	Current_Actual_Value :INT;
	Following_Error_Actual_Value :DINT;
END_STRUCT
END_TYPE

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
Otherwise, you can also modify lib like below.
*******************************)
(*
TYPE OpenSML_Axis:
STRUCT
//Output
	ControlWord AT %Q*:UINT;
	Modes_of_operation AT %Q*:SINT;
	Target_Position AT %Q*:DINT;
	Profile_Velocity AT %Q*:UDINT;
	Target_Velocity AT %Q*:DINT;
	Target_torque AT %Q*:INT;
//Input
	StatusWord AT %I*:UINT;
	Modes_of_operation_display AT %I*:SINT;
	Torque_Actual_Value AT %I*:INT;
	Position_Actual_Value AT %I*:DINT;
	Velocity_Actual_Value AT %I*:DINT;
	Current_Actual_Value AT %I*:INT;
	Following_Error_Actual_Value AT %I*:DINT;
END_STRUCT
END_TYPE
*)
