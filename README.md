# [Mila]

[Mila] is an API for video behavioural classification.

## Example:
```sh
curl -X POST -F 'video=@*.mp4' -F 'data=@*.mila' https://PUBLIC_DNS/retrain
```

#### /classify result:
```sh
curl -X POST -F 'video=@*.mp4' https://PUBLIC_DNS/classify
```
```json
{
    "outputs": {
        "data": [
            {
                "label": "_label",
                "predictions": [
                    {
                        "accuracy": 0.39634,
                        "time": 0
                    },
                    {
                        "accuracy": 0.40814,
                        "time": 1
                    }
                ]
            }
        ]
    },
    "status": {
        "code": 200,
        "description": "OK"
    }
}
```

### Tech

[Mila] uses a number of open source projects to work properly:

* [node.js] - evented I/O for the backend; similar to [python], functional scripting asynchronous
* [Express] - fast node.js network app framework used to handle request to the server
* [Tensorflow] - TensorFlow™ is an open source software library for numerical computation using data flow graphs; It is used to train and classify based on its neural network. 
* [AWS EC2] - Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides resizable compute capacity in the cloud; Used to host the app in the cloud.
* [Python] - The core of extensible programming is defining functions; Scripting language used for parsing data and neural network algorithms. 

### Installation

[Mila] requires [Node.js](https://nodejs.org/) to run and with Tensorflow there are a few dependencies needed to be installed.

Follow the instructions from [expressionflow](http://expressionflow.com/2016/10/09/installing-tensorflow-on-an-aws-ec2-p2-gpu-instance/) if you are installing it on an AWS EC2 Instance.

### Development
Currently [Mila] is in alpha status. We are working now working on developing the API as we have a prototype working locally on our machines.

## Details
### [Python] scripts:
* mila.py
    * Is a [python] script **(deprecated)** used to create a data structure that transforms a video accordingly to a mila data file in to a json object. It then saves different frames of the video into the [tensorflow] folder to retrain the neural network. Please also check about mila data files.
    * The script can be run with the following command:
            ```
            python mila.py video.mp4 data.mila
            ```
            
* label_video.py
    * Is a [python] script used to parse a mila data file into a dictionary that is then converted to a JSON Object to be sent to the [node.js] app.

* retrain.py
	* Is a [python] script used to retrain the tensorflow neural network.

* run_on_image.py
	* Is a [python] script that *classifies* an image. Returns an object containig the labels and their accuracies.

* run_on_video.py
	* Is a [python] script that runs the algorithm for *classification* on each frame (every 1 second) of the video. It then prints the resulted object on each iteration.

### [Node.js] scripts:
* mila.js
    * Is a [node.js] script that communicates with label_data [python] script to parse the mila data file and convert the video into a objects used in the tensorflow algorithm.
    * It creates screenshots for the timemarks specified in the mila data file.
    * The screenshots are placed in the tensorflow folder accordingly to the category and the label.
    * The screenshots are used to retrain the neural network from the [tensorflow] algorithm
* app.js
    * The main app which starts the server on *port :80* accessible on http && https via the public DNS.
    * The server API allows a user to retrain the neural network by uploading a video with a mila data file.
    * The server responds on ```/retrain``` with a unique key that identifies the call
    * The server classifies a video or an image by requesting with a file at ```/classify```

### [Mila] files
* data.mila
    * With the following syntax:
``` 
category
label1:A-B,C-D,
label2:E-F
```
**where A,B,C,D,E,F are integers that represents the range in seconds within the video for that label* and label1, label2 are strings that represent the labels to train the neural network on the specified ranges*

### [Shell] files
* mila.sh
   * contains a python call with different options for tensorflow to retrain its database to recognise the new categories and labels

License
----

[MIT]

   [Tensorflow]: <https://www.tensorflow.org/>
   [node.js]: <http://nodejs.org>
   [express]: <http://expressjs.com>
   [AWS EC2]: <https://aws.amazon.com/ec2/>
   [Mila]: <https://www.mila.ai/>
   [Python]: <https://www.python.org/>
   [Shell]: <https://en.wikipedia.org/wiki/Shell_script>
   [MIT]: <https://github.com/razvangeangu/MilaAPI/blob/master/License>
