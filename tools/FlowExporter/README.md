# Flow Exporter for Node-RED on BlueMix
This flow provides a bare-bones web interface to export flows from BlueMix-deployed NodeRED, by extracting the relevant information directly from the supporting Cloudant Database

__Note that this will work only for Node-RED deployed on BlueMix__

##How to use it
1. Import the flow from the clipping below, or from the [Flow Exporter](Flow_Exporter.json)   
```
[{"id":"d56921de.7860f","type":"http in","name":"","url":"/flowmgr","method":"get","swaggerDoc":"","x":82,"y":41,"wires":[["79360200.b034e4"]]},{"id":"2947203d.d24ae8","type":"http response","name":"WebList","x":842,"y":184,"wires":[]},{"id":"87834066.009cf","type":"cloudant in","service":"XXX-cloudantNoSQLDB","cloudant":"","name":"Get flows","database":"nodered","search":"_id_","design":"","index":"","x":344,"y":104,"wires":[["894170fc.38526"]]},{"id":"fc435622.821e2","type":"template","name":"Reply flow list","field":"payload","format":"handlebars","template":"<h1>List nodes</h1>\n<h1>There are {{tabs.length}} tabs:</h1>\n\n<ul>\n{{#tabs}}\n    <li>\n        <a href=\"/flowmgr?tabid={{id}}\" target=\"_blank\">{{label}}</a>: {{nodes.length}} nodes\n    </li>\n{{/tabs}}\n</ul>\n","x":688,"y":187,"wires":[["2947203d.d24ae8"]]},{"id":"79360200.b034e4","type":"change","name":"Set _id","rules":[{"t":"set","p":"tabid","to":"msg.payload.tabid"},{"t":"set","p":"payload","to":"XXX/flow"}],"action":"","property":"","from":"","to":"","reg":false,"x":217,"y":105,"wires":[["87834066.009cf"]]},{"id":"74095518.82ebe4","type":"function","name":"List flows","func":"var tabs=[];\nnode.warn('p='+msg.payload.tabid);\nnode.warn('px='+msg.tabid);\nfor(var iF in msg.payload.flow) {\n    var flow=msg.payload.flow[iF];\n    if(flow.type == \"tab\") {\n        flow.nodes=[];\n        tabs.push(flow);\n    } else if(flow.z!==null) {\n        // this is part of tab\n        for(var iT in tabs) {\n            var tab=tabs[iT];\n            if(tab.id==flow.z) {\n                tab.nodes.push(flow);\n            }\n        }\n    }\n}\nmsg.tabs=tabs;\nreturn msg;","outputs":1,"noerr":0,"x":503,"y":191,"wires":[["fc435622.821e2"]]},{"id":"feb96900.4acd1","type":"http response","name":"jsonfile","x":846,"y":245,"wires":[]},{"id":"e11aa0d6.4166d","type":"function","name":"Extract flow nodes","func":"var tabid=msg.tabid;\nvar tab;\n\nfor(var iF in msg.payload.flow) {\n    var flow=msg.payload.flow[iF];\n    if(flow.type == \"tab\" && flow.id==tabid) {\n        flow.nodes=[];\n        tab=flow;\n    } else if(flow.z!==null && flow.z==tabid) {\n        // this is part of tab\n        if(tab.id==flow.z) {\n            tab.nodes.push(flow);\n        }\n    }\n}\n\nfunction replacer(key, value) {\n    // round x, y and drop z\n    if (key === \"z\") {\n        return undefined;\n    } else if (typeof value === \"number\") {\n        return Math.round(value);\n    }\n  \n    return value;\n}\n\nmsg.payload=JSON.stringify(tab.nodes,replacer);\nvar attch='attachment; filename='+encodeURIComponent(tab.label)+'.json';\nmsg.headers={// 'content-length': '123',\n    'content-type': 'application/octet-stream',\n    'content-disposition': attch};\nreturn msg;","outputs":1,"noerr":0,"x":537,"y":247,"wires":[["feb96900.4acd1"]]},{"id":"894170fc.38526","type":"switch","name":"Check tabid","property":"tabid","rules":[{"t":"null"},{"t":"else"}],"checkall":"false","outputs":2,"x":268,"y":223,"wires":[["74095518.82ebe4"],["e11aa0d6.4166d"]]},{"id":"536497c4.dd5918","type":"comment","name":"Setup Instructions","info":"Make sure to replace `XXX` by your own NODE-RED app name in `Set _id` and `Get flows` nodes","x":286,"y":70,"wires":[]}]
```
2. Edit the `Cloudant` 'Get flows' nodes to select your own Cloudant DB name, in the Service field select the `XXX-cloudantNoSQL` proposal, and edit the `Set _id` node to replace `XXX/flow` with your own Node-RED app name (this is case sensitive!)
3. Deploy and point your browser to` xxx.mybluemix.net/flowmgr`
4. You should be getting a list of the tabs, select the hyperlink which will prompt for a .flow file to download