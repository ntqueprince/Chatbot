<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Ask to Shivang</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@fortawesome/fontawesome-free@6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            transition: background-color 0.3s, color 0.3s;
            height: 100vh;
            overscroll-behavior: none;
        }
        
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
        
        .bot-message {
            background-color: #f0f0f0;
            border-radius: 18px 18px 18px 4px;
        }
        
        .dark-mode .bot-message {
            background-color: #1e1e30;
        }
        
        .typing-indicator {
            display: flex;
            align-items: center;
        }
        
        .typing-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            margin: 0 1px;
            background-color: #888;
            animation: typingAnimation 1.4s infinite ease-in-out;
        }
        
        .typing-dot:nth-child(1) {
            animation-delay: 0s;
        }
        
        .typing-dot:nth-child(2) {
            animation-delay: 0.2s;
        }
        
        .typing-dot:nth-child(3) {
            animation-delay: 0.4s;
        }
        
        @keyframes typingAnimation {
            0%, 100% {
                transform: translateY(0);
            }
            50% {
                transform: translateY(-5px);
            }
        }
        
        .message-content pre {
            background-color: rgba(0, 0, 0, 0.05);
            border-radius: 8px;
            padding: 1rem;
            overflow-x: auto;
            margin: 0.5rem 0;
        }
        
        .dark-mode .message-content pre {
            background-color: rgba(255, 255, 255, 0.05);
        }
        
        .message-content code {
            font-family: 'Consolas', 'Monaco', monospace;
            font-size: 0.9rem;
        }
        
        .message-content p {
            margin-bottom: 0.75rem;
        }
        
        .message-content ul, .message-content ol {
            padding-left: 1.5rem;
            margin-bottom: 0.75rem;
        }
        
        .message-content ul {
            list-style-type: disc;
        }
        
        .message-content ol {
            list-style-type: decimal;
        }
        
        @media (max-width: 640px) {
            .chat-container {
                height: calc(100vh - 140px);
            }
            
            .user-message, .bot-message {
                max-width: 85%;
            }
        }
    </style>
</head>
<body class="flex flex-col h-full">
    <!-- Header -->
    <header class="bg-white dark:bg-gray-800 shadow p-4 flex justify-between items-center sticky top-0 z-10">
        <div class="flex items-center">
            <div class="w-8 h-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center text-white font-bold mr-3">S</div>
            <h1 class="text-xl font-semibold">Ask to Shivang</h1>
        </div>
        <button id="theme-toggle" class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700">
            <i class="fas fa-moon dark:hidden"></i>
            <i class="fas fa-sun hidden dark:block text-yellow-300"></i>
        </button>
    </header>

    <!-- Chat Container -->
    <main class="flex-1 p-4 overflow-hidden">
        <div id="chat-container" class="chat-container h-full overflow-y-auto pb-4">
            <div class="bot-message p-3 mb-4 shadow-sm max-w-3xl mx-auto">
                <div class="flex items-start">
                    <div class="w-8 h-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center text-white font-bold mr-3 flex-shrink-0">S</div>
                    <div class="message-content">
                        <p>Hello! I'm Shivang's AI assistant. How can I help you today?</p>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <!-- Input Area -->
    <footer class="bg-white dark:bg-gray-800 border-t p-3 sticky bottom-0">
        <div class="max-w-3xl mx-auto">
            <form id="chat-form" class="flex items-center">
                <input id="user-input" type="text" placeholder="Type your message..." 
                    class="flex-1 p-3 rounded-l-full border border-gray-300 dark:border-gray-600 dark:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-500" autocomplete="off">
                <button type="submit" class="bg-blue-500 hover:bg-blue-600 text-white p-3 rounded-r-full transition">
                    <i class="fas fa-paper-plane"></i>
                </button>
            </form>
        </div>
    </footer>

    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <script>
        // Constants and Variables
        const API_KEY = "AIzaSyB6LyW82yNa6LUtMm1Ghvpaxc-r8Y2NtrI";
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;
        
        let chatHistory = [];
        let controller = null;
        
        // DOM Elements
        const chatContainer = document.getElementById('chat-container');
        const chatForm = document.getElementById('chat-form');
        const userInput = document.getElementById('user-input');
        const themeToggle = document.getElementById('theme-toggle');
        
        // Check for saved theme preference or use device preference
        if (localStorage.getItem('darkMode') === 'true' || 
            (localStorage.getItem('darkMode') === null && 
             window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.body.classList.add('dark-mode');
        }
        
        // Theme Toggle
        themeToggle.addEventListener('click', () => {
            document.body.classList.toggle('dark-mode');
            localStorage.setItem('darkMode', document.body.classList.contains('dark-mode'));
        });
        
        // Initialize by scrolling to bottom
        scrollToBottom();
        
        // Handle Form Submission
        chatForm.addEventListener('submit', async (e) => {
            e.preventDefault();
            
            const userMessage = userInput.value.trim();
            if (!userMessage) return;
            
            // Clear input
            userInput.value = '';
            
            // Add user message to chat
            appendMessage('user', userMessage);
            
            // Show typing indicator
            const botMessageId = showTypingIndicator();
            
            try {
                // Generate response
                const response = await generateResponse(userMessage);
                
                // Replace typing indicator with actual response
                updateBotMessage(botMessageId, response);
            } catch (error) {
                // Update with error message
                updateBotMessage(botMessageId, "Sorry, I encountered an error. Please try again.");
                console.error('Error:', error);
            }
        });
        
        // Generate response using Gemini API
        async function generateResponse(userMessage) {
            // Add to history
            chatHistory.push({
                role: "user",
                parts: [{ text: userMessage }]
            });
            
            // Create AbortController for potential request cancellation
            controller = new AbortController();
            
            try {
                const response = await fetch(API_URL, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        contents: chatHistory,
                        generationConfig: {
                            temperature: 0.7,
                            topK: 40,
                            topP: 0.95,
                            maxOutputTokens: 1024,
                        }
                    }),
                    signal: controller.signal
                });
                
                if (!response.ok) {
                    const errorData = await response.json();
                    throw new Error(errorData.error?.message || "API request failed");
                }
                
                const data = await response.json();
                const botResponse = data.candidates[0].content.parts[0].text;
                
                // Add to history
                chatHistory.push({
                    role: "model",
                    parts: [{ text: botResponse }]
                });
                
                return botResponse;
            } catch (error) {
                if (error.name === 'AbortError') {
                    return "Request was cancelled.";
                }
                throw error;
            }
        }
        
        // Add message to chat
        function appendMessage(sender, message) {
            const messageDiv = document.createElement('div');
            messageDiv.className = sender === 'user' ? 
                'user-message p-3 mb-4 ml-auto shadow-sm max-w-3xl' : 
                'bot-message p-3 mb-4 shadow-sm max-w-3xl';
            
            if (sender === 'user') {
                messageDiv.innerHTML = `
                    <div class="flex justify-end">
                        <div class="message-content">
                            <p>${escapeHTML(message)}</p>
                        </div>
                    </div>
                `;
            } else {
                // Process markdown for bot messages
                const renderedContent = marked.parse(message);
                
                messageDiv.innerHTML = `
                    <div class="flex items-start">
                        <div class="w-8 h-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center text-white font-bold mr-3 flex-shrink-0">S</div>
                        <div class="message-content">${renderedContent}</div>
                    </div>
                `;
            }
            
            chatContainer.appendChild(messageDiv);
            scrollToBottom();
            
            // For bot messages, return id for potential updates
            if (sender !== 'user') {
                return Date.now().toString();
            }
        }
        
        // Show typing indicator
        function showTypingIndicator() {
            const messageId = Date.now().toString();
            const messageDiv = document.createElement('div');
            messageDiv.className = 'bot-message p-3 mb-4 shadow-sm max-w-3xl';
            messageDiv.id = `message-${messageId}`;
            
            messageDiv.innerHTML = `
                <div class="flex items-start">
                    <div class="w-8 h-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center text-white font-bold mr-3 flex-shrink-0">S</div>
                    <div class="typing-indicator">
                        <div class="typing-dot"></div>
                        <div class="typing-dot"></div>
                        <div class="typing-dot"></div>
                    </div>
                </div>
            `;
            
            chatContainer.appendChild(messageDiv);
            scrollToBottom();
            return messageId;
        }
        
        // Update bot message with actual content
        function updateBotMessage(messageId, content) {
            const messageElement = document.getElementById(`message-${messageId}`);
            if (messageElement) {
                // Process markdown
                const renderedContent = marked.parse(content);
                
                messageElement.innerHTML = `
                    <div class="flex items-start">
                        <div class="w-8 h-8 rounded-full bg-gradient-to-r from-blue-500 to-purple-600 flex items-center justify-center text-white font-bold mr-3 flex-shrink-0">S</div>
                        <div class="message-content">${renderedContent}</div>
                    </div>
                `;
                scrollToBottom();
            }
        }
        
        // Scroll chat to bottom
        function scrollToBottom() {
            setTimeout(() => {
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }, 50);
        }
        
        // Helper for escaping HTML
        function escapeHTML(text) {
            const div = document.createElement('div');
            div.textContent = text;
            return div.innerHTML;
        }
        
        // Handle mobile keyboard issues
        userInput.addEventListener('focus', () => {
            setTimeout(scrollToBottom, 300);
        });
        
        window.addEventListener('resize', scrollToBottom);

        // Fix for iOS Safari 100vh issue
        function setVH() {
            let vh = window.innerHeight * 0.01;
            document.documentElement.style.setProperty('--vh', `${vh}px`);
        }
        
        setVH();
        window.addEventListener('resize', setVH);
    </script>
</body>
</html>
