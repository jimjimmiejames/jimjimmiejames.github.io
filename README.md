<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Anonymous Live Chat</title>
  <!-- Twitter Card meta tags for player deep linking -->
  <meta name="twitter:card" content="player">
  <meta name="twitter:site" content="@jimjimmiejames">
  <meta name="twitter:title" content="Anonymous Live Chat">
  <meta name="twitter:description" content="Chat on X. Anonymously.">
  <!-- URL of this page; update to your actual domain -->
  <meta name="twitter:player" content="https://example.com/index.html">
  <meta name="twitter:player:width" content="600">
  <meta name="twitter:player:height" content="400">
  <!-- Optional: Indicate that the player stream is interactive -->
  <meta name="twitter:player:stream" content="true">
  
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      margin: 0;
      padding: 20px;
    }
    #chat-container {
      max-width: 600px;
      margin: 0 auto;
      background: #fff;
      border: 1px solid #ddd;
      padding: 10px;
      border-radius: 5px;
    }
    #chat-log {
      height: 300px;
      overflow-y: scroll;
      border: 1px solid #ddd;
      padding: 10px;
      margin-bottom: 10px;
      background: #fafafa;
    }
    #chat-input {
      width: 100%;
      padding: 10px;
      border: 1px solid #ddd;
      border-radius: 3px;
      box-sizing: border-box;
    }
    .chat-message {
      margin-bottom: 8px;
    }
    .chat-message span {
      font-weight: bold;
    }
  </style>
</head>
<body>
  <div id="chat-container">
    <div id="chat-log"></div>
    <input type="text" id="chat-input" placeholder="Type a message and hit Enter...">
  </div>

  <!-- Firebase Libraries -->
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>
  
  <script>
    // Replace with your own Firebase project configuration
    var firebaseConfig = {
      apiKey: "AIzaSyCTgowP7gAYC2sO7liFX_a2-F9zR27APLA",
      authDomain: "x-chat-frame.firebaseapp.com",
      databaseURL: "https://x-chat-frame-default-rtdb.firebaseio.com",
      projectId: "x-chat-frame",
      storageBucket: "x-chat-frame.firebasestorage.app",
      messagingSenderId: "719998956141YOUR_MESSAGING_SENDER_ID",
      appId: "1:719998956141:web:709c3f13dce76f1f51c564"
    };
    
    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    var db = firebase.database();

    // Generate an anonymous username
    function generateAnonymousName() {
      return "ANON" + Math.floor(Math.random() * 1000);
    }
    var userName = generateAnonymousName();
    console.log("Your anonymous name is: " + userName);

    // Reference to the chat input and log
    var chatInput = document.getElementById("chat-input");
    var chatLog = document.getElementById("chat-log");

    // Send message on Enter key press
    chatInput.addEventListener("keypress", function(e) {
      if (e.key === "Enter" && chatInput.value.trim() !== "") {
        var message = chatInput.value;
        // Push the message to Firebase
        db.ref("messages").push({
          name: userName,
          message: message,
          timestamp: Date.now()
        });
        chatInput.value = "";
      }
    });

    // Listen for new messages and update chat log
    db.ref("messages").limitToLast(50).on("child_added", function(snapshot) {
      var data = snapshot.val();
      var msgDiv = document.createElement("div");
      msgDiv.classList.add("chat-message");
      msgDiv.innerHTML = "<span>" + data.name + ":</span> " + data.message;
      chatLog.appendChild(msgDiv);
      // Auto-scroll to the bottom of the chat log
      chatLog.scrollTop = chatLog.scrollHeight;
    });
  </script>
</body>
</html>
