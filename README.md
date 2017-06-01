# node-red-contrib-file-buffer

[Node-RED](http://nodered.org) Node to be used in conjunction with the
http multipart in node.
As the multipart node does not provide a buffer, this node can be used to load a
file using a path into a buffer.


## Install

Run the following command in the root directory of your Node-RED install:

````
    npm install node-red-contrib-file-buffer
````

## Usage

### Input
Set msg.payload to a file path. If you are using the http in multipart node then the path for the first file will be

````
    var files = Object.keys(msg.req.files);

    msg.payload = msg.req.files[files[0]][0].path;
````


### Output
msg.payload will be a buffer containing the file contents.

### Sample follow
This flow uses a multipart form to send a file to Watson Recognition for
classification.

````
[{"id":"4f54d763.49da18","type":"http response","z":"c3a7481d.5f0bc8","name":"","x":638,"y":60,"wires":[]},{"id":"33272863.0de6d8","type":"httpInMultipart","z":"c3a7481d.5f0bc8","name":"Classify Image","url":"/classify","method":"post","fields":"[{ \"name\":\"pic\"}]","swaggerDoc":"","x":109,"y":118,"wires":[["5b709929.6ae608"]]},{"id":"dfe7c8b4.baff78","type":"http in","z":"c3a7481d.5f0bc8","name":"Get Home Page","url":"/homepage","method":"get","swaggerDoc":"","x":108,"y":48,"wires":[["c5fec891.752e18"]]},{"id":"c5fec891.752e18","type":"template","z":"c3a7481d.5f0bc8","name":"Form and Response Template","field":"payload","fieldType":"msg","format":"handlebars","syntax":"mustache","template":" <html>\n     <body>\n        <form action=\"/classify\" method=\"post\" enctype=\"multipart/form-data\">\n            <input type=\"file\" name=\"pic\" accept=\"image/*\"><br>\n            <input type=\"submit\" value=\"Submit\">\n        </form> \n        <div>Classifications:</div>\n        <div>\n            {{#result}}\n            <table>\n            <tr>\n                <th>Class</th>\n                <th>Score</th> \n            </tr>\n            {{#images}}\n            {{#.}}\n            {{#classifiers}}\n            {{#.}}\n            {{#classes}}\n            {{#.}}\n                <tr>\n                    <td>{{class}}</td>\n                    <td>{{score}}</td> \n                </tr>                \n            {{/.}} \n            {{/classes}}            \n            {{/.}}            \n            {{/classifiers}}\n            {{/.}}\n            {{/images}}\n            </table>\n            {{/result}}\n        </div>\n     </body>\n</html>","x":387.5,"y":58,"wires":[["4f54d763.49da18"]]},{"id":"5b709929.6ae608","type":"function","z":"c3a7481d.5f0bc8","name":" Determine File Path","func":"if (msg.req.files) {\n    var files = Object.keys(msg.req.files);\n    msg.payload = msg.req.files[files[0]][0].path;    \n}\nreturn msg;","outputs":1,"noerr":0,"x":132.5,"y":175,"wires":[["ac80e960.e88008"]]},{"id":"a324ec33.2fa43","type":"visual-recognition-v3","z":"c3a7481d.5f0bc8","name":"","apikey":"","image-feature":"classifyImage","lang":"en","x":368,"y":174,"wires":[["72ec6a35.512944","c5fec891.752e18"]]},{"id":"72ec6a35.512944","type":"debug","z":"c3a7481d.5f0bc8","name":"","active":true,"console":"false","complete":"result","x":579.5,"y":218,"wires":[]},{"id":"ac80e960.e88008","type":"file-buffer","z":"c3a7481d.5f0bc8","name":"","x":167,"y":248,"wires":[["a324ec33.2fa43"]]}]
````

## Contributing
For simple typos and fixes please just raise an issue pointing out our mistakes. If you need to raise a pull request please read our [contribution guidelines](https://github.com/ibm-early-programs/node-red-contrib-file-buffer/blob/master/CONTRIBUTING.md) before doing so.
## Copyright and license

Copyright 2017 IBM Corp. under the Apache 2.0 license.
