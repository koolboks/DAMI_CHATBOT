```markdown
# Chatbot Web Application Documentation

This documentation provides a step-by-step guide on how the front end sends and receives messages from the back end using jQuery. It also includes examples of how to interact with the back end using JavaScript, Python, and cURL.

## Table of Contents
1. [Introduction](#introduction)
2. [Front-End Communication with Back End](#front-end-communication-with-back-end)
3. [Examples](#examples)
   - [JavaScript Client](#javascript-client)
   - [Python Client](#python-client)
   - [cURL Client](#curl-client)

## Introduction
This chatbot web application allows users to send messages and receive responses from a chatbot. The front end communicates with the back end to exchange messages, maintaining a conversation history and a conversation ID.

## Front-End Communication with Back End
The front end uses jQuery to send and receive messages from the back end. Here is the core functionality:

1. **User Submits a Message**: When a user submits a message, it is appended to the chat history and displayed in the chat box.
2. **AJAX Request**: The front end sends an AJAX request to the back end with the user's message, chat history, user ID, and conversation ID.
3. **Receive Response**: The back end processes the request and returns a response along with an updated conversation ID.
4. **Display Response**: The response from the back end is appended to the chat history and displayed in the chat box.

### jQuery Example

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
            url: "/chat",
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

You can use the Fetch API to send a message to the back end from a JavaScript client.

```javascript
const chatHistory = [];
let conversationId = null;

function sendMessage(message) {
    chatHistory.push({"role": "user", "content": message});

    fetch('/chat', {
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

You can use the `requests` library to send a message to the back end from a Python client.

```python
import requests
import json

chat_history = []
conversation_id = None

def send_message(message):
    global conversation_id
    chat_history.append({"role": "user", "content": message})

    response = requests.post(
        url='http://your-backend-url/chat',
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

You can use cURL to send a message to the back end from the command line.

```sh
chat_history='[{"role": "user", "content": "Hello, chatbot!"}]'
user_id="unique_user_id"
conversation_id=null

curl -X POST http://your-backend-url/chat \
     -H "Content-Type: application/json" \
     -d '{
           "message": "Hello, chatbot!",
           "chat_history": '"${chat_history}"',
           "user_id": "'"${user_id}"'",
           "conversation_id": '"${conversation_id}"'
         }'
```

This concludes the documentation for the chatbot web application. Follow these steps to set up and run the application, and ensure your backend is properly configured to handle chat requests.
```
