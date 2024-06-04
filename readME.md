```markdown
# Chatbot Web Application Documentation

This documentation provides a step-by-step guide on how to set up and run the chatbot web application.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [File Structure](#file-structure)
4. [HTML Structure](#html-structure)
5. [CSS Styling](#css-styling)
6. [JavaScript Functionality](#javascript-functionality)
7. [Backend Integration](#backend-integration)

## Introduction
This is a simple chatbot web application built with HTML, CSS, and JavaScript, using jQuery and Bootstrap for styling and functionality. It allows users to send messages and receive responses from a chatbot.

## Prerequisites
Before you begin, ensure you have the following:
- A web browser (e.g., Chrome, Firefox)
- Basic knowledge of HTML, CSS, and JavaScript
- A text editor (e.g., VSCode, Sublime Text)

## File Structure
The project consists of a single HTML file. Here is the basic file structure:
```
chatbot/
│
└── index.html
```

## HTML Structure
The HTML file contains the structure of the chatbot application.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css">
    <title>Chatbot</title>
    <script src="https://code.jquery.com/jquery-3.5.1.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/marked/12.0.2/marked.min.js"></script>
    <style>
        body { padding: 20px; display: flex; justify-content: center; }
        .chat-container { width: 100%; max-width: 600px; }
        .chat-box { border: 1px solid #ccc; padding: 20px; border-radius: 10px; max-height: 500px; overflow-y: scroll; }
        .chat-message { margin-bottom: 10px; padding: 10px; border-radius: 5px; }
        .chat-message.user { text-align: right; background-color: #e1ffc7; }
        .chat-message.assistant { text-align: left; background-color: #f1f1f1; }
    </style>
</head>
<body>
    <div class="container chat-container">
        <h2>Chatbot</h2>
        <div class="chat-box" id="chat-box">
            <!-- Chat messages will be appended here -->
        </div>
        <form id="chat-form">
            <div class="form-group">
                <input type="text" id="message" class="form-control" placeholder="Type your message...">
            </div>
            <button type="submit" class="btn btn-primary">Send</button>
        </form>
    </div>
    <script>
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
                    data: JSON.stringify({message: message, chat_history: chatHistory, user_id: "ayoabmi", conversation_id: conversationId}),
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
                if (role === "assistant") {
                    // Render Markdown content using marked.js
                    messageElement.html(marked.parse(content));
                } else {
                    messageElement.text(content);
                }
                chatBox.append(messageElement);
                chatBox.scrollTop(chatBox[0].scrollHeight);
            }
        });
    </script>
</body>
</html>
```

## CSS Styling
The CSS styles are embedded within the HTML file. They style the body, chat container, chat box, and chat messages.

```css
<style>
    body { padding: 20px; display: flex; justify-content: center; }
    .chat-container { width: 100%; max-width
