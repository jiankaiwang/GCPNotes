# Implementing an AI Chatbot with Dialogflow (GSP078)



keywords: `Dialogflow`, `Chatbot`, `Datasore`



## Reference

* Qwiklabs: https://www.qwiklabs.com/focuses/634?parent=catalog



## Quick Notes

The following is the flow of Dialogflow.

![](https://cdn.qwiklabs.com/ReDcMuScQnqMsZlUP0kSd3oR3NtLKZphb8LpQ%2BBdLX4%3D)



### Dialogflow Concepts and Constructs

* Dialogflow
* Agents: an NLU module
* Intents: Process a mapping between user's requests and a action's response from its software.
  * interfaces: user says, action, response, contexts
* Entities: Extract parameter values from natural language inputs.
  * types: System, Developer, User
* Contexts: current user's request
* Fulfillment: a webhook to allow you to send information to a web service and fetch the result from it



### Deploy a simple Dialogflow application to submit helpdesk tickets

* Enable `Dialogflow API` by clicking `Navigation menu` > `APIs & Services` > `Libraries`.
* Surf the web (https://dialogflow.com/), click `sign up for free` and click `sign-in with google`.

* After accepting the terms, click `Create Agent`.
  * Enter a `Agent Name`.
  * Select `Default Time zone`.
  * Select `Project ID`.

* Create a Context using `Default Welcome Intent`.
  * Add some training phrases.
  * Add some reponses in text.
  * Click `Save`. (Top Right)
  * Type some words in `Try it now`. (Right panel)



### Create Intents

* Click `Intents`, and then `Create Intent`.
* Add some training phrases and some responses as well.
* Back to intent list and hover over the new created intent, click `Add follow-up intent`.



### Allow Fulfillment to Store Help Ticket Data

* Click `Fulfillment` on the left panel.
* Enable `In-line Editor` and add the following script (`index.js`). 
  * Edit `projectId`.

```js
'use strict';
const http = require('http');
// Imports the Google Cloud client library
const Datastore = require('@google-cloud/datastore');
// Your Google Cloud Platform project ID
const projectId = 'REPLACE_WITH_YOUR_PROJECT_ID';
// Instantiates a client
const datastore = Datastore({
  projectId: projectId
});
// The kind for the new entity
const kind = 'ticket';
exports.dialogflowFirebaseFulfillment = (req, res) => {
  console.log('Dialogflow Request body: ' + JSON.stringify(req.body));
  // Get the city and date from the request
  let ticketDescription = req.body.queryResult['queryText']; // incidence is a required param
  //let name = req.body.result.contexts[0].parameters['given-name.original'];
  let username = req.body.queryResult.outputContexts[1].parameters['given-name.original'];
  let phone_number = req.body.queryResult.outputContexts[1].parameters['phone-number.original'];
  console.log('description is ' +ticketDescription);
  console.log('name is '+ username);
  console.log('phone number is '+ phone_number);
  function randomIntInc (low, high) {
    return Math.floor(Math.random() * (high - low + 1) + low);
  }
  let ticketnum = randomIntInc(11111,99999);
  // The Cloud Datastore key for the new entity
  const taskKey = datastore.key(kind);
  // Prepares the new entity
  const task = {
    key: taskKey,
    data: {
      description: ticketDescription,
      username: username,
      phoneNumber: phone_number,
      ticketNumber: ticketnum
    }
  };
  console.log("incidence is  " , task);
  // Saves the entity
  datastore.save(task)
  .then(() => {
    console.log(`Saved ${task.key}: ${task.data.description}`);
    res.setHeader('Content-Type', 'application/json');
    //Response to send to Dialogflow
    res.send(JSON.stringify({ 'fulfillmentText': "I have successfully logged your ticket, the ticket number is " + ticketnum + ". Someone from the helpdesk will reach out to you within 24 hours."}));
    //res.send(JSON.stringify({ 'fulfillmentText': "I have successfully logged your ticket, the ticket number is " + ticketnum + ". Someone from the helpdesk will reach out to you within 24 hours.", 'fulfillmentMessages': "I have successfully logged your ticket, the ticket number is " + ticketnum +  ". Someone from the helpdesk will reach out to you within 24 hours."}));
  })
  .catch((err) => {
    console.error('ERROR:', err);
    res.setHeader('Content-Type', 'application/json');
    res.send(JSON.stringify({ 'speech': "Error occurred while saving, try again later", 'displayText': "Error occurred while saving, try again later" }));    
  });
}
```

* In `package.js`, replace with the following script.

```js
{
  "name": "dialogflowFirebaseFulfillment",
  "description": "This is the default fulfillment for a Dialogflow agents using Cloud Functions for Firebase",
  "version": "0.0.1",
  "private": true,
  "license": "Apache Version 2.0",
  "author": "Google Inc.",
  "engines": {
    "node": "8"
  },
  "scripts": {
    "start": "firebase serve --only functions:dialogflowFirebaseFulfillment",
    "deploy": "firebase deploy --only functions:dialogflowFirebaseFulfillment"
  },
  "dependencies": {
    "actions-on-google": "^2.2.0",
    "firebase-admin": "^5.13.1",
    "firebase-functions": "^2.0.2",
    "dialogflow": "^0.6.0",
    "dialogflow-fulfillment": "^0.5.0",
    "@google-cloud/datastore": "^1.1.0"
  }
}
```

* Click `Deploy`.
* Add a new intent on the end of the previous one.
  * Add some training phrases.
  * In Fulfillment, enable webhook call for this intent.



### Verify that Tickets are Logged in Datastore

* Click Datastore in GCP console, you would see the ticket information showed on Dialogflow.



### Testing your Chatbot

Click `Integrations`, you can select the service your want to integrate. The simple way is to click the `Web demo` to run on the webpage.







