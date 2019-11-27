# Entity and Sentiment Analysis with the Natural Language API (GSP038)



keywords: `Natural Language API`, `Entity Analysis`, `Sentiment Analysis`, `Syntax Analysis`, `Part of Speech`, `Linguistic Analysis`



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

The example result.

```json
{
  "documentSentiment": {
    "magnitude": 0.8,
    "score": 0.4
  },
  "language": "en",
  "sentences": [
    {
      "text": {
        "content": "Harry Potter is the best book.",
        "beginOffset": 0
      },
      "sentiment": {
        "magnitude": 0.7,
        "score": 0.7
      }
    },
    {
      "text": {
        "content": "I think everyone should read it.",
        "beginOffset": 31
      },
      "sentiment": {
        "magnitude": 0.1,
        "score": 0.1
      }
    }
  ]
}
```

>* The score is a number ranging from -1 to 1 and represents how the negative and the positive sentiment the sentence is.
>* The magnitude represents the sentiment weight of a sentence regardless that it is the positive or negative sentiment.



### Analyzing entity sentiment

Analyze the text and reveal the sentiment with entities. For example, the following sentence `I liked the sushi but the service was terrible.` composed of both positive and negative sentiment with different entities or ideas. The API provides breaking down the sentence and analyze its entities in a sentiment way. 

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

The example result.

```json
{
  "entities": [
    {
      "name": "sushi",
      "type": "CONSUMER_GOOD",
      "metadata": {},
      "salience": 0.52716845,
      "mentions": [
        {
          "text": {
            "content": "sushi",
            "beginOffset": 12
          },
          "type": "COMMON",
          "sentiment": {
            "magnitude": 0.9,
            "score": 0.9
          }
        }
      ],
      "sentiment": {
        "magnitude": 0.9,
        "score": 0.9
      }
    },
    {
      "name": "service",
      "type": "OTHER",
      "metadata": {},
      "salience": 0.47283158,
      "mentions": [
        {
          "text": {
            "content": "service",
            "beginOffset": 26
          },
          "type": "COMMON",
          "sentiment": {
            "magnitude": 0.9,
            "score": -0.9
          }
        }
      ],
      "sentiment": {
        "magnitude": 0.9,
        "score": -0.9
      }
    }
  ],
  "language": "en"
}
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

The example result.

```json
{
  "entities": [
    {
      "name": "日本",
      "type": "LOCATION",
      "metadata": {
        "mid": "/m/03_3d",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Japan"
      },
      "salience": 0.23854347,
      "mentions": [
        {
          "text": {
            "content": "日本",
            "beginOffset": 0
          },
          "type": "PROPER"
        }
      ]
    },
    {
      "name": "グーグル",
      "type": "ORGANIZATION",
      "metadata": {
        "mid": "/m/045c7b",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Google"
      },
      "salience": 0.21155767,
      "mentions": [
        {
          "text": {
            "content": "グーグル",
            "beginOffset": 9
          },
          "type": "PROPER"
        }
      ]
    },
    ...
  ]
  "language": "ja"
}
```

> Judge the language of a sentence and do an entity analysis on it.



