# AT COMMANDS

A list of At commands usefull for the sim800L module ( Arduino/Raspberry )

## INDEX

- [Play with the SIM with the AT commands](#setup)
- [Use SIM800L as main internet source for RPI](#use-sim800-as-main-internet-source) ( source [https://www.rhydolabz.com/wiki/?p=16325#comment-57356](https://www.rhydolabz.com/wiki/?p=16325#comment-57356))

### SETUP

- `$ sudo minicom -b 115000 -o -D /dev/serial0` ( *Open the editor for the serial comunication with the SIM800L where you can type the commands* )

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Ping if module is up        | AT                  | OK                   |
| Enable full functionlities                | AT+CFUN=1                   | OK                  |
| Check if sim is blocked                | AT+CPIN?                   | +CPIN:READY                 |
| Check signal strenght                | AT+CSQ                   | num in range of 0 - 31 ( 31 is the best )                 |
| Read sim informations                | AT+CCID                  | 8939103320004650065f ( similar ) |
| Check if is registered to the network                | AT+CREG?                   | +CREG: 0,1 |
| to attach GPRS        | AT+CGATT =1                  |               |
| enable the GMS location                 | AT+SAPBR=3,1,"CONTYPE","GPRS"                    |                  |
| Check if sim is blocked                | AT+SAPBR=3,1,"APN","{here goes your APN}"                   | |
| Check signal strenght                | AT+SAPBR=3,1,"USER","{here goes your APN username}"                   | |
| Read sim informations                | AT+SAPBR=3,1,"PWD","{here goes your APN password}"                 | |
| Allow freelly comunication with the GSM network    | AT+SAPBR=1,1                   |  |
| Get you sim ip add            | AT+SAPBR=2,1                  |  |

### SIM NETWORK MODES

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
|  flight mode      | AT+CFUN=0                  |       	OK        |
|  nomral mode               | AT+CFUN=1                    |   OK              |

### GET THE SIM OPERATOR

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
|        | AT+COPS=3,0                  |       	OK        |
|                 | AT+COPS?                    |   +COPS: 0,0,"vodafone"              |

### SENDING SMS 

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Get current encoding used        | AT+CSCS?                  |               |
| Set GSM as encoding                 | AT+CSCS="GSM"                    |                  |
| Configuring Text Mode               | AT+CMGF=1                   |       |
| Set the number to which send the sms (ZZ is the region num prefix )   | AT+CMGS="+ZZxxxxxxxxxx" |
| Type the message and press Ctrl-Z when done               |   | +CMGS: 2 \n OK|

### CALL

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| set the speaker volume (0-100)       | AT+CLVL=80                 |       	OK        |
| Start a call (change ZZ with country code and xxxxxxxxxxx with phone number to dial)      | ATD+ +ZZxxxxxxxxxx;                |       	OK        |
| Answer incoming call       | ATA               |       	OK        |
| End a call       | ATH                |       	OK        |

### SENDING HTTP GET REQUEST

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Enabling HTTP mode       |  AT+HTTPINIT                 |       	OK        |
|  If you are using or SLL set                 | AT+HTTPSSL=1                   |   OK              |
| Setting HTTP bearer profile               | AT+HTTPPARA="CID",1                  |   OK      |
| Start HTTP GET Session    | AT+HTTPACTION=0 | +HTTPACTION: 0,200,84 |
| Read the content of webpage  | AT+HTTPREAD |  +HTTPREAD: 84 Congratulation! your SIM800L http Request Test has been successfull!!! -miliohm.com- |
| Terminate the session   | AT+HTTPTERM |    |


### ENABLING GSM LOCATION ( not work more correctly right now Nov 2020 )

| Description                 | Command                        | Response                       |
|-------------                | -------------                  | -------------                  |
| Request sim location           | AT+CIPGSMLOC=1,1               |  |


### USE SIM800 AS MAIN INTERNET SOURCE

THis guide will help you creating a PPP connection thourgh your SIM800 module. PPP or Point to Point Protocol establishes a Node to Node communication using serial interface.

- `$ sudo apt-get update`
- `$ sudo apt-get install ppp screen elinks`
- `$ sudo nano 	/etc/ppp/peers/rnet`

- paste in the fil the following lines. You have to edit the serial port and the apn ( line 4 and line 5 )
```
#imis/internet is the apn for idea connection
connect "/usr/sbin/chat -v -f /etc/chatscripts/gprs -T { YOUR APN }"
 
# For Raspberry Pi3 use /dev/ttyS0 as the communication port:
/dev/ttyS0 { <--- REPLACE WITH SIM800L SERIAL PORT } 
 
# Baudrate
115200
 
# Assumes that your IP address is allocated dynamically by the ISP.
noipdefault
 
# Try to get the name server addresses from the ISP.
usepeerdns
 
# Use this connection as the default route to the internet.
defaultroute
 
# Makes PPPD "dial again" when the connection is lost.
persist
 
# Do not ask the remote to authenticate.
noauth
 
# No hardware flow control on the serial link with GSM Modem
nocrtscts
 
# No modem control lines with GSM Modem
local
```

- `$ sudo nano /etc/chatscripts/gprs` ( *a file now should be open on the terminal, If the SIM card needs a PIN to unlock, uncomment the line AT+CPIN=1234* )


- `$ sudo pon rnet` ( *run the script for create the PPP connection* ) 
- `$ cat /var/log/syslog | grep pppd` ( *print the logs* ) 
- `$ ifconfig` ( *now you should se a ppp0 line* )
- `$ sudo poff rnet` ( *shut down the PPP connection* )



