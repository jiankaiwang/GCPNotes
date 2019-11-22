# Google Cloud Speech API: Qwik Start (GSP119)



The Speech API provides you to send audio and receive a text transcription from the service.



keywords: `Speech API`, `Speech-to-Text`



## Reference

* Qwiklabs: [https://www.qwiklabs.com/focuses/588?catalog_rank=%7B%22rank%22%3A1%2C%22num_filters%22%3A0%2C%22has_search%22%3Atrue%7D&parent=catalog&search_id=3863232](https://www.qwiklabs.com/focuses/588?catalog_rank={"rank"%3A1%2C"num_filters"%3A0%2C"has_search"%3Atrue}&parent=catalog&search_id=3863232)
* API Reference: <https://cloud.google.com/speech-to-text/?hl=zh-tw>



## Quick Note

Objective

-   Create an API key
-   Create a Speech API request
-   Call the Speech API request





## The Google Cloud Shell





## Create an API Key

Under Google Cloud Shell, export the environment variable.

```sh
export API_KEY=AIzaSyBBM6MCT3W-1cce2OmXJ5p32DjgFQVacHQ
echo $API_KEY
```

Create a request file in json to use the API.

```sh
touch request.json
```

Here we use a example audio file (https://storage.cloud.google.com/speech-demo/brooklyn.wav). And the file is already uploaded to the bucket and is public to all users with read permission.

And add the following content to the file.

```json
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```

`FLAC` is the encoding type for `.raw` files. The link (<https://cloud.google.com/speech/reference/rest/v1/RecognitionConfig>) provides more information.



## Call the Speech API

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```

The Speech API supports both `synchronous` and `asynchronous` speech to text transcription. In this example you sent it a complete audio file, but you can also use the `syncrecognize` method to perform streaming speech to text transcription while the user is still speaking.