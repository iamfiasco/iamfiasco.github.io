---
layout: post
title: Hello Hackathon Part 2
---

During the Hackathon Finals the Chief Guest was Mr. Nitin Jairam Gadkari. I'm have never been a fan of a politician but Mr. Gadkari's Inauguration speech
at SIH finals captivated me like no other speaker had before. His words were.

> In this competition you aren't competing with people around you, You will be competing with yourself and in any situation you wont lose. You will always be rewarded, You will win the best version of yourself. At the end of this Hackathon you'll be surprised by the ginormous amount of work you did in this short interval. 

There were students all around India, I was privileged to be standing there. It was one of those moments which I wont ever forget. Setting aside these pep talks lets move to the Object Detection Module.

## Object Detection by HAAR based Cascade Classifier

Before 2001 object detection algorithms were pretty slow. Multiple reasons were behind this slowness, the complexity of algorithms, low processing power of Computers at that time are a few of them.
Viola and Jones in 2001 released their work on a real time object detetection algorithm. This algorithm was based on Haar Features and the classifier is called Haar Cascade Classifier.

### What actually are Haar Features ?

Similar to a convolution kernel which is slided over the image to obtain a result (multidimensional vector/matrix), A Haar Feature is slid over the image. The haar feature contains a black
and white segments. We calculate the weighted sum of the overlap (The haar feature and considered image) and then we take the difference of the two. This difference gives a value between 1 and 0.

If we take a look at a face of a person you'll see many sections of face, humans are good at segmenting and recognizing entities in an image. It takes a lot of time and effort for designing a facial recognition algorithm but humans do it with much ease. All sciences are about witnessing the actions and behavior of nature and to re-enact them to create feasable actions. When we look at a face we see the most important features the eyes, the nose and the mouth. These may described as edges. An edge in computer vision is an abrupt change in  intensity values in the neighborhood of a particular section. If you are not sure about the edge thing. Think of them as changes in colors in an image. Colors intensity can be used to segment entities in an image. There's a problem with this however which we will deal later in this post.

<img src="https://iamfiasco.github.io/images/sih2018-2/haar_cascade.png">

> Haar Features

<br>

<img src="https://iamfiasco.github.io/images/sih2018-2/6088364490_37bdbf776b.jpg">

> Regions with high matching scores. Matching score is calulated by difference in weighted sum of black and white regions. This values lies in between 0 and 1.


We have the Haar Features and an image. When we slid the feature over the kernel and find the weighted sum and we average them we get a value between 0 and 1. Closer the value to 1 higher will
be the match extent. 




Apart from facial detection they are used in vehicle license plate detection, vehicle detection and etc. In our project we used HaarCascadeClassifier to detect Cars. 

## How to use a Haar Cascade Classifier ?

first we import OpenCV in out script by

> import cv2

then we initialize the Classifiers using their respetive URI's. Replace uri1, uri2 with
the face classifier and eye classifier URI, respectively. For the sake of simplicity I
keep them in same directory so I dont have to deal with the full path.
Then we initialize the camera. We've passed 0 to VideoCapture method because we used the
primary camera and in our case the primary camera was the webcam in laptop.

```python
import cv2
faceclassf=cv2.CascadeClassifier(uri1)
eyeclassf=cv2.CascadeClassifier(uri2)
a=cv2.VideoCapture(0)
ret,img = a.read()
```

Then we need to grab images and convert them to grayscale or black and white. This conversion leads to reduce the complexity of algorithms. The RGB colored images have depth of 3, i.e think of them as a 3d matrix of shape m,n, 3 where m,n are width and height of image. The Plane cutting the matrix in 3 parts in the direction of z gives the color plane of particular color (Red, Green or Blue). These plane indicate the extent of redness, greenessand blueness of an image. Superimposing these 3 planes we get colored images. Since we generally dont need the color  representations in edge detections, we convert them to grayscale. These images have depth of 1. You guessed it right. A 2d matrix of image of shape m,n,1  or m,n. This matrix has value ranging between 0 and 255. The value 0 represents darkest shade of black and the value 255 represent the brightest white.

```python
bw = cv2.cvtColor(img,cv2.COLORBGR2GRAY)
faces = faceclassf.detectMultiScale(bw,1.3,5)
```

Scale Factor(1.3)
Parameter specifying how much the image size is reduced at each image scale. In our case we are reducing the size of the image by 30%. 
if 1.03% is used then the reduction is of 3%

MinNeighbors(5)
Parameter specifying how many neighbors each candidate rectangle should have to retain it. This parameter will affect the quality of the detected faces: higher value results in less detections but with higher quality. 
Play around with this value to find the optimal value.

```python
for (x,y,w,h) in faces:
        cv.rectangle(img,(x,y),(x+w,y+h),(0,250,0),3)
cv2.imshow('detection in image',img)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

Press any key to terminate the image. Dont try to close the image with cross button. It sucks
but most of the time Qt Freezes.

The following is complete code for face detection.

```python
import cv2
#read the image
color=cv2.imread(imgpath,1)
#convert to black and white
bw = cv2.cvtColor(color,cv2.COLOR_BGR2GRAY)
#importing the haar Cascader
classf= cv2.CascadeClassifier(cascade_classfier_path)
#detecting faces parameters = (image, scale factor, neighbors) higher the neighbors lower will be errors.
#dont make the neighbors too much as it may not detect any faces. optimum value = 3-6.
#scale factor is amount to which the image is reduced. 1.3 indicates reduction in size by 30%
faces=classf.DetectMultiScale(bw,1.3,3)
#searching for all faces and drawing rectangles around them
for (x,y,w,h) in faces:
    cv2.rectangle(color,(x,y),(x+w,y+h),(0,0,255),5)
#writing output to file
cv2.imwrite("output.jpg",color)
```

<img src="https://iamfiasco.github.io/images/sih2018-2/output.jpg">

> This is my team which participated in Smart India Hackathon 2018. This was clicked in Shri Ramdeobaba College of Management and Engineering Nagpur during SIH 2018 onsite Finals.

Earlier I told about the demerits of the haar classifier. Well the demerit is it doesnt handle outliers effectively. In the above case the person standing to the left is not looking to the camera straight. He is looking in a oblique manner so the classifier doesnt detect him. Its a very simple algorithm and still finds its use in smartphone camera app. Even some vendors take a step forward
and tell the age of the person (in Xiaomi's Mi ROMs) which in my opinion is complete fiasco. 



## Vehicle Detection

Lets get back to our source code of Vehicle Detection.

```python
import cv2
while True:
    
    ret, frames = cap.read()
     
    
    gray = cv2.cvtColor(frames, cv2.COLOR_BGR2GRAY)
    #r,thr=cv2.threshold(frames,150,200,cv2.THRESH_BINARY)

 
    
    cars = car_cascade.detectMultiScale(gray, 1.1, 1)
     
    
    for (x,y,w,h) in cars:
        # submit the periphery of the box
    	submitselection(x,y,x+w,y+h)
    	cv2.rectangle(frames,(x,y),(x+w,y+h),(0,0,255),2)
    	cv2.imshow('output',frames)
    	cv2.imwrite('ria.jpeg',frames)
    showselections()

    if cv2.waitKey(33) == 27:
        cap.release()
        break
        

cv2.destroyAllWindows()

```

The above code grabs a frame from the camera then converts it to Grayscale. Then using this HaarCascade we detect the cars in image. The submitselection() function submits the coordinates of cars to the server. Then we draw bounding box usin. cv2.rectangle() method. This code processes the footage in real time. In order to terminate script press Esc. The waitKey(33) == 27 corresponds to Esc Key. 

# Results

<img src="https://iamfiasco.github.io/images/sih2018-2/detect.png">

Here in this image you may see the rectangles drawn around the vehicles. The implementation of submit selection is not shown because it is outside the scope of the blog and it only POSTs details of vehicles to a form which is handled by Node.js.
