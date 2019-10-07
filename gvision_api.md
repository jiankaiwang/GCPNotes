# Google Vision API



## Reference



* official API document: <https://cloud.google.com/vision/docs/?hl=zh-tw>



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
          "type": "<the type for API calls>",
          "maxResults": 10
        }
      ]
    }
  ]
}
```

* listing API types in request

| Purposes                | API Type in Request  |
| ----------------------- | -------------------- |
| Label Detection         | `LABEL_DETECTION`    |
| Web Detection           | `WEB_DETECTION`      |
| Face and Land Detection | `LANDMARK_DETECTION` |



## Quick Note

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

