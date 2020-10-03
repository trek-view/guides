---
description: 'Photos belong to Sequences and have labels, object detections and view points.'
---

# Photos

### About

Every sequence consists of two or more images \(Photos\). Photos are either 2D or 360 imported from Mapillary.

Photos can only belong to one sequence.

It is useful to have a human searchable record of what is inside an image. For example; show me all photos that contain cars.

Photos therefore have 4 types of associated metadata

* Sequence data: the sequence information for the sequence they belong to
* Image data: such as capture time, camera type, etc.
* Image labels
  * Manual: labels added to images by MTPW users
  * Auto detected: object and feature detections discovered by computer vision

### Sequence data

[Because each image belongs to a sequence, it inherits the sequence data.](create.md)

### Image data

This data is essentially the metadata added to the image when it was taken and/or processed \(things like location, elevation, camera type, etc.\).

### Image labels

When in Photo view, you will be able to see image labels in an image. When viewing a photo you can toggle the original/detection tab to enter detection mode.

#### Auto detected

![](../../../.gitbook/assets/89c7003b-ac28-4791-ab88-5b20923b2222.png)

You'll see Mapillary detections that have been reported by Mapillary for the image.

You can read more about Mapillary:

* [Object detections](https://help.mapillary.com/hc/en-us/articles/115000967191-Object-detections)
* [Map features](https://help.mapillary.com/hc/en-us/articles/115002332165)

#### Manual

![](../../../.gitbook/assets/download-2-%20%281%29.png)

You might notice objects that have not been labelled in an image. In which case, you can add them yourself.

You can also add your own labels by tracing around the object you wish to label in the image.

#### How to manually label an object

To do this:

1. In detection mode select "Add label"
2. Select the label you want to add
   1. If you don't see the type of label you want to add, [you can create a Challenge](../challenges.md). This will not only add the label to our label database, but also encourage others to use this label across all Map the Paths imagery
3. Draw the outline of the object using the polygon creation tool. Once the Polygon has been drawn, your label will be added





