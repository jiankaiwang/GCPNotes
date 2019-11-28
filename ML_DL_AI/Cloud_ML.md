# Extract, Analyze, and Translate Text from Images with the Cloud ML APIs (GSP075)



Keywords: `Vision API`, `OCR`, `Translation API`, `Natural Language API`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1836?parent=catalog
* Translation API supports 100+ languages, refer to https://cloud.google.com/translate/docs/languages.



## Quick Note



## Create an API Key

Create an API key from APIs & Services (You have to make sure you has already activated the Cloud Machine Learning Engine service.) and then export it as the environment variable on the cloud shell.

```sh
export API_KEY=AIzaSyBxNIzOzQGso_OJo7Uba81W6Xk2Ii5PSZI
```



## Upload an image to a cloud storage bucket

Create a bucket named `jkw-image-bucket` from google storage.

Upload an image.

![](../static/sign.jpg)

Set the permission for all users with read (ENTITY: `Group`, ACCESS: `Reader`).



## Create your Vision API request

```sh
touch ocr-request.json
```

Add the following content to it.

```json
{
  "requests": [
      {
        "image": {
          "source": {
              "gcsImageUri": "gs://jkw-image-bucket/sign.jpg"
          }
        },
        "features": [
          {
            "type": "TEXT_DETECTION",
            "maxResults": 10
          }
        ]
      }
  ]
}
```



## Call the Vision API's text detection method

Send a request to access the API.

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY}
```

The OCR method is able to extract lots of text from our image.

Save the result for further using translation API.

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @ocr-request.json  https://vision.googleapis.com/v1/images:annotate?key=${API_KEY} -o ocr-response.json
```



## Sending text from the image to the Translation API

We can use Translation API to translate multiple languages. We create a request content for translation in json.

```sh
touch translation-request.json
```

Add the following content.

```json
{
  "q": "your_text_here",
  "target": "en"
}
```

Extract the ocr result in oct-response.json and replace the query string in request.json.

```sh
STR=$(jq .responses[0].textAnnotations[0].description ocr-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" translation-request.json
```

Now you can send the translation-request.json to API.

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @translation-request.json https://translation.googleapis.com/language/translate/v2?key=${API_KEY} -o translation-response.json
```

Inspect the response file.

```sh
cat translation-response.json
```



## Analyzing the image's text with the Natural Language API

The Natural Language API helps us understand text by extracting entities, analyzing sentiment and syntax, and classifying text into categories. Use the `analyzeEntities` method to see what entities the Natural Language API can find in the text from your image.

Create a request.json with the following content.

```sh
touch nl-request.json
```

```json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"your_text_here"
  },
  "encodingType":"UTF8"
}
```

* **type:** Supported type values are `PLAIN_TEXT` or `HTML`.
* **content:** pass the text to send to the Natural Language API for analysis. The Natural Language API also supports sending files stored in Cloud Storage for text processing. To send a file from Cloud Storage, you would replace `content` with `gcsContentUri` and use the value of the text file's uri in Cloud Storage.
* **encodingType:** tells the API which type of text encoding to use when processing the text. The API will use this to calculate where specific entities appear in the text.



Extract the response text and replace the content in nl-request.json.

```sh
STR=$(jq .data.translations[0].translatedText  translation-response.json) && STR="${STR//\"}" && sed -i "s|your_text_here|$STR|g" nl-request.json
```



Call the `analyzeEntities` endpoint of the Natural Language API with this `curl`request:

```sh
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @nl-request.json
```
























