# Translate Text with the Cloud Translation API



## Quick Note

Reference:

*   Google Translate API : <https://cloud.google.com/translate/docs/>



## Google Cloud Shell



## Create an API Key

Create a new API key from `APIs & Services`. And export it as the environment variable on the cloud shell.

```sh
export API_KEY=AIzaSyALKBdoGWhojsEz-vG8Q8IEC_gI8E45Zpc
echo $API_KEY
```



## Translate Text (translate/version/)

In the example, we will translate the string "My name is JKW." into Spanish.

```sh
# define the text for translation
TEXT="My%20name%20is%20JKW"
# access the API
curl "https://translation.googleapis.com/language/translate/v2?target=es&key=${API_KEY}&q=${TEXT}"
```

In the request url, we set up target as the target language (here, `es` stands for Spanish).



## Detect Language (translate/version/detect)

The Translation API also supports detecting language of the text.

```sh
# define two text strings in different languages
TEXT_ONE="Meu%20nome%20é%20JKW"
TEXT_TWO="日本のグーグルのオフィスは、東京の六本木ヒルズにあります"
# access the API
curl "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}&q=${TEXT_ONE}&q=${TEXT_TWO}"
```

Notice the `language` field on the result is referred to the `ISO 639-1` (<https://en.wikipedia.org/wiki/ISO_639-1>).  And all supported languages are listed at <https://cloud.google.com/translate/docs/languages>.



Let's try another language.

```sh
# define a text string in different languages
TEXT_ONE="我的名字是JKW"
# access the API
curl "https://translation.googleapis.com/language/translate/v2/detect?key=${API_KEY}&q=${TEXT_ONE}"
```

