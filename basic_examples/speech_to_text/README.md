#Speech to Text

Note: To use the Speech To Text node we ran into some restrictions, which will be solved soon. This is why this lab is based on a stand-alone system.

Speech to Text service can be used anywhere voice-interactivity is needed. The service is great for mobile experiences, transcribing media files, call centre transcriptions, voice control of embedded systems, or converting sound to text to then make data searchable. Supported languages include:
- US English 
- Spanish 
- Japanese 
- Brazilian Portuguese
- Mandarin. 

To use the Speech To Text service in Node-RED you first need to make this service available in a way Node-RED can connect to that service. 

You need to have a local instance of Node-RED with IBM nodes available. If you don't have that yet, you can go [here](/introduction_to_node_red/README.md).
Then go to the Bluemix catalog and go to the Speech to Text service and click on it. Make sure that there is no app bound to this service and click 'Use"
Wait until this service is deployed and click on 'Show Credentials', you will need the username and the password later on in this lab.

![`S2TOverview`](images/s2t_overview.jpg)

In this lab an audio file will be transcribed. This audiofile can be downloaded [here](audio_message.wav). 
In the following screenshots you can see how the nodes are configured.

First you start with an Inject node, to start uploading of the .Wav file from your local machine into the Speech To Text service in Bluemix

The inject node is configured like this:

![`S2Tinject`](images/s2t_inject.jpg)

The next node is a File in node. This node points to the file on the local system.It is configured in the following way

![`S2TFileIn`](images/s2t_filein.jpg)

Then the Speech to text node will be added. In the image below you can see how it is configured. You will need the credentials from the Bluemix service.

![`S2TFileIn`](images/s2t_config.jpg)

The last node is the Debug node. You need to configure this for getting the output in the debug window. The Speech to text node, will put the transcribed text into msg.transcription.

Then you need to wire all the nodes together and press on 'Deploy'

When you click on the Inject node, you will see the transcribed text from the audio file in the debug window.

The complete flow can be found here: [Text To Speech lab flow](s2t_flow.json)

Optional:

If you want to use the transcribed text in another applications, you can easily bring the transcribed text to Bluemix.
Therefore you need to add a few extra nodes, the flow will look like te following:

![`S2TOverviewExtra`](images/s2t_overview_extra.jpg)

First you add the 'Change' node to move the transcript to the payload. You configure this node as shown below:

![`S2TChange`](images/s2t_change.jpg)

Next you add the 'MQTT out' node to the canvas. This node will send the payload to a broker with a certain topic. You can use any MQTT broker with your own topic. You can configure this node like this:

![`S2TMQTTOut`](images/s2t_mqttout.jpg)

Note: someone else can use the same broker and topic (if the topic is known), and will see the date being send.

Then click on deploy. If everything went allright, you will see that the MQTT node is connected.

Then go to your Node-RED application in Bluemix. You have to build a small flow to get the transcript of your Speech to Text in your application. The flow would look like this:

![`S2TBluemix`](images/s2t_bluemix.jpg)

First you need to add a 'MQTT In' node. This node must subscribe to the same topic as the MQTT out node you configured previously. It would look like this:

![`S2TMQTTOin`](images/s2t_mqttin.jpg)

Then you can connect any node to this, like [Language Identification](/basic_examples/language_identification/README.md) to identify the language of the transcript. I this case I added a debug node, to see the output. The transcript is in the message.payload:

![`S2TDebugBL`](images/s2t_debugbl.jpg)

The extended flow can be found here: [Extended Text To Speech lab flow ](s2t_flow_extended.json)
The flow in Node-Red in Bluemix can be found here: [Text To Speech lab flow for Bluemix](s2t_flow_bluemix.json)










