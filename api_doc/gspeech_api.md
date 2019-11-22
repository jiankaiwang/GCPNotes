# Google Speech API



## Reference



* official API document: <https://cloud.google.com/speech-to-text/docs/?hl=zh-tw>>



## Request


* a simple request (`request.json`) to API in json

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

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"
```



## Quick Note

* Time Length: Differentiate the API calls by the time length of an audio file.

| Types (Length) | Limits  | Call Methods             |
| -------------- | ------- | ------------------------ |
| Short          | < 1 min | Restful API, gRPC, Sync  |
| Long           | > 1min  | Restful API, gRPC, Async |
| Streaming      | -       | gRPC                     |





