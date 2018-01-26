"LoPy-GPS-ThingsNetwork-Dragino" 


This project uses a LoRa device (Pycom LoPy) and a GPS sensor (via Pycom PyTracker board) to send a LoRa message to a single channel LoRa
gateway (Dragino LG01). The sensor data will be forwarded from the Dragino gateway, to The Things Network (TTN) backend. 

The code running on the single channel gateway only supports ABP (Activation by Personalization), so we will use ABP mode on the LoPy. 
The ABP values used by the LoPy device must correspond to the values generated by The Things Network (TTN) backend when we create the device in the TTN console/dashboard. 

TTN supports the ability to trigger a HTTP request when it receives the LoRa message. To do this you confiure the HTTP in TTN backend, to forward the message to another server (an IoT platform supporting REST API). The tricky bit here is getting the TTN HTTP integration to succesfully send a "decoded" version of the payload which contain the GPS coordinates. By default the payload forwared to the server in the HTTP Post/Put would be encoded/encrypted, and require some keys (Network session key, and App Session key) to be decoded at the server side. However TTN has a feature called Payload formats, and by adding some code here we can decode the payload to extract the GPS within TTN. 

To sucesfully be able to pass on the decoded payload from TTN HTTP integration, the Pycom LoPy device cannot send the GPS as a float or a string, and must encode accordingly. You will see a function on the LoPy that encodes the GPS coordinates into a byte array. By sending it in this format, we can ensure TTN HTTP payload format can contain a decoded version of the GPS data. 

The code that is used on TTN network (to decode the encoded GPS) is contained in a file called TTNDecoder.txt (this will be copied into your TTN backend payload formats custom decoder configuration). 

