# Google Natural Language API



## Reference



* official API document:
  * Introduction: <https://cloud.google.com/natural-language/docs/basics>
  * API Document: <https://cloud.google.com/natural-language/docs/>



## Request


* a simple request (`request.json`) to API in json

```json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"A Smoky Lobster Salad With a Tapa Twist. This spin on the Spanish pulpo a la gallega skips the octopus, but keeps the sea salt, olive oil, pimentón and boiled potatoes."
  },
  "encodingType":"UTF8"
}
```



## Quick Note

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>
curl "https://language.googleapis.com/v1/documents:classifyText?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```

* Analyze Entities

```sh
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @nl-request.json
```

