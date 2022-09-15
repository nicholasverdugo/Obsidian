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
Cerebro-->My Module : Requests AP data, may be MQTT subscriber
Cerebro : Cloud Management Server
Cerebro..WiFi APs : Monitors via MyModule
My Module-->ZCom WiFi Controller : Requests data
My Module<--ZCom WiFi Controller : Sends data
My Module-->Cerebro : Sends JSON formatted data via HTTP or MQTT?
My Module : Acts as MQTT broker to Cerebro
My Module : formats data as JSON
My Module : needs auth token for Cerebro connect
My Module : could maintain asset/device list to send to Cerebro
My Module : could maintain AP locations for Cerebro
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
MyModule-->MQTT Broker : Launches
MyModule : Contains security token to connect to Cerebro
MyModule..>Main Class : Begins loop on launch
MQTT Broker-->Main Class : Waits for data from
Cerebro-->MQTT Broker : Subscribes to
Cerebro<--MQTT Broker : Publishes to
Main Class-->WiFiController : Requests data on start of each loop
WiFiController : Power States
WiFiController : MAC Address List
WiFiController : LAN Status List
WiFiController : LAN Rate List
WiFiController : Any other desired data
WiFiController-->Main Class : Sends raw data to
Main Class-->JSONFormatter : Calls in order to format data
JSONFormatter-->MQTTHelper : Calls in order to hand data to broker
MQTTHelper-->MQTT Broker : Hands JSON data to
WiFiController-->Many APs : Gets data from
```