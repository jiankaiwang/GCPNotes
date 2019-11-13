# Classify Text into Categories with the Natural Language API



## Quick Note

### Reference

BigQuery SQL: <https://cloud.google.com/bigquery/docs/reference/standard-sql/functions-and-operators>

Data Studio with BigQuery: <https://cloud.google.com/bigquery/docs/visualize-data-studio>



## Google Cloud Shell



## API Services

### Step

*   Enable the Cloud Natural Language API service.

*   Create an API key to access `Google CLoud Natural Language API`.
    *   Export the API key as an environment variable.

        ```sh
        export API_KEY=AIzaSyAi8Aac2697FpRBpsPkejJxseVcBTMlpNM
        echo $API_KEY
        ```


## Classify a news article

The content classification of Cloud Natural Language API: <https://cloud.google.com/natural-language/docs/categories>

Create a request content in json.

```sh
touch request.json
```

Add the following content to it.

```json
{
  "document":{
    "type":"PLAIN_TEXT",
    "content":"A Smoky Lobster Salad With a Tapa Twist. This spin on the Spanish pulpo a la gallega skips the octopus, but keeps the sea salt, olive oil, pimentón and boiled potatoes."
  }
}
```

Send a request to the API.

```sh
curl "https://language.googleapis.com/v1/documents:classifyText?key=${API_KEY}" \
  -s -X POST -H "Content-Type: application/json" --data-binary @request.json
```



## Classifying a large text dataset

Here we use BBC public datasets (<http://mlg.ucd.ie/datasets/bbc.html>).

The util `gsutil` provides a command line interface for Cloud Storage. Here we use a preloaded dataset on google storage.

```sh
gsutil cat gs://text-classification-codelab/bbc_dataset/entertainment/001.txt
```



## Creating a BigQuery table for our categorized text data



### Step

*   We first create a BigQuery table on the `BigQuery` Sevice and then `CREATE DATASET`.

*   Create a table (`CREATE TABLE`) named **article_data**.



## Classifying news data and storing the result in BigQuery

Before writing a script to send the news data to the Natural Language API, you need to create a **service account**. This will be used to authenticate to the Natural Language API and BigQuery from a Python script.

```sh
export PROJECT=qwiklabs-gcp-02a2dd58fe12407f
```

Run the commands from Cloud Shell to create a service account.

```sh
gcloud iam service-accounts create my-account --display-name my-account

gcloud projects add-iam-policy-binding $PROJECT --member=serviceAccount:my-account@$PROJECT.iam.gserviceaccount.com --role=roles/bigquery.admin

gcloud iam service-accounts keys create key.json --iam-account=my-account@$PROJECT.iam.gserviceaccount.com

export GOOGLE_APPLICATION_CREDENTIALS=key.json
echo $GOOGLE_APPLICATION_CREDENTIALS
```

Create a file called `classify-text.py` and add the following into it. Remenber replacing GCP Project ID.

```python
from google.cloud import storage, language, bigquery

# Set up our GCS, NL, and BigQuery clients
storage_client = storage.Client()
nl_client = language.LanguageServiceClient()
# TODO: replace YOUR_PROJECT with your project id below
bq_client = bigquery.Client(project='qwiklabs-gcp-02a2dd58fe12407f')

dataset_ref = bq_client.dataset('news_classification_dataset')
dataset = bigquery.Dataset(dataset_ref)
table_ref = dataset.table('article_data') # Update this if you used a different table name
table = bq_client.get_table(table_ref)

# Send article text to the NL API's classifyText method
def classify_text(article):
        response = nl_client.classify_text(
                document=language.types.Document(
                        content=article,
                        type=language.enums.Document.Type.PLAIN_TEXT
                )
        )
        return response

rows_for_bq = []
files = storage_client.bucket('text-classification-codelab').list_blobs()
print("Got article files from GCS, sending them to the NL API (this will take ~2 minutes)...")

# Send files to the NL API and save the result to send to BigQuery
for file in files:
        if file.name.endswith('txt'):
                article_text = file.download_as_string()
                nl_response = classify_text(article_text)
                if len(nl_response.categories) > 0:
                        rows_for_bq.append((article_text, nl_response.categories[0].name, nl_response.categories[0].confidence))

print("Writing NL API article data to BigQuery...")
# Write article text + category data to BQ
errors = bq_client.insert_rows(table, rows_for_bq)
assert errors == []
```

Run the script.

```sh
python classify-text.py
```

Navigate to the `article_data` table in the BigQuery and click Query Table. Start a new query.

```sh
SELECT * FROM `qwiklabs-gcp-02a2dd58fe12407f.news_classification_dataset.article_data`
```



## Analyzing categorized news data in BigQuery

First, count all categories which are most common in dataset.

```sql
SELECT
  category,
  COUNT(*) c
FROM
  `qwiklabs-gcp-02a2dd58fe12407f.news_classification_dataset.article_data`
GROUP BY
  category
ORDER BY
  c DESC
```

Find out the specific category.

```sql
SELECT * FROM `qwiklabs-gcp-02a2dd58fe12407f.news_classification_dataset.article_data`
WHERE category = "/Arts & Entertainment/Music & Audio/Classical Music"
```

Find out the articles whose confidence score greater than 90%.

```sql
SELECT * FROM `qwiklabs-gcp-02a2dd58fe12407f.news_classification_dataset.article_data`
WHERE cast(confidence as float64) >= 0.9
```











