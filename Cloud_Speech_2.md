# Speech to Text Transcription with the Cloud Speech API





## Quick Note

Qwiklab Link: <https://google.qwiklabs.com/focuses/2187?parent=catalog>



## Google Cloud Shell





## Create an API Key

Create an API from `APIs & Services`. And set the environment variable to the gcp shell.

```sh
export API_KEY=AIzaSyBSn6AvfU2uvaUlqLj3VSFdDuNxmZXYq0k
echo $API_KEY
```



## Create your Speech API request

Here we use the example audio file (https://storage.cloud.google.com/speech-demo/brooklyn.wav). This audio file was already uploaded to the bucket and set to the public for all users.

Create a request file in json for further sending to the API.

```sh
touch request.json
```

Add the following content to the file.

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

In `config`, the field `encoding` is the only one required for the API.



## Call the Speech API

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```



## Speech to text transcription in different languages

More than 100 languages are supported on the Speech API.

You first upload the sample file to the bucket and then can simply change the `languageCode` file in the `config` section of the request.json file.

```json
 {
  "config": {
      "encoding":"FLAC",
      "languageCode": "fr"
  },
  "audio": {
      "uri":"gs://speech-language-samples/fr-sample.flac"
  }
}
```

Alternatively, you can pass a `base64` encoded string of your audio content.

Here we use an audio in France example for the API and fetch the result.

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```













