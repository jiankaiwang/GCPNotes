# Google Cloud Speech API: Qwik Start



The Speech API provides you to send audio and receive a text transcription from the service.



## Quick Note

Objective

-   Create an API key
-   Create a Speech API request
-   Call the Speech API request

API Reference: <https://cloud.google.com/speech-to-text/?hl=zh-tw>





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