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
SmartConnect<--AP Monitoring Module : is one part of
AP Monitoring Module : Handles AP Data
AP Monitoring Module : could maintain asset/device list to send to Cerebro
AP Monitoring Module : could maintain AP locations for Cerebro
AP Monitoring Module : formats data as JSON
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
SmartConnect-->MQTT Broker : Subs to
SmartConnect : Contains security token to connect to Cerebro
SmartConnect..>APMonitor : Begins loop on launch
MQTT Broker-->WiFiController : Waits for data from
Cerebro<--SmartConnect : Receives data from
APMonitor-->WiFiController : Requests data on start of each loop
WiFiController : Power States
WiFiController : MAC Address List
WiFiController : LAN Status List
WiFiController : LAN Rate List
WiFiController : Any other desired data
WiFiController-->Main Class : Sends raw data to
APMonitor-->JSONFormatter : Calls in order to format data
JSONFormatter<--MQTTHelper : Hands raw data to
JSONFormatter-->APMonitor : Hands JSON data to
WiFiController-->Many APs : Gets data from
```