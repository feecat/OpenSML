(* https://github.com/feecat/opensml *)
FUNCTION_BLOCK OpenSML_Stop
VAR_IN_OUT
	Axis:OpenSML_Axis;
END_VAR
VAR_INPUT
	xStop: BOOL;
END_VAR
VAR_OUTPUT
END_VAR
VAR
END_VAR



(*
MC_Stop
Software Stop, True=Stop
*)
Axis.ControlWord.8:=xStop;