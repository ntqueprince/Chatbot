<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ask to Shivang</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/@tabler/icons-webfont@2.30.0/tabler-icons.min.css">
    <script src="https://cdn.jsdelivr.net/npm/marked@4.0.0/marked.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/core.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/languages/javascript.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/languages/python.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/languages/bash.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/languages/html.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/lib/languages/css.min.js"></script>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/highlight.js@11.7.0/styles/github-dark.min.css">
    <style>
        /* Custom styles */
        .dark {
            color-scheme: dark;
        }
        .chat-container {
            height: calc(100vh - 170px);
        }
        .message {
            max-width: 85%;
            border-radius: 0.5rem;
            word-wrap: break-word;
        }
        .bot-message {
            border-radius: 0 1rem 1rem 1rem;
        }
        .user-message {
            border-radius: 1rem 0 1rem 1rem;
        }
        .typing::after {
            content: '...';
            animation: typing 1.5s infinite;
        }
        pre {
            white-space: pre-wrap;
            padding: 1rem;
            border-radius: 0.5rem;
        }
        code {
            font-family: monospace;
        }
        .gradient-text {
            background: linear-gradient(to right, #6366F1, #8B5CF6);
            -webkit-background-clip: text;
            background-clip: text;
            -webkit-text-fill-color: transparent;
            text-fill-color: transparent;
        }
        @keyframes typing {
            0%, 100% { content: '.'; }
            33% { content: '..'; }
            66% { content: '...'; }
        }
        /* Mobile scroll fixes */
        @media (max-width: 640px) {
            .chat-container {
                height: calc(100vh - 160px);
            }
        }
        /* Markdown styles */
        .markdown-content h1 { font-size: 1.8rem; font-weight: bold; margin: 1rem 0; }
        .markdown-content h2 { font-size: 1.5rem; font-weight: bold; margin: 0.8rem 0; }
        .markdown-content h3 { font-size: 1.2rem; font-weight: bold; margin: 0.6rem 0; }
        .markdown-content ul { list-style-type: disc; margin-left: 1.5rem; }
        .markdown-content ol { list-style-type: decimal; margin-left: 1.5rem; }
        .markdown-content p { margin: 0.5rem 0; }
        .markdown-content a { color: #6366F1; text-decoration: underline; }
        .markdown-content blockquote { border-left: 4px solid #6366F1; padding-left: 1rem; margin: 0.5rem 0; font-style: italic; }
    </style>
</head>
<body class="bg-gray-50 dark:bg-gray-900 transition-colors duration-200">
    <div class="container mx-auto px-4 pb-4">
        <!-- Header -->
        <header class="flex justify-between items-center py-4 border-b dark:border-gray-700">
            <div class="flex items-center">
                <h1 class="text-2xl sm:text-3xl font-bold gradient-text">Ask to Shivang</h1>
            </div>
            <button id="theme-toggle" class="p-2 rounded-full hover:bg-gray-200 dark:hover:bg-gray-800 transition-colors">
                <i class="ti ti-sun text-gray-600 dark:text-gray-300 text-xl dark:hidden"></i>
                <i class="ti ti-moon text-gray-300 text-xl hidden dark:inline"></i>
            </button>
        </header>

        <!-- Chat container -->
        <div id="chat-container" class="chat-container my-4 overflow-y-auto">
            <div id="welcome-message" class="flex flex-col items-center justify-center h-full text-center px-4">
                <h2 class="text-3xl sm:text-4xl font-bold text-gray-800 dark:text-gray-200 mb-4">Welcome to Ask to Shivang</h2>
                <p class="text-gray-600 dark:text-gray-400 text-lg mb-6">I'm your AI assistant powered by Gemini. How can I help you today?</p>
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 w-full max-w-2xl">
                    <div class="bg-white dark:bg-gray-800 p-4 rounded-lg shadow-md hover:shadow-lg transition cursor-pointer" onclick="suggestPrompt('Explain the concept of machine learning in simple terms')">
                        <h3 class="font-medium text-gray-800 dark:text-gray-200">Machine Learning Basics</h3>
                        <p class="text-gray-600 dark:text-gray-400 text-sm">Explain the concept of machine learning in simple terms</p>
                    </div>
                    <div class="bg-white dark:bg-gray-800 p-4 rounded-lg shadow-md hover:shadow-lg transition cursor-pointer" onclick="suggestPrompt('Write a Python function to find the fibonacci sequence')">
                        <h3 class="font-medium text-gray-800 dark:text-gray-200">Coding Help</h3>
                        <p class="text-gray-600 dark:text-gray-400 text-sm">Write a Python function to find the fibonacci sequence</p>
                    </div>
                    <div class="bg-white dark:bg-gray-800 p-4 rounded-lg shadow-md hover:shadow-lg transition cursor-pointer" onclick="suggestPrompt('What are the best practices for responsive web design?')">
                        <h3 class="font-medium text-gray-800 dark:text-gray-200">Web Development</h3>
                        <p class="text-gray-600 dark:text-gray-400 text-sm">What are the best practices for responsive web design?</p>
                    </div>
                    <div class="bg-white dark:bg-gray-800 p-4 rounded-lg shadow-md hover:shadow-lg transition cursor-pointer" onclick="suggestPrompt('Suggest a workout routine for beginners')">
                        <h3 class="font-medium text-gray-800 dark:text-gray-200">Health & Fitness</h3>
                        <p class="text-gray-600 dark:text-gray-400 text-sm">Suggest a workout routine for beginners</p>
                    </div>
                </div>
            </div>
            <div id="messages" class="space-y-4"></div>
        </div>

        <!-- Input area -->
        <div class="fixed bottom-0 left-0 right-0 bg-gray-50 dark:bg-gray-900 p-4 border-t dark:border-gray-700">
            <div class="container mx-auto">
                <form id="chat-form" class="flex items-center gap-2">
                    <div class="relative flex-1">
                        <input type="text" id="user-input" class="w-full p-3 pr-10 rounded-full border border-gray-300 dark:border-gray-600 bg-white dark:bg-gray-800 text-gray-900 dark:text-gray-100 focus:outline-none focus:ring-2 focus:ring-indigo-500" placeholder="Type a message..." autocomplete="off">
                        <button type="button" id="clear-button" class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600 dark:hover:text-gray-300 hidden">
                            <i class="ti ti-x text-lg"></i>
                        </button>
                    </div>
                    <button type="submit" id="send-button" class="p-3 rounded-full bg-indigo-600 hover:bg-indigo-700 text-white disabled:opacity-50 disabled:cursor-not-allowed transition-all">
                        <i class="ti ti-send text-lg"></i>
                    </button>
                </form>
                <p class="text-xs text-gray-500 dark:text-gray-400 mt-2 text-center">Powered by Gemini AI. Responses may not always be accurate.</p>
            </div>
        </div>
    </div>

    <script>
        // Constants
        const API_KEY = "AIzaSyB6LyW82yNa6LUtMm1Ghvpaxc-r8Y2NtrI";
        const API_URL = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;
        
        // DOM Elements
        const chatForm = document.getElementById('chat-form');
        const userInput = document.getElementById('user-input');
        const sendButton = document.getElementById('send-button');
        const clearButton = document.getElementById('clear-button');
        const messagesContainer = document.getElementById('messages');
        const welcomeMessage = document.getElementById('welcome-message');
        const chatContainer = document.getElementById('chat-container');
        const themeToggle = document.getElementById('theme-toggle');
        
        // State
        const chatHistory = [];
        let controller = null;
        
        // Initialize theme from localStorage
        if (localStorage.getItem('theme') === 'dark' || 
            (!localStorage.getItem('theme') && window.matchMedia('(prefers-color-scheme: dark)').matches)) {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
        
        // Event Listeners
        document.addEventListener('DOMContentLoaded', () => {
            userInput.focus();
            
            // Input field events
            userInput.addEventListener('input', () => {
                clearButton.style.display = userInput.value ? 'block' : 'none';
            });
            
            clearButton.addEventListener('click', () => {
                userInput.value = '';
                clearButton.style.display = 'none';
                userInput.focus();
            });
            
            // Submit form
            chatForm.addEventListener('submit', handleSubmit);
            
            // Theme toggle
            themeToggle.addEventListener('click', () => {
                document.documentElement.classList.toggle('dark');
                localStorage.setItem('theme', document.documentElement.classList.contains('dark') ? 'dark' : 'light');
            });
        });

        // Handle suggested prompts
        function suggestPrompt(prompt) {
            userInput.value = prompt;
            clearButton.style.display = 'block';
            chatForm.dispatchEvent(new Event('submit'));
        }
        
        // Handle form submission
        async function handleSubmit(e) {
            e.preventDefault();
            
            const userMessage = userInput.value.trim();
            if (!userMessage) return;
            
            // Clear input and disable form
            userInput.value = '';
            clearButton.style.display = 'none';
            disableForm(true);
            
            // Remove welcome screen if visible
            if (welcomeMessage.style.display !== 'none') {
                welcomeMessage.style.display = 'none';
            }
            
            // Add user message to UI
            addMessage(userMessage, 'user');
            
            // Add to history
            chatHistory.push({
                role: "user",
                parts: [{ text: userMessage }]
            });
            
            // Create bot message with loading state
            const botMessageElement = createBotMessageElement("Thinking...");
            botMessageElement.classList.add('typing');
            messagesContainer.appendChild(botMessageElement);
            scrollToBottom();
            
            try {
                // Make API call
                const response = await fetchGeminiResponse(userMessage);
                
                // Update UI with response
                botMessageElement.classList.remove('typing');
                const formattedResponse = formatMarkdown(response);
                botMessageElement.innerHTML = formattedResponse;
                
                // Add to history
                chatHistory.push({
                    role: "model",
                    parts: [{ text: response }]
                });
                
                // Apply syntax highlighting to code blocks
                applyCodeHighlighting();
            } catch (error) {
                // Handle error
                botMessageElement.classList.remove('typing');
                botMessageElement.innerHTML = `<p class="text-red-500">Error: ${error.message}</p>`;
            } finally {
                // Re-enable form
                disableForm(false);
                scrollToBottom();
                userInput.focus();
            }
        }
        
        // Fetch response from Gemini API
        async function fetchGeminiResponse(userMessage) {
            controller = new AbortController();
            
            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        contents: chatHistory,
                        generationConfig: {
                            temperature: 0.7,
                            topK: 40,
                            topP: 0.95,
                            maxOutputTokens: 2048,
                        }
                    }),
                    signal: controller.signal
                });
                
                const data = await response.json();
                
                if (data.error) {
                    throw new Error(data.error.message || 'Failed to get response');
                }
                
                return data.candidates[0].content.parts[0].text;
            } catch (error) {
                if (error.name === 'AbortError') {
                    throw new Error('Request was cancelled');
                }
                throw error;
            } finally {
                controller = null;
            }
        }
        
        // UI Helper Functions
        function addMessage(text, sender) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('flex', sender === 'user' ? 'justify-end' : 'justify-start');
            
            const messageContent = document.createElement('div');
            messageContent.classList.add(
                'message', 
                'p-3', 
                sender === 'user' ? 'user-message' : 'bot-message',
                sender === 'user' ? 'bg-indigo-600' : 'bg-gray-200 dark:bg-gray-800',
                sender === 'user' ? 'text-white' : 'text-gray-800 dark:text-gray-200'
            );
            
            messageContent.textContent = text;
            messageElement.appendChild(messageContent);
            messagesContainer.appendChild(messageElement);
            scrollToBottom();
        }
        
        function createBotMessageElement(initialText) {
            const messageElement = document.createElement('div');
            messageElement.classList.add('flex', 'justify-start');
            
            const messageContent = document.createElement('div');
            messageContent.classList.add(
                'message', 
                'bot-message',
                'p-3', 
                'bg-gray-200', 
                'dark:bg-gray-800',
                'text-gray-800', 
                'dark:text-gray-200',
                'markdown-content'
            );
            
            messageContent.textContent = initialText;
            messageElement.appendChild(messageContent);
            return messageContent;
        }
        
        function disableForm(disabled) {
            userInput.disabled = disabled;
            sendButton.disabled = disabled;
        }
        
        function scrollToBottom() {
            setTimeout(() => {
                chatContainer.scrollTop = chatContainer.scrollHeight;
            }, 10);
        }
        
        function formatMarkdown(text) {
            marked.setOptions({
                breaks: true,
                gfm: true,
                headerIds: false
            });
            
            const renderer = new marked.Renderer();
            
            // Customize code block rendering
            renderer.code = function(code, language) {
                return `<pre><code class="hljs language-${language || 'plaintext'}">${code}</code></pre>`;
            };
            
            return marked.parse(text, { renderer });
        }
        
        function applyCodeHighlighting() {
            document.querySelectorAll('pre code').forEach((block) => {
                hljs.highlightElement(block);
            });
        }
    </script>
</body>
</html>
