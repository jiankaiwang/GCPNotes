# Entity and Sentiment Analysis with the Natural Language API (GSP038)



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/1843?parent=catalog



## Quick Notes



### Create a API Key

```sh
export API_KEY=<your api key generated from or listed in API Credentials>
```





### Make an Entity Analysis Request

Analyze the text with entities.

Request information (request.json)

```json
{
	"document":{
		"type":"PLAIN_TEXT",
		"content":"Joanne Rowling, who writes under the pen names J. K. Rowling and Robert Galbraith, is a British novelist and screenwriter who wrote the Harry Potter fantasy series."
	},
	"encodingType":"UTF8"
}
```

Make a request call.

```sh
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json > result.json
```



### Sentiment analysis with the Natural Language API

Analyze the text and reveal the sentiment with sentences.

Request information (request.json)

```json
{
	"document":{
		"type":"PLAIN_TEXT",
		"content":"Harry Potter is the best book. I think everyone should read it."
	},
	"encodingType": "UTF8"
}
```

Make a request call.

```sh
curl "https://language.googleapis.com/v1/documents:analyzeSentiment?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```



### Analyzing entity sentiment

Analyze the text and reveal the sentiment with entities.

Request information (request.json)

```json
 {
  "document":{
    "type":"PLAIN_TEXT",
    "content":"I liked the sushi but the service was terrible."
  },
  "encodingType": "UTF8"
}
```

Make a request call.

```sh
curl "https://language.googleapis.com/v1/documents:analyzeEntitySentiment?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```



### Analyzing syntax and parts of speech

Analyze the syntax of the sentence and part of the speech.

Request information (request.json)

```json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content": "Joanne Rowling is a British novelist, screenwriter and film producer."
  },
  "encodingType": "UTF8"
}
```

Make a request call.

```sh
curl "https://language.googleapis.com/v1/documents:analyzeSyntax?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```



### Multilingual natural language processing

APIs also support multiple languages.

Request information (request.json)

```json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"日本のグーグルのオフィスは、東京の六本木ヒルズにあります"
  }
}
```

Make a request call.

```sh
curl "https://language.googleapis.com/v1/documents:analyzeEntities?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```





