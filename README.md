# 2.5D Visual Relationship Detection (2.5VRD)

2.5VRD studies the depth and occlusion relationships between two arbitrary objects. In particular, there are two scenarios: within-image (depth and occlusion) and cross-image (depth only). For more details on the dataset, please read [this paper](https://arxiv.org/abs/2104.12727).

## Dataset

See data/*.csv

## Data Description

The 2.5VRD dataset contains 110,894 images and 511,454 objects (bounding boxes and class names) from the [Open Images Dataset V4 (OID V4)](https://storage.googleapis.com/openimages/web/download_v4.html), where 219,570 pairs of objects are annotated with the depth and occlusion relationships. The table below provides a breakdown of the number of instances in training, validation, and test sets for within-image and cross-image scenarios. We collect exhaustive annotations for the validation and test sets and sparse annotations for the training set.

|               |                | Training | Validation | Test   |
|---------------|----------------|----------|------------|--------|
| Within image  | # images       | 105,694  | 1,200      | 4,000  |
|               | # object pairs | 105,694  | 6,339      | 23,724 |
| Across images | # image pairs  | 52,484   | 600        | 2,000  |
|               | # object pairs | 52,484   | 6,868      | 24,461 |


 
## Data Format

### \*image_object\*.csv

| image_id         | object_id | entity    | xmin         | xmax         | ymin         | ymax         |
|------------------|-----------|-----------|--------------|--------------|--------------|--------------|
| 0002ab0af02e4a77 |         0 | /m/09j2d  | 0.4077819884 | 0.8457270265 | 0.3854120076 | 0.9997109771 |
| 0002ab0af02e4a77 |         1 | /m/04hgtk | 0.5341079831 | 0.7458209991 | 0.1274199933 | 0.4624019861 |

* image_id: the unique id for each image from OID V4
* object_id: the object id in the corresponding image
* entity: the entity names defined in OID V4. You can convert it to a class name using OID V4’s class-descriptions-boxable.csv.
* xmin, xmax, ymin, ymax: coordinates of a bounding box, normalized to [0.0, 1.0]


### \*images_vrd\*.csv

| image_id_1       | object_id_1 | image_id_2       | object_id_2 | distance | occlusion | raw_distance | raw_occlusion |
|------------------|-------------|------------------|-------------|----------|-----------|--------------|---------------|
| 0002ab0af02e4a77 |           0 | 0002ab0af02e4a77 |           1 |        0 |         0 | 0,0,0,0,0    | 0,0,0,0,0     |
| 0003d84e0165d630 |           0 | 0003d84e0165d630 |           1 |        2 |         0 | 2,2,2,2,2    | 0,0,0,0,0     |

* image_id_*: the unique id for each image from OID V4
* object_id_*: the object id in the corresponding image
* distance: the depth relationship, taking one of the following values
    * -1: no majority 
    * 0: not sure
    * 1: object_id_1 is closer to the camera than object_id_2 
    * 2: object_id_2 is closer to the camera than object_id_1 
    * 3: object_id_1 is about the same distance to the camera as object_id_2 
* occlusion: the occlusion relationship. We set it to 0 for across_image_vrd_*.csv since the occlusion relationship does not apply to the cross-image scenario. For within-image scenarios, it takes one of the following values
    * -1: no majority
    * 0: no occlusion
    * 1: object_id_1 occludes object_id_2
    * 2: object_id_2 occludes object_id_1 
    * 3: object_id_1 occludes and is occluded by object_id_2
* raw_distance: the raw distance relationships labeled by five or more raters
* raw_occlusion: the occlusion relationships labeled by five or more raters

 
## Evaluation
We provide exhaustive annotations for the validation and test sets (i.e., for every pair of objects), allowing us to evaluate a model’s predictions using precision, recall, and the F-score. Please see the evaluation script (coming soon) for a description of the prediction format that your model should output.

## Contact us
If you have a question regarding our dataset, code, or publication, please create an issue in this repository. This is the fastest way to reach us.
If you would like to share feedback or report concerns, please email us at 2.5vrd@google.com.
 

# Copyright notice
This is the work of 2.5vrd@google.com from Google LLC, made available under the Creative Commons Attribution 4.0 License. A full copy of the license can be found at https://creativecommons.org/licenses/by/4.0/
 
 

