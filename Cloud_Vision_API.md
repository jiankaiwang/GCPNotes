# Detect Labels, Faces, and Landmarks in Images with the Cloud Vision API



## Quick Notes

Objective

*   Creating a Vision API request and calling the API with curl

*   Using the label, face, and landmark detection methods of the vision API



## Activate Google Cloud Shell

*   Google Cloud Shell
*   GCP command line: `gcloud`
```cmd
gcloud --version
gcloud auth list
gcloud config list project
```



## Create an API Key

Using Cloud Vision API requires a API key and it can be generated from `APIs & Services`.

In the cloud shell, export the key as the environment variables.

```cmd
export API_KEY=AIzaSyD9rvBzW40BcVnV6DwOsdcdII5k_jUk3x0
```



## Upload an Image to a Cloud Storage bucket

There are two ways to send an image to the Vision API for image detection: 

*   by sending the API a base64 encoded image string, 
*   or passing it the URL of a file stored in Google Cloud Storage.

Here we use Google Cloud Storage bucket. First we have to **create a bucket** (called `jkwtest-images-bucket`) in Storage service.

After we created a bucket, we upload a image to the bucket.

Next we have to edit the permission for all users that can read the image.



## Create your Vision API request

Create a `request.json` using tools like `vim`, etc.

```sql
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://jkwtest-images-bucket/donuts.png"
          }
        },
        "features": [
          {
            "type": "LABEL_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
```



## Label Detection

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

The result is like the below.

```json
          "mid": "/m/02wbm",
          "description": "Food",
          "score": 0.9424965,
          "topicality": 0.9424965
        },
        {
          "mid": "/m/0hnyx",
          "description": "Pastry",
          "score": 0.8173416,
          "topicality": 0.8173416
        },
        {
          "mid": "/m/02q08p0",
          "description": "Dish",
          "score": 0.8076026,
          "topicality": 0.8076026
        },
        {
          "mid": "/m/01ykh",
          "description": "Cuisine",
          "score": 0.79036003,
          "topicality": 0.79036003
        },
        {
          "mid": "/m/03nsjgy",
          "description": "Kourabiedes",
          "score": 0.77726763,
          "topicality": 0.77726763
        },
        {
          "mid": "/m/06gd3r",
          "description": "Angel wings",
          "score": 0.73792106,
          "topicality": 0.73792106
        },
        {
          "mid": "/m/06x4c",
          "description": "Sugar",
          "score": 0.71921736,
          "topicality": 0.71921736
        },
        {
          "mid": "/m/01zl9v",
          "description": "Zeppole",
          "score": 0.7111677,
          "topicality": 0.7111677
        }
      ]
    }
  ]
}
```

`mid` value that maps to the item's `mid` in Google's [Knowledge Graph](https://www.google.com/intl/bn/insidesearch/features/search/knowledge.html). You can use the `mid` when calling the [Knowledge Graph API](https://developers.google.com/knowledge-graph/) to get more information on the item.



## Web Detection

In addition to getting labels on what's in your image, the Vision API can also search the Internet for additional details on your image.

Edit the `requests.json` in changing **type** from `LABEL_DETECTION` to `WEB_DETECTION` .

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```



## Face and Landmark Detection

The face detection method returns data on faces found in an image, including the emotions of the faces and their location in the image.

Landmark detection can identify common (and obscure) landmarks. It returns the name of the landmark, its latitude and longitude coordinates, and the location of where the landmark was identified in an image.

```json
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://jkwtest-images-bucket/selfie.png"
          }
        },
        "features": [
          {
            "type": "FACE_DETECTION"
          },
          {
            "type": "LANDMARK_DETECTION"
          }
        ]
      }
  ]
}
```



## **Calling the Vision API and parsing the response**

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```



## Explore other Vision API methods

More methods for example are `Logo detection`, `Safe search detection`, and `Text detection` and so on.

Method: images.annotate: <https://cloud.google.com/vision/docs/reference/rest/v1/images/annotate#Feature>