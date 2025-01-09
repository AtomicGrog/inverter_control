# inverter_control

This repo is in place to provide a set of Home Assistant Blueprints, Scripts, Automations etc. as an example/usable automation tool for home inverter control.

It's principally designed around using the EMHASS Home Assistant Automation addon to provide a set of input conditions, along with swiches to set run/no-run etc. conditions and defaults to leveraging scripts provided by the MKAISER/Sungrow Home Automation Addon. In addition the control includes a spike based on Amber Addon sensor and a evening curtail i.e. revert to normal operation after a given time and once battery has gone below a given threshhold. Both the input conditions and output controls defined within the Blueprint can be altered to use alternatives and in some cases such as master, spike and evening control are externalised via switches.

The main control blueprint seeks to put the inverter (and assumed battery) into a set of modes.

1. Self consumption i.e. normal operation
2. Charge battery e.g. suppliment any solar energy being provided with grid power to charge battery. In this mode house would also/effectively be consuming grid power
3. Discharge battery e.g. export battery, alongside solar to grid, noting that household consumption typically comes out of this source as well.
4. Preserve battery e.g. stop using battery for a source or destination of power and rely on solar/grid
5. Spike e.g. force export on the basis that there is sufficient energy in battery and that grid feed in tarrif has been depicted as in a spike condition.
6. Evening e.g. after a given time and once battery as reduced below a given level the automation will put inverter into normal consumption upto midnight at which point control will revert to normal outcomes.

All of these outcomes are controlled by master switch. If the configured switch is disabled/off no action will take place, including depicting state.

The main control blueprint has a set of inputs defined. These are broadly categorised as:

1. Switches for controling master operation, spike and evening control mode enabled (e.g. nether can be an automated outcome unless both the switches are enabled and the conditions are met)
2. The triggers. The master switch, battery power forecaste, grid power forecast and spike flag. If any of these change state then the automation script will initiate
3. A battery pause level and pause time that are setting for the evening mode
4. A post action delay which will continue the automation script after final action to prevent an immediate re-run (in HA automation single mode any attempts to run a script already active will be rejected with a warning. This is useful for this control as often the trigger fire in parallel which could cause inverter control to send too many actions to the inverter simulataneously.
5. A text notifcation helper which receives updates based on last automation outcome e.g. normal, charge, discarge. etc.
6. Four scripts that are invoked to put the inverter/battery into desired mode
7. A input number used to define the charge/discharge mode batter charge/discharge level.
