#Lab: Watson Language Identification with Node-RED

##Overview

The Language Translator service can be used to identify one or more languages in text. The input and output for identification by the service are defined as follows:
 - Input: The input text to identify language against, in UTF-8 format.
 - Output: The identified language or array of identified languages in JSON or plain text format with the associated confidence score.
This service can identify many languages: Arabic; Chinese (Simplified); Chinese (Traditional); Cyrillic; Danish; Dutch; English; Farsi; Finnish; French; German; Greek; Hebrew; Hindi; Icelandic; Italian; Japanese; Korean; Norwegian (Bokmal); Norwegian (Nynorsk); Portuguese; Spanish; Swedish; Turkish; Urdu.

## Node-RED Watson Language Identification node
The Node-RED ![`LanguageIdentification`](images/language-identify-node.png) node provides a very easy wrapper node that takes text as input, and produces a set of values related to the identified language and the confidence rating.

## Watson Language Identification Flow Construction
In this exercise, we will show you how to set up the Language Translator service to identify language of the input text.

### Prerequisites and setup
To get the Language Translator service credentials on IBM Cloud automatically filled-in by Node-RED, you should connect the Language Translator service to the Node-RED application in IBM Cloud.

![`LIservice`](images/language-translator-service.jpg)

Please refer to the [Node-RED setup lab](/introduction_to_node_red/README.md) for instructions.

## Building the flow
You must provide an input to the Language Identification node (msg.payload) of either :
- a string
- a Node.js Buffer
In this example we will inject a string using the standard Inject node.

### Use an Inject node as input
Start by adding an inject node to the canvas. Double-click the node, then change the input type to string and add your required text. 
![`LIinject`](images/language-inject.jpg)

### Add the Language Identification node
Drag and drop a Language Identification node from the nodes palette, and wire it to your input node.

### Add a debug node
Drag and drop a debug node from the nodes palette, and wire it to your Language Identification node. Double-click the node, then change the output to msg.lang. 
![`LIdebug`](images/language-identified-debug.jpg)

The final flow should look like 
![`LIflow`](images/language-flow.jpg)

## Flow Source
The complete flow is available [here](flow.json).

## Language Translator Documentation
To find more information on the Watson Language Translator underlying service, visit these webpages :
- [Language Translator Documentation](https://console.bluemix.net/docs/services/language-translator/index.html#about)
- [Language Translator API Documentation](https://www.ibm.com/watson/developercloud/language-translator/api/v2/)
