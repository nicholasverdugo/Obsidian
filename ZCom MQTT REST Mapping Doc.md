# Goal
Cerebro will use MQTT to maintain statuses of connected devices and alert if changes occur.

ZCom APs communicate via HTTP, transferring JSON formatted information. 

## Control Flow
### Loop
1. AP sends JSON via HTTP to Cerebro
2. This is mapped to an MQTT message
3. JSON is handed to MQTT broker after being mapped

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

Convert JSON to string, and get byte array of the string in order to add to MQTT message
- Must be UTF-8 charset
- If using Python, can leverage json.dumps(json as dict) in order to publish to the broker (Cerebro)