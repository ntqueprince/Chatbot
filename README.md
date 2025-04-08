<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ask to Shivang - Bilingual Chatbot</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
    <link href="https://cdn.jsdelivr.net/npm/@mdi/font@6.9.96/css/materialdesignicons.min.css" rel="stylesheet">
</head>
<body class="bg-gray-100 flex flex-col h-screen">
    <header class="bg-gray-100 p-4 border-b border-gray-300">
        <div class="flex justify-between items-center">
            <div class="text-blue-600 text-2xl font-bold">Chatbot</div>
            <div class="flex space-x-2">
                <button id="toggleLanguage" class="px-2 py-1 bg-blue-100 rounded text-blue-600 text-sm font-medium">हिंदी</button>
                <span class="text-yellow-500 text-xl cursor-pointer" id="themeToggle">🌙</span>
            </div>
        </div>
    </header>
    
    <div class="flex items-center p-4 border-b border-gray-300">
        <div class="w-10 h-10 rounded-full bg-blue-500 text-white flex items-center justify-center font-bold text-lg mr-3">S</div>
        <div class="text-xl font-bold text-gray-800" id="chatbotTitle">Ask to Shivang</div>
    </div>
    
    <div class="flex-1 overflow-y-auto p-4" id="chatContainer">
        <div class="mb-4 flex">
            <div class="w-8 h-8 rounded-full bg-blue-500 text-white flex items-center justify-center font-bold mr-2">S</div>
            <div class="bg-gray-200 p-3 rounded-lg rounded-tl-none max-w-xs md:max-w-md" id="welcomeMessage">
                Hello! I'm Shivang's AI assistant. How can I help you today?
            </div>
        </div>
    </div>
    
    <div class="p-4 border-t border-gray-300 bg-gray-100">
        <div class="flex justify-between items-center">
            <input type="text" id="messageInput" class="w-full p-3 border border-gray-300 rounded-full focus:outline-none focus:ring-2 focus:ring-blue-500" placeholder="Type your message...">
            <button id="sendButton" class="ml-3 bg-blue-500 text-white rounded-full w-10 h-10 flex items-center justify-center disabled:opacity-50 disabled:cursor-not-allowed">
                <span class="mdi mdi-send"></span>
            </button>
        </div>
        <div class="text-red-500 text-center mt-2 hidden" id="errorMessage"></div>
    </div>
    
    <div class="flex justify-around items-center p-3 bg-white border-t border-gray-300">
        <button class="text-gray-600 text-2xl"><span class="mdi mdi-menu"></span></button>
        <button class="text-gray-600 text-2xl"><span class="mdi mdi-circle-outline"></span></button>
        <button class="text-gray-600 text-2xl"><span class="mdi mdi-chevron-left"></span></button>
    </div>

    <script>
        // DOM Elements
        const messageInput = document.getElementById('messageInput');
        const sendButton = document.getElementById('sendButton');
        const chatContainer = document.getElementById('chatContainer');
        const errorMessageElement = document.getElementById('errorMessage');
        const themeToggle = document.getElementById('themeToggle');
        const toggleLanguageButton = document.getElementById('toggleLanguage');
        const welcomeMessageElement = document.getElementById('welcomeMessage');
        const chatbotTitleElement = document.getElementById('chatbotTitle');

        // API Key
        const API_KEY = "AIzaSyDN6BM_VoqrEMbvr3G4oUpTVHldkPLKEz0";
        
        // Language state (default: English)
        let currentLanguage = 'en';
        
        // Translations
        const translations = {
            en: {
                title: "Ask to Shivang",
                welcome: "Hello! I'm Shivang's AI assistant. How can I help you today?",
                placeholder: "Type your message...",
                languageButton: "हिंदी",
                errorMessage: "Sorry, I encountered an error. Please try again later.",
                thinking: "Thinking..."
            },
            hi: {
                title: "शिवांग से पूछें",
                welcome: "नमस्ते! मैं शिवांग का AI सहायक हूँ। आज मैं आपकी कैसे मदद कर सकता हूँ?",
                placeholder: "अपना संदेश टाइप करें...",
                languageButton: "English",
                errorMessage: "क्षमा करें, मुझे एक त्रुटि मिली। कृपया बाद में पुनः प्रयास करें।",
                thinking: "सोच रहा हूँ..."
            }
        };
        
        // Event Listeners
        messageInput.addEventListener('input', () => {
            sendButton.disabled = messageInput.value.trim() === '';
        });
        
        sendButton.addEventListener('click', sendMessage);
        
        messageInput.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' && !e.shiftKey && messageInput.value.trim() !== '') {
                e.preventDefault();
                sendMessage();
            }
        });
        
        themeToggle.addEventListener('click', toggleTheme);
        toggleLanguageButton.addEventListener('click', toggleLanguage);

        // Initialize
        sendButton.disabled = true;
        
        // Toggle language
        function toggleLanguage() {
            currentLanguage = currentLanguage === 'en' ? 'hi' : 'en';
            updateUILanguage();
        }
        
        // Update UI with selected language
        function updateUILanguage() {
            const lang = translations[currentLanguage];
            
            toggleLanguageButton.textContent = lang.languageButton;
            chatbotTitleElement.textContent = lang.title;
            welcomeMessageElement.textContent = lang.welcome;
            messageInput.placeholder = lang.placeholder;
            errorMessageElement.textContent = lang.errorMessage;
            
            // Set direction for RTL languages if needed
            // document.dir = currentLanguage === 'ar' ? 'rtl' : 'ltr';
        }
        
        // Toggle theme
        function toggleTheme() {
            document.body.classList.toggle('dark');
            const isDark = document.body.classList.contains('dark');
            
            if (isDark) {
                themeToggle.textContent = '☀️';
                document.documentElement.classList.add('dark');
                // Apply dark theme styles
                document.body.style.backgroundColor = '#1a1a1a';
                document.body.style.color = '#f5f5f5';
                
                // Header and input area
                document.querySelector('header').classList.remove('bg-gray-100');
                document.querySelector('header').classList.add('bg-gray-900');
                document.querySelector('.p-4.border-t').classList.remove('bg-gray-100');
                document.querySelector('.p-4.border-t').classList.add('bg-gray-900');
                
                // Footer
                document.querySelector('.flex.justify-around').classList.remove('bg-white');
                document.querySelector('.flex.justify-around').classList.add('bg-gray-900');
                
                // Borders
                document.querySelectorAll('.border-gray-300').forEach(el => {
                    el.classList.remove('border-gray-300');
                    el.classList.add('border-gray-700');
                });
                
                // Input
                messageInput.classList.add('bg-gray-800', 'text-white');
                messageInput.classList.remove('border-gray-300');
                messageInput.classList.add('border-gray-700');
            } else {
                themeToggle.textContent = '🌙';
                document.documentElement.classList.remove('dark');
                // Restore light theme
                document.body.style.backgroundColor = '';
                document.body.style.color = '';
                
                // Header and input area
                document.querySelector('header').classList.add('bg-gray-100');
                document.querySelector('header').classList.remove('bg-gray-900');
                document.querySelector('.p-4.border-t').classList.add('bg-gray-100');
                document.querySelector('.p-4.border-t').classList.remove('bg-gray-900');
                
                // Footer
                document.querySelector('.flex.justify-around').classList.add('bg-white');
                document.querySelector('.flex.justify-around').classList.remove('bg-gray-900');
                
                // Borders
                document.querySelectorAll('.border-gray-700').forEach(el => {
                    el.classList.add('border-gray-300');
                    el.classList.remove('border-gray-700');
                });
                
                // Input
                messageInput.classList.remove('bg-gray-800', 'text-white');
                messageInput.classList.add('border-gray-300');
                messageInput.classList.remove('border-gray-700');
            }
        }
        
        // Show error message
        function showError(message) {
            errorMessageElement.textContent = message || translations[currentLanguage].errorMessage;
            errorMessageElement.classList.remove('hidden');
            setTimeout(() => {
                errorMessageElement.classList.add('hidden');
            }, 5000);
        }
        
        // Add message to chat
        function addMessage(message, sender) {
            const messageElement = document.createElement('div');
            messageElement.className = 'mb-4 flex ' + (sender === 'user' ? 'justify-end' : '');
            
            if (sender === 'user') {
                messageElement.innerHTML = `
                    <div class="bg-blue-500 text-white p-3 rounded-lg rounded-tr-none max-w-xs md:max-w-md">
                        ${message}
                    </div>
                `;
            } else {
                messageElement.innerHTML = `
                    <div class="w-8 h-8 rounded-full bg-blue-500 text-white flex items-center justify-center font-bold mr-2">S</div>
                    <div class="bg-gray-200 p-3 rounded-lg rounded-tl-none max-w-xs md:max-w-md">
                        ${message}
                    </div>
                `;
            }
            
            chatContainer.appendChild(messageElement);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }
        
        // Add a typing indicator
        function addTypingIndicator() {
            const typingElement = document.createElement('div');
            typingElement.className = 'mb-4 flex typing-indicator';
            typingElement.id = 'typingIndicator';
            
            typingElement.innerHTML = `
                <div class="w-8 h-8 rounded-full bg-blue-500 text-white flex items-center justify-center font-bold mr-2">S</div>
                <div class="bg-gray-200 p-3 rounded-lg rounded-tl-none">
                    <div class="flex space-x-1">
                        <div class="w-2 h-2 bg-gray-500 rounded-full animate-bounce" style="animation-delay: 0s;"></div>
                        <div class="w-2 h-2 bg-gray-500 rounded-full animate-bounce" style="animation-delay: 0.2s;"></div>
                        <div class="w-2 h-2 bg-gray-500 rounded-full animate-bounce" style="animation-delay: 0.4s;"></div>
                    </div>
                </div>
            `;
            
            chatContainer.appendChild(typingElement);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }
        
        // Remove typing indicator
        function removeTypingIndicator() {
            const typingElement = document.getElementById('typingIndicator');
            if (typingElement) {
                typingElement.remove();
            }
        }
        
        // Send message
        async function sendMessage() {
            const message = messageInput.value.trim();
            if (message === '') return;
            
            // Add user message to chat
            addMessage(message, 'user');
            
            // Clear input
            messageInput.value = '';
            sendButton.disabled = true;
            
            // Show typing indicator
            addTypingIndicator();
            
            try {
                // Prepare the prompt based on language
                let systemPrompt = '';
                if (currentLanguage === 'hi') {
                    systemPrompt = `तुम शिवांग का AI सहायक हो। कृपया हिंदी में उत्तर दें। उपयोगकर्ता का संदेश है: "${message}"`;
                } else {
                    systemPrompt = `You are Shivang's AI assistant. Please respond in English. The user's message is: "${message}"`;
                }
                
                // Make API call
                const url = `https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`;
                
                const response = await fetch(url, {
                    method: 'POST',
                    headers: {
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        contents: [{
                            parts: [{
                                text: systemPrompt
                            }]
                        }],
                        generationConfig: {
                            temperature: 0.7,
                            maxOutputTokens: 800
                        },
                        safetySettings: [
                            {
                                category: "HARM_CATEGORY_HARASSMENT",
                                threshold: "BLOCK_MEDIUM_AND_ABOVE"
                            },
                            {
                                category: "HARM_CATEGORY_HATE_SPEECH",
                                threshold: "BLOCK_MEDIUM_AND_ABOVE"
                            },
                            {
                                category: "HARM_CATEGORY_SEXUALLY_EXPLICIT",
                                threshold: "BLOCK_MEDIUM_AND_ABOVE"
                            },
                            {
                                category: "HARM_CATEGORY_DANGEROUS_CONTENT",
                                threshold: "BLOCK_MEDIUM_AND_ABOVE"
                            }
                        ]
                    })
                });
                
                if (!response.ok) {
                    throw new Error(`API Error: ${response.status}`);
                }
                
                const data = await response.json();
                
                // Remove typing indicator
                removeTypingIndicator();
                
                // Check if we have valid response
                if (data.candidates && data.candidates[0] && data.candidates[0].content) {
                    const botMessage = data.candidates[0].content.parts[0].text;
                    addMessage(botMessage, 'assistant');
                } else if (data.promptFeedback && data.promptFeedback.blockReason) {
                    // Handle content blocked by safety settings
                    const blockMessage = currentLanguage === 'hi' 
                        ? 'सुरक्षा कारणों से इस संदेश का जवाब नहीं दिया जा सकता है।'
                        : 'This message cannot be responded to for safety reasons.';
                    addMessage(blockMessage, 'assistant');
                } else {
                    throw new Error('Invalid API response structure');
                }
                
            } catch (error) {
                console.error('Error:', error);
                // Remove typing indicator
                removeTypingIndicator();
                // Show error message in chat
                const errorMsg = translations[currentLanguage].errorMessage;
                addMessage(errorMsg, 'assistant');
                // Show error message below input
                showError(error.message);
            }
        }
    </script>
</body>
</html>
