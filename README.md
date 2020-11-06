# AT COMMANDS

A list of At commands usefull for the sim800L module ( Arduino/Raspberry )

### SETUP

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Ping if module is up        | AT                  | OK                   |
| Enable full functionlities                | AT+CFUN=1                   | OK                  |
| Check if sim is blocked                | AT+CPIN?                   | +CPIN:READY                 |
| Check signal strenght                | AT+CSQ                   | num in range of 0 - 31 ( 31 is the best )                 |
| Read sim informations                | AT+CCID                  | 8939103320004650065f ( similar ) |
| Check if is registered to the network                | AT+CREG?                   | +CREG: 0,1 |
| Check signal strenght                | AT+CSQ                   | num in range of 0 - 31 ( 31 is the best )                 |
| to attach GPRS        | AT+CGATT =1                  |               |
| enable the GMS location                 | AT+SAPBR=3,1,"CONTYPE","GPRS"                    |                  |
| Check if sim is blocked                | AT+SAPBR=3,1,"APN","{here goes your APN}"                   | |
| Check signal strenght                | AT+SAPBR=3,1,"USER","{here goes your APN username}"                   | |
| Read sim informations                | AT+SAPBR=3,1,"PWD","{here goes your APN password}"                 | |
| Allow freelly comunication with the GSM network    | AT+SAPBR=1,1                   |  |
| Get you sim ip add            | AT+SAPBR=2,1                  |  |

### SENDING SMS 

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Get current encoding used        | AT+CSCS?                  |               |
| Set GSM as encoding                 | AT+CSCS="GSM"                    |                  |
| Configuring Text Mode               | AT+CMGF=1                   |       |
| Set the number to which send the sms (ZZ is the region num prefix )   | AT+CMGS="+ZZxxxxxxxxxx" |
| Type the message and press Ctrl-Z when done               |   | +CMGS: 2 \n OK|

### SENDING HTTP REQUEST

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Start wireless connection with the GPRS       | AT+CIICR                  |       	OK        |
| Gets the SIM IP address                 | AT+CIFSR                    |   100.73.110.9               |
| Starts an TCP connection to the website on port 80               | AT+CIPSTART="TCP","exploreembedded.com",80                  | OK
CONNECT OK      |
| Indicates that we will be making a HTTP request whose length is 63 characters   | AT+CIPSEND=63 | |
| Make a HTTP GET request.               | GET exploreembedded.com/wiki/images/1/15/Hello.txt HTTP/1.0  | SEND OK HTTP/1.0 200 OK|



### ENABLING GSM LOCATION ( not work more correctly right now Nov 2020 )

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Request sim location           | AT+CIPGSMLOC=1,1               |  |

