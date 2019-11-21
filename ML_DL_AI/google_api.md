# Introduction to APIs in Google (GSP294)



Keywords: `API`, `Endpoints`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/3473?parent=catalog



## Quick Notes



###  The definitions

* HTTP protocol and methods: Services or APIs exchange data over the internet. The APIs are not limited to the HTTP protocol. The APIs utilizing the HTTP protocol uses HTTP methods, these are `POST`, `PUT`, `GET`, and `DELETE`.
* Endpoints: The Endpoints are the access point to the resource, in other words, URI. These URIs are usually appended after URLs, like `https://example.com/`. The services can request the APIs' addresses, like `https://example.com/recoginze/` or `https://example.com/recoginze/v1/`.
* REST (Representational State Transfer): The key to REST is a resource-oriented design system.
* RESTful System: In Google description, a RESTful system is a resource allowing a request from a client and send the response back after the resource was processed completely.
* RESTful API: REST is the most widely used framework of APIs. Nowadays, the RESTful APIs are most using the HTTP protocol. The RESTful APIs consist of two parts, **head** and **body**. The head describes the details of an HTTP request and the body is the data for exchanging.
* Authentication: The process of determining a client's identity.
* Authorization: The process of determining what permissions an authenticated client has to the resource.



### APIs

The following is an example to create a cloud storage via JSON API.

* Create a file storing information for a new cloud storage.

```json
{  
  "name": "<YOUR_BUCKET_NAME>",
  "location": "us",
  "storageClass": "multi_regional"
}
```

* Access OAuth 2.0 Playground (https://developers.google.com/oauthplayground/) and get the `access_token` from the output in json.
* Create a cloud storage.

```sh
export OAUTH2_TOKEN=<YOUR_TOKEN>
export PROJECT_ID=<YOUR_PROJECT_ID>
curl -X POST --data-binary @values.json \
    -H "Authorization: Bearer $OAUTH2_TOKEN" \
    -H "Content-Type: application/json" \
    "https://www.googleapis.com/storage/v1/b?project=$PROJECT_ID"
```





