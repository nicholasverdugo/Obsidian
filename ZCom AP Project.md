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
flowchart LR
	Cerebro<-- Receives JSON from -->SmartConnect;
	SmartConnect<-- pubs/subs to -->MQTTBroker;
	WiFiController-- Sends AP Data to -->MQTTBroker;
	APMonitor-- is a part of -->SmartConnect;
	JSONFormatter<-->APMonitor;
	ManyAPs-- Sends AP data to -->WiFiController;	
```
