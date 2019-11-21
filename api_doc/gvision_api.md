# Google Vision API



## Reference



* official API document: <https://cloud.google.com/vision/docs/?hl=zh-tw>
* quick request doc: https://cloud.google.com/vision/docs/tutorials?hl=zh-tw



## Request


* a simple request (`request.json`) to API in json

```json
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
          "type": "<the first type for API calls>",
          "maxResults": 10
        },
        {
          "type": "<the second type for API calls>",
          "maxResults": 10
        }
      ]
    }
  ]
}
```

* listing API types in curl request

| Purposes                | API Type in Request  | Description |
| ----------------------- | -------------------- | --|
| Label Detection         | `LABEL_DETECTION`    |Detect the label, object or content.|
| Web Detection           | `WEB_DETECTION`      |Search the content for more information via the internet like wikipedia or URL, etc.|
| Face and | `FACE_DETECTION` | Detect the faces and their emotions. |
| Land Detection | `LANDMARK_DETECTION` |Detect the locations in an image.|
| Object detection | `OBJECT_LOCALIZATION` |Detect objects in an image.|
| Logo detection | | Search common logos. |
| Safe search detection | | Used for filtering the image by its content.  |
| text detection || Run a OCR analysis to extract text from images. |
| text detection || More advanced results are in document-like structure. |
| Crop Hints || Crop a part of a image as a optimized hint. |


## Quick Note

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

