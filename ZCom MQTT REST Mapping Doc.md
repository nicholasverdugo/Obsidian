# Goal
Cerebro will use MQTT to maintain statuses of connected devices and alert if changes occur.
ZCom APs communicate via HTTP, transferring JSON formatted information. 
This JSON data must be received by Cerebro, mapped to MQTT, and received by Cerebro's MQTT broker.

## Control Flow
### Main Function
1. **Send GET** request via HTTP to all desired APs
2. **Run map function** *for all* APs that respond
3. **Publish mapped information** to Cerebro via MQTT
4. **Repeat**

### Mapping Function
Receive JSON via HTTP (RESTful API)
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

Convert JSON to string (if not already encoded as one), and get byte array of the string in order to add to MQTT message. Format MQTT as usual and send.

## Considerations
- Must be UTF-8 charset when mapping to byte array
- Need to decide MQTT QoS level (likely 1?)
- Potential for creation of aliases for needed information to reduce data load
- Will the client be subscribing to Cerebro? Likely not, 