# TwinCAT Winch Demo

## Implementation Map
- `TwinCAT Winch/TwinCAT Winch/PLC/DUTs/DUT_WinchMode.TcDUT`: mode enum.
- `TwinCAT Winch/TwinCAT Winch/PLC/DUTs/DUT_WinchTypes.TcDUT`: HMI, command, and status structures.
- `TwinCAT Winch/TwinCAT Winch/PLC/POUs/FB_HmiInterface.TcPOU`: HMI Interface module.
- `TwinCAT Winch/TwinCAT Winch/PLC/POUs/FB_WinchControl.TcPOU`: Winch Control module.
- `TwinCAT Winch/TwinCAT Winch/PLC/POUs/PRG_WinchDemo.TcPOU`: top-level wiring example.
- `TwinCAT Winch/TwinCAT Winch/PLC/Tests/FB_WinchDemoTests.TcPOU`: unit-test FB covering mode, manual, auto, limits, and E-stop requirements.
- `TwinCAT Winch/TwinCAT Winch/PLC/Tests/PRG_UnitTests.TcPOU`: unit-test runner program.

## HMI I/O
### Inputs (`ST_HmiInputs`)
- Mode: `xModeOff`, `xModeManual`, `xModeAuto`
- Manual: `xEnable`, `xDisable`, `xHome`, `xStop`, `xRunFwd`, `xRunRev`
- Auto: `xAutoStart`, `xAutoStop`
- Alarm reset: `xResetAlarms`

### Outputs (`ST_HmiOutputs`)
- `fPositionMm`
- `xAxisEnabled`
- `eMode` (Off/Manual/Auto)
- Alarm booleans: `xAlarmTravelLimit`, `xAlarmEStop`, `xAlarmEStopStopTimeout`
- `xAutoBusy`

## Modes and transitions
Allowed transitions only:
- Off -> Manual
- Manual -> Off
- Off -> Auto
- Auto -> Off

Any other request is rejected and the current mode is held.

## Alarms and reset behavior
- Travel alarm sets when position leaves `[-5, 505] mm`; motion is stopped.
- E-stop alarm sets whenever E-stop is active.
- E-stop stop-timeout alarm sets if commanded stop has not reached zero speed within 100 ms while E-stop is active.
- Alarm behavior is latched.
- `xResetAlarms` clears alarm latches.
