# tririga-assistant-proxy
Proxy for forwarding requests to Watson Assistant assistant.


The steps below use a new IBM Cloud product, currently in beta, called IBM Code Engine.  You can find out more at [https://cloud.ibm.com/docs/codeengine](https://cloud.ibm.com/docs/codeengine).  But feel free to host this code however you wish.


#### 1. Install the IBM Cloud CLI by following the [Installing the IBM Cloud CLI instructions](https://cloud.ibm.com/docs/codeengine?topic=codeengine-install-cli#cli-setup).

#### 2. Clone or download the files from this repo.

#### 3. Copy the sample config file.

`cp config.sample config`

#### 4. Edit `config` with your favorite editor and replace the following strings:

a. `YOUR_WA_API_KEY_HERE` - replace with the API key from your Watson Assistant service.

b. `SOME_KEY_YOU_GIVE_TA_TEAM` - replace with some string you make up or generate one using https://guidgenerator.com/

c. `YOUR_ASSISTANT_ID` - replace with the ID for your Assistant created in your Watson Assistant service.

#### 5. Create and select an IBM Code Engine project.

`ibmcloud ce project create --name myproj --select`

#### 6. Execute the `./run` script.

If something goes wrong and you want to start over, use `./run clean` to delete everything.

#### 7. Test the proxy using the following cURL command.  Replace the following strings in the command below:

a. `YOUR_PROXY_URL` - You should find the URL for your proxy near the end of the script output (starts with `https://assistant-proxy` and ends with `codeengine.appdomain.cloud`)  

b. `SOME_KEY_YOU_GIVE_TA_TEAM` - same string you used above

c. `SOME_CODE_ADDED_AS_AN_INTENT_EXAMPLE` - a string with no spaces that you have added as an intent example that the TRIRIGA Assistant will send to your assistant (e.g. create-lease, move-desk)

```
curl --location --request POST 'YOUR_PROXY_URL' \
--header 'Content-Type: application/json' \
--data-raw '{
    "sessionId": "",
    "integration_id": "SOME_KEY_YOU_GIVE_TA_TEAM",
    "wa_payload": {
        "input": {
            "message_type": "text",
            "text": "SOME_CODE_ADDED_AS_AN_INTENT_EXAMPLE",
            "options": {
                "return_context": true,
                "debug": true
            }
        }
    }
}'
```

The JSON response should contain an appropriate response from your Watson Assistant skill.

#### 8. Send information to the TRIRIGA Assistant team:

a. The string you replaced `SOME_KEY_YOU_GIVE_TA_TEAM` with, and 

b. the URL to your assistant proxy.