---
id: tl7sszarx8645gp6btklo80
title: Commissioning
desc: ''
updated: 1787807393833
created: 1787785449249
---

We have finally produced steam in the steam generating heat pump yesterday! Here are some of the key insights I've learned throughout the process:

You very quickly get an ear for when the compressor is running normally and when it is not - you can hear the engine sounds a lot worse with liquid in it, you can hear it cough if the vsd frequency changes briefly, and you can tell if it's running at a state that it's "happy" with very easily. A very interesting thing to do would be to put a bunch of microphones everywhere as feedback - you can probably tell which pumps are running etc, when another one starts up, and if a potential problem is happening - just from that - with some sort of abnormality detection algorithm. You would have to figure out how to isolate voice from it though as a signal you don't care about. 

Another interesting idea is to run some real-time models to flag things that are unexpected or not normal in the data - and then automatically dispatch an AI agent to investigate what is happening at that time.

Using influxdb cli with an AI agent can be pretty nice for producing reports based on the time series data. 

I have learned a lot about startup and shutdown procedures. E.g we have to be very careful when we startup the compressor, as we don't want liquid in the suction line. Bitzer compressors have "a reputation for being tanks" (according to Tim Walmsley), and we got away with running it at 30 Hz with a lot of liquid bubbling in the suction line - I should have got a photo, it's very obvious that there's a problem there. Still, to make startup better we try to evacuate the suction line on shutdown. This means when we call a shutdown, instead of shutting down immediately we drop the compressor to its lowest setting, close the solenoid valve, and then


My modelling helped a lot in getting me familiar with the system and the expected system's response before I had actually seen and used the system,  and I could get used to understanding how PID worked etc. However, I didn't model the dynamics very well, because I didn't do the calculations based on pipe volumes and flow rates etc to calculate the expected first order or time-delay response of the systems. That meant I had to calibrate the PID manually again when we had the system. I think that modelling everything first, even just with a first order model, would have helped me.


We spent a lot of time on safety, as the Butane High-Side Pressure relief valve is at 450 psi, around 31 bar. You really don't want that thing going, because then you don't know how much butane you have lost to the atmosphere. Thus we had a few levels of protection below it, including setting a hardware pressure limit switch to around 27 bar. Below that, we also have software limits to slow down the compressor if it was getting close to this value, start shutdown procedure if it was closer, and if it was over to stop it completely. These are things that I didn't model in my PLC control digital twin but it would have been good to. The PLC also doesn't have that good ways of validating that your logic is correct without running it on the physical plant/plc, so creating a digital twin where you can reliably update process conditions to trigger these software failsafes and verify that they actually trigger properly would be a very good thing.

A lot of time was spent getting sensors to read - 4-20 mA was pretty easy but some of the others that used modbus or other protocols took a bit longer. There might be some advantage in virtualising those with either a hardware emulator or a software emulator - but ideally this is something that a manufacturer would make. A similar process of connecting these emulators to report the readings provided from an MQTT tag would be good -then you could still use the Ahuora digital twin model to provide the data for them, but it would be translated to the expected protocol so you can test the plc's physical and communication layer too.

One problem that we had that took a while for us to solve on the commissioning day on the 26th august was the expansion valve control was backwards - 90% meant 90% closed rather than 90% open. So we were trying to have the expansion valve mostly closed to test things and the pressure was not dropping! once we finally figured that out things got a lot better and we were able to get the heat pump running. This issue was hard to detect earlier, because we had no way of seeing what the expansion valve was doing before as it was in the system and we didn't have any visibility into it until the compressor was running. We eventually figured it out when Tim noticed the mass flow went up when we closed the expansion valve.  Possibly we could make some tools to help with this type of issue? We didn't log the mass flow so didn't notice on the trends, as we hadn't got the mass flow meter working yet.


# Additional Documents and Analysis

Additional information/review of what happened and when can be found in the [Commissioning Data Review Document](assets/SGHP_Commissioning_Data_Review_2026-08-26.pdf)

You can also view the [SGHP P&ID](assets/SGHP%20P&ID%20V22.pdf)







# AI Summary of notes I wrote in Google Keep

1. Compressor Startup, Shutdown, and Control
Startup/Shutdown Protocol: Verify all lines are actively flowing before starting the compressor. Implement safeguards to ensure the compressor cannot be forcefully shut down bypassing protocols.

Safety Interlocks: Ensure the solenoid valve shuts automatically if the compressor trips.

Pressure Limits: Set the pressure switch to 420 PSI and configure the compressor to turn off automatically when it hits this maximum threshold.

Speed & Temperature: Implement a P&ID loop for the Variable Speed Drive (VSD) to control compressor speed. Actively manage and monitor the compressor winding temperatures.

2. System Observations & Hardware Troubleshooting
Valve Orientation: The expansion valve is currently installed backwards. (Test parameters logged at 30 Hz, 4pc exp).

Vibration & Resonance: A rattling pipe was observed, likely caused by flash gas. Additionally, there is a resonant frequency issue at 50 Hz. 

We briefly had steam come out of the warm water line when the pump wasn't running one time and so it heated up too much, and the pressure relief valve tripped.

Meter Malfunctions: The mass flow meters are currently not working. Furthermore, the calculated steam values are not aligning correctly with the temperature readings.

Interesting fact I learned: some places that have other refrigerants use a few grams of propane or pentane to sweep the oil out of the refrigeration lines. We don't model things like oil getting stuck in lines etc, idk if we should but it's prolly not important. 

Operational Log: The internal heat exchanger (IHX) was opened at 10:34, and the cooling side was closed at 10:43.

3. Sensors & Calculations
Superheat: Swap out sensor hppt06 for hppt01 when calculating superheat.

Duty & Levels: Calculate the heat duty across the compressor. A tank level sensor needs to be fabricated or installed.

4. PLC, Data Logging, and Tagging
Data Formatting: Improve the Modbus logging in the PHNIX. Data needs proper formatting and system tags applied (e.g., confirm the WW system tags), rather than just logging raw device registers. This should potentially be handled directly on the PLC.

VSD Integration: Wire the pump VSD and configure the PLC to actively publish the VSD information.

Control Tuning: Set the minimum and maximum PID values.

Telemetry: Ensure there is support for boolean values over MQTT/Telegraf.


5. Modeling, Automation, and Next Steps
Digital Twin/Flowsheet: Build flowsheet models of the current state using steam-generating heat data and live readings. Use these models to diagnose data issues, reproduce the current state of the existing pump, and plan how parts fit together.

Economic Analysis: Conduct an economic analysis of the system components.

AI & Database Integration: Write a quick reference guide for the InfluxDB heat files (and how to browse them) to lay the groundwork for future AI analysis.

Home Automation: Connect the system to Home Assistant for centralized heat control.