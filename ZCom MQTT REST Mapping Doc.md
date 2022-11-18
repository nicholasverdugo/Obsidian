# Goal
Cerebro will use MQTT to maintain statuses of connected devices and alert if changes occur.
ZCom APs communicate via HTTP, transferring JSON formatted information. 
This JSON data must be received by Cerebro, mapped to MQTT, and received by Cerebro's MQTT broker.

## Control Flow
### Loop
1. AP sends JSON via HTTP to Cerebro
2. This is mapped to an MQTT message
3. JSON is delivered to MQTT broker after being mapped
4. Data can be parsed and acted upon from here

### Mapping Function
Receive JSON via HTTP (REST API)
Can contain data as follows:
```
{
	"power" : "boolean",
	"status" : "string",
	"Traffic_info" : "string",
	"AP_grouplist" : {
		...
	},
	...
}
```

Data available for request from APs:

| AP Messsage               | STA Message      |
| ------------------------- | ---------------- |
| MAC_addr                  | MAC_addr         |
| AP_IP                     | status           |
| ssid                      | connect_AP_name  |
| Engineering_info          | connect_AP_MAC   |
| AP_grouplist              | IP_addr          |
| AP_Basic_template_info    | wireless_mode    |
| AP_VAP_template_info      | SSID             |
| AP_network_element_coding | Traffic_info     |
| AP_version_info           | Negotiation_rate |
| AP_networkcard_type       | RSSI             |
| channel                   | username         |
| AP_RX, AP_TX              | time_stamp       |
| gateway_addr              |                  |
| LAN_status                |                  |
| LAN_rate                  |                  |
| power                     |                  |
| time_stamp                |                  |

Convert JSON to string, and get byte array of the string in order to add to MQTT message. Format MQTT as usual and send.

## Considerations
- Must be UTF-8 charset when mapping to byte array
- Python includes a json library that can be leveraged to encode and decode messages from APs and to broker
- Need to decide MQTT QoS level (likely 1?)