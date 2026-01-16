# Webex-AI-Bot-via-n8n
A basic framework for a responsive Webex Bot, utilising OpenAI via n8n. JSON template is attached.

<img width="1630" height="556" alt="image" src="https://github.com/user-attachments/assets/448e36ba-4b40-4c11-903f-eeb20bcb2346" />


Navigate to developer.webex.com log in and select "Create a Bot", providing a Name and Username. Once registered, record your Bot Token (Bearer Token) and your Bot ID. The Bearer token will be input to a Custom Auth credential, in the format shown in the "Custom Auth" file.

![image](https://github.com/user-attachments/assets/121a43c6-77df-46b7-8b94-3fdc68136476)

Create 2 Webhook nodes in n8n. This extracts the message ID of the message the Webhook heard. It will generate a unique URL. Note there are different URLs for Test and Prod, and you will need separate Webhooks for both URLs if you are testing too. We have 1 Webhook for where the bot is mentioned in a space, and another for where the bot is direct messaged.

![image](https://github.com/user-attachments/assets/099a7ced-122c-46e2-bcec-f118ecd8bb9e)

HTTP Method: POST
Auth: none
Respond: Immediately

After the Webhook nodes, we have two "Switch" nodes. One will halt the workflow if the message received came from the bot itself, so ensure you update this node to include the @webex.bot address for your bot.

<img width="512" height="367" alt="image" src="https://github.com/user-attachments/assets/92f77abb-06be-4782-92dc-1a173db08486" />

The next "Switch" node will only continue the workflow if the sender is on an approved list, so put your own email and anyone else approved to use the bot in the array of this node.

<img width="507" height="380" alt="image" src="https://github.com/user-attachments/assets/ae465faf-242f-455d-a95b-4bd3d4f2f13d" />

Next is a HTTP Request node which will GET the message contents based on the ID received from the Webhook.

<img width="517" height="433" alt="image" src="https://github.com/user-attachments/assets/e4dcee80-75b2-4b99-ba83-df6f2f90b5b1" />

Click Create a new Credential, select Custom Auth, and then input your bearer token in the format shared in the "Custom Auth" file.

<img width="1483" height="578" alt="image" src="https://github.com/user-attachments/assets/0dcc7293-ced6-459a-a09c-83aa08ace418" />

Next I created a sessionId JSON object, which is necessary for more complex workflows with chat histories and multiple users.

<img width="509" height="669" alt="image" src="https://github.com/user-attachments/assets/101d73df-94e4-4f62-a938-08c041dee129" />

Create a "AI Agent" node

<img width="505" height="508" alt="image" src="https://github.com/user-attachments/assets/698e6ed9-9c88-436b-8b70-997738d15bbe" />

Attach your selected model to the "Chat Model" endpoint, ensuring that you create the necessary auth with your OpenAI API

![image](https://github.com/user-attachments/assets/2bb28f0f-2ed7-42ca-84da-12d3c2f7eb5c)


Create a HTTP Request and enter details as pictured below, with a roomId for the relevant space.

![image](https://github.com/user-attachments/assets/2953e9c2-b0d2-452c-812b-1605f718806d) <img width="502" height="700" alt="image" src="https://github.com/user-attachments/assets/8b5d8c5f-76d5-4445-8ad4-d3a0c0156fff" />

Navigate to https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook to create the 4 Webhooks. 2 which listen to messages tagging your Bot; 2 which listen for direct messages. Input your bearer token at the top. Details for each of the 4 are listed below

![image](https://github.com/user-attachments/assets/ed6900bc-8d81-4880-8787-8130282e4bdc) <img width="511" height="496" alt="image" src="https://github.com/user-attachments/assets/a84c9a02-210b-48f3-b126-b5f2fc6847a3" />

```
Name: Bot Direct Message
Target URL: The Production URL from inside your Direct Messaged Webhook node
Resource: messages
Event: Created
Filter: roomType=direct
```
```
Name: Bot Direct Message TEST
Target URL: The Test URL from inside your Direct Messaged Webhook node
Resource: messages
Event: Created
Filter: roomType=direct
```

```
Name: Bot Tagged
Target URL: The Production URL from inside your Webhook node
Resource: messages
Event: Created
Filter: mentionedPeople=me
```
```
Name: Bot Tagged TEST
Target URL: The Test URL from inside your Webhook node
Resource: messages
Event: Created
Filter: mentionedPeople=me
```

Press "RUN" and you should see an HTTP 200:OK response to indicate that the Webhook is up and persistent

Once this is complete, you should be able to add your bot to a space, send a message that tags them, or message them directly, and recieve a response in the relevant room

![image](https://github.com/user-attachments/assets/50332951-cdd8-4bca-9b90-f4a40eeda712)


With these basics working, you can expand your workflow endlessly, adding things like MCP tools, or a PostgreSQL database for chat history and RAG
<img width="1840" height="868" alt="image" src="https://github.com/user-attachments/assets/28feaa74-20e8-4fca-81f2-facb7fb22f44" />


