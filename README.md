# Step 2. Fulfillments

In [Step 1. Hello World](https://github.com/tibbing/jwy-dlgflow-demo/tree/steps/1.HelloWorld), you created your first Intent. When the user says **"My first intent"**, the bot replies with **"Hello world!"**.

In this step, you will learn how to create a custom response using a fulfillment. A fulfillment is basically a way to create a dynamic response, based on the input phrase or question - instead of having pre-defined, static responses. 


----
## Enable fulfillments

1. Navigate to **Fulfillments** in the left menu
2. Enable the **Inline Editor** switch
3. This will create a Cloud function for Firebase that can handle requests from Dialogflow for you. You can, optionally create your own backend and call it using a webhook instead. 
4. In the **Inline Editor**:

Paste the following code: ([Source code](/src/fulfillment-fn-node6.js))

    'use strict';

    const functions = require('firebase-functions');
    const {WebhookClient} = require('dialogflow-fulfillment');

    process.env.DEBUG = 'dialogflow:debug'; // enables lib debugging statements

    exports.dialogflowFirebaseFulfillment = functions.https.onRequest((request, response) => {
      const agent = new WebhookClient({ request, response });

      function welcome (agent) {
        agent.add(`Welcome to my agent!`);
      }

      function fallback (agent) {
        agent.add(`I didn't understand`);
        agent.add(`I'm sorry, can you try again?`);
      }

      function helloWorld (agent) {
        agent.add(`Hello World from Firebase!`);
      }

      let intentMap = new Map();
      intentMap.set('Default Welcome Intent', welcome);
      intentMap.set('Default Fallback Intent', fallback);
      intentMap.set('HelloWorld', helloWorld);
      agent.handleRequest(intentMap);
    });

This piece of code utilizes the Dialogflow Fulfillment SDK for Node.js, and defines handlers for three different intents: **Default Welcome Intent**, **Default Fallback intent**, and **HelloWorld**. 

The function **helloWorld**(*agent*) is adds a response when the HelloWorld intent is triggered. This response can, of course be anything you like and also depend on your application's state or the context that is provided with the request. 

You may also provide multiple responses if you like, as displayed in the fallback intent. 


![Fulfillment Inline Editor](FulfillmentInline.JPG?raw=true "Fulfillment Inline Editor")



----
## Using the fulfillment

1. Now, in order to use our new fulfillment, we need to enable fulfillments on the intent we created earlier.
2. Go back to the **Intents** page, and select the **HelloWorld** Intent.
3. Scroll down to the bottom of the page, and expand the **Fulfillment** section.
4. Click the switch next to **Enable webhook call for this intent** to tell Dialogflow to use our fulfillment for the HelloWorld intent.
5. Click **Save** and wait for the message saying *Agent training completed*

![Enable webhook call](HelloWorldWithFulfillment.JPG?raw=true "Enable webhook call")


----
## Testing

1. In the top right panel, type **"My first intent"** and hit enter.
2. The default response should be **"Hello world from Firebase!"**


![Testing](HelloWorldWithFulfillmentTest.JPG?raw=true "Testing")

----

You have now learned how to programmatically respond to questions asked by the user. This is useful in most applications as you probably won't want to only serve static content. 
 
Now proceed to [Step 3. Follow-up questions](https://github.com/tibbing/jwy-dlgflow-demo/tree/steps/3.FollowUps)

