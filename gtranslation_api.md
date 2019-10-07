# Google Translation API



## Reference



* official API document: https://cloud.google.com/translate/docs/



## Quick Note

* a general API call

```sh
export API_KEY=<API key fetched from google cloud console>

# define two text strings in different languages
TEXT_ONE="Meu%20nome%20é%20JKW"
TEXT_TWO="日本のグーグルのオフィスは、東京の六本木ヒルズにあります"

# access the API
curl "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}&q=${TEXT_ONE}&q=${TEXT_TWO}"
```

