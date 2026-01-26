---
id: cgyd9mdwfx8m3kscv1f7zei
title: Lab Heat Pump
desc: ''
updated: 1769465708133
created: 1769457612229
---

> For more information, see [https://github.com/bertkdowns/heat-pump-modbus-decoding](https://github.com/bertkdowns/heat-pump-modbus-decoding)

![Flowsheet of the heat pump testing rig](assets/heat-pump-flow-diagram.drawio.svg)


This heat pump is setup as a simple testing rig, heating up water in a simple loop. a heat exchanger is used that is in a seperate loop with a cooling tower to cool down the heated water, and then the water is heated up again by the heat pump.

![Diagram of the heat pump data and control system](assets/heat-pump-data-architecture.drawio.svg)

# Some things about this setup

- The valve we had was a ball valve, which doesn't make precise control easy. It was hard to get a consistent operating point. 
- We reverse engineered both the modbus protocol (using a digital logic analyser and software to figure out the baud rate etc) and the HIOKI power logger. This made things take a bit longer than they would if we had good SDKs or explanations for both of these.
- The common communication layer ended up being MQTT - because this makes it easy to have one system subscribe to everything and store all history (influxdb), and its simple enough to get working with pretty much anything.  Inside a private network we don't have to worry as much about security, but adding security doesn't seem too bad if it is needed too. Retained messages is also super helpful, though outside of retained messages you kinda got to keep track of the state yourself. Could be paired with OPC-UA well potentially, where opc-ua keeps the current state for things that are run periodically, and mqtt is used to update it when anything changes. There might be libraries that help with this too. A good question around DTs is if they should be event driven, or just run periodically.
- I think generally, getting all data into the same format as fast as possible is really important. MQTT worked well for this, but if you had a different system, potentially you could just use a NI CDAQ data logger for everything for example. However, that is probably not realistic.
- Home assistant actually worked really well for a simple control system. It has made me wonder about the need for conventional SCADA systems even. InfluxDB was also a really good data collection platform. Aparently Grafana is good for viewing the data too.

