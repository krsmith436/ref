Message Queuing Telemetry Transport (MQTT) is a Client Server publish/subscribe messaging transport protocol. It is light weight, open, simple, and designed to be easy to implement.

## 1. SHSF MQTT Message Table

| Topic                             | Direction | QoS | Description |
|-----------------------------------|:---------:|:---:|-------------|
| <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Giebel Throttle__&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> |
| shsf/giebel\_throttle/commands    |  Pub      | 0   | Commands to Smiths Valley |
| shsf/giebel\_throttle/status      |  Pub      | 0   | Status from Giebel Throttle |
| shsf/giebel\_throttle/rssi        |  Pub      | 0   | WiFi Signal Strength |
| *shsf/giebel\_throttle/responses* | *Sub*     | *0* | *Responses from Smiths Valley* |
| *shsf/heartbeat*                  |  *Sub*    | *0* | *Heartbeat from Huotari Hub* |
| <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Acer Laptop__&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> |
| shsf/acer\_laptop/commands        |  Pub      | 0   | Commands to Smiths Valley |
| *shsf/acer\_laptop/responses*     |  *Sub*    | *0* | *Responses from Smiths Valley* |
| <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;__Huotari Hub__&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;</mark> | <mark>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</mark> |
| shsf/giebel\_throttle/responses   |  Pub      | 0   | Smiths Valley responses to Giebel Throttle |
| shsf/acer\_laptop/responses       |  Pub      | 0   | Smiths Valley responses to Acer Laptop |
| shsf/heartbeat                    |  Pub      | 0   | Heartbeat sent every 10 seconds |
| *shsf/+/commands*                 |  *Sub*    | *0* | *Commands to Smiths Valley from ALL devices* |