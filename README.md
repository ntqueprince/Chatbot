<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot</title>
    <style>
    body {
        font-family: Arial, sans-serif;
        margin: 0;
        padding: 0;
        background-color: #f7f7f7;
    }
    
    .container {
        max-width: 100%;
        margin: 0 auto;
        padding: 10px;
    }
    
    .header {
        text-align: center;
        padding: 10px 0;
    }
    
    .header h1 {
        font-size: 20px; /* छोटा किया गया हेडिंग */
        margin: 5px 0;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    .avatar {
        width: 30px;
        height: 30px;
        background-color: #5271ff;
        border-radius: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        font-weight: bold;
        margin-right: 10px;
    }
    
    .chat-container {
        height: calc(100vh - 180px);
        overflow-y: auto;
        padding: 10px;
        background-color: #fff;
        border-radius: 10px;
        box-shadow: 0 2px 5px rgba(0,0,0,0.1);
    }
    
    .message {
        margin-bottom: 10px;
        padding: 10px;
        max-width: 80%;
        word-wrap: break-word;
    }
    
    .bot-message {
        background-color: #f0f0f0;
        border-radius: 18px 18px 18px 4px;
        margin-right: auto;
        display: flex;
        align-items: flex-start;
    }
    
    .input-container {
        display: flex;
        margin-top: 15px;
    }
    
    .message-input {
        flex: 1;
        padding: 12px 15px;
        border: 1px solid #ddd;
        border-radius: 25px;
        font-size: 14px;
        outline: none;
    }
    
    .send-button {
        background-color: #5271ff;
        color: white;
        border: none;
        border-radius: 50%;
        width: 45px;
        height: 45px;
        margin-left: 10px;
        cursor: pointer;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    
    .toggle-theme {
        position: absolute;
        top: 20px;
        right: 20px;
        background: none;
        border: none;
        font-size: 20px;
        cursor: pointer;
    }
    
    /* डार्क मोड और अन्य स्टाइल्स आप दिए गए CSS से जोड़ सकते हैं */
    .dark-mode {
        background-color: #1a1a2e;
        color: #f0f0f0;
    }
    
    .chat-container {
        scrollbar-width: thin;
        scrollbar-color: rgba(155, 155, 155, 0.5) transparent;
    }
    
    .chat-container::-webkit-scrollbar {
        width: 6px;
    }
    
    .chat-container::-webkit-scrollbar-track {
        background: transparent;
    }
    
    .chat-container::-webkit-scrollbar-thumb {
        background-color: rgba(155, 155, 155, 0.5);
        border-radius: 20px;
    }
    
    .user-message {
        background-color: #e6f2ff;
        border-radius: 18px 18px 4px 18px;
    }
    
    .dark-mode .user-message {
        background-color: #293145;
    }
    
    .dark-mode .bot-message {
        background-color: #1e1e30;
    }
    
    /* अन्य स्टाइल्स आपके दिए CSS से */
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1><div class="avatar">S</div>Ask to Shivang</h1>
            <button class="toggle-theme" id="themeToggle">🌙</button>
        </div>
        
        <div class="chat-container" id="chatContainer">
            <div class="message bot-message">
                <div class="avatar">S</div>
                <div class="message-content">
                    Hello! I'm Shivang's AI assistant. How can I help you today?
                </div>
            </div>
            <!-- अन्य संदेश यहां जुड़ेंगे -->
        </div>
        
        <div class="input-container">
            <input type="text" class="message-input" id="messageInput" placeholder="Type your message...">
            <button class="send-button" id="sendButton">
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="currentColor" viewBox="0 0 16 16">
                    <path d="M15.964.686a.5.5 0 0 0-.65-.65L.767 5.855a.5.5 0 0 0-.042.945l4.988 1.328a.5.5 0 0 0 .64-.19l2.664-3.838a.5.5 0 0 1 .745.145.5.5 0 0 1-.114.742l-3.678 2.671a.5.5 0 0 0-.172.648l1.881 3.763a.5.5 0 0 0 .944-.47l4.986-10.238a.5.5 0 0 0-.227-.616z"/>
                </svg>
            </button>
        </div>
    </div>

    <script>
        // यहां आपका जावास्क्रिप्ट कोड आएगा
        const themeToggle = document.getElementById('themeToggle');
        const body = document.body;
        
        themeToggle.addEventListener('click', () => {
            body.classList.toggle('dark-mode');
            themeToggle.textContent = body.classList.contains('dark-mode') ? '☀️' : '🌙';
        });
        
        // चैट फंक्शनैलिटी यहां जोड़ें
    </script>
</body>
</html>
