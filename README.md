
# Object Detection in a video using Centroid Approach and Yolov5

Object detection is a computer vision approach for detecting things in photos and videos. To obtain relevant results, object detection algorithms often use machine learning or deep learning. We can recognise and locate objects of interest in photos or video in a handful of seconds when we glance at them. The purpose of object detection is to use a computer to imitate this intelligence.
## What is Object Tracking
### Object tracking is the process of:

1. Taking an initial set of object detections (such as an input set of bounding box coordinates)
2. Creating a unique ID for each of the initial detections
3. And then tracking each of the objects as they move around frames in a video, maintaining the assignment of unique IDs

## The Centroid Tracking procedure
The centroid tracking algorithm is a multi-step process. We will review each of the tracking steps in this section.

### Step 1: Accept bounding box coordinates and compute centroids



![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

The centroid tracking algorithm implies that each identified item in each frame is given a set of bounding box (x, y)-coordinates.

These bounding boxes can be generated using any form of object detector (colour thresholding + contour extraction, Haar cascades, HOG + Linear SVM, SSDs, Faster R-CNNs, and so on), as long as they are computed for each frame in the video.

We must compute the "centroid," or the centre (x, y)-coordinates, of the bounding box after we obtain the bounding box coordinates. Accepting a set of bounding box coordinates and determining the centroid is demonstrated in Figure 1 above.

We'll give these bounding boxes unique IDs because they're the first ones our system sees.

### Step 2: Compute Euclidean distance between new bounding boxes and existing objects

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

We repeat Step #1 of computing object centroids for each subsequent frame in our video stream; however, instead of assigning a new unique ID to each detected object (which would defeat the purpose of object tracking), we must first determine if the new object centroids (yellow) can be associated with the old object centroids (red) (purple).
We compute the Euclidean distance (highlighted with green arrows) between each pair of existing object centroids and input object centroids to complete this operation.

As you can see in the above figure, we have this time spotted three things in our image. Two existent objects make up the two pairings that are near together.

The Euclidean distances between each pair of original centroids (yellow) and new centroids are then calculated (purple). But how can we truly match and correlate these points using the Euclidean distances between them?

The answer can be found in Step #3.

### Step 3: Update (x, y)-coordinates of existing objects

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

The centroid tracking algorithm's key assumption is that a given object may move between frames, but that the distance between the centroids for frames F t and F t + 1 will be lower than all other distances between objects.

As a result, we may construct our object tracker by associating centroids with minimal distances between consecutive frames.

Our centroid tracker system chooses to correlate centroids that minimise their respective Euclidean distances, as shown in Figure 3.

What about the lone point in the bottom-left corner?

What are we going to do with it now that it hasn't been affiliated with anything?

### Step 4: Register new objects

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

We must register the new object if there are more input detections than there are existing objects being tracked. The term "registration" simply refers to the process of adding a new object to our list of tracked objects by:

1. Assigning it a new object ID
2. Storing the centroid of the bounding box coordinates for that object

Then we can return to Step #2 and repeat the process for each frame in our video stream.

Figure 4 depicts the process of associating current object IDs with minimum Euclidean distances before registering a new object.

### Step 5: Deregister old objects

Any good object tracking method must be able to detect when an object has vanished, vanished from view, or vanished from the field of view.

The exact way you handle these scenarios depends on where your object tracker is supposed to go, but in this version, we'll deregister old objects if they can't be matched to any existing objects for a total of N frames.

  
