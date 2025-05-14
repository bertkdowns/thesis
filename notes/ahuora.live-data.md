---
id: wylqb8pl4gns4jgwmppfeyp
title: Live Data
desc: ''
updated: 1747092345522
created: 1747092343855
---
# Problem

We want the platform to be useful to simulate real-time conditions in a factory. To do this, we need to be able to get data from sensors, e.g from OPC-UA sensors, a SCADA system, or IoT sensors, etc. 

All those sensors could use different protocols, send data at different rates, etc. They have different reliabilities, scales, and uses. So, using a platform like [node-red](https://nodered.org/) might make it much easier, as users can setup custom ways of handling all the data there. There is also support for lots of different preprocessing and [machine learning techniuqes](https://flows.nodered.org/collection/nRKXqYoATZfZ) through all their libraries and add ons.

# What we need to do

##  Try model an example scenario in Node-Red

I've (copilot) made  some [python scripts to simulate a couple of different sensor readings](https://github.com/bertkdowns/sensor_sim). They include Websocket, HTTP, and TCP endpoints for wind speed, solar irradiance, and hydro level.

 We could make a model in the platform that uses these live values and other hardcoded parameters to calculate the total amount of power produced by this generation setup.  Node-Red by default includes nodes for HTTP, TCP, and Websocket, which should enable getting data from these.

We can use node red to make HTTP requests  to our backend api to update PropertyValues and solve. See http://localhost:8001/api/schema/swagger-ui/ to view all the endpoints, or open [the schema](https://github.com/waikato-ahuora-smart-energy-systems/Ahuora-Adaptive-Digital-Twin-Platform/blob/3767816637b146317aef410374362946bb7952af/frontend/src/api/api_schema.yml#L4) in editor.swagger.io .

Then the results of that solve (e.g the total power currently generated) can be then logged in node-red's console or something else. In reality they would be used in further flows such as stored in a database or logged to grafana.

## Use this example to find a way to systematize it.

- Is this the best way of linking node-red/live data up with the platform? 
- How hard is it to get a model to solve like this? should we make a new backend endpoint so that it's easier?
- Should we make a library for node-red to make connecting to the platform easier?
- What about authentication?
