# Google Translation API



## Reference



* official API document: https://cloud.google.com/translate/docs/



## Restful APIs

### Detect Languages

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>

# define two text strings in different languages
TEXT_ONE="Meu%20nome%20é%20JKW"
TEXT_TWO="日本のグーグルのオフィスは、東京の六本木ヒルズにあります"

# access the API
curl "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}&q=${TEXT_ONE}&q=${TEXT_TWO}"
```

* a request from a local file

```sh
curl -s -X POST -H "Content-Type: application/json" --data-binary @translation-request.json https://translation.googleapis.com/language/translate/v2?key=${API_KEY} -o translation-response.json
```

### Translate Languages

```sh
export API_KEY=<API key fetched from google cloud console>

# define the query string
TEXT="My%20name%20is%20JKW"

# send a request to translate the query into the target language
curl "https://translation.googleapis.com/language/translate/v2?target=es&key=${API_KEY}&q=${TEXT}"
```

