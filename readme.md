# Using Deep Learning to Identify Cyclists Risk Factors in London
[Jupyter Notebook](https://github.com/warcraft12321/RoadSafety/blob/master/main.ipynb) | [Report](https://github.com/warcraft12321/RoadSafety/blob/master/text/report.pdf) | [Presentation](https://github.com/warcraft12321/RoadSafety/blob/master/text/presentation.pdf)

### Aim

The aim of this project was to use imagery to estimate safety on the roads of London, from a cyclist’s perspective. After
a brief introduction to the most important road safety indicators, a ranked list with several risk factors was compiled.
Risk factors were obtained from Google StreetView (GSV) imagery dataset using the object detection YOLOv5 (released in June 2020 by Glenn Jocher) and
image segmentation PSPNet101 (Pyramid Scene Parsing Network) (released in July 2017 by Hengshuang Zhao et al.).

Imagery dataset contains 518 350 images of greater London, distributed across 4833 boroughs. Each image is labeled in accordance
to the LSOA it belongs. Images are organized in sets of 4 which corresponds to 4 90º angles from a total of 129 588 points.

Both YOLOv5 and PSPNet101 were benchmarked and validated using a set of 1 image per LSOA from the dataset.

Data was storaged and processed in the secure High Performance Cluster from Imperial College London.

### GSV Dataset

**Description**

Along this project, it was used a Google StreetView imagery dataset from Greater London. It includes, approximately,
1/2 million images distributed across all LSOAs. For each data point there are 4 images ranging from 0º to 360º. These
images were previously pre-processed (not as part of this project) to guarantee uniformity across them. More details
are provided below.

**Number of images per LSOA in Greater London**

![](./img/dataset/number_images.png)

**Distribution by latitude and longitude of all image locations**

![](./img/dataset/images_distribution.png)

**Example of data point with 4 images covering 360º angle**

![](./img/dataset/gsv_img_angles.png)

img_id = 23052

**Statistics on the number of available images per LSOA in the dataset**

Minimum | Maximum | Mean | Standard Deviation | Mode | Median
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
1 | 211 | 27 | 24 | 25 | 11

**Total number of available images**

Number Images in GSV Dataset | Number of LSOA identified Images (image_labels.csv) | Number of Non-Repeated LSOA identified Images (image_labels.csv) | Number of Image Identified LSOAs (image_labels.csv)
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
518 350 | 512 812 | 478 724 | 4832

**Generated files**

File            |  Description
:-------------------------:|:-------------------------:
[imgId_lsoa.json](https://github.com/warcraft12321/RoadSafety/blob/master/imgId_lsoa.json) |  File linking image ids to the LSOAs they belong.
[lsoa_number_images.json](https://github.com/warcraft12321/RoadSafety/blob/master/yolov5/lsoa_number_images.json) |  Number of available GSV images per LSOA.

### Object Detection | [YOLOv5](https://github.com/ultralytics/yolov5)

**Description**

YOLOv5 is the most recent version of YOLO which was originally developed by Joseph Redmon. First version runs in a framework
called Darknet which was purposely built to execute YOLO.

Version 5 is the 2nd model which was not developed by Joseph Redmon (after version 4) and the first running in the
state-of-the-art machine learning framework, in this case, PyTorch.

This model was pre-trained using Coco dataset. Thus, it is able to identify 80 object categories. Distributed
over 11 categories.

<details>
  <summary>Full list of MS Coco categories</summary>

Person | Vehicle | Outdoor | Animal | Accessory | Sports | Kitchen | Food | Furniture | Electronic | Appliance | Indoor
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
Person | Bicycle | Traffic Light | Bird | Backpack | Frisbee | Bottle | Banana | Chair | TV | Microwave | Book
|  | Car | Fire Hydrant | Cat | Umbrella | Skis | Wine Glass | Apple | Couch | Laptop | Oven | Clock
|  | Motorcycle | Stop Sign | Dog | Handbag | Snowboard | Cup | Sandwich | Potted Plant | Mouse | Toaster | Vase
|  | Airplane | Parking Meter | Horse | Tie | Sports Ball | Fork | Orange | Bed | Remote | Sink | Scissors
|  | Bus | Bench | Sheep | Suitcase | Kite | Knife | Broccoli | Dinning Table | Keyboard | Refrigerator | Teddy Bear
|  | Train |  | Cow |  | Baseball Bat | Spoon | Carrot | Toilet | Cell Phone | | Hair Drier
|  | Truck |  | Elephant |  | Baseball Glove | Bowl | Hot dog | | | | Toothbrush
|  | Boat |  | Bear |  | Skateboard | | Pizza | | | |
|  |  |  | Zebra |  | Surfboard | | Donut | | | |
|  |  |  | Giraffe |  | Tennis Racket | | Cake | | | |
|  |  |  |  |  |  | | | | | |
|  |  |  |  |  |  |  | | | | |
|  |  |  |  |  |  |  | | | | |
|  |  |  |  |  |  |  | | | | |
|  |  |  |  |  |  |  | | | | |
|  |  |  |  |  |  |  | | | | |
|  |  |  |  |  |  |  | | | | |

</details>

**YOLOv5 executed in a static image from the dataset**

![](./img/yolov5/YOLOv5.png)

**YOLOv5 executed in real-time in a video from London**

[![YOLOv5 | London](http://img.youtube.com/vi/ncwcWl-zOws/0.jpg)](http://www.youtube.com/watch?v=ncwcWl-zOws "YOLOv5 | London")

**Number of detections to the top 15 most common objects**

Object            |  Number Detections* | Object            |  Number Detections* | Object            |  Number Detections*
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
Car  |  1 509 344 | Bicycle  |  10 894 | Chair  |  2191
Person  |  107 266 | Motorcycle  |  8970 | Handbag  |  2090
Truck  |  70 083 | Traffic Light  |  6310 | Backpack  |  1939
Potted Plant  |  37 917 | Bench  |  5013 | Stop Sign  |  1282
Bus  |  11 512 | Clock  |  2750 | Fire Hydrant  |  1168

\* >= 0.5 YOLOv5 score

**LSOA objects distribution in Greater London**

Bicycle (&#8593;)          |  Bus (&#8595;)
:-------------------------:|:-------------------------:
![](./img/yolov5/lsoas/bicycle.png)  |  ![](./img/yolov5/lsoas/bus.png)

Car (&#8595;) | Motorcycle (&#8595;)
:-------------------------:|:-------------------------:
![](./img/yolov5/lsoas/car.png)  |  ![](./img/yolov5/lsoas/motorcycle.png)

Parking Meter (&#8595;) | Person (&#8593;)
:-------------------------:|:-------------------------:
![](./img/yolov5/lsoas/parking_meter.png)  |  ![](./img/yolov5/lsoas/person.png)

Stop Sign (&#8593;) | Traffic Light (&#8593;)
:-------------------------:|:-------------------------:
![](./img/yolov5/lsoas/stop_sign.png)  | ![](./img/yolov5/lsoas/traffic_light.png)

Train (&#8595;) | Truck (&#8595;)
:-------------------------:|:-------------------------:
![](./img/yolov5/lsoas/train.png)  | ![](./img/yolov5/lsoas/truck.png)

\* &#8593; and &#8595; were positively and negatively associated to road safety, respectively.

** Combining some of the previous risk factors **

Pedestrians and Cyclists in Greater London (average number per image) (&#8593;)          |  Traffic (buses, cars and trucks) in Greater London (average number per image) (&#8595;)
:-------------------------:|:-------------------------:
![](./img/yolov5/normalized_pedestrians_score.png)  |  ![](./img/yolov5/normalized_traffic_score.png)

**Combination of the 2 previous LSOAs**

![](./img/yolov5/normalized_safety_score.png)

**Top 15 detected objects correlation matrix**

![](./img/yolov5/correlation_matrix_p_values.png)

**Top 15 detected objects distribution**

![](./img/yolov5/object_detection_distribution.png)

**Detailed object detection information for all categories in MS Coco, present in the GSV imagery**

<details>
  <summary>COCO Objects Stats for all LSOAs</summary>

Category | Total Number Occurrences | Minimum | Maximum | Mean
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
Person | 107 266 | 0 | 695 | 22
Bicycle | 10 894 | 0 | 144 | 2
Car | 1 509 344 | 13 | 1891 | 312
Motorcycle | 8970 | 0 | 74 | 1
Airplane | 234 | 0 | 4 | 0
Bus | 11 512 | 0 | 36 | 2
Train | 657 | 0 | 5 | 0
Truck | 70 083 | 0 | 192 | 14
Boat | 971 | 0 | 22 | 0
Traffic Light | 6310 | 0 | 54 | 1
Fire Hydrant | 1168 | 0 | 11 | 0
Stop Sign | 1282 | 0 | 8 | 0
Parking Meter | 968 | 0 | 7 | 0
Bench | 5013 | 0 | 23 | 1
Bird | 509 | 0 | 9 | 0
Cat | 27 | 0 | 2 | 0
Dog | 419 | 0 | 3 | 0
Horse | 35 | 0 | 2 | 0
Sheep | 13 | 0 | 5 | 0
Cow | 79 | 0 | 2 | 0
Elephant | 2 | 0 | 1 | 0
Bear | 3 | 0 | 1 | 0
Zebra | 5 | 0 | 1 | 0
Giraffe | 22 | 0 | 1 | 0
Backpack | 1939 | 0 | 20 | 0
Umbrella | 378 | 0 | 9 | 0
Handbag | 2090 | 0 | 28 | 0
Tie | 39 | 0 | 5 | 0
Suitcase | 467 | 0 | 8 | 0
Frisbee | 384 | 0 | 4 | 0
Skis | 2 | 0 | 1 | 0
Snowboard | 0 | 0 | 0 | 0
Sports Ball | 102 | 0 | 4 | 0
Kite | 465 | 0 | 16 | 0
Baseball Bat | 7 | 0 | 3 | 0
Baseball Glove | 1 | 0 | 1 | 0
Skateboard | 245 | 0 | 3 | 0
Surfboard | 80 | 0 | 2 | 0
Tennis Racket | 13 | 0 | 1 | 0
Bottle | 71 | 0 | 9 | 0
Wine Glass | 1 | 0 | 1 | 0
Cup | 9 | 0 | 2 | 0
Fork | 0 | 0 | 0 | 0
Knife | 0 | 0 | 0 | 0
Spoon | 1 | 0 | 1 | 0
Bowl | 6 | 0 | 2 | 0
Banana | 6 | 0 | 3 | 0
Apple | 6 | 0 | 2 | 0
Sandwich | 8 | 0 | 3 | 0
Orange | 2 | 0 | 1 | 0
Broccoli | 1 | 0 | 1 | 0
Carrot | 0 | 0 | 0 | 0
Hot Dog | 1 | 0 | 1 | 0
Pizza | 4 | 0 | 2 | 0
Donut | 3 | 0 | 1 | 0
Cake | 1 | 0 | 1 | 0
Chair | 2191 | 0 | 56 | 0
Couch | 16 | 0 | 2 | 0
Potted Plant | 37 917 | 0 | 406 | 7
Bed | 30 | 0 | 2 | 0
Dining Table | 133 | 0 | 9 | 0
Toilet | 30 | 0 | 3 | 0
Tv | 68 | 0 | 2 | 0
Laptop | 1 | 0 | 1 | 0
Mouse | 0 | 0 | 0 | 0
Remote | 0 | 0 | 0 | 0
Keyboard | 0 | 0 | 0 | 0
Cell Phone | 21 | 0 | 2 | 0
Microwave | 4 | 0 | 1 | 0
Oven | 6 | 0 | 1 | 0
Toaster | 0 | 0 | 0 | 0
Sink | 4 | 0 | 1 | 0
Refrigerator | 320 | 0 | 7 | 0
Book | 11 | 0 | 7 | 0
Clock | 2750 | 0 | 31 | 0
Vase | 17 | 0 | 4 | 0
Scissors | 1 | 0 | 1 | 0
Teddy Bear | 4 | 0 | 1 | 0
Hair Dryer | 0 | 0 | 0 | 0
Toothbrush | 0 | 0 | 0 | 0
Total | 1 785 642 | 0 | 1891 | 370

</details>

**Generated Files**

File            |  Description
:-------------------------:|:-------------------------:
[total_stats.json](https://github.com/warcraft12321/RoadSafety/blob/master/yolov5/total_stats.json) |  Number of objects detected by YOLOv5 in GSV imagery by class.
[lsoa_objects_number.json](https://github.com/warcraft12321/RoadSafety/blob/master/yolov5/lsoa_objects_number.json) |  Number of objects detected by YOLOv5 in GSV imagery by class and LSOA.
[lsoa_objects_number_average_per_image.json](https://github.com/warcraft12321/RoadSafety/blob/master/yolov5/lsoa_objects_number_average_per_image.json) |  Average number of objects detected by YOLOv5 in GSV imagery per image (includes all classes and LSOAs). JSON format.
[lsoa_objects_number_average_per_image.csv](https://github.com/warcraft12321/RoadSafety/blob/master/yolov5/lsoa_objects_number_average_per_image.csv) |  Average number of objects detected by YOLOv5 in GSV imagery per image (includes all classes and LSOAs). CSV format.
[yolov5_lsoa](https://drive.google.com/drive/folders/1G-EdZtO3bqRzG-OqnumDWjP08yihJ05q?usp=sharing) |  1 image per London LSOA with the detected objects identified.

**Future Directions**

Analysis of a significant set of GSV images in London unveiled meaningful LSOA level patterns. One is the
airplane distribution in the areas closer to the 2 airports in Greater London. Second, the presence of potted plants
was found to be more significant around the biggest parks.
This shows the potential of GSV imagery analysis is not limited to assess road safety.

Airplane | Potted Plant
:-------------------------:|:-------------------------:
![](./img/yolov5/airplane_marked.png) | ![](./img/yolov5/potted_plant_marked.png)

Correlations | Top Misclassification
:-------------------------:|:-------------------------:
![](./img/yolov5/person_handbag.gif) | ![](./img/yolov5/misclassifications.png)

### Image Segmentation | [PSPNet101](https://github.com/hellochick/PSPNet-tensorflow)

**Description**

Image segmentation models reached a precision plateau (in terms of average IoU) in the previous 2 years. Due to their
long execution times, it was chosen the model executing faster and with the higher precision.

PSPNet101 was pre-trained in Cityscapes dataset. This way, it was able to label all pixels from an image across 100
categories.

<details>

  <summary>Full list of Cityscapes categories</summary>

Void | Flat | Construction | Object | Nature | Sky | Human | Vehicle
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
Unlabeled | Road | Building | Pole | Vegetation | Sky | Person | Car
Ego Vehicle | Sidewalk | Wall | Polegroup | Terrain |  | Rider | Truck
Rectification Border | Parking | Fence | Traffic Light |  |  |  | Bus
Out of ROI | Road | Guard Rail | Traffic Sign |  |  |  | Caravan
Static |  | Bridge |  |  |  |  | Trailer
Dynamic |  | Tunnel |  |  |  |  | Train
Ground |  |  |  |  |  |  | Motorcycle
|  |  |  |  |  |  |  | Bicycle
|  |  |  |  |  |  |  | License Plate

</details>

**Example of a segmented image with identified labels included**
![](./img/pspnet101/PSPNet101.png)

**Segmented images distribution by number of pixels**
![](./img/pspnet101/image_segmentation_distribution2.png)

**Number of labeled pixels for the top 20 most common categories**

Pixel Label            |  Number Pixels | Pixel Label            |  Number Pixels | Pixel Label            |  Number Pixels | Pixel Label            |  Number Pixels
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------:
Building  |  47 394 852 284 | Sidewalk  |  2 772 560 820 | Motorcycle  |  299 507 380 | Traffic Sign | 58 135 598
Sky  |  38 423 367 965 | Fence  |  2 177 733 764 | Person  |  232 309 236 | Rider | 13 948 361
Road  |  38 235 843 337 | Terrain  |  1 787 689 493 | Bicycle  |  95 469 333 | Traffic Light | 12 472 659
Vegetation  |  30 977 112 560 | Wall  |  765 524 909 | Truck  |  91 256 316 | Train | 6 842 318
Car  |  9 830 297 990 | Pole  |  303 407 190 | Bus  |  81 476 810 | |

**Generated Files**

File            |  Description
:-------------------------:|:-------------------------:
[total_stats.json](https://github.com/warcraft12321/RoadSafety/blob/master/pspnet101/total_stats.json) |  Total Number of Pixels per Cityscapes Label in GSV Dataset.
[rgb_label.json](https://github.com/warcraft12321/RoadSafety/blob/master/pspnet101/rgb_label) |  Conversion from RGB values to a Cityscapes label.
[pspnet101_lsoa](https://drive.google.com/drive/folders/1fel8ew7h2eNJRMkXpv9lF4Zl1pydo4h-?usp=sharing) |  Folder with 1 segmented image per London LSOA.

### Supervisors
[Majid Ezzati](https://www.imperial.ac.uk/people/majid.ezzati) (Imperial College London) | [Ricky Nathvani](https://www.imperial.ac.uk/people/r.nathvani) (Imperial College London)

Featured in Towards Data Science (Medium) -> [Article](https://towardsdatascience.com/imperial-college-london-1c9bb442926)

Roadmap -> [Wiki](https://github.com/warcraft12321/RoadSafety/wiki)

Draft -> [Google Doc](https://docs.google.com/document/d/1_THCRamy1gcYiNmc30Uqsic7OGh7pB3A4WUXo2TqV7g/edit?usp=sharing)