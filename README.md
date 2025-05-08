# Webex-AI-Bot-via-n8n
A basic framework for a responsive Webex Bot, utilising OpenAI via n8n

![image](https://github.com/user-attachments/assets/59fb46a2-140b-4556-aefa-120436855e41)



Navigate to developer.webex.com log in and select "Create a Bot", providing a Name and Username. Once registered, record your Bot Token (Bearer Token) and your Bot ID
![image](https://github.com/user-attachments/assets/121a43c6-77df-46b7-8b94-3fdc68136476)

Create a Webhook node in n8n. This extracts the message ID of the message the Webhook heard. It will generate a unique URL. Note there are different URLs for Test and Prod, and you will need separate Webhooks for both URLs if you are testing too. 
![image](https://github.com/user-attachments/assets/099a7ced-122c-46e2-bcec-f118ecd8bb9e)
HTTP Method: POST
Auth: none
Respond: Immediately

Create a "Webex by Cisco" node to pull the message contents
![image](https://github.com/user-attachments/assets/893ffddd-657d-4cf7-8b56-31b6f527f16d)
This will use the Message ID number received by the Webhook, to pull the actual message contents.

Create a "AI Agent" node
![image](https://github.com/user-attachments/assets/4b73bee3-af32-40d7-ac8f-03ab10bf8d26)
This format only passes the message received by the previous node, but further parameters can be passed as needed

Attach your selected model to the "Basic LLM Chain" node, ensuring that you create the necessary auth with your OpenAI API
![image](https://github.com/user-attachments/assets/2bb28f0f-2ed7-42ca-84da-12d3c2f7eb5c)


Create a HTTP Request and enter details as pictured below, with a roomId for the relevant space. You can pass this in as a JSON object from the initial message's origin, however I'm having some challenges with this that we've not overcome yet.
![image](https://github.com/user-attachments/assets/2953e9c2-b0d2-452c-812b-1605f718806d) ![image](https://github.com/user-attachments/assets/1c53c7db-825e-4a82-b484-6f576fc5f608)

Navigate to https://developer.webex.com/docs/api/v1/webhooks/create-a-webhook to create the Webhook which listens to messages tagging your Bot. Input your bearer token at the top
![image](https://github.com/user-attachments/assets/ed6900bc-8d81-4880-8787-8130282e4bdc)
Name: as you wish
Resource: messages
Event: Created
Filter: mentionedPeople=me

Press "RUN" and you should see an HTTP 200:OK response to indicate that the Webhook is up and persistent

Once this is complete, you should be able to add your bot to a space, send a message that tags them, and you should recieve a response in the relevant room

