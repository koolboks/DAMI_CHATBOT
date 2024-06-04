
# Connecting to a Bubble Chat Template

This documentation provides a step-by-step guide on how to connect your chatbot web application to a Bubble chat template.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Step-by-Step Guide](#step-by-step-guide)
   - [Step 1: Create a Bubble Account](#step-1-create-a-bubble-account)
   - [Step 2: Create a New App](#step-2-create-a-new-app)
   - [Step 3: Install the Chat Template](#step-3-install-the-chat-template)
   - [Step 4: Set Up API Workflow](#step-4-set-up-api-workflow)
   - [Step 5: Connect Front End to Bubble](#step-5-connect-front-end-to-bubble)
4. [Examples](#examples)

## Introduction
Bubble is a no-code platform that allows you to build web applications visually. By using a Bubble chat template, you can quickly integrate a chat interface into your web application.

## Prerequisites
Before you begin, ensure you have the following:
- A Bubble account (sign up at [Bubble](https://bubble.io/))
- Basic knowledge of Bubble's visual programming interface
- A working chatbot web application

## Step-by-Step Guide

### Step 1: Create a Bubble Account
1. Go to [Bubble](https://bubble.io/) and sign up for a free account if you don't have one already.

### Step 2: Create a New App
1. Once logged in, click on the "New App" button.
2. Enter a name for your app and choose a template (or start from scratch).
3. Click "Create a new app".

### Step 3: Install the Chat Template
1. In the Bubble editor, go to the "Plugins" tab on the left sidebar.
2. Click "Add plugins".
3. Search for a chat template or plugin that suits your needs and install it.

### Step 4: Set Up API Workflow
1. In the Bubble editor, go to the "API Workflow" tab.
2. Create a new API endpoint for handling chat messages.
3. Define the necessary fields for the API (e.g., `message`, `chat_history`, `user_id`, `conversation_id`).
4. Set up the workflow to process incoming messages and return a response.

### Step 5: Connect Front End to Bubble
1. Update your front-end JavaScript code to send AJAX requests to the Bubble API endpoint.

#### Example jQuery AJAX Request

```javascript
$(document).ready(function() {
    const chatBox = $("#chat-box");
    const chatForm = $("#chat-form");
    const messageInput = $("#message");
    let chatHistory = [];
    let conversationId = null;

    chatForm.on("submit", function(event) {
        event.preventDefault();
        const message = messageInput.val();
        if (message.trim() === "") return;

        appendMessage("user", message);
        chatHistory.push({"role": "user", "content": message});

        $.ajax({
            url: "https://I_WILL_GIVE_YOU/chat",  // Replace with your AWS API endpoint
            method: "POST",
            contentType: "application/json",
            data: JSON.stringify({
                message: message,
                chat_history: chatHistory,
                user_id: "unique_user_id",
                conversation_id: conversationId
            }),
            success: function(response) {
                const botMessage = response.response;
                conversationId = response.conversation_id; // Update the conversation ID
                appendMessage("assistant", botMessage);
                chatHistory.push({"role": "assistant", "content": botMessage, "conversation_id": conversationId});
            },
            error: function() {
                appendMessage("assistant", "There was an error. Please try again.");
            }
        });

        messageInput.val("");
    });

    function appendMessage(role, content) {
        const messageElement = $("<div>").addClass("chat-message").addClass(role);
        messageElement.text(content);
        chatBox.append(messageElement);
        chatBox.scrollTop(chatBox[0].scrollHeight);
    }
});
```

## Examples

### JavaScript Client

You can use the Fetch API to send a message to the Bubble API from a JavaScript client.

```javascript
const chatHistory = [];
let conversationId = null;

function sendMessage(message) {
    chatHistory.push({"role": "user", "content": message});

    fetch('https://I_WILL_GIVE_YOU/chat', {  // Replace with your AWS API endpoint
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify({
            message: message,
            chat_history: chatHistory,
            user_id: "unique_user_id",
            conversation_id: conversationId
        })
    })
    .then(response => response.json())
    .then(data => {
        const botMessage = data.response;
        conversationId = data.conversation_id;
        chatHistory.push({"role": "assistant", "content": botMessage});
        console.log('Assistant:', botMessage);
    })
    .catch(error => {
        console.error('Error:', error);
    });
}

sendMessage("Hello, chatbot!");
```

### Python Client

You can use the `requests` library to send a message to the Bubble API from a Python client.

```python
import requests
import json

chat_history = []
conversation_id = None

def send_message(message):
    global conversation_id
    chat_history.append({"role": "user", "content": message})

    response = requests.post(
        url='I_WILL_GIVE_YOU/chat',  # Replace with your AWS API endpoint
        headers={'Content-Type': 'application/json'},
        data=json.dumps({
            'message': message,
            'chat_history': chat_history,
            'user_id': 'unique_user_id',
            'conversation_id': conversation_id
        })
    )
    
    if response.status_code == 200:
        data = response.json()
        bot_message = data['response']
        conversation_id = data['conversation_id']
        chat_history.append({"role": "assistant", "content": bot_message})
        print('Assistant:', bot_message)
    else:
        print('Error:', response.status_code)

send_message("Hello, chatbot!")
```

### cURL Client

You can use cURL to send a message to the Bubble API from the command line.

```sh
chat_history='[{"role": "user", "content": "Hello, chatbot!"}]'
user_id="unique_user_id"
conversation_id=null

curl -X POST https://I_WILL_GIVE_YOU/chat \  # Replace with your AWS API endpoint
     -H "Content-Type: application/json" \
     -d '{
           "message": "Hello, chatbot!",
           "chat_history": '"${chat_history}"',
           "user_id": "'"${user_id}"'",
           "conversation_id": '"${conversation_id}"'
         }'
```

This concludes the documentation for connecting your chatbot web application to a Bubble chat template. Follow these steps to set up and integrate your front end with the Bubble Front end.
```
