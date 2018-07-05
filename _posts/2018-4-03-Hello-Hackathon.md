---
layout: post
title: Hello Hackathon
---

Competitions are so cool you get to learn a lot of things from people you are competing with. In these tough moments you comprehend
and transcend you limits. The best thing about them are that it has limited times and often you find yourself competing with 
Time itself.
The next time you encounter a competion or hackathon do step forward and let the adrenaline take over  
# Prologue

Our team participated in Smart India Hackathon 2018 (Ministry of Road Transport and Highways). It was weird that our team's name 
was AlgoGeeks. We bagged the 9th position in Smart India Hackathon 2018 Nagpur Finals, so yeah we did a great job. 

Our project was based on Smart Parking Systems. Parking systems need to be more transparent just like the Movie seats booking
where the customer has complete control over chosing the seat location. We wanted to provide real time tracking and monitoring of
cars. The intention was to minimize the number of man power so we need to code scripts to track cars and to distinguish them
by their number plate. 
I was handed the task of Image Processing and pipelining different the data via various APIs written in Nodejs and Python.

We used a Raspberry Pi and Night Vision Camera. Yes the RPi wasnt capable doing image processing so we needed
to send the images to the backend (I used my own Laptop) for this purpose. 

## Raspberry Configuration

We used Raspbian OS and picamera module to control the camera with Python. The code clicked an image every second and not all
images were sent to the backend. Images were sent only when the previous image and present image were different i.e. There 
was a movement. We calculated the difference using euclidean distance and the threshhold was set to 80%. To know more about 
Euclidean Distance visit [Euclidean Distance](https://en.wikipedia.org/wiki/Euclidean_distance). Fun fact this method of difference
calculation is so powerful that it is used even in Fingerprint matching. My Team Leader Sudhanshu and I were the writers of the code.

## RPi Code to send images to Backend

We used requests to upload files via a POST form.
following is the code for the above purpose

```python
import requests
imgname = "URI OF IMAGE"
url = "http://localhost:5000/"
fin = open('{}'.format(imgname), 'rb')
files = {'file': fin}
try:
  r = requests.post(url, files=files)
	print r.text
finally:
	fin.close()
```
## Backend (Image Processing)

The backend of comprises of the Image Processing Code and the API which interfaces the RPi
We used Express.js which is a Nodejs framework which removes a lot of boilerplate code from out sourcecode and the code looks beautiful. Its similar to Flask Web Framework.
We used multer to handle multipart forms.

In case You dont have express and multer but have Node.js and are wondering to get the code running
make sure you have a package.json. Try creating one with npm init and by following the instructions.

> npm install --save express multer

The following code is for the API which handles incoming files from Rpi

```javascript
const express = require('express');
const multer = require('multer');
const upload = multer({
  dest: 'uploads/' // this saves your file into a directory called "uploads"
}); 

const app = express();

app.get('/', (req, res) => {
  res.sendFile(__dirname + '/index.html');
});


app.post('/', upload.single('file'), (req, res) => {
console.log(res.statusCode)
  res.redirect('/')
});
```

And finally here's the HTML code for the form

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>AlgoGeeks !</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
  </head>
  <body>
    <form action="/" enctype="multipart/form-data" method="post">
      <input type="file" name="file" id="xyz">
      <input type="submit" value="Upload">
    </form>  
  </body>
</html>
```

Initally my intention was to write the complete walkthrough of project in one blog but I gave up on that because this part of the blog deals with the APIs. In the next section we will
deal with the car detections and in part 3 we will deal with the mighty Image Processing section which will need a lot of explanation.

Hope you enjoyed the post.
If you find errors in the post please do mail the corrections at [stormivofficial@gmail.com](mailto:stormivofficial@gmail.com)
