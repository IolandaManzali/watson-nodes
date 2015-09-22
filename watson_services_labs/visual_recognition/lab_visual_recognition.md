#Visual Recognition

##  Watson Visual Recognition
### Overview
The Watson  Visual Recognition service allows to analyse the contents of an image and produce a series of text classifiers with a confidence index.

### Node-RED Watson Visual Recognition node
The Node-RED ![`VisualRecognition`](images/node-red_WatsonVisualRecognition.png) node provides a very easy wrapper node that takes an image URL or binary stream as input, and produces a set of image labels as output.

### Basic Watson Visual Recognition Flow
In this exercise, we will show how to simply generate the labels from an image URL.

The flow will present a simple Web page with a text field where to input the image's URL, then submit it to Watson Visual Recognition, and output the labels that have been found on the reply Web page.
![Reco-Lab-VisualRecognitionFlow.png](images/Reco-Lab-VisualRecognitionFlow.png)
The nodes required to build this flow are:

 - A ![`HTTPInput`](../../node-RED_labs/images/node-red_HTTPInput.png) node, configured with a `/reco` URL
 - A ![`switch`](../../node-RED_labs/images/node-red_switch.png) node which will test for the presence of the `imageurl` query parameter:
   ![Reco-Lab-Switch-Node-Props](images/Reco-Lab-Switch-Node-Props.png)
 - A first ![template](../../node-RED_labs/images/node-red_template.png) node, configured to output an HTML input field and suggest a few selected images taken from the main Watson Visual Recognition demo web page:
```HTML
    <h1>Welcome to the Watson Visual Recognition Demo on Node-RED</h1>
    <h2>Select an image URL</h2>
    <form  action="{{req._parsedUrl.pathname}}">
        <img src="http://visual-recognition-demo.mybluemix.net/images/horses.jpg" height='100'/>
        <img src="http://visual-recognition-demo.mybluemix.net/images/73388.jpg" height='100'/>
        <img src="http://visual-recognition-demo.mybluemix.net/images/26537.jpg" height='100'/>
        <img src="http://visual-recognition-demo.mybluemix.net/images/4068.jpg" height='100'/>
        <br/>Copy above image location URL or enter any image URL:<br/>
        <input type="text" name="imageurl"/>
        <input type="submit" value="Analyze"/>
    </form>
```
![Reco-Lab-Template1-Node-Props](images/Reco-Lab-Template1-Node-Props.png)

 - The ![Watson Visual Recognition](images/node-red_WatsonVisualRecognition.png) node, preceded by a ![change](../../node-RED_labs/images/node-red_change.png) node to extract the `imageurl` query parameter from the web request and assign it to the payload to be provided as input to the Visual Recognition node:
 
![Reco-Lab-Change_and_Reco-Node-Props](images/Reco-Lab-Change_and_Reco-Node-Props.png)

 - And a final  ![`template`](../../node-RED_labs/images/node-red_template.png) node linked to the ![`HTTPResponse`](../../node-RED_labs/images/node-red_HTTPResponse.png) output node. The template will format the output returned from the Visual Recognition node into an HTML table for easier reading:
```HTML
    <h1>Visual Recognition</h1>
    <p>Analyzed image: {{payload}}<br/><img src="{{payload}}" height='100'/></p>
    <table border='1'>
        <thead><tr><th>Name</th><th>Score</th></tr></thead>
        {{#labels}}
          <tr><td><b>{{label_name}}</b></td><td><i>{{label_score}}</i></td></tr>
        {{/labels}}
    </table>
    <form  action="{{req._parsedUrl.pathname}}">
        <input type="submit" value="Try again"/>
    </form>
```
![Reco-Lab-TemplateReport-Node-Props](images/Reco-Lab-TemplateReport-Node-Props.png)

To run the web page, point your browser to  `/http://xxxx.mybluemix.net/reco` and enter the URL of some  image. The URL of the listed images can be copied to clipboard and pasted into the text field.

The complete flow is available at [Reco-Lab-WebPage](Reco-Lab-WebPage.json).


