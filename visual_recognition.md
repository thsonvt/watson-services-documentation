# Visual Recognition
## API Endpoint

https://gateway.watsonplatform.net/visual-recognition-beta/api

## Authentication
```
curl -u "{username}":"{password}" "https://gateway.watsonplatform.net/visual-recognition-beta/api/"
```

##Methods
Method | Usage
------- |-------
POST /v2/classify | Classify an image
GET /v2/classifiers | Retrieve a list of classifiers
GET /v2/classifiers/{classifier_id} | Retrieve classifier details
POST /v2/classifiers | Create a classifier
DELETE /v2/classifiers/{classifier_id} | Delete a classifier


### Classify an image
```
POST /v2/classify
```
#### Request
Parameter | Type | Description
------- |------- |-------
version | Query | (Required) The release date of the version of the API you want to use. Specify dates in YYYY-MM-DD format.
images_file | multipart/form-data | (Required) The image file (.jpg, .png, .jpg) or compressed (.zip) file of images to classify.
classifier_ids | multipart/form-data | A JSON object containing an array of classifier IDs to check images against. Omit this parameter to use all classifiers.

##### Exammple Request
```
curl -u "{username}":"{password}" \
-X POST \
-F "images_file=@test.jpg" \
-F "classifier_ids=<classifierlist.json" \
"https://gateway.watsonplatform.net/visual-recognition-beta/api/v2/classify?version=2015-12-02"
```


#### Response 
Name | Description
------- |------- 
images | The array of images
image | The file name of the image.
scores | An array of classifiers identified in the image and their corresponding scores.
classifier_id | The ID of a classifier identified in the image.
name | The name of a classifier identified in the image.
score | The score of a classifier identified in the image.

##### Example Response
```
{
  "images": [
    {
      "image": "test.jpg",
      "scores": [
        {
          "classifier_id": "sports",
          "name": "Sports",
          "score": 0.700104
        },
        {
          "classifier_id": "cricket_1234",
          "name": "Cricket",
          "score": 0.689532
        }
      ]
    }
  ]
}
```

#### Code Snippet
##### NodeJS
```
var params = {
	images_file: fs.createReadStream('./car.jpg'),
	classifier_ids: [classifierId]
};


visual_recognition.classify(params, function  (err, res) {
	if (err)
		console.log(err);
	else
		console.log(JSON.stringify(res, null, 2));
})
```


### Retrieve a list of classifiers
```
GET /v2/classifiers
```

#### Request

Parameter | Type | Description
------- |------- |-------
version | Query | (Required) The release date of the version of the API you want to use. Specify dates in YYYY-MM-DD format.
verbose | Query | Specify true to return classifier details. Omit this parameter to return a brief list of classifiers.

##### Example Request
```
curl -u "{username}":"{password}" \
-X GET \
"https://gateway.watsonplatform.net/visual-recognition-beta/api/v2/classifiers?version=2015-12-02"
```

#### Response
Name | Description
------- |------- 
classifiers | The array of classifier names.
name | The name of the classifier.
classifier_id | The ID of the classifier.
created | The time and date when classifier was created. Returned only when verbose=true.
owner | The username of user who created the classifier. Returned only when verbose=true.


##### Example Response
```
{
  "classifiers": [
    {
      "classifier_id": "nightvsday_11138698",
      "name": "night vs day"
    },
    {
      "classifier_id": "tiger_458617357",
      "name": "tiger"
    },
    {
      "classifier_id": "Black",
      "name": "Black"
    },
    {
      "classifier_id": "Blue",
      "name": "Blue"
    }
 ]
}
```

### Retrieve classifier details
Retrieve information about a specific classifier
```
GET /v2/classifiers/{classifier_id}
```

#### Request

Parameter | Type | Description
------- |------- |-------
version | Query | (Required) The release date of the version of the API you want to use. Specify dates in YYYY-MM-DD format.
classifier_id | Path | Required. The ID of the classifier for which you want details.


##### Example Request
```
curl -u "{username}":"{password}" \
-X GET \
"https://gateway.watsonplatform.net/visual-recognition-beta/api/v2/classifiers/tiger_1234?version=2015-12-02"
```

#### Response
Name | Description
------- |------- 
name | The name of the specified classifier.
classifier_id | The ID of the specified classifier.
created | The time and date when classifier was created.
owner | The username of user who created the classifier.


##### Example Response
```
{
   "name": "tiger",
   "classifier_id": "tiger_1234",
   "created": "2015-03-25T12:00:00",
   "owner": "ss324f-23sf65-sdf321-dfsgh87j"   
}
```
### Creat a classifier
Create a classifierTrain a new classifier on the uploaded image data. Upload a compressed (.zip) file of images (.jpg, .png, or .gif) with positive examples of the visual concept you want your new classifier to identify, and another compressed file with negative examples that are similar to but do NOT show that concept.

```
POST /v2/classifiers
```
#### Request

Parameter | Type | Description
------- |------- |-------
version | Query | (Required) The release date of the version of the API you want to use. Specify dates in YYYY-MM-DD format.
positive_examples | multipart/form-data | (Required) A compressed (.zip) file of images that show the new classifier. Must contain a minimum of 10 images.
negative_examples | multipart/form-data | Required) A compressed (.zip) file of images that are similar to but do NOT show the new classifier. Must contain a minimum of 10 images.
name | multipart/form-data | (Required) The name of the new classifier.


##### Example Request
```
curl -u "{username}":"{password}" \
-X POST \
-F "positive_examples=@tigers.zip" \
-F "negative_examples=@lions_leopards.zip" \
-F "name=tiger" \
"https://gateway.watsonplatform.net/visual-recognition-beta/api/v2/classifiers?version=2015-12-02"
```

#### Response
Name | Description
------- |------- 
name | The name of the new classifier.
classifier_id | The ID of the new classifier.
created | The time and date when classifier was created.
owner | The username of user who created the classifier.



##### Example Response
```
{
  "name": "tiger",
  "classifier_id": "tiger_1234",
  "created": "2015-11-23 17:43:11+00:00",
  "owner": "dkfj32-al543-324js-382js"
}
```

### Delete a classifier
Delete a custom classifier with the specified classifier ID.

```
DELETE /v2/classifiers/{classifier_id}
```

#### Request

Parameter | Type | Description
------- |------- |-------
version | Query | (Required) The release date of the version of the API you want to use. Specify dates in YYYY-MM-DD format.
classifier_id | Query | (Required) The ID of the classifier you want to delete.

##### Example Request
curl -u "{username}":"{password}" \
-X DELETE \
"https://gateway.watsonplatform.net/visual-recognition-beta/api/v2/classifiers/tiger_1234?version=2015-12-02"


#### Response
Upon successful deletion, the service returns code 200 and no JSON response.

##### Example Response
```
{}
```




