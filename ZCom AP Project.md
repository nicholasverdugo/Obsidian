> Well begun is half done.
>
> - <cite>Aristotle</cite>✍️

#ZComProject
- Monitor data codes and AP statuses actively
	- Need to decide on the time interval for this - s, ms?
- #JSON is the preferred file format (my module will be formatting the data from the APs as JSON and sending to Cerebro)
- Backend is written in Java
- Device I/O API should be able to send the majority of the information we need
	- However, what we may want to achieve may not be provided by this API
- If the device doesn't support #MQTT we could cross that bridge to use webhooks
- Need to get to the point where we can negotiate with #ZCom to ask if they can add functionality that we need
	- Do we put the responsibility on our team or their team?
- Reference AP Network Topology
- Write out the design logic in a document, topology-wise

TODO:
- Get deeper into the AP's capabilities - find out how we can get data from it ( #MQTT, #HTTP, proprietary method)

9/12 Flowcharts for network and module (High Level, still in-process planning)

<h1> High Level Network View </h1>
```mermaid
classDiagram
Cerebro<--SmartConnect : Sends JSON formatted data via HTTP
Cerebro : Cloud Management Server
Cerebro..WiFi APs : Monitors via SmartConnect
SmartConnect-->MQTT : Subs to
SmartConnect<--APMonitor : is one part of
APMonitor : Handles AP Data
APMonitor : can maintain asset/device list to send to Cerebro
APMonitor : can maintain AP locations for Cerebro
APMonitor : formats data as JSON
MQTT<--ZCom WiFi Controller : Data is sent to
SmartConnect : needs auth token for Cerebro connect
ZCom WiFi Controller-->WiFi APs : Requests data
ZCom WiFi Controller<--WiFi APs : Sends data
WiFi APs : power
WiFi APs : MAC_addr
WiFi APs : LAN_status
WiFi APs : LAN_rate
``` 

<h1>High Level Module View</h1>
```mermaid
classDiagram
Cerebro<--SmartConnect : Receives JSON via HTTP
SmartConnect<-->MQTTBroker : pubs/subs to 
SmartConnect-->APMonitor : calls
APMonitor-->JSONFormatter : Calls to format AP data as JSON
MQTTBroker<--WiFiController : Sends AP Data to
JSONFormatter-->SmartConnect : Sends JSON data to

class SmartConnect {
	access token to Cerebro
	Runs APMonitor module
}
class APMonitor {
	subscribes to MQTT broker for AP data
	runs loop every x time to convert new data to JSON and give to SmartConnect
}
```

Meeting 10/6 Re: diagrams
- RESTful API layer -> MQTT layer translation needs to be removed, ideally we want ZCom to be able to connect to MQTT on their end
- My application layer
	- need to design it so we can develop an API to hook into ZCOM's RESTful APIs
	- Cloud has MQTT broker in place already, but in other areas we may need a standalone deployment so this requires us to use more resources on site if that needs to happen
	- We have to build something to connect to the ZCOM APs
	- Need to get MOVING
	- Meeting w Dmitry next week
	- Need to be able to answer the whys of the problem
- Dmitry meeting
	- MQTT is the way to go, as it is the standard 
	- AWS makes it easy, lots of documentation
	- Main work will be with getting communication working and implemented with on-site devices
	- We need to find out (re: speak with Sid) how to get a connector with MQTT implemented on ZCom's end.
		- Should be straightforward to implement this
	- Remove the application layer, and have the connector reside on the embedded level
	- We already have a few devices working with our system
		- Smart Pole
	- Get Python scripts implemented in order to test system connections
	- Dmitry will send me the details of getting this done to me
		- Between Dmitry and Lobo, who is an embedded engineer
		- Dev for smart gateway
		- Thread goes over getting that connected to MQTT over AWS
		- Calvin has a script, Dmitry will have him send to me