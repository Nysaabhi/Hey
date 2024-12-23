<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Deep Chatbot - On-Demand Services</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.css" rel="stylesheet">
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <style>
        :root { --primary-color: #FFD700; --secondary-color: #FDB931; --background-dark: #1a1a1f; --text-light: #ffffff; --text-dark: #000000; --accent-gradient: linear-gradient(135deg, var(--primary-color), var(--secondary-color)); --error-color: #ff4444; --success-color: #00C851; }
        
        * { box-sizing: border-box; margin: 0; padding: 0; }
        
        body { 
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif; 
  background-color: var(--background-dark); 
  color: var(--text-light); 
  line-height: 1.6; 
  overflow-x: hidden; 
}
        
        .header { padding: 8px 16px; background: rgba(26, 26, 31, 0.95); position: fixed; width: 100%; top: 0; left: 0; z-index: 30; border-bottom: 2px solid var(--primary-color); backdrop-filter: blur(10px); height: 60px; }
        
        .header-content { max-width: 1400px; margin: 0 auto; display: flex; justify-content: space-between; align-items: center; height: 100%; }
        
        .logo { font-size: 1.8em; font-weight: 900; background: var(--accent-gradient); -webkit-background-clip: text; color: transparent; letter-spacing: 1.2px; }
        
        .menu-button { padding: 10px 20px; background: var(--accent-gradient); border: none; border-radius: 50px; color: var(--text-dark); font-weight: 600; cursor: pointer; transition: all 0.3s ease; font-size: 0.95em; }
        
        .chatbot-container { position: fixed; top: 60px; left: 0; right: 0; bottom: 60px; background: rgba(26, 26, 31, 0.95); display: flex; flex-direction: column; }
        
        .chat-header { padding: 15px; background: linear-gradient(135deg, rgba(255, 215, 0, 0.1), rgba(253, 185, 49, 0.1)); border-bottom: 1px solid rgba(255, 215, 0, 0.2); display: flex; justify-content: space-between; align-items: center; }
        
        .chat-status { display: flex; align-items: center; color: var(--primary-color); font-weight: 600; }
        
        .status-dot { width: 8px; height: 8px; background: var(--primary-color); border-radius: 50%; margin-right: 8px; animation: pulse 2s infinite; }
        
        .chat-messages { flex: 1; overflow-y: auto; padding: 20px; scroll-behavior: smooth; }
        
        .message { display: flex; gap: 10px; max-width: 85%; margin-bottom: 16px; animation: messageSlide 0.3s ease-out; }
        
        .bot-message { align-self: flex-start; }
        
        .user-message { align-self: flex-end; flex-direction: row-reverse; }
        
        .message-avatar { width: 36px; height: 36px; min-width: 36px; min-height: 36px; background: rgba(255, 215, 0, 0.1); border-radius: 50%; display: flex; align-items: center; justify-content: center; color: var(--primary-color); flex-shrink: 0; }
        
        .message-content { background: rgba(255, 255, 255, 0.05); padding: 12px 16px; border-radius: 16px; color: var(--text-light); }
        
        .user-message .message-content { background: rgba(255, 215, 0, 0.1); }
        
        .chat-input-container {
  padding: 20px;
  background: rgba(26, 26, 31, 0.98);
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  
  /* Desktop and laptop styles */
  @media (min-width: 769px) {
    width: 60%;
    margin-left: auto;
    margin-right: auto;
  }
  
  /* Mobile styles (full width) */
  @media (max-width: 768px) {
    width: 100%;
    padding: 10px;
  }
}

#chatInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px 20px;
  color: var(--text-light);
  font-size: 0.95em;
  
  /* Desktop and laptop styles */
  @media (min-width: 769px) {
    width: 100%;
  }
  
  /* Mobile styles (full width) */
  @media (max-width: 768px) {
    width: 100%;
  }
}

        .input-actions { display: flex; gap: 8px; }
        
        .input-actions button { padding: 20px 20px; background: var(--accent-gradient); border: none; border-radius: 8px; color: #ffffff; cursor: pointer; }
        
        .send-message i { font-size: 1.5em; }
        
        .alert { position: fixed; top: 20px; right: 20px; padding: 15px 20px; border-radius: 8px; color: var(--text-light); z-index: 1000; animation: slideIn 0.3s ease-out; }
        
        .alert-success { background: var(--success-color); }
        
        .alert-error { background: var(--error-color); }
        
        .bottom-nav { position: fixed; bottom: 0; left: 0; right: 0; height: 60px; background: rgba(26, 26, 31, 0.98); padding: 8px 0; border-top: 1px solid rgba(255, 215, 0, 0.2); }
        
        .nav-container { display: flex; justify-content: space-around; align-items: center; height: 100%; max-width: 600px; margin: 0 auto; }
        
        .nav-item { display: flex; flex-direction: column; align-items: center; text-decoration: none; color: #fff; transition: all 0.3s ease; padding: 5px; cursor: pointer; font-family: 'Poppins', sans-serif; }
        
        .nav-item i { color: #fff; font-size: 20px; margin-bottom: 4px; }
        
        .nav-item span { font-size: 12px; font-family: 'Poppins', sans-serif; font-weight: 400; }
        
        @keyframes pulse { 0% { opacity: 1; } 50% { opacity: 0.5; } 100% { opacity: 1; } }
        
        @keyframes messageSlide { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        @keyframes slideIn { from { opacity: 0; transform: translateX(100px); } to { opacity: 1; transform: translateX(0); } }
        
        @media (max-width: 768px) { .message { max-width: 90%; } }
        
        body > h1:first-of-type:not(.heading) { display: none !important; }
        
        .markdown-body h1:first-child { display: none !important; }
        
        .position-relative h1:first-child { display: none !important; }
                
        .modal-overlay { position: absolute; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0, 0, 0, 0.5); display: flex; justify-content: center; align-items: center; padding: 20px; }
        
        .modal-content { background-color: #1a1a1f; border-radius: 12px; width: 100%; max-width: 600px; max-height: 90vh; overflow-y: auto; box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15); }
        
        .modal-header { padding: 20px; border-bottom: 1px solid #eee; display: flex; justify-content: space-between; align-items: center; }
        
        .modal-header h2 { margin: 0; font-size: 1.5rem; color: #fffdfd; }
        
        .close-button { background: none; border: none; font-size: 1.5rem; cursor: pointer; color: #b1b1b1; padding: 5px; }
        
        .modal-body { padding: 20px; }
                        
        .menu-overlay { display: none; position: fixed; top: 60px; left: 0; right: 0; bottom: 60px; background: rgba(26, 26, 31, 0.98); z-index: 25; padding: 0; animation: fadeIn 0.3s ease-out; overflow-y: auto; }
        
        .menu-sections { padding: 20px; }
        
        .menu-section { margin-bottom: 24px; }
        
        .menu-section-title { color: var(--primary-color); font-size: 1.2em; font-weight: 600; margin-bottom: 12px; padding-bottom: 8px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); }
        
        .menu-items { display: flex; flex-direction: column; gap: 8px; }
        
        .menu-item { padding: 12px 16px; background: rgba(255, 255, 255, 0.05); border-radius: 8px; color: var(--text-light); cursor: pointer; transition: all 0.3s ease; display: flex; justify-content: space-between; align-items: center; }
        
        .menu-item:hover { background: rgba(255, 215, 0, 0.1); }
        
        .menu-item-price { color: var(--primary-color); font-weight: 600; }
        
        .social-links { display: flex; gap: 12px; }
        
        .social-link { width: 40px; height: 40px; border-radius: 50%; background: rgba(255, 255, 255, 0.05); display: flex; align-items: center; justify-content: center; color: var(--text-light); text-decoration: none; transition: all 0.3s ease; }
        
        .social-link:hover { background: var(--primary-color); color: var(--text-dark); }
        
        .about-content { line-height: 1.6; color: #cccccc; }
        
        .menu-item { animation: slideIn 0.3s ease-out; animation-fill-mode: both; }
        
        .menu-item:nth-child(1) { animation-delay: 0.1s; }
        .menu-item:nth-child(2) { animation-delay: 0.2s; }
        .menu-item:nth-child(3) { animation-delay: 0.3s; }
        .menu-item:nth-child(4) { animation-delay: 0.4s; }
        .menu-item:nth-child(5) { animation-delay: 0.5s; }
        
        @keyframes slideIn { from { opacity: 0; transform: translateX(-20px); } to { opacity: 1; transform: translateX(0); } }
                
        .whatsapp-item { background-color: #25D366; color: white; border-radius: 8px; padding: 10px; text-align: center; }
        
        .whatsapp-item a i { color: white; }
        
        .whatsapp-item:hover { background-color: #1ebe57; color: white; }
        
        .confirmation-icon { font-size: 80px; color: #4CAF50; margin-bottom: 20px; }
        
        .confirmation-icon i { display: block; }
        
        .alert { position: fixed; top: 20px; left: 50%; transform: translateX(-50%); padding: 10px 20px; border-radius: 5px; z-index: 1100; }
        
        .alert-success { background-color: #4CAF50; color: white; }
        
        .alert-error { background-color: #f44336; color: white; }
        
        .location-links, .partner-links { display: flex; flex-direction: column; } .location-link, .partner-link { text-decoration: none; color: #ffffff; padding: 8px 10px; transition: all 0.3s ease; border-radius: 4px; } .location-link:hover, .partner-link:hover { background-color: #f0f0f0; color: #333; } .location-link i, .partner-link i { margin-right: 10px; color: #fff; } .location-link:hover i, .partner-link:hover i { color: #2c7cd1; }

        .header-button-group {
    display: flex;
    align-items: center;
    gap: 5px; /* Consistent spacing between buttons */
}

.town-button, 
.catalogue-button, 
.deals-button {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 15px;
    background: none;
    border: none;
    color: var(--primary-color);
    cursor: pointer;
    transition: background-color 0.3s ease;
    border-radius: 4px;
    width: 40px; /* Fixed width */
    height: 40px; /* Fixed height */
}

.town-button:hover, 
.catalogue-button:hover, 
.deals-button:hover {
    background: rgba(255, 215, 0, 0.1);
}

.town-button i, 
.catalogue-button i, 
.deals-button i {
    font-size: 18px; /* Consistent icon size */
}

                 </style>        
        </head>
        <body>
            <header class="header">
                <div class="header-content">
                    <a href="https://nysaabhi.github.io/A2" class="logo">Deep</a>
                    <button class="menu-button" id="menuButton">
                        <i class="fas fa-bars"></i>
                     Menu
                    </button>
            </div>
            </header>
            
            <div class="chatbot-container">
                <div class="chat-header">
                    <div class="chat-status">
                        <span class="status-dot"></span>
                        Deep AI Assistant
                    </div>
                    <div class="header-button-group">
                        <button class="town-button" id="townButton">
                            <i class="fas fa-map-marker-alt"></i>
                        </button>
                        <button class="catalogue-button" id="catalogueButton">
                            <i class="fas fa-image"></i>
                        </button>
                        <button class="deals-button" id="dealsButton">
                            <i class="fas fa-gift"></i>
                        </button>
                    </div>
                </div>
        
                <div class="chat-messages" id="chatMessages">
                    <div class="category-grid" id="categoryGrid">
                        <!-- Categories will be dynamically populated -->
                    </div>
                             
                    <div class="woohoo" id="woohoo">
                        <div class="hahaha">
                    </div>
                </div>
            </div>

            
            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                           <i class="fas fa-user-tie"></i>
                        <span>Services</span>
                    </div>
                    <div class="nav-item" data-page="reserve">
                            <i class="fas fa-calendar-check"></i>
                        <span>Business</span>
                    </div>
                    <div class="nav-item whatsapp-item" data-page="chat">
                        <a href="https://wa.me/9111478356" target="_blank">
                            <i class="fas fa-message"></i>
                        </a>
                        <span>WhatsApp</span>
                    </div>
                    <div class="nav-item" data-page="slot">
                            <i class="fas fa-calendar-alt"></i>
                     <span>Places</span>
                   </div>
                    <div class="nav-item" data-page="franchise">
                            <i class="fas fa-store"></i>
                        <span>Rentals</span>
                    </div>
                </div>
            </nav>
            
            <div class="menu-overlay" id="menuOverlay">
                <div class="menu-sections">
                    <div class="menu-section">
                        <div class="menu-section-title">About Us</div>
                        <div class="about-content">
                            Deep Chatbot is your premier destination for on-demand services. We connect you with skilled professionals to meet all your service needs with just a few taps.
                        </div>
                    </div>
                                        
                    <div class="menu-section">
                        <div class="menu-section-title">Connect With Us</div>
                        <div class="social-links">
                            <a href="https://nysaabhi.github.io/chat" class="social-link">
                                <i class="fab fa-facebook-f"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-twitter"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-instagram"></i>
                            </a>
                            <a href="#" class="social-link">
                                <i class="fab fa-linkedin-in"></i>
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Our Locations</div>
                        <div class="location-links">
                            <a href="location.html" class="location-link">
                                <i class="fas fa-map-marker-alt"></i> India
                            </a>
                        </div>
                    </div>
                    
                    <div class="menu-section">
                        <div class="menu-section-title">Deep Partners</div>
                        <div class="partner-links">
                            <a href="https://www.example1.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Service Provider
                            </a>
                            <a href="https://www.example2.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Vendors
                            </a>
                            <a href="https://www.example3.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Clients
                            </a>
                            <a href="https://www.example4.com" class="partner-link">
                                <i class="fas fa-handshake"></i> Smart Home Innovations
                            </a>
                        </div>
                    </div>
                </div>
            </div>
                
            <div class="chat-input-container">
                <input type="text" id="chatInput" placeholder="Search for services or ask a question..." />
                <div class="input-actions">
                    <button class="send-message" id="sendButton">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>
        
        
        
            <script src="https://cdnjs.cloudflare.com/ajax/libs/flatpickr/4.6.13/flatpickr.min.js"></script>
            <script>                        
                // Configuration object for search and chat functionality
                const searchConfig = {
    qaDatabase: {
        'how do i book a service': { 
            answer: 'To book a service, click on the category, select a service, choose a package, and fill out the booking form. You can also call us directly at +91 9669181789 for immediate assistance.', 
            category: 'Booking',
            keywords: ['book', 'service', 'booking', 'schedule', 'appointment']
        },
        'what services do you offer': { 
            answer: 'We offer a wide range of home and occasion services including Plumbing, Electrical, Carpentry, Painting, Appliance Repair, AC/Heating Repair, Cleaning Services, and Skills Training.', 
            category: 'Services',
            keywords: ['services', 'offer', 'available', 'provide', 'types']
        }
    },
    fallbackResponses: [
        "I apologize, but I couldn't find an exact match for your query. Could you please rephrase your question?",
        "I'm not sure about that specific question. Would you like to know about our available services or booking process instead?",
        "I couldn't find a direct answer to your question. Would you like to speak with a customer service representative?"
    ]
};

// Main Chat System Class
class ChatSystem {
    constructor() {
        this.messageHistory = [];
        this.typingTimeout = null;
        this.similarityThreshold = 0.5;
    }

    initialize() {
        this.chatInput = document.getElementById('chatInput');
        this.chatMessages = document.getElementById('chatMessages');
        this.sendButton = document.getElementById('sendButton');
        
        this.setupEventListeners();
        this.displayWelcomeMessage();
    }

    setupEventListeners() {
        this.sendButton.addEventListener('click', () => this.handleUserInput());
        this.chatInput.addEventListener('keypress', (e) => {
            if (e.key === 'Enter') {
                e.preventDefault();
                this.handleUserInput();
            }
        });
        this.chatInput.addEventListener('input', () => this.handleTypingIndicator());
    }

    handleTypingIndicator() {
        clearTimeout(this.typingTimeout);
        const typingIndicator = document.getElementById('typingIndicator');
        
        if (!typingIndicator) {
            const indicator = document.createElement('div');
            indicator.id = 'typingIndicator';
            indicator.className = 'message bot-message typing-indicator';
            indicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
            this.chatMessages.appendChild(indicator);
        }

        this.typingTimeout = setTimeout(() => {
            const indicator = document.getElementById('typingIndicator');
            if (indicator) indicator.remove();
        }, 1000);
    }

    displayWelcomeMessage() {
        const welcomeMessage = `
            <div id="welcomeMessage" class="message bot-message welcome-message">
                <div class="message-avatar"><i class="fas fa-message"></i></div>
                <div class="message-content">
                    <p>👋 Hello! I'm your On-Demand services assistant.</p>
                    <p>I can help you with:</p>
                    <ul>
                        <li>Booking services</li>
                        <li>Information about our services</li>
                        <li>Pricing and packages</li>
                    </ul>
                    <p>How can I assist you today?</p>
                </div>
            </div>`;
        this.chatMessages.innerHTML = welcomeMessage;
    }

    async handleUserInput() {
        const query = this.chatInput.value.trim();
        if (!query) return;

        this.displayMessage(query, 'user');
        this.chatInput.value = '';

        await this.simulateProcessing();
        
        const response = this.processQuery(query);
        this.displayMessage(response, 'bot');
        
        this.messageHistory.push({ query, response, timestamp: new Date() });
        this.scrollToBottom();
    }

    processQuery(query) {
        const results = this.searchDatabase(query);
        
        if (results.length > 0) {
            results.sort((a, b) => b.similarity - a.similarity);
            const topResult = results[0];
            return `<strong>${topResult.data.category}:</strong> ${topResult.data.answer}`;
        }

        return this.getRandomFallbackResponse();
    }

    searchDatabase(query) {
        const normalizedQuery = query.toLowerCase();
        const words = normalizedQuery.split(/\s+/);
        const results = [];

        for (const [question, data] of Object.entries(searchConfig.qaDatabase)) {
            if (normalizedQuery === question.toLowerCase()) {
                results.push({ similarity: 1, data });
                continue;
            }

            const keywordMatch = data.keywords?.some(keyword => 
                words.includes(keyword.toLowerCase())
            );
            if (keywordMatch) {
                results.push({ similarity: 0.8, data });
                continue;
            }

            const similarity = this.calculateSimilarity(normalizedQuery, question.toLowerCase());
            if (similarity > this.similarityThreshold) {
                results.push({ similarity, data });
            }
        }

        return results;
    }

    calculateSimilarity(str1, str2) {
        const words1 = str1.split(/\s+/);
        const words2 = str2.split(/\s+/);
        
        const set1 = new Set(words1);
        const set2 = new Set(words2);
        const intersection = new Set([...set1].filter(x => set2.has(x)));
        const union = new Set([...set1, ...set2]);
        
        const jaccardSimilarity = intersection.size / union.size;
        
        let orderSimilarity = 0;
        words1.forEach((word, index) => {
            const pos2 = words2.indexOf(word);
            if (pos2 !== -1) {
                orderSimilarity += 1 - Math.abs(index - pos2) / Math.max(words1.length, words2.length);
            }
        });
        
        return (jaccardSimilarity * 0.7) + (orderSimilarity / words1.length * 0.3);
    }

    getRandomFallbackResponse() {
        const responses = searchConfig.fallbackResponses;
        return responses[Math.floor(Math.random() * responses.length)];
    }

    displayMessage(content, type) {
        const messageDiv = document.createElement('div');
        messageDiv.className = `message ${type}-message`;
        
        if (type === 'bot') {
            messageDiv.innerHTML = `
                <div class="message-avatar"><i class="fas fa-comments"></i></div>
                <div class="message-content">${content}</div>`;
        } else {
            messageDiv.innerHTML = `
                <div class="message-content">${content}</div>`;
        }
        
        this.chatMessages.appendChild(messageDiv);
    }

    async simulateProcessing() {
        const typingIndicator = document.createElement('div');
        typingIndicator.className = 'message bot-message typing-indicator';
        typingIndicator.innerHTML = '<div class="dots"><span>.</span><span>.</span><span>.</span></div>';
        
        this.chatMessages.appendChild(typingIndicator);
        this.scrollToBottom();
        
        await new Promise(resolve => setTimeout(resolve, 1000));
        typingIndicator.remove();
    }

    scrollToBottom() {
        this.chatMessages.scrollTop = this.chatMessages.scrollHeight;
    }
}

// Menu System Implementation
function setupMenu() {
    const menuButton = document.getElementById('menuButton');
    const menuOverlay = document.getElementById('menuOverlay');
    const menuItems = document.querySelectorAll('.menu-item');
    const chatMessages = document.getElementById('chatMessages');
    
    const closeButton = document.createElement('button');
    closeButton.innerHTML = '<i class="fas fa-long-arrow-alt-left"></i>';
    closeButton.className = 'menu-close-button';
    menuOverlay.appendChild(closeButton);

    function toggleMenu(show) {
        menuOverlay.style.display = show ? 'block' : 'none';
        if (show) {
            menuOverlay.classList.add('fade-in');
            document.body.style.overflow = 'hidden';
        } else {
            menuOverlay.classList.remove('fade-in');
            document.body.style.overflow = '';
        }
    }

    menuButton.addEventListener('click', () => toggleMenu(true));
    closeButton.addEventListener('click', () => toggleMenu(false));
    
    menuOverlay.addEventListener('click', (e) => {
        if (e.target === menuOverlay) {
            toggleMenu(false);
        }
    });

    menuItems.forEach(item => {
        item.addEventListener('click', () => {
            const selectedItem = item.textContent.trim();
            const message = document.createElement('div');
            message.className = 'message bot-message';
            message.innerHTML = `
                <div class="message-avatar"><i class="fas fa-comments"></i></div>
                <div class="message-content">You selected: ${selectedItem}. How can I assist you further?</div>
            `;
            chatMessages.appendChild(message);
            chatMessages.scrollTop = chatMessages.scrollHeight;
            toggleMenu(false);
        });
    });

    document.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
            toggleMenu(false);
        }
    });
}

// Initialize everything when the DOM is loaded
document.addEventListener('DOMContentLoaded', function() {
    // Initialize Chat System
    const chatSystem = new ChatSystem();
    chatSystem.initialize();

    // Initialize Menu System
    setupMenu();

    // Initialize Date/Time Pickers if they exist
    if (typeof flatpickr !== 'undefined') {
        flatpickr("#dateInput", {
            minDate: "today",
            maxDate: new Date().fp_incr(30),
            dateFormat: "Y-m-d"
        });

        flatpickr("#timeInput", {
            enableTime: true,
            noCalendar: true,
            dateFormat: "H:i",
            minTime: "09:00",
            maxTime: "18:00",
            minuteIncrement: 30
        });
    }
});

// Add necessary styles
const style = document.createElement('style');
style.textContent = `
    .typing-indicator {
        padding: 20px;
    }

    .typing-indicator .dots {
        display: inline-flex;
        gap: 4px;
    }

    .typing-indicator .dots span {
        width: 8px;
        height: 8px;
        background: #666;
        border-radius: 50%;
        animation: bounce 1.4s infinite ease-in-out;
    }

    .typing-indicator .dots span:nth-child(1) { animation-delay: -0.32s; }
    .typing-indicator .dots span:nth-child(2) { animation-delay: -0.16s; }

    @keyframes bounce {
        0%, 80%, 100% { transform: scale(0); }
        40% { transform: scale(1); }
    }

    .message {
    display: flex; 
    gap: 10px; 
    max-width: 85%; 
    margin-bottom: 16px; 
    animation: messageSlide 0.3s ease-out; 
    }

    .bot-message { 
  align-self: flex-start; 
}
   
   .user-message {
        background: #lalala;
        color: white;
        margin-left: auto;
    }

    .message-avatar {
        width: 30px;
        height: 30px;
        background: #lalala;
        color: #FFD700;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .welcome-message ul {
        margin: 10px 0;
        padding-left:20px;
    }

    .menu-close-button {
        position: absolute;
        top: 15px;
        right: 15px;
        background: #1a1a1f;
        border: none;
        color: #FFD700;
        font-size: 24px;
        cursor: pointer;
        padding: 5px;
        z-index: 1001;
    }

    .menu-close-button:hover {
        color: #FFD700;
    }

    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(10px); }
        to { opacity: 1; transform: translateY(0); }
    }
`;
document.head.appendChild(style);

                const townsData = [
  {
    name: "Mumbai",
    image: "https://i.pinimg.com/originals/2c/72/40/2c72408b797d4841b6fe6c395183ba35.png",
    quote: "The town of dreams where ambitions take flight and opportunities never sleep"
  },
  {
    name: "Delhi",
    image: "https://www.mistay.in/travel-blog/content/images/2020/07/travel-4813658_1920.jpg",
    quote: "Where ancient heritage meets modern aspirations in perfect harmony"
  },
  {
    name: "Jaipur",
    image: "https://www.tripsavvy.com/thmb/Afl1v6bgmGid9kPfseymDiAYWa0=/3595x2397/filters:no_upscale():max_bytes(150000):strip_icc()/GettyImages-518387310-04a30994bfb1461bb8000f1864ca1fc5.jpg",
    quote: "Pink Town, where royal heritage colors modern dreams"
  }
];

function showTownsOverlay() {
  let overlay = document.getElementById('townsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'townsOverlay';
    overlay.className = 'towns-overlay';
    document.body.appendChild(overlay);
  }
  overlay.innerHTML = `
    <div class="towns-content">
      <div class="towns-header">
        <h2>Cities We Serve</h2>
        <button class="close-towns" onclick="hideTownsOverlay()">
          <i class="fas fa-long-arrow-alt-left"></i>
        </button>
      </div>
      <div class="towns-grid">
        ${townsData.map(town => `
          <div class="town-card" onclick="selectTown('${town.name}')">
            <div class="town-image-container">
              <img src="${town.image}" alt="${town.name}" class="town-image">
              <div class="town-overlay"></div>
            </div>
            <div class="town-info">
              <h3 class="town-name">${town.name}</h3>
              <p class="town-quote">${town.quote}</p>
            </div>
          </div>
        `).join('')}
      </div>
    </div>
  `;
  setTimeout(() => overlay.classList.add('active'), 10);

  const style = document.createElement('style');
  style.textContent = `
.towns-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background:  #000000;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
}

.towns-overlay.active {
  opacity: 1;
  visibility: visible;
}

.towns-content {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.towns-header {
  padding: 8px 16px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 2px solid var(--primary-color);
  backdrop-filter: blur(10px);
  height: 65px;
  /* Flexbox for horizontal layout */
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
}

.towns-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
  flex-grow: 0;
}

.close-towns {
  background: none;
  border: none;
  color: var(--primary-color);
  font-size: 1.5em;
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.towns-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  margin-top: 10px; /* Ensure content starts below the header */
}

.town-card {
  border-radius: 12px;
  overflow: hidden;
  background: rgba(26, 26, 31, 0.98);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.town-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
}

.town-image-container {
  position: relative;
  width: 100%;
  height: 200px;
  overflow: hidden;
}

.town-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.3s ease;
}

.town-card:hover .town-image {
  transform: scale(1.1);
}

.town-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

.town-info {
  padding: 20px;
  position: relative;
  z-index: 1;
}

.town-name {
  color: var(--text-light);
  margin: 0 0 10px 0;
  font-size: 1.4em;
}

.town-quote {
  color: #cccccc;
  font-size: 0.9em;
  line-height: 1.4;
  margin: 0;
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

@media (max-width: 768px) {
  .towns-grid {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
  }
  
  .town-image-container {
    height: 160px;
  }
}
    `;
  document.head.appendChild(style);
}

function hideTownsOverlay() {
  const overlay = document.getElementById('townsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

function selectTown(townName) {
  console.log(`Selected town: ${townName}`);
  showAlert(`Selected location: ${townName}`, 'success');
  hideTownsOverlay();
}
              
document.addEventListener('DOMContentLoaded', function() {
    // Select the location link in the menu section
    const menuLocationLink = document.querySelector('.menu-section .location-link');
    
    if (menuLocationLink) {
        menuLocationLink.addEventListener('click', function(e) {
            e.preventDefault(); // Prevent default link behavior
            showTownsOverlay(); // Call the existing function to show towns overlay
        });
    }
});

const locatorsData = [
  {
    category: "Grocery",
    metropolis: "Delhi",
    details: {
      name: "Fresh Mart",
      image: "https://media.bizj.us/view/img/11970454/produce-dept*1200xx3000-1688-0-156.jpg",
      description: "Your one-stop shop for fresh produce and daily essentials.",
      address: "Mayur Vihar, Delhi",
      timings: "Mon-Sun: 8 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/fresh-mart",
      menu: [
        { name: "Apples", price: "$2.99/lb" },
        { name: "Milk", price: "$3.49" },
        { name: "Bread", price: "$2.50" }
      ]
    }
  },
  {
    category: "Pharmacy",
    metropolis: "Mumbai",
    details: {
      name: "Health Plus Pharmacy",
      image: "https://i.pinimg.com/originals/14/42/56/144256f85c60d55c5a0018a55458ac7a.jpg",
      description: "Get all your prescriptions and health products here.",
      address: "Borivali East, Mumbai",
      timings: "Mon-Sat: 9 AM - 8 PM, Sun: Closed",
      payment: "Cash, Credit/Debit Cards, Insurance",
      mapLink: "https://maps.app.goo.gl/3TpCCDaVnKzvWvMc6"
    }
  },
  {
    category: "Restaurant",
    metropolis: "Bhopal",
    details: {
      name: "Tasty Bites",
      image: "https://images.adsttc.com/media/images/5e4c/1025/6ee6/7e0b/9d00/0877/large_jpg/feature_-_Main_hall_1.jpg?1582043123",
      description: "Delicious meals and quick service.",
      address: "New Market,Bhopal",
      timings: "Mon-Sun: 11 AM - 10 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/tasty-bites",
      menu: [
        { name: "Burger", price: "$8.99" },
        { name: "Pizza", price: "$12.50" },
        { name: "Salad", price: "$6.99" }
      ]
    }
  },
  {
    category: "Electronics",
    metropolis: "Delhi",
    details: {
      name: "Tech World",
      image: "https://sc01.alicdn.com/kf/H57aba8c0cd5042bfbdbee9a5cd2f44c6s/222471043/H57aba8c0cd5042bfbdbee9a5cd2f44c6s.jpg",
      description: "Latest gadgets and electronics at unbeatable prices.",
      address: "Karol Bagh, Delhi",
      timings: "Mon-Sat: 10 AM - 8 PM, Sun: Closed",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/tech-world"
    }
  },
  {
    category: "Fitness",
    metropolis: "Mumbai",
    details: {
      name: "FitZone Gym",
      image: "https://example.com/fitzone.jpg",
      description: "State-of-the-art gym and fitness equipment.",
      address: "Andheri West, Mumbai",
      timings: "Mon-Sun: 6 AM - 10 PM",
      payment: "Cash, Credit/Debit Cards, Membership Plans",
      mapLink: "https://maps.example.com/fitzone"
    }
  },
  {
    category: "Bookstore",
    metropolis: "Bhopal",
    details: {
      name: "Readers' Paradise",
      image: "https://example.com/readers-paradise.jpg",
      description: "Find books from all genres for every reader.",
      address: "MP Nagar, Bhopal",
      timings: "Mon-Sun: 9 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/readers-paradise"
    }
  },
  {
    category: "Clothing",
    metropolis: "Delhi",
    details: {
      name: "Style Hub",
      image: "https://example.com/style-hub.jpg",
      description: "Trendy apparel for men, women, and kids.",
      address: "Connaught Place, Delhi",
      timings: "Mon-Sun: 10 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/style-hub"
    }
  },
  {
    category: "Pet Supplies",
    metropolis: "Mumbai",
    details: {
      name: "Pet Care Store",
      image: "https://example.com/pet-care.jpg",
      description: "Everything you need for your furry friends.",
      address: "Powai, Mumbai",
      timings: "Mon-Sun: 10 AM - 7 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/pet-care-store"
    }
  },
  {
    category: "Salon",
    metropolis: "Bhopal",
    details: {
      name: "Glam Studio",
      image: "https://example.com/glam-studio.jpg",
      description: "Pamper yourself with the best salon services.",
      address: "Arera Colony, Bhopal",
      timings: "Mon-Sun: 9 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/glam-studio"
    }
  },
  {
    category: "Jewelry",
    metropolis: "Delhi",
    details: {
      name: "Golden Shine",
      image: "https://example.com/golden-shine.jpg",
      description: "Exquisite jewelry for every occasion.",
      address: "Chandni Chowk, Delhi",
      timings: "Mon-Sat: 10 AM - 8 PM, Sun: Closed",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/golden-shine"
    }
  },
  {
    category: "Cinema",
    metropolis: "Mumbai",
    details: {
      name: "City Lights Cinema",
      image: "https://example.com/city-lights.jpg",
      description: "Enjoy the latest blockbusters in comfort and style.",
      address: "Juhu, Mumbai",
      timings: "Mon-Sun: 10 AM - 11 PM",
      payment: "Cash, Credit/Debit Cards, Online Payments",
      mapLink: "https://maps.example.com/city-lights"
    }
  },
  {
    category: "Tailoring",
    metropolis: "Bhopal",
    details: {
      name: "Perfect Stitch",
      image: "https://example.com/perfect-stitch.jpg",
      description: "Custom tailoring for all your fashion needs.",
      address: "TT Nagar, Bhopal",
      timings: "Mon-Sun: 9 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/perfect-stitch"
    }
  },
  {
    category: "Music Store",
    metropolis: "Delhi",
    details: {
      name: "Melody Zone",
      image: "https://example.com/melody-zone.jpg",
      description: "Wide range of musical instruments and accessories.",
      address: "South Ex, Delhi",
      timings: "Mon-Sat: 10 AM - 8 PM, Sun: Closed",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/melody-zone"
    }
  },
  {
    category: "Photography",
    metropolis: "Mumbai",
    details: {
      name: "Shutterbug Studio",
      image: "https://example.com/shutterbug.jpg",
      description: "Professional photography services for all events.",
      address: "Bandra, Mumbai",
      timings: "Mon-Sun: 9 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/shutterbug"
    }
  },
  {
    category: "Gaming",
    metropolis: "Bhopal",
    details: {
      name: "Game Hub",
      image: "https://example.com/game-hub.jpg",
      description: "Experience the ultimate gaming arena.",
      address: "Indrapuri, Bhopal",
      timings: "Mon-Sun: 10 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/game-hub"
    }
  },
  {
    category: "Bakery",
    metropolis: "Delhi",
    details: {
      name: "Sweet Treats",
      image: "https://example.com/sweet-treats.jpg",
      description: "Freshly baked goodies every day.",
      address: "Rajouri Garden, Delhi",
      timings: "Mon-Sun: 8 AM - 9 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/sweet-treats"
    }
  },
  {
    category: "Furniture",
    metropolis: "Mumbai",
    details: {
      name: "Home Comforts",
      image: "https://example.com/home-comforts.jpg",
      description: "Modern and classic furniture for every home.",
      address: "Lower Parel, Mumbai",
      timings: "Mon-Sun: 10 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/home-comforts"
    }
  },
  {
    category: "Toys",
    metropolis: "Bhopal",
    details: {
      name: "Toy World",
      image: "https://example.com/toy-world.jpg",
      description: "Toys for kids of all ages.",
      address: "Habibganj, Bhopal",
      timings: "Mon-Sun: 9 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/toy-world"
    }
  },
  {
    category: "Home Decor",
    metropolis: "Delhi",
    details: {
      name: "DecoStyle",
      image: "https://example.com/decostyle.jpg",
      description: "Unique decor items to beautify your home.",
      address: "Lajpat Nagar, Delhi",
      timings: "Mon-Sun: 10 AM - 8 PM",
      payment: "Cash, Credit/Debit Cards, UPI",
      mapLink: "https://maps.example.com/decostyle"
    }
  },
  {
    category: "Garden Supplies",
    metropolis: "Mumbai",
    details: {
      name: "Green Fingers",
      image: "https://example.com/green-fingers.jpg",
      description: "Everything you need for your garden.",
      address: "Malad, Mumbai",
      timings: "Mon-Sun: 10 AM - 7 PM",
      payment: "Cash, Credit/Debit Cards",
      mapLink: "https://maps.example.com/green-fingers"
    }
  }
];

// Debounce function to limit the rate of search execution
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Function to normalize text for better matching
function normalizeText(text) {
  return text.toLowerCase().trim();
}

// Function to check if a string contains words in any order
function containsWordsInAnyOrder(text, searchWords) {
  text = normalizeText(text);
  return searchWords.every(word => text.includes(word));
}

// Main search function
function searchLocators(searchQuery) {
  const locatorsGrid = document.getElementById('locatorsGrid');
  if (!searchQuery.trim()) {
    locatorsGrid.innerHTML = renderLocatorCards(locatorsData);
    return;
  }

  // Normalize search query and split into words
  const searchWords = normalizeText(searchQuery).split(/\s+/);

  // Search through locations with intent understanding
  const results = locatorsData.filter(locator => {
    const searchableText = `
      ${locator.category} 
      ${locator.metropolis} 
      ${locator.details.name} 
      ${locator.details.description} 
      ${locator.details.address}
      ${locator.details.payment || ''}
      ${(locator.details.menu || []).map(item => item.name).join(' ')}
    `.toLowerCase();

    // Check for exact matches first
    if (searchableText.includes(searchQuery.toLowerCase())) {
      return true;
    }

    // Check for category + city combinations
    if (searchWords.includes(normalizeText(locator.category)) && 
        searchWords.includes(normalizeText(locator.metropolis))) {
      return true;
    }

    // Check for words in any order
    return containsWordsInAnyOrder(searchableText, searchWords);
  });

  // Sort results by relevance
  const sortedResults = results.sort((a, b) => {
    const aText = `${a.category} ${a.metropolis} ${a.details.name}`.toLowerCase();
    const bText = `${b.category} ${b.metropolis} ${b.details.name}`.toLowerCase();
    
    // Prioritize exact matches
    const aExactMatch = aText.includes(searchQuery.toLowerCase());
    const bExactMatch = bText.includes(searchQuery.toLowerCase());
    
    if (aExactMatch && !bExactMatch) return -1;
    if (!aExactMatch && bExactMatch) return 1;
    
    return 0;
  });

  // Update the grid with search results
  if (sortedResults.length > 0) {
    locatorsGrid.innerHTML = renderLocatorCards(sortedResults);
  } else {
    locatorsGrid.innerHTML = `
      <div style="
        grid-column: 1 / -1;
        text-align: center;
        padding: 40px;
        color: #aaa;
        font-size: 1.1em;
      ">
        No results found for "${searchQuery}". Try different keywords or check the spelling.
      </div>`;
  }
}

function showLocatorsOverlay() {
  let overlay = document.getElementById('locatorsOverlay');
  if (!overlay) {
    overlay = document.createElement('div');
    overlay.id = 'locatorsOverlay';
    overlay.className = 'locators-overlay';
    document.body.appendChild(overlay);
  }

  // Get unique categories and metropolises, with 'All' as the first option
  const categories = ['All', ...new Set(locatorsData.map(loc => loc.category))];
  const metropolises = ['All', ...new Set(locatorsData.map(loc => loc.metropolis))];

  overlay.innerHTML = `
    <div class="locators-content">
      <div class="locators-header">
        <h2>Locator</h2>
        <div class="header-buttons-container">
          <button class="select-metropolis" onclick="showMetropolisModal()">
            <i class="fas fa-map-marker-alt"></i> Select City
          </button>
          <button class="close-locators" onclick="hideLocatorsOverlay()">
            <i class="fas fa-long-arrow-alt-left"></i>
          </button>
        </div>
      </div>

      <div class="locators-filter-container">
        <div class="locators-filter-buttons">
          ${categories.map(category => `
            <button 
              class="filter-button ${category === 'All' ? 'active' : ''}" 
              data-category="${category}" 
              onclick="filterLocators('${category}', 'category')"
            >
              ${category}
            </button>
          `).join('')}
        </div>
      </div>

      <div class="locators-grid" id="locatorsGrid">
        ${renderLocatorCards(locatorsData)}
      </div>
    </div>

    <div class="chat-container" style="margin-top: 0; padding-bottom: 10px;">
      <input type="text" id="exploreInput" placeholder="Search for services or locations..." />
      <div class="input-actions">
        <button class="send-message" id="exploreButton">
          <i class="fas fa-paper-plane"></i>
        </button>
      </div>
    </div>
  `;

  // Initialize search functionality
  const exploreInput = document.getElementById('exploreInput');
  const exploreButton = document.getElementById('exploreButton');

  // Debounced search for real-time filtering
  const debouncedSearch = debounce((query) => searchLocators(query), 300);

  // Event listeners
  exploreInput.addEventListener('input', (e) => {
    debouncedSearch(e.target.value);
  });

  exploreButton.addEventListener('click', () => {
    searchLocators(exploreInput.value);
  });

  // Handle Enter key
  exploreInput.addEventListener('keypress', (e) => {
    if (e.key === 'Enter') {
      searchLocators(exploreInput.value);
    }
  })

  setTimeout(() => overlay.classList.add('active'), 10);


  const style = document.createElement('style');
  style.textContent = `
  .locators-overlay {
  position: fixed;
  top: 60px;
  left: 0;
  right: 0;
  bottom: 60px;
  background: #1a1a1f;
  z-index: 1000;
  opacity: 0;
  visibility: hidden;
  transition: all 0.3s ease;
  overflow-y: auto;
  width: 100%;
  display: flex;
  flex-direction: column;
  scrollbar-width: none;
  -ms-overflow-style: none;
}

.locators-overlay::-webkit-scrollbar {
  display: none;
}

.locators-overlay.active {
  opacity: 1;
  visibility: visible;
}

.locators-content {
  flex: 1;
  width: 100%;
  max-width: 100%;
  padding: 20px;
  margin: 0 auto;
  position: relative;
}

.locators-header {
  padding: 8px 16px;
  background: rgba(26, 26, 31, 0.95);
  position: fixed;
  width: 100%;
  top: 0;
  left: 0;
  z-index: 30;
  border-bottom: 2px solid var(--primary-color);
  backdrop-filter: blur(10px);
  height: 65px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 5px;
}

.locators-header h2 {
  color: var(--primary-color);
  margin: 0;
  font-size: 1.5em;
}

.header-buttons-container {
  display: flex;
  align-items: center;
  gap: 12px;
}

.select-metropolis {
  background: rgba(255, 215, 0, 0.1);
  color: #e6e6e6;
  border: none;
  padding: 10px 15px;
  border-radius: 20px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 10px;
  transition: all 0.3s ease;
}

.select-metropolis:hover {
  background: #ffd700;
  color: #1a1a1f;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.close-locators {
  background: none;
  border: none;
  color: var(--primary-color);
  font-size: 1.5em;
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-left: 0px;
}

.locators-filter-container {
  position: fixed;
  top: 70px;
  left: 0;
  width: 100vw;
  max-width: 100vw;
  z-index: 10;
  background: #1a1a1f;
  padding: 10px 15px;
  box-sizing: border-box;
}

.locators-filter-buttons {
  display: flex;
  justify-content: flex-start;
  gap: 4px;
  padding-bottom: 10px;
  width: 100vw;
  margin-left: calc(-50vw + 50%);
  overflow-x: auto;
  scrollbar-width: none;
}

.locators-filter-buttons::-webkit-scrollbar {
  height: 0px;
}

.locators-filter-buttons::-webkit-scrollbar-thumb {
  background-color: var(--primary-color);
  border-radius: 3px;
}

.filter-button {
  margin-top: 2px;
  margin-left: 2px;
  margin-right: 5px;
  flex-shrink: 0;
  background: rgba(255, 215, 0, 0.1);
  color: var(--text-light);
  border: 1px solid rgba(255, 215, 0, 0.2);
  padding: 10px 15px;
  border-radius: 25px;
  transition: all 0.3s ease;
  cursor: pointer;
  white-space: nowrap;
  font-weight: 500;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
}

.filter-button:hover {
  background: var(--primary-color);
  color: black;
  transform: scale(1.05);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.filter-button.active {
  background: var(--primary-color);
  color: black;
  font-weight: 700;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.3);
}

.locators-grid-container {
  flex: 1;
  width: 100%;
  padding-bottom: 20px;
  position: relative;
}

.locators-grid {
  margin-top: 70px;
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(400px, 1fr));
  gap: 20px;
  animation: fadeInUp 0.5s ease-out;
  padding-bottom: 140px;
  width: 100%;
}

.locator-card {
  border-radius: 12px;
  overflow: hidden;
  background: rgba(255, 215, 0, 0.1);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.locator-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.locator-info {
  padding: 20px;
}

.action-button {
  display: inline-block;
  margin: 5px;
  padding: 10px;
  background: var(--primary-color);
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: all 0.3s ease;
  text-decoration: none;
}

.action-button:hover {
  background-color: #f39c12;
  transform: scale(1.05);
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.2);
}

.action-button i {
  margin-right: 5px;
}

.chat-container {
  position: fixed;
  bottom: 60px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

#exploreInput {
  flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px;
  color: var(--text-light);
  font-size: 0.95em;
  width: 100%;
}

/* Desktop and laptop styles */
@media (min-width: 769px) {
  .chat-container {
    width: 100%;
    display: flex;
    justify-content: center;
  }
  
  #exploreInput {
    width: 60%;
    max-width: 1200px;
  }
  
  .locators-grid {
    padding-bottom: 180px;
  }
}

/* Mobile styles */
@media (max-width: 768px) {
  .chat-container {
    width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  .locators-grid {
    grid-template-columns: 1fr;
    padding: 0 10px;
    gap: 15px;
  }

  .locator-card {
    width: 100%;
    margin: 0;
  }

  .locators-grid-container {
    width: 100%;
    max-width: 100%;
    padding: 0;
  }

  .locators-content {
    padding: 10px 0;
    padding-bottom: 200px;
  }

  #exploreInput {
    width: 100%;
    padding: 20px 20px;
  }
}
   `;
  document.head.appendChild(style);
}

// Function to show details page
function showLocatorDetails(category) {
  const locator = locatorsData.find(loc => loc.category === category);
  if (!locator) return;

  // Create details container
  const detailsPage = document.createElement('div');
  detailsPage.className = 'locator-details-page';
  
  // Generate category-specific content
  const categoryContent = getCategorySpecificContent(locator);
  
  detailsPage.innerHTML = `
    <div class="details-header">
      <button class="back-button" onclick="closeLocatorDetails()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>${locator.details.name}</h2>
    </div>
    
    <div class="details-content">
      <div class="image-carousel">
        <div class="carousel-container">
          <img src="${locator.details.image}" alt="${locator.details.name}" class="active-slide">
          <button class="carousel-button prev" onclick="changeSlide(-1)">❮</button>
          <button class="carousel-button next" onclick="changeSlide(1)">❯</button>
        </div>
        <div class="carousel-dots">
          <span class="dot active" onclick="currentSlide(1)"></span>
          <span class="dot" onclick="currentSlide(2)"></span>
          <span class="dot" onclick="currentSlide(3)"></span>
        </div>
      </div>

      <div class="details-info">
        <div class="basic-info">
          <h3>About</h3>
          <p>${locator.details.description}</p>
          
          <div class="info-grid">
            <div class="info-item">
              <i class="fas fa-map-marker-alt"></i>
              <div>
                <h4>Location</h4>
                <p>${locator.details.address}</p>
              </div>
            </div>
            
            <div class="info-item">
              <i class="fas fa-clock"></i>
              <div>
                <h4>Hours</h4>
                <p>${locator.details.timings}</p>
              </div>
            </div>
            
            <div class="info-item">
              <i class="fas fa-credit-card"></i>
              <div>
                <h4>Payment Methods</h4>
                <p>${locator.details.payment}</p>
              </div>
            </div>
          </div>
        </div>

        ${categoryContent}
        
        <div class="action-buttons">
          <a href="${locator.details.mapLink}" target="_blank" class="detail-action-button">
            <i class="fas fa-directions"></i> Get Directions
          </a>
          <button class="detail-action-button share-button" onclick="shareLocation('${locator.details.name}')">
            <i class="fas fa-share-alt"></i> Share
          </button>
        </div>
      </div>
    </div>
  `;

  // Add styles
  const style = document.createElement('style');
  style.textContent = `
    .locator-details-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from {
        transform: translateX(100%);
      }
      to {
        transform: translateX(0);
      }
    }

    .details-header {
      position: sticky;
      top: 0;
      background: rgba(26, 26, 31, 0.95);
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-bottom: 2px solid var(--primary-color);
      backdrop-filter: blur(10px);
      z-index: 10;
    }

    .back-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 10px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .back-button:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: scale(1.1);
    }

    .details-content {
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
    }

    .image-carousel {
      position: relative;
      margin-bottom: 30px;
      border-radius: 15px;
      overflow: hidden;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    }

    .carousel-container {
      position: relative;
      aspect-ratio: 16/9;
      background: #000;
    }

    .carousel-container img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      transition: opacity 0.3s ease;
    }

    .carousel-button {
      position: absolute;
      top: 50%;
      transform: translateY(-50%);
      background: rgba(0, 0, 0, 0.5);
      color: white;
      border: none;
      padding: 15px;
      cursor: pointer;
      font-size: 1.2em;
      transition: all 0.3s ease;
    }

    .carousel-button:hover {
      background: rgba(0, 0, 0, 0.8);
    }

    .prev { left: 10px; }
    .next { right: 10px; }

    .carousel-dots {
      text-align: center;
      padding: 10px 0;
    }

    .dot {
      height: 10px;
      width: 10px;
      margin: 0 5px;
      background-color: #bbb;
      border-radius: 50%;
      display: inline-block;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .dot.active {
      background-color: var(--primary-color);
      transform: scale(1.2);
    }

    .details-info {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 15px;
      padding: 30px;
      margin-top: 20px;
    }

    .info-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 20px;
      margin: 30px 0;
    }

    .info-item {
      display: flex;
      gap: 15px;
      padding: 15px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      transition: all 0.3s ease;
    }

    .info-item:hover {
      transform: translateY(-5px);
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }

    .info-item i {
      font-size: 1.5em;
      color: var(--primary-color);
    }

    .info-item h4 {
      margin: 0 0 5px 0;
      color: var(--text-light);
    }

    .info-item p {
      margin: 0;
      color: #aaa;
    }

    .action-buttons {
      display: flex;
      gap: 15px;
      margin-top: 30px;
    }

    .detail-action-button {
      flex: 1;
      padding: 15px;
      border: none;
      border-radius: 10px;
      background: var(--primary-color);
      color: #1a1a1f;
      font-weight: bold;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 10px;
      transition: all 0.3s ease;
      text-decoration: none;
    }

    .detail-action-button:hover {
      transform: translateY(-2px);
      box-shadow: 0 5px 15px rgba(255, 215, 0, 0.3);
    }

    .share-button {
      background: rgba(255, 215, 0, 0.1) !important;
      color: var(--primary-color) !important;
    }

    /* Category-specific styles */
    .menu-grid {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
      gap: 20px;
      margin-top: 20px;
    }

    .menu-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 10px;
      padding: 15px;
      transition: all 0.3s ease;
    }

    .menu-item:hover {
      transform: translateY(-5px);
      background: rgba(255, 255, 255, 0.1);
    }

    .product-list {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
      gap: 15px;
      margin-top: 20px;
    }

    .product-item {
      background: rgba(255, 255, 255, 0.05);
      border-radius: 10px;
      padding: 15px;
      text-align: center;
    }

    @media (max-width: 768px) {
      .details-content {
        padding: 10px;
      }

      .info-grid {
        grid-template-columns: 1fr;
      }

      .action-buttons {
        flex-direction: column;
      }

      .menu-grid {
        grid-template-columns: 1fr;
      }
    }
  `;
  document.head.appendChild(style);
  document.body.appendChild(detailsPage);
}

// Function to get category-specific content
function getCategorySpecificContent(locator) {
  switch (locator.category) {
    case 'Grocery':
      return `
        <div class="category-specific">
          <h3>Featured Products</h3>
          <div class="product-list">
            ${locator.details.menu?.map(item => `
              <div class="product-item">
                <h4>${item.name}</h4>
                <p>${item.price}</p>
              </div>
            `).join('') || ''}
          </div>
        </div>
      `;

    case 'Bakery':
      return `
        <div class="category-specific">
          <h3>Today's Specials</h3>
          <div class="menu-grid">
            ${locator.details.menu?.map(item => `
              <div class="menu-item">
                <h4>${item.name}</h4>
                <p class="price">${item.price}</p>
              </div>
            `).join('') || ''}
          </div>
        </div>
      `;

    case 'Garden Supplies':
      return `
        <div class="category-specific">
          <h3>Services</h3>
          <div class="services-list">
            <div class="service-item">
              <h4>Consultations</h4>
              <p>Expert advice on plant care and garden design</p>
            </div>
            <div class="service-item">
              <h4>Delivery</h4>
              <p>Free delivery for orders above $50</p>
            </div>
          </div>
        </div>
      `;

    default:
      return '';
  }
}

// Function to close details page
function closeLocatorDetails() {
  const detailsPage = document.querySelector('.locator-details-page');
  if (detailsPage) {
    detailsPage.style.transform = 'translateX(100%)';
    setTimeout(() => detailsPage.remove(), 300);
  }
}

// Carousel functions
let currentSlideIndex = 1;

function changeSlide(n) {
  showSlides(currentSlideIndex += n);
}

function currentSlide(n) {
  showSlides(currentSlideIndex = n);
}

function showSlides(n) {
  const dots = document.getElementsByClassName("dot");
  if (n > dots.length) currentSlideIndex = 1;
  if (n < 1) currentSlideIndex = dots.length;

  // Update dots
  for (let i = 0; i < dots.length; i++) {
    dots[i].className = dots[i].className.replace(" active", "");
  }
  dots[currentSlideIndex-1].className += " active";
}

// Share function
function shareLocation(name) {
  if (navigator.share) {
    navigator.share({
      title: name,
      text: `Check out ${name} on our platform!`,
      url: window.location.href
    }).catch(console.error);
  } else {
    // Fallback
    const dummy = document.createElement('input');
    document.body.appendChild(dummy);
    dummy.value = window.location.href;
    dummy.select();
    document.execCommand('copy');
    document.body.removeChild(dummy);
    
    alert('Link copied to clipboard!');
  }
}

// First, modify the renderLocatorCards function to add the chat button
function renderLocatorCards(filteredData) {
  return filteredData.map(locator => `
    <div class="locator-card">
      <img src="${locator.details.image}" alt="${locator.details.name}" class="locator-image">
      <div class="locator-info">
        <h3>${locator.details.name}</h3>
        <p>${locator.details.description}</p>
        <p><strong>Address:</strong> ${locator.details.address}</p>
        <p><strong>Timings:</strong> ${locator.details.timings}</p>
        <div class="card-actions">
          ${locator.details.menu ? `
            <button class="action-button" onclick="showMenu(event, '${locator.category}')">
              <i class="fas fa-utensils"></i> Menu
            </button>
          ` : ''}
          <a href="${locator.details.mapLink}" target="_blank" class="action-button">
            <i class="fas fa-map-marker-alt"></i> Map
          </a>
          <button class="action-button" onclick="showInfo('${locator.category}')">
            <i class="fas fa-info-circle"></i> Info
          </button>
          <button class="action-button details-button" onclick="showLocatorDetails('${locator.category}')">
            <i class="fas fa-external-link-alt"></i> Details
          </button>
          <button class="action-button chat-button" onclick="showChatSupport('${locator.category}')">
            <i class="fas fa-comments"></i> Chat
          </button>
        </div>
      </div>
    </div>
  `).join('');
}

// FAQ data based on categories
const faqData = {
  'Grocery': [
    {
      question: "What are your delivery hours?",
      answer: "We deliver from 9 AM to 8 PM every day. Same-day delivery is available for orders placed before 2 PM."
    },
    {
      question: "Do you have a minimum order value for delivery?",
      answer: "Yes, the minimum order value for free delivery is $30. Orders below this amount will incur a $5 delivery charge."
    },
    {
      question: "How do I track my order?",
      answer: "Once your order is confirmed, you'll receive a tracking link via SMS and email to monitor your delivery in real-time."
    }
  ],
  'Toys': [
    {
      question: "What is your return policy?",
      answer: "We offer a 30-day return policy for unopened items. Opened items can be returned within 7 days if defective."
    },
    {
      question: "Do you offer gift wrapping?",
      answer: "Yes, we provide gift wrapping services for a nominal fee of $3 per item."
    },
    {
      question: "Are your toys age-appropriate labeled?",
      answer: "Yes, all our toys are clearly labeled with age recommendations and safety warnings."
    }
  ],
  'Home Decor': [
    {
      question: "Do you offer installation services?",
      answer: "Yes, we provide professional installation services for wall décor, lighting, and large furniture pieces."
    },
    {
      question: "What is your custom order policy?",
      answer: "Custom orders typically take 2-3 weeks to process and require a 50% advance payment."
    },
    {
      question: "Do you provide interior design consultation?",
      answer: "Yes, we offer free 30-minute consultations with our design experts for purchases above $500."
    }
  ],
  'Garden Supplies': [
    {
      question: "Do you offer plant care advice?",
      answer: "Yes, our experts provide free plant care guidance and troubleshooting support."
    },
    {
      question: "What is your plant guarantee policy?",
      answer: "We offer a 14-day guarantee on all plants. If your plant dies within this period, we'll replace it free of charge."
    },
    {
      question: "Do you deliver large items like soil bags?",
      answer: "Yes, we deliver bulk items. Free delivery for orders above $100, otherwise a delivery fee applies based on weight and distance."
    }
  ]
};

// Contact information for each category
const supportContacts = {
  'Grocery': {
    email: "support@freshmart.com",
    phone: "+1-800-GROCERY"
  },
  'Toys': {
    email: "help@toyworld.com",
    phone: "+1-800-TOYSRUS"
  },
  'Home Decor': {
    email: "support@decostyle.com",
    phone: "+1-800-DECORATE"
  },
  'Garden Supplies': {
    email: "help@greenfingers.com",
    phone: "+1-800-GARDEN"
  }
};

function showChatSupport(category) {
  const chatPage = document.createElement('div');
  chatPage.className = 'chat-support-page';
  
  const faqs = faqData[category] || [];
  const contacts = supportContacts[category] || {};
  
  chatPage.innerHTML = `
    <div class="chat-support-header">
      <button class="back-button" onclick="closeChatSupport()">
        <i class="fas fa-arrow-left"></i> Back
      </button>
      <h2>Chat Support - ${category}</h2>
    </div>
    
    <div class="chat-support-content">
      <div class="faq-section" id="faqSection">
        <h3>Frequently Asked Questions</h3>
        <div class="faq-list">
          ${faqs.map((faq, index) => `
            <div class="faq-item">
              <div class="faq-question" onclick="toggleFAQ(${index})">
                <span>${faq.question}</span>
                <i class="fas fa-chevron-down"></i>
              </div>
              <div class="faq-answer" id="faq-answer-${index}">
                ${faq.answer}
              </div>
            </div>
          `).join('')}
        </div>
      </div>

      <div class="contact-support" id="contactSupport">
        <div class="contact-support-header">
          <h3>Need More Help?</h3>
          <button class="refresh-button" onclick="resetChatSupport('${category}')">
            <i class="fas fa-sync-alt"></i>
          </button>
        </div>
        <p>We couldn't find what you were looking for. Our support team is here to help!</p>
        <div class="contact-info">
          <a href="mailto:${contacts.email}" class="contact-button">
            <i class="fas fa-envelope"></i>
            <span>${contacts.email}</span>
          </a>
          <a href="tel:${contacts.phone}" class="contact-button">
            <i class="fas fa-phone"></i>
            <span>${contacts.phone}</span>
          </a>
        </div>
      </div>

      <div class="chat-search-section">
        <div class="chat-search-container">
          <input 
            type="text" 
            id="chatSearchInput" 
            placeholder="Search for answers or ask a question..." 
            onkeyup="handleSearch(event, '${category}')"
          />
          <button onclick="handleSearch(null, '${category}')">
            <i class="fas fa-paper-plane"></i>
          </button>
        </div>
        
        <div id="searchResults" class="search-results"></div>
      </div>
    </div>
  `;

  // Add styles
  const style = document.createElement('style');
  style.textContent = `
    .chat-support-page {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: #1a1a1f;
      z-index: 2000;
      overflow-y: auto;
      animation: slideIn 0.3s ease-out;
    }

    @keyframes slideIn {
      from { transform: translateX(100%); }
      to { transform: translateX(0); }
    }

    @keyframes fadeIn {
      from { opacity: 0; }
      to { opacity: 1; }
    }

    .chat-support-header {
      position: sticky;
      top: 0;
      background: rgba(26, 26, 31, 0.95);
      padding: 20px;
      display: flex;
      align-items: center;
      gap: 20px;
      border-bottom: 2px solid var(--primary-color);
      backdrop-filter: blur(10px);
      z-index: 10;
    }

    .back-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 10px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .back-button:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .chat-support-content {
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
    }

    .faq-section {
      margin-bottom: 80px;
      transition: all 0.3s ease;
    }

    .faq-item {
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      margin-bottom: 10px;
      overflow: hidden;
      transition: all 0.3s ease;
    }

    .faq-question {
      padding: 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      cursor: pointer;
      color: var(--text-light);
      font-weight: 500;
      transition: all 0.3s ease;
    }

    .faq-question:hover {
      background: rgba(255, 215, 0, 0.1);
    }

    .faq-question i {
      transition: transform 0.3s ease;
    }

    .faq-question.active i {
      transform: rotate(180deg);
    }

    .faq-answer {
      padding: 0 20px;
      max-height: 0;
      overflow: hidden;
      transition: all 0.3s ease;
      color: #aaa;
    }

    .faq-answer.active {
      padding: 20px;
      max-height: 500px;
      border-top: 1px solid rgba(255, 215, 0, 0.1);
    }

    .contact-support {
      display: none;
      animation: fadeIn 0.3s ease-out;
      padding: 40px 20px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 15px;
      text-align: center;
      margin: 20px auto;
      max-width: 600px;
    }

    .contact-support-header {
      display: flex;
      justify-content: center;
      align-items: center;
      gap: 15px;
      margin-bottom: 20px;
    }

    .refresh-button {
      background: none;
      border: none;
      color: var(--primary-color);
      font-size: 1.2em;
      cursor: pointer;
      padding: 8px;
      border-radius: 50%;
      transition: all 0.3s ease;
    }

    .refresh-button:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: rotate(180deg);
    }

    .contact-button {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 15px 25px;
      background: rgba(255, 215, 0, 0.1);
      border-radius: 10px;
      color: var(--primary-color);
      text-decoration: none;
      transition: all 0.3s ease;
      margin: 10px 0;
    }

    .contact-button:hover {
      background: rgba(255, 215, 0, 0.2);
      transform: translateX(5px);
    }

    .chat-search-container {
  position: fixed;
  bottom: 2px;
  left: 0;
  right: 0;
  width: 100%;
  max-width: 100%;
  z-index: 1000;
  padding: 20px;
  background: #1a1a1f;
  border-top: 1px solid rgba(255, 215, 0, 0.1);
  display: flex;
  gap: 12px;
  align-items: center;
  box-sizing: border-box;
}

#chatSearchInput {
    flex: 1;
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 215, 0, 0.2);
  border-radius: 12px;
  padding: 20px;
  color: var(--text-light);
  font-size: 0.95em;
  width: 100%;
}

.chat-search-container button {
        padding: 20px 20px; 
    background: var(--accent-gradient); 
    border: none; 
    border-radius: 8px; 
    color: #ffffff; 
    cursor: pointer; 
}

.chat-search-container i { 
    font-size: 1.5em; 
}

    .search-results {
      margin: 20px 0;
      padding-bottom: 80px;
    }

    .search-result-item {
      padding: 15px;
      background: rgba(255, 215, 0, 0.05);
      border-radius: 10px;
      margin-bottom: 10px;
      cursor: pointer;
      transition: all 0.3s ease;
    }

    .search-result-item:hover {
      background: rgba(255, 215, 0, 0.1);
      transform: translateX(10px);
    }

/* Mobile styles */
@media (max-width: 768px) {
    .chat-search-container {
        width: 100%;
    left: 0;
    transform: none;
    padding: 10px;
    gap: 8px;
  }

  #chatSearchInput {
    width: 100%;
    padding: 20px 20px;
  }
}

    @media (max-width: 768px) {
      .chat-support-content {
        padding: 15px;
      }

      .chat-search-container {
        padding: 10px;
      }

      #chatSearchInput {
        padding: 20px 20px;
      }

      .chat-search-container button {
        padding: 20px 20px;
      }

      .faq-question {
        padding: 15px;
      }

      .contact-button {
        padding: 12px 20px;
      }
    }
  `;
  
  document.head.appendChild(style);
  document.body.appendChild(chatPage);

  // Initially hide contact support
  const contactSupport = document.getElementById('contactSupport');
  contactSupport.style.display = 'none';
}

function handleSearch(event, category) {
  if (event && event.key && event.key !== 'Enter') return;
  
  const searchInput = document.getElementById('chatSearchInput');
  const searchResults = document.getElementById('searchResults');
  const faqSection = document.getElementById('faqSection');
  const contactSupport = document.getElementById('contactSupport');
  const query = searchInput.value.toLowerCase().trim();
  
  if (!query) {
    resetChatSupport(category);
    return;
  }
  
  const faqs = faqData[category] || [];
  const matchingFAQs = faqs.filter(faq => 
    faq.question.toLowerCase().includes(query) || 
    faq.answer.toLowerCase().includes(query)
  );
  
  if (matchingFAQs.length > 0) {
    searchResults.innerHTML = matchingFAQs.map((faq, index) => `
      <div class="search-result-item" onclick="showAnswer('${faq.question}', '${faq.answer}')">
        <strong>${faq.question}</strong>
      </div>
    `).join('');
    
    faqSection.style.display = 'block';
    contactSupport.style.display = 'none';
  } else {
    searchResults.innerHTML = '';
    faqSection.style.display = 'none';
    contactSupport.style.display = 'block';
    contactSupport.style.animation = 'fadeIn 0.3s ease-out';
  }
}

function resetChatSupport(category) {
  const searchInput = document.getElementById('chatSearchInput');
  const searchResults = document.getElementById('searchResults');
  const faqSection = document.getElementById('faqSection');
  const contactSupport = document.getElementById('contactSupport');
  
  searchInput.value = '';
  searchResults.innerHTML = '';
  
  faqSection.style.display = 'block';
  faqSection.style.animation = 'fadeIn 0.3s ease-out';
  
  contactSupport.style.display = 'none';
}

function toggleFAQ(index) {
  const answer = document.getElementById(`faq-answer-${index}`);
  const question = answer.previousElementSibling;
  
  if (answer.classList.contains('active')) {
    answer.classList.remove('active');
    question.classList.remove('active');
  } else {
    // Close any other open FAQ
    const allAnswers = document.querySelectorAll('.faq-answer.active');
    const allQuestions = document.querySelectorAll('.faq-question.active');
    
    allAnswers.forEach(item => item.classList.remove('active'));
    allQuestions.forEach(item => item.classList.remove('active'));
    
    // Open the clicked FAQ
    answer.classList.add('active');
    question.classList.add('active');
  }
}

function showAnswer(question, answer) {
  const searchResults = document.getElementById('searchResults');
  searchResults.innerHTML = `
    <div class="faq-item">
      <div class="faq-question active">
        <span>${question}</span>
        <i class="fas fa-chevron-down"></i>
      </div>
      <div class="faq-answer active">
        ${answer}
        <div class="answer-feedback">
          <p>Was this answer helpful?</p>
          <div class="feedback-buttons">
            <button onclick="handleFeedback(true)" class="feedback-button">
              <i class="fas fa-thumbs-up"></i> Yes
            </button>
            <button onclick="handleFeedback(false)" class="feedback-button">
              <i class="fas fa-thumbs-down"></i> No
            </button>
          </div>
        </div>
      </div>
    </div>
  `;

  // Add styles for feedback section if not already added
  if (!document.querySelector('#feedbackStyles')) {
    const feedbackStyles = document.createElement('style');
    feedbackStyles.id = 'feedbackStyles';
    feedbackStyles.textContent = `
      .answer-feedback {
        margin-top: 20px;
        padding-top: 20px;
        border-top: 1px solid rgba(255, 215, 0, 0.1);
        text-align: center;
      }

      .feedback-buttons {
        display: flex;
        justify-content: center;
        gap: 15px;
        margin-top: 10px;
      }

      .feedback-button {
        background: rgba(255, 215, 0, 0.1);
        border: none;
        border-radius: 8px;
        padding: 8px 15px;
        color: var(--text-light);
        cursor: pointer;
        transition: all 0.3s ease;
        display: flex;
        align-items: center;
        gap: 8px;
      }

      .feedback-button:hover {
        background: rgba(255, 215, 0, 0.2);
        transform: translateY(-2px);
      }
    `;
    document.head.appendChild(feedbackStyles);
  }
}

function handleFeedback(isHelpful) {
  const feedbackButtons = document.querySelector('.feedback-buttons');
  feedbackButtons.innerHTML = `
    <div class="feedback-response">
      ${isHelpful ? 
        'Thank you for your feedback! We\'re glad this was helpful.' : 
        'Thank you for your feedback. We\'ll work on improving this answer.'}
    </div>
  `;

  // Add styles for feedback response if not already added
  if (!document.querySelector('#feedbackResponseStyles')) {
    const responseStyles = document.createElement('style');
    responseStyles.id = 'feedbackResponseStyles';
    responseStyles.textContent = `
      .feedback-response {
        color: var(--primary-color);
        padding: 10px;
        border-radius: 8px;
        background: rgba(255, 215, 0, 0.1);
        animation: fadeIn 0.3s ease-out;
      }
    `;
    document.head.appendChild(responseStyles);
  }
}

function closeChatSupport() {
  const chatPage = document.querySelector('.chat-support-page');
  if (chatPage) {
    chatPage.style.animation = 'slideOut 0.3s ease-out forwards';
    
    // Add slide out animation if not already added
    if (!document.querySelector('#slideOutStyles')) {
      const slideOutStyles = document.createElement('style');
      slideOutStyles.id = 'slideOutStyles';
      slideOutStyles.textContent = `
        @keyframes slideOut {
          from { transform: translateX(0); }
          to { transform: translateX(100%); }
        }
      `;
      document.head.appendChild(slideOutStyles);
    }
    
    setTimeout(() => chatPage.remove(), 300);
  }
}

// Helper function to escape HTML special characters
function escapeHTML(str) {
  const div = document.createElement('div');
  div.textContent = str;
  return div.innerHTML;
}

function showMetropolisModal() {
  // Enhanced data processing with more robust error handling
  const metropolisGroups = locatorsData.reduce((acc, locator) => {
    if (!locator || !locator.metropolis) return acc;
    
    const metropolis = locator.metropolis.trim();
    if (!metropolis) return acc;

    if (!acc[metropolis]) {
      acc[metropolis] = {
        name: metropolis,
        categories: new Set(),
        count: 0,
        uniqueCategories: new Set() // Track unique categories more precisely
      };
    }
    
    // Only add if category is unique for this metropolis
    if (locator.category && !acc[metropolis].uniqueCategories.has(locator.category)) {
      acc[metropolis].categories.add(locator.category);
      acc[metropolis].uniqueCategories.add(locator.category);
    }
    acc[metropolis].count++;
    return acc;
  }, {});

  // More sophisticated sorting with secondary criteria
  const metropoliesToShow = Object.values(metropolisGroups)
    .sort((a, b) => {
      // Primary sort by number of locations
      if (b.count !== a.count) return b.count - a.count;
      
      // Secondary sort by number of unique categories
      return b.categories.size - a.categories.size;
    });

  const modal = document.createElement('div');
  modal.id = 'metropolisSelectionModal';
  modal.setAttribute('role', 'dialog');
  modal.setAttribute('aria-labelledby', 'metropolis-modal-title');
  modal.className = 'metropolis-selection-modal';
  
  modal.innerHTML = `
    <div class="metropolis-modal-content" role="document">
      <div class="metropolis-modal-header">
        <h2 id="metropolis-modal-title">Select Your City</h2>
        <p>Explore services across different metropolises</p>
      </div>
      
      <div class="metropolis-grid" id="metropolisGrid">
        ${metropoliesToShow.map(metropolis => `
          <div 
            class="metropolis-card" 
            onclick="selectMetropolis('${metropolis.name}')"
            onkeydown="handleMetropolisKeydown(event, '${metropolis.name}')"
            tabindex="0"
            role="button"
            aria-label="Select ${metropolis.name}"
          >
            <div class="metropolis-card-inner">
              <div class="metropolis-icon">
                <i class="fas fa-city" aria-hidden="true"></i>
              </div>
              <div class="metropolis-details">
                <h3>${metropolis.name}</h3>
                <div class="metropolis-meta">
                  <span class="locations-count">
                    ${metropolis.count} Location${metropolis.count !== 1 ? 's' : ''}
                  </span>
                  <span class="categories-available" title="Available Categories">
                    ${Array.from(metropolis.categories).slice(0, 3).join(', ')}
                    ${metropolis.categories.size > 3 ? '...' : ''}
                  </span>
                </div>
              </div>
              <div class="metropolis-select-icon" aria-hidden="true">
                <i class="fas fa-chevron-right"></i>
              </div>
            </div>
          </div>
        `).join('')}
      </div>

      <div class="metropolis-modal-footer">
        <button 
          onclick="closeMetropolisModal()" 
          class="close-metropolis-modal"
          aria-label="Close City Selection Modal"
        >
          Close
        </button>
      </div>
    </div>
  `;
  
  // Add styles
const style = document.createElement('style');
style.textContent = `
.metropolis-selection-modal {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(26, 26, 31, 0.95);
      z-index: 1500;
      display: flex;
      align-items: center;
      justify-content: center;
      backdrop-filter: blur(10px);
      animation: fadeIn 0.3s ease-out;
      overscroll-behavior: contain;
    }

    .metropolis-selection-modal:focus-within {
      outline: 2px solid var(--primary-color);
    }

    .metropolis-modal-content {
      width: 90%;
      max-width: 700px;
      background: #2a2a35;
      border-radius: 15px;
      box-shadow: 0 15px 30px rgba(0, 0, 0, 0.3);
      padding: 20px;
      max-height: 80vh;
      overflow-y: auto;
      border: 2px solid var(--primary-color);
      transition: all 0.3s ease;
      outline: none;
    }

  .metropolis-modal-header {
    text-align: center;
    margin-bottom: 20px;
    color: var(--primary-color);
  }

  .metropolis-modal-header h2 {
    margin-bottom: 10px;
    font-size: 1.8em;
    line-height: 1.2;
  }

  .metropolis-modal-header p {
    color: #aaa;
    font-size: 0.9em;
    line-height: 1.4;
  }

  .metropolis-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 15px;
    max-height: 60vh;
    overflow-y: auto;
  }

  .metropolis-card {
    transition: all 0.3s ease;
    background: rgba(255, 215, 0, 0.05);
    border-radius: 10px;
    cursor: pointer;
    border: 1px solid rgba(255, 215, 0, 0.2);
    overflow: hidden;
  }

  .metropolis-card:hover {
    transform: scale(1.02);
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
    border-color: var(--primary-color);
  }

  .metropolis-card-inner {
    display: flex;
    align-items: center;
    padding: 15px;
    gap: 15px;
  }

  .metropolis-icon {
    background: rgba(255, 215, 0, 0.1);
    width: 60px;
    height: 60px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: var(--primary-color);
    font-size: 1.5em;
  }

  .metropolis-details {
    flex-grow: 1;
  }

  .metropolis-details h3 {
    margin: 0 0 10px 0;
    color: var(--text-light);
    font-size: 1.1em;
    line-height: 1.3;
  }

  .metropolis-meta {
    display: flex;
    flex-direction: column;
    color: #aaa;
    font-size: 0.8em;
  }

  .locations-count {
    margin-bottom: 5px;
    color: var(--primary-color);
  }

  .categories-available {
    opacity: 0.7;
  }

  .metropolis-select-icon {
    color: var(--primary-color);
    opacity: 0.6;
    font-size: 1.2em;
  }

  .metropolis-modal-footer {
    margin-top: 20px;
    text-align: center;
  }

  .close-metropolis-modal {
    background: rgba(255, 215, 0, 0.1);
    color: var(--text-light);
    border: none;
    padding: 10px 20px;
    border-radius: 25px;
    cursor: pointer;
    transition: all 0.3s ease;
    font-size: 0.9em;
  }

  .close-metropolis-modal:hover {
    background: var(--primary-color);
    color: #1a1a1f;
    transform: scale(1.05);
  }

  @keyframes fadeIn {
    from {
      opacity: 0;
    }
    to {
      opacity: 1;
    }
  }

  @media (max-width: 768px) {
    .metropolis-modal-content {
      width: 95%;
      padding: 15px;
    }

    .metropolis-grid {
      grid-template-columns: 1fr;
    }

    .metropolis-icon {
      width: 50px;
      height: 50px;
      font-size: 1.2em;
    }
  }
`;
document.head.appendChild(style);
document.body.appendChild(modal);


  // Add keyboard trap and focus management
  const firstFocusableElement = modal.querySelector('.metropolis-card');
  const lastFocusableElement = modal.querySelector('.close-metropolis-modal');

  modal.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') {
      closeMetropolisModal();
    }

    // Keyboard trap
    if (e.key === 'Tab') {
      if (e.shiftKey && document.activeElement === firstFocusableElement) {
        lastFocusableElement.focus();
        e.preventDefault();
      } else if (!e.shiftKey && document.activeElement === lastFocusableElement) {
        firstFocusableElement.focus();
        e.preventDefault();
      }
    }
  });

  // Focus the first metropolis card when modal opens
  firstFocusableElement?.focus();
}

function handleMetropolisKeydown(event, metropolis) {
  // Allow selection via keyboard
  if (event.key === 'Enter' || event.key === ' ') {
    selectMetropolis(metropolis);
  }
}

function selectMetropolis(metropolis) {
  // More robust selection process
  if (!metropolis) return;
  
  // Close the modal with animation
  closeMetropolisModal();
  
  // Filter locators by selected metropolis
  const filteredData = locatorsData.filter(loc => 
    loc && loc.metropolis && loc.metropolis.trim() === metropolis
  );
  
  // Update the grid with filtered locators
  const locatorsGrid = document.getElementById('locatorsGrid');
  if (locatorsGrid) {
    locatorsGrid.innerHTML = renderLocatorCards(filteredData);
    
    // Announce the change for screen readers
    const announcement = document.createElement('div');
    announcement.setAttribute('role', 'status');
    announcement.setAttribute('aria-live', 'polite');
    announcement.textContent = `Showing locations for ${metropolis}`;
    document.body.appendChild(announcement);
    setTimeout(() => announcement.remove(), 3000);
  }
  
  // Update active filter buttons
  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => button.classList.remove('active'));
}

function closeMetropolisModal() {
  const modal = document.getElementById('metropolisSelectionModal');
  if (modal) {
    modal.style.opacity = '0';
    modal.style.transform = 'scale(0.9)';
    
    // Improved exit animation with callback
    setTimeout(() => {
      modal.remove();
      // Return focus to the element that opened the modal
      document.querySelector('[data-metropolis-trigger]')?.focus();
    }, 300);
  }
}

function filterLocators(value, filterType = 'category') {
  // Remove active class from all filter buttons
  const filterButtons = document.querySelectorAll('.filter-button');
  filterButtons.forEach(button => button.classList.remove('active'));

  // Add active class to the clicked button
  const activeButton = document.querySelector(`.filter-button[data-category="${value}"]`);
  if (activeButton) activeButton.classList.add('active');

  // Filter locators
  const locatorsGrid = document.getElementById('locatorsGrid');
  const filteredData = value === 'All' 
    ? locatorsData 
    : locatorsData.filter(loc => loc[filterType] === value);

  // Update grid with filtered results
  locatorsGrid.innerHTML = renderLocatorCards(filteredData);
}

function hideLocatorsOverlay() {
  const overlay = document.getElementById('locatorsOverlay');
  if (overlay) {
    overlay.classList.remove('active');
    setTimeout(() => overlay.remove(), 300);
  }
}

// Additional placeholder functions (you might want to implement these)
function selectLocator(category) {
  console.log(`Selected locator in category: ${category}`);
}

function showMenu(event, category) {
  event.stopPropagation();
  console.log(`Showing menu for category: ${category}`);
}

function showInfo(category) {
  console.log(`Showing info for category: ${category}`);
}

function showMenu(event, category) {
  event.stopPropagation();
  const locator = locatorsData.find(loc => loc.category === category);
  if (locator && locator.details.menu) {
    const menuContent = locator.details.menu.map(item => `
      <li>${item.name} - ${item.price}</li>
    `).join('');

    const notification = document.createElement('div');
    notification.className = 'menu-notification';
    notification.innerHTML = `
      <div class="menu-header">
        <h3>Menu for ${locator.details.name}</h3>
        <button onclick="closeNotification(this)">Close</button>
      </div>
      <ul>${menuContent}</ul>
    `;
    document.body.appendChild(notification);

    const notificationStyle = document.createElement('style');
    notificationStyle.textContent = `
      .menu-notification {
        position: fixed;
        top: 20%;
        left: 50%;
        transform: translate(-50%, -20%);
        background: #333;
        border-radius: 12px;
        box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        padding: 25px;
        z-index: 1100;
        width: 320px;
        max-width: 90%;
        transition: transform 0.3s ease-out, opacity 0.3s ease-out;
        outline: 4px solid #FFD700;
      }

      .menu-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
      }

      .menu-header h3 {
        margin: 0;
        font-size: 1.4em;
        color: #fff;
        font-weight: 600;
      }

      .menu-header button {
        background: none;
        border: none;
        color: #ff4d4d;
        cursor: pointer;
        font-size: 1.3em;
        transition: color 0.2s ease, transform 0.2s ease;
      }

      .menu-header button:hover {
        color: #ff1a1a;
        transform: scale(1.1);
      }

      ul {
        list-style: none;
        padding: 0;
        margin: 0;
      }

      li {
        margin-bottom: 8px;
        font-size: 1.1em;
        color: #ccc;
        transition: all 0.2s ease;
      }

      li:hover {
        color: #fff;
        background: rgba(255, 255, 255, 0.1);
        padding-left: 10px;
      }
    `;
    document.head.appendChild(notificationStyle);
  }
}

function showInfo(category) {
  const locator = locatorsData.find(loc => loc.category === category);
  if (locator) {
    const notification = document.createElement('div');
    notification.className = 'info-notification';
    notification.innerHTML = `
      <div class="info-header">
        <h3>${locator.details.name}</h3>
        <button onclick="closeNotification(this)">Close</button>
      </div>
      <p><strong>Description:</strong> ${locator.details.description}</p>
      <p><strong>Address:</strong> ${locator.details.address}</p>
      <p><strong>Timings:</strong> ${locator.details.timings}</p>
      <p><strong>Payments:</strong> ${locator.details.payment}</p>
      ${locator.details.menu ? `
        <div><strong>Menu:</strong>
          <ul>${locator.details.menu.map(item => `<li>${item.name} - ${item.price}</li>`).join('')}</ul>
        </div>` : ''}
    `;
    document.body.appendChild(notification);

    const notificationStyle = document.createElement('style');
    notificationStyle.textContent = `
      .info-notification {
        position: fixed;
        top: 20%;
        left: 50%;
        transform: translate(-50%, -20%);
        background: #333;
        border-radius: 12px;
        box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
        padding: 25px;
        z-index: 1100;
        width: 320px;
        max-width: 90%;
        transition: transform 0.3s ease-out, opacity 0.3s ease-out, box-shadow 0.3s ease-out;
        outline: 4px solid #FFD700;
      }

      .info-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 15px;
      }

      .info-header h3 {
        margin: 0;
        font-size: 1.4em;
        color: #fff;
        font-weight: 600;
        letter-spacing: 0.5px;
      }

      .info-header button {
        background: none;
        border: none;
        color: #ff4d4d;
        cursor: pointer;
        font-size: 1.3em;
        transition: color 0.2s ease, transform 0.2s ease;
      }

      .info-header button:hover {
        color: #ff1a1a;
        transform: scale(1.1);
      }

      ul {
        list-style: none;
        padding: 0;
        margin: 0;
      }

      li {
        margin-bottom: 8px;
        font-size: 1.1em;
        color: #ccc;
        transition: all 0.2s ease;
      }

      li:hover {
        color: #fff;
        background: rgba(255, 255, 255, 0.1);
        padding-left: 10px;
      }
    `;
    document.head.appendChild(notificationStyle);
  }
}

function closeNotification(button) {
  const notification = button.closest('.menu-notification, .info-notification');
  if (notification) {
    notification.style.opacity = '0';
    notification.style.transform = 'translate(-50%, -20%) scale(0.8)';
    setTimeout(() => {
      notification.remove();
    }, 300);
  }
}

function selectLocator(category) {
  // Optional: Add any specific action when a locator card is clicked
  console.log(`Selected locator in category: ${category}`);
}

document.addEventListener('DOMContentLoaded', function() {
  // Attach click event to town button if it exists
  const townButton = document.querySelector('.town-button#townButton');
  if (townButton) {
    townButton.addEventListener('click', function(e) {
      e.preventDefault();
      showLocatorsOverlay();
    });
  }
});

const reserveData=[{category:"Venue",items:[{name:"Royal Wedding Hall",city:"Udaipur",image:"http://www.udaipurweddings.com/wp-content/uploads/2017/11/13-3.jpg",description:"Elegant venue for grand celebrations",price:"₹50,000 - ₹1,00,000",details:{capacity:"500-1000 guests",facilities:["AC","Stage","Parking","Catering Area"],amenities:["Sound System","Decoration Services","Bridal Room"]},whatsappNumber:"+917869809022"},{name:"Riverside Resort",city:"Jaipur",image:"https://di5fgdew4nptq.cloudfront.net/api2/media/images/140a5288-3ccf-eb11-80dd-f8bc124783a3",description:"Scenic location for intimate gatherings",price:"₹30,000 - ₹75,000",details:{capacity:"100-300 guests",facilities:["Open Area","River View","Garden"],amenities:["Accommodation","Bonfire Area","Photography Spots"]},whatsappNumber:"+917869809022"}]},{category:"Entertainment",items:[{name:"Traditional Dance Troupe",city:"Udaipur",image:"https://example.com/dance-troupe.jpg",description:"Authentic cultural performance",price:"₹25,000 - ₹50,000",details:{duration:"1-2 hours",performers:"5-10 artists",styles:["Bharatanatyam","Kathak","Folk Dances"]},whatsappNumber:"+917869809022"},{name:"Live Musical Ensemble",city:"Jaipur",image:"https://example.com/music-ensemble.jpg",description:"Professional live music band",price:"₹35,000 - ₹75,000",details:{duration:"2-3 hours",musicians:"4-6 members",genres:["Classical","Bollywood","Fusion"]},whatsappNumber:"+917869809022"}]},{category:"Pooja Services",items:[{name:"Traditional Wedding Rituals",city:"Udaipur",image:"https://example.com/wedding-rituals.jpg",description:"Complete wedding ceremony arrangement",price:"₹1,00,000 - ₹3,00,000",details:{services:["Priest","Mandap Decoration","Ritual Accessories"],duration:"Full Day",includes:["Vedic Ceremonies","Customized Rituals"]},whatsappNumber:"+917869809022"}]}];

function showLocationOverlay(onCitySelect) {
    // Create location overlay
    const locationOverlay = document.createElement('div');
    locationOverlay.id = 'locationOverlay';
    locationOverlay.className = 'location-overlay';

    // Get unique cities
    const cities = [...new Set(reserveData.flatMap(category => 
        category.items.map(item => item.city)
    ))];

    locationOverlay.innerHTML = `
        <div class="location-content">
            <div class="location-header">
                <h2>Select Your City</h2>
                <button class="close-location" onclick="hideLocationOverlay()">
                    <i class="fas fa-long-arrow-alt-left"></i>
                </button>
            </div>
            
            <div class="cities-grid">
                ${cities.map(city => `
                    <div class="city-card" onclick="selectCity('${city}')">
                        <h3>${city}</h3>
                    </div>
                `).join('')}
            </div>
        </div>
    `;

    document.body.appendChild(locationOverlay);

    // Add styles for location overlay
    const style = document.createElement('style');
    style.textContent = `
.location-overlay { position: fixed; top: 0; left: 0; right: 0; bottom: 0; background: rgba(26, 26, 31, 0.98); z-index: 1100; opacity: 0; visibility: hidden; transition: all 0.3s ease; overflow-y: auto; } .location-overlay.active { opacity: 1; visibility: visible; } .location-content { padding: 20px; max-width: 1200px; margin: 0 auto; } .location-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 20px; padding-bottom: 15px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); } .close-location { background: none; border: none; color: #FFD700; cursor: pointer; font-size: 24px; } .cities-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); gap: 20px; } .city-card { background: rgba(255, 215, 0, 0.1); border-radius: 12px; padding: 20px; text-align: center; cursor: pointer; transition: all 0.3s ease; } .city-card:hover { background: var(--primary-color); color: black; }
    `;
    document.head.appendChild(style);

    // Window method to select city
    window.selectCity = (city) => {
        hideLocationOverlay();
        onCitySelect(city);
    };

    // Activate overlay
    setTimeout(() => {
        locationOverlay.classList.add('active');
    }, 10);
}

function hideLocationOverlay() {
    const overlay = document.getElementById('locationOverlay');
    if (overlay) {
        overlay.classList.remove('active');
        setTimeout(() => overlay.remove(), 300);
    }
}

function showReserveOverlay(selectedCity = null) {
    // Create overlay container
    let overlay = document.createElement('div');
    overlay.id = 'reserveOverlay';
    overlay.className = 'reserve-overlay';
    document.body.appendChild(overlay);
    
    // Generate filter buttons
    const categories = ['All', ...new Set(reserveData.flatMap(category => category.category))];
    const filterButtons = categories.map(cat => 
        `<button class="filter-btn" data-category="${cat}">${cat}</button>`
    ).join('');

    function searchReserveItems(searchTerm, category = 'All', city = null) {
    // Normalize search term
    const normalizedSearchTerm = searchTerm.toLowerCase().trim();

    // Advanced search parsing
    const searchContext = {
        city: null,
        priceRange: null,
        intent: null,
        keywords: []
    };

    // Price range detection regex
    const priceRangeRegex = /(?:under|below|max|maximum)\s*(\d+(?:,\d{3})*)/i;
    const priceMatch = normalizedSearchTerm.match(priceRangeRegex);

    // City detection
    const cityKeywords = [
        'in', 'at', 'near', 'around', 'at location'
    ];
    const cityRegex = new RegExp(`(${cityKeywords.join('|')})\\s+([a-zA-Z\\s]+)`, 'i');
    const cityMatch = normalizedSearchTerm.match(cityRegex);

    // Intents and their associated keywords
    const intents = [
        { 
            name: 'wedding', 
            keywords: ['wedding', 'marriage', 'ceremony', 'wedding hall', 'wedding venue']
        },
        { 
            name: 'resort', 
            keywords: ['resort', 'holiday', 'vacation', 'stay', 'accommodation']
        },
        { 
            name: 'cultural', 
            keywords: ['folk dance', 'cultural', 'tradition', 'performance', 'art']
        }
    ];

    // Price range extraction
    if (priceMatch) {
        searchContext.priceRange = parseInt(priceMatch[1].replace(',', ''));
    }

    // City extraction
    if (cityMatch) {
        searchContext.city = cityMatch[2].trim();
    } else if (city) {
        searchContext.city = city;
    }

    // Intent and keyword extraction
    intents.forEach(intent => {
        const matchedIntent = intent.keywords.some(keyword => 
            normalizedSearchTerm.includes(keyword)
        );
        
        if (matchedIntent) {
            searchContext.intent = intent.name;
        }
    });

    // Extract remaining keywords (remove price, city, and intent keywords)
    const keywordsToRemove = [
        ...cityKeywords,
        ...(searchContext.intent ? intents.find(i => i.name === searchContext.intent).keywords : []),
        ...(priceMatch ? [priceMatch[0]] : []),
        ...(cityMatch ? [cityMatch[0]] : [])
    ];

    searchContext.keywords = normalizedSearchTerm
        .split(/\s+/)
        .filter(word => 
            word.length > 2 && 
            !keywordsToRemove.some(removeWord => word.includes(removeWord))
        );

    // Search and filter function
    const searchFilter = (item) => {
        // Define searchable fields with additional nested search
        const searchFields = [
            item.name.toLowerCase(),
            item.city.toLowerCase(),
            item.description.toLowerCase(),
            item.price.toLowerCase(),
            ...(item.details.facilities || []).map(f => f.toLowerCase()),
            ...(item.details.amenities || []).map(a => a.toLowerCase()),
            ...Object.values(item.details).flatMap(val => 
                Array.isArray(val) 
                    ? val.map(String).map(v => v.toLowerCase()) 
                    : [String(val).toLowerCase()]
            )
        ];

        // City filtering
        if (searchContext.city && !item.city.toLowerCase().includes(searchContext.city.toLowerCase())) {
            return false;
        }

        // Price range filtering
        if (searchContext.priceRange) {
            const itemPrice = parseFloat(item.price.replace(/[^\d.]/g, ''));
            if (itemPrice > searchContext.priceRange) {
                return false;
            }
        }

        // Intent filtering
        if (searchContext.intent) {
            const intentMatched = intents
                .find(i => i.name === searchContext.intent)
                .keywords.some(keyword => 
                    searchFields.some(field => field.includes(keyword))
                );
            
            if (!intentMatched) {
                return false;
            }
        }

        // Keyword matching
        const keywordsMatched = searchContext.keywords.length === 0 || 
            searchContext.keywords.some(keyword => 
                searchFields.some(field => field.includes(keyword))
            );

        return keywordsMatched;
    };

    // Apply search filter to items
    let items = reserveData.flatMap(cat => cat.items);

    // Category filtering if not 'All'
    if (category !== 'All') {
        items = items.filter(item => 
            reserveData.find(cat => cat.category === category)?.items.includes(item)
        );
    }

    // Final item filtering
    return items.filter(searchFilter);
}

    // Generate reserve items HTML with search support
    const generateReserveItems = (category = 'All', city = null, searchTerm = '') => {
        let items = searchTerm 
            ? searchReserveItems(searchTerm, category, city) 
            : searchReserveItems('', category, city);

        // Display "No results" if no items found
        if (items.length === 0) {
            return `
                <div class="no-results">
                    <i class="fas fa-search"></i>
                    <h3>No Results Found</h3>
                    <p>Try a different search term or category</p>
                </div>
            `;
        }

        return items.map(item => `
            <div class="reserve-card" data-category="${item.category}" data-city="${item.city}">
                <div class="reserve-image-container">
                    <img src="${item.image}" alt="${item.name}" class="reserve-image">
                    <div class="reserve-image-overlay">
                        <span class="city-tag">${item.city}</span>
                    </div>
                </div>
                <div class="reserve-info">
                    <h3 class="reserve-name">${item.name}</h3>
                    <p class="reserve-description">${item.description}</p>
                    <div class="reserve-price">${item.price}</div>
                    <div class="reserve-actions">
                        <button class="book-now-btn" onclick="showBookingForm('${item.name}')">Book Now</button>
                        <button class="chat-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                            <i class="fab fa-whatsapp"></i> Chat
                        </button>
                        <button class="info-btn" onclick="showItemDetails('${encodeURIComponent(JSON.stringify(item))}')">
                            <i class="fas fa-info-circle"></i>
                        </button>
                    </div>
                </div>
            </div>
        `).join('');
    };

    // Overlay content
    overlay.innerHTML = `
        <div class="reserve-content">
            <div class="reserve-header">
                <h2>Deep Reserve</h2>
                <div class="header-actions">
                    <button class="location-btn" onclick="showLocationOverlay(filterByCity)">
                        <i class="fas fa-map-marker-alt"></i> 
                        ${selectedCity || 'Select City'}
                    </button>
                    <button class="close-reserve" onclick="hideReserveOverlay()">
                        <i class="fas fa-long-arrow-alt-left"></i>
                    </button>
                </div>
            </div>
            
            <div class="filter-container">
                <div class="filter-buttons">
                    ${filterButtons}
                </div>
            </div>

            <div class="reserve-grid-container">
                <div class="reserve-grid" id="reserveItemsContainer">
                    ${generateReserveItems('All', selectedCity)}
                </div>
            </div>

            <!-- Chat Input Container with Search -->
            <div class="reserve-input-container" style="margin-top: 0; padding-bottom: 60px;">
                <input type="text" id="searchInput" placeholder="Search for services or locations..." />
                <div class="input-actions">
                    <button class="send-message" id="searchButton">
                        <i class="fas fa-paper-plane"></i>
                    </button>
                </div>
            </div>

            <!-- Bottom Navigation -->
            <nav class="bottom-nav">
                <div class="nav-container">
                    <div class="nav-item" data-page="services">
                           <i class="fas fa-user-tie"></i>
                        <span>Services</span>
                    </div>
                    <div class="nav-item" data-page="reserve">
                            <i class="fas fa-calendar-check"></i>
                        <span>Reserve</span>
                    </div>
                    <div class="nav-item whatsapp-item" data-page="chat">
                        <a href="https://wa.me/9111478356" target="_blank">
                            <i class="fas fa-message"></i>
                        </a>
                        <span>WhatsApp</span>
                    </div>
                    <div class="nav-item" data-page="slot">
                            <i class="fas fa-calendar-alt"></i>
                     <span>Slot</span>
                   </div>
                   <div class="nav-item" data-page="franchise">
                            <i class="fas fa-store"></i>
                        <span>Franchise</span>
                    </div>
                </div>
            </nav>
        </div>
    `;

    setTimeout(() => {
    overlay.classList.add('active');
    const searchInput = document.getElementById('searchInput');
    const searchButton = document.getElementById('searchButton');
    const filterBtns = overlay.querySelectorAll('.filter-btn');

    // Debounce function to prevent too frequent searches
    const debounce = (func, delay) => {
        let debounceTimer;
        return function() {
            const context = this;
            const args = arguments;
            clearTimeout(debounceTimer);
            debounceTimer = setTimeout(() => func.apply(context, args), delay);
        }
    };

    // Search button click handler
    searchButton.addEventListener('click', performSearch);

    // Enter key press handler
    searchInput.addEventListener('keypress', (e) => {
        if (e.key === 'Enter') {
            performSearch();
        }
    });

    // Optional: Add live search with debounce
    searchInput.addEventListener('input', debounce(function() {
        if (this.value.trim().length > 2) {
            performSearch();
        }
    }, 300));

    // Search function with advanced handling
    function performSearch() {
        const searchTerm = searchInput.value.trim();
        const activeCategory = overlay.querySelector('.filter-btn.active')?.dataset.category || 'All';
        const currentCity = overlay.querySelector('.location-btn').textContent.trim().replace('Select City', '');

        // Get search results using the advanced search function
        const searchResults = searchReserveItems(
            searchTerm, 
            activeCategory, 
            currentCity === '' ? null : currentCity
        );

        // Update results display
        const reserveItemsContainer = document.getElementById('reserveItemsContainer');
        
        if (searchResults.length === 0) {
            // No results handling
            reserveItemsContainer.innerHTML = `
                <div class="no-results">
                    <i class="fas fa-search"></i>
                    <h3>No Results Found</h3>
                    <p>Try a different search term or adjust your filters</p>
                    ${searchTerm ? `<small>Search: "${searchTerm}"</small>` : ''}
                </div>
            `;
        } else {
            // Render search results
            reserveItemsContainer.innerHTML = searchResults.map(item => `
                <div class="reserve-card" data-category="${item.category}" data-city="${item.city}">
                    <div class="reserve-image-container">
                        <img src="${item.image}" alt="${item.name}" class="reserve-image">
                        <div class="reserve-image-overlay">
                            <span class="city-tag">${item.city}</span>
                        </div>
                    </div>
                    <div class="reserve-info">
                        <h3 class="reserve-name">${item.name}</h3>
                        <p class="reserve-description">${item.description}</p>
                        <div class="reserve-price">${item.price}</div>
                        <div class="reserve-actions">
                            <button class="book-now-btn" onclick="showBookingForm('${item.name}')">Book Now</button>
                            <button class="chat-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                                <i class="fab fa-whatsapp"></i> Chat
                            </button>
                            <button class="info-btn" onclick="showItemDetails('${encodeURIComponent(JSON.stringify(item))}')">
                                <i class="fas fa-info-circle"></i>
                            </button>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        // Optional: Add search term highlight
        if (searchTerm) {
            const searchTermHighlight = (text) => {
                const regex = new RegExp(`(${searchTerm})`, 'gi');
                return text.replace(regex, '<mark>$1</mark>');
            };

            document.querySelectorAll('.reserve-name, .reserve-description').forEach(el => {
                el.innerHTML = searchTermHighlight(el.textContent);
            });
        }
    }

    // Filter buttons event listeners
    filterBtns.forEach(btn => {
        btn.addEventListener('click', function() {
            // Remove active class from all buttons
            filterBtns.forEach(b => b.classList.remove('active'));
            this.classList.add('active');

            // Filter items
            const category = this.dataset.category;
            const currentCity = overlay.querySelector('.location-btn').textContent.trim().replace('Select City', '');
            const searchTerm = searchInput.value.trim();

            // Get search results using the advanced search function
            const searchResults = searchReserveItems(
                searchTerm, 
                category, 
                currentCity === '' ? null : currentCity
            );

            // Update results display (similar to performSearch logic)
            const reserveItemsContainer = document.getElementById('reserveItemsContainer');
            
            if (searchResults.length === 0) {
                reserveItemsContainer.innerHTML = `
                    <div class="no-results">
                        <i class="fas fa-search"></i>
                        <h3>No Results Found</h3>
                        <p>Try a different category or filter</p>
                    </div>
                `;
            } else {
                reserveItemsContainer.innerHTML = searchResults.map(item => `
                    <div class="reserve-card" data-category="${item.category}" data-city="${item.city}">
                        <div class="reserve-image-container">
                            <img src="${item.image}" alt="${item.name}" class="reserve-image">
                            <div class="reserve-image-overlay">
                                <span class="city-tag">${item.city}</span>
                            </div>
                        </div>
                        <div class="reserve-info">
                            <h3 class="reserve-name">${item.name}</h3>
                            <p class="reserve-description">${item.description}</p>
                            <div class="reserve-price">${item.price}</div>
                            <div class="reserve-actions">
                                <button class="book-now-btn" onclick="showBookingForm('${item.name}')">Book Now</button>
                                <button class="chat-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                                    <i class="fab fa-whatsapp"></i> Chat
                                </button>
                                <button class="info-btn" onclick="showItemDetails('${encodeURIComponent(JSON.stringify(item))}')">
                                    <i class="fas fa-info-circle"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `).join('');
            }
        });
    });
}, 10);

    // Define a method to filter by city
    window.filterByCity = (city) => {
        const titleElement = overlay.querySelector('.location-btn');
        titleElement.innerHTML = `<i class="fas fa-map-marker-alt"></i> ${city}`;
        
        // Regenerate items with city filter
        document.getElementById('reserveItemsContainer').innerHTML = generateReserveItems('All', city);

        // Update filter buttons state
        const filterBtns = overlay.querySelectorAll('.filter-btn');
        filterBtns.forEach(btn => btn.classList.remove('active'));
        filterBtns[0].classList.add('active'); // 'All' category
    };

// Add styles
const style = document.createElement('style');
style.textContent = `
.reserve-overlay { 
    position: fixed; 
    top: 0; 
    left: 0; 
    right: 0; 
    bottom: 0; 
    background: #1a1a1f; 
    z-index: 1000; 
    opacity: 0; 
    visibility: hidden; 
    transition: all 0.3s ease; 
    display: flex; 
    flex-direction: column; 
    width: 100%; 
} 
.reserve-overlay.active { 
    opacity: 1; 
    visibility: visible; 
} 
.reserve-content { 
    flex: 1; 
    display: flex; 
    flex-direction: column; 
    overflow: hidden; 
    width: 100vw; 
    max-width: 100vw; 
    padding: 20px; 
    margin: 0; 
    box-sizing: border-box; 
    align-self: center; 
} 
.reserve-header { 
    padding: 10px 20px; 
    background: rgba(26, 26, 31, 0.9); 
    position: fixed; 
    width: 100%; 
    top: 0; 
    left: 0; 
    z-index: 30; 
    border-bottom: 2px solid var(--primary-color); 
    backdrop-filter: blur(12px); 
    height: 70px; 
    display: flex; 
    align-items: center; 
    justify-content: space-between; 
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); 
    transition: background 0.3s, height 0.3s; 
} 
.header:hover { 
    background: rgba(26, 26, 31, 0.85); 
    height: 72px; 
} 
.close-reserve { 
    background: none; 
    border: none; 
    color: #FFD700; 
    cursor: pointer; 
    font-size: 24px; 
} 
.filter-container { 
    width: 100vw; 
    max-width: 100vw; 
    margin-left: calc(-50vw + 50%); 
    padding: 0 15px; 
    margin-top: 60px; 
    box-sizing: border-box; 
} 
.filter-container::-webkit-scrollbar { 
    display: none; 
} 
@media (max-width: 768px) { 
    .reserve-content, .filter-container { 
        width: 100vw; 
        margin-left: calc(-50vw + 50%); 
        padding: 10px; 
    } 
} 
.filter-buttons { 
    display: flex; 
    justify-content: flex-start; 
    gap: 4px; 
    padding-bottom: 10px; 
    width: 100vw; 
    margin-left: calc(-50vw + 50%); 
    overflow-x: auto; 
    scrollbar-width: none; 
} 
.filter-buttons::-webkit-scrollbar { 
    display: none; 
} 
.filter-btn { 
    margin-top: 2px; 
    margin-left: 2px; 
    margin-right: 5px; 
    flex-shrink: 0; 
    background: rgba(255, 215, 0, 0.1); 
    color: var(--text-light); 
    border: 1px solid rgba(255, 215, 0, 0.2); 
    padding: 10px 15px; 
    border-radius: 25px; 
    transition: all 0.3s ease; 
    cursor: pointer; 
    white-space: nowrap; 
    font-weight: 500; 
    box-shadow: 0 2px 5px rgba(0,0,0,0.1); 
} 
.filter-btn:hover { 
    background: var(--primary-color); 
    color: black; 
    transform: scale(1.05); 
    box-shadow: 0 4px 8px rgba(0,0,0,0.2); 
} 
.filter-btn:first-child { 
    margin-left: 20px; 
} 
.filter-btn.active { 
    background: var(--primary-color); 
    color: black; 
    font-weight: 700; 
    box-shadow: 0 4px 10px rgba(0,0,0,0.3); 
} 
.reserve-grid-container { 
    flex-grow: 1; 
    overflow-y: scroll; 
    padding-bottom: 20px; 
    scrollbar-width: none; 
} 
.reserve-grid-container::-webkit-scrollbar { 
    display: none; 
} 
.reserve-grid { 
    margin-top: 10px; 
    display: grid; 
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); 
    gap: 20px; 
    animation: fadeInUp 0.5s ease-out; 
    padding-bottom: 20px; 
} 
.reserve-card { 
    background: rgba(255, 215, 0, 0.1); 
    border-radius: 12px; 
    overflow: hidden; 
    transition: all 0.3s ease; 
} 
.reserve-image-container { 
    height: 200px; 
    overflow: hidden; 
    position: relative; 
} 
.reserve-image { 
    width: 100%; 
    height: 100%; 
    object-fit: cover; 
    transition: transform 0.3s ease; 
} 
.reserve-card:hover .reserve-image { 
    transform: scale(1.1); 
} 
.reserve-info { 
    padding: 15px; 
    color: var(--text-light); 
} 
.reserve-actions { 
    display: flex; 
    justify-content: space-between; 
    margin-top: 15px; 
} 
.book-now-btn, .chat-btn, .info-btn { 
    background: var(--primary-color); 
    border: none; 
    padding: 10px 15px; 
    border-radius: 5px; 
    cursor: pointer; 
    transition: all 0.3s ease; 
} 
.header-actions { 
    display: flex; 
    align-items: center; 
    gap: 15px; 
} 
.location-btn { 
    background: rgba(255, 215, 0, 0.1); 
    color: var(--text-light); 
    border: none; 
    padding: 10px 15px; 
    border-radius: 20px; 
    cursor: pointer; 
    display: flex; 
    align-items: center; 
    gap: 10px; 
    transition: all 0.3s ease; 
} 
.location-btn:hover { 
    background: var(--primary-color); 
    color: black; 
} 
@media (max-width: 768px) { 
    .reserve-grid { 
        grid-template-columns: 1fr; 
    } 
} 
.reserve-input-container {
    margin-bottom: 2px;
    padding: 20px;
    background: rgba(26, 26, 31, 0.98);
    border-top: 1px solid rgba(255, 215, 0, 0.1);
    display: flex;
    gap: 12px;
    align-items: center;
    
    /* Desktop and laptop styles */
    @media (min-width: 769px) {
        width: 60%;
        margin-left: auto;
        margin-right: auto;
    }
    
    /* Mobile styles (full width) */
    @media (max-width: 768px) {
        width: 100%;
        padding: 10px;
    }
}

#searchInput {
    flex: 1;
    background: rgba(255, 255, 255, 0.05);
    border: 1px solid rgba(255, 215, 0, 0.2);
    border-radius: 12px;
    padding: 20px 20px;
    color: var(--text-light);
    font-size: 0.95em;
    
    /* Desktop and laptop styles */
    @media (min-width: 769px) {
        width: 100%;
    }
    
    /* Mobile styles (full width) */
    @media (max-width: 768px) {
        width: 100%;
    }
}
    
    .no-results { 
    text-align: center; 
    padding: 50px 20px; 
    color: var(--text-light); 
    grid-column: 1 / -1; 
} 
.no-results i { 
    font-size: 64px; 
    color: var(--primary-color); 
    margin-bottom: 20px; 
} 
.no-results h3 { 
    margin: 10px 0; 
    font-size: 24px; 
} 
.no-results p { 
    color: rgba(255, 255, 255, 0.7); 
} 
`;
document.head.appendChild(style);
}

function hideReserveOverlay() {
    const overlay = document.getElementById('reserveOverlay');
    if (overlay) {
        overlay.classList.remove('active');
        setTimeout(() => overlay.remove(), 300);
    }
}

function showBookingForm(itemName) {
    // Create booking form overlay
    const formOverlay = document.createElement('div');
    formOverlay.id = 'bookingFormOverlay';
    formOverlay.className = 'booking-form-overlay';
    formOverlay.innerHTML = `
        <div class="booking-form-content">
            <h2>Book ${itemName}</h2>
            <form id="bookingForm">
                <input type="text" name="Name" placeholder="Full Name" required>
                <input type="tel" name="Phone" placeholder="Phone Number" required>
                <input type="email" name="Email" placeholder="Email Address" required>
                <input type="date" name="Date" placeholder="Preferred Date" required>
                <textarea name="Additional Requirements" placeholder="Additional Requirements"></textarea>
                <button type="submit">Book Now</button>
            </form>
            <button class="close-form" onclick="closeBookingForm()">Close</button>
        </div>
    `;
    document.body.appendChild(formOverlay);

    // Form submission handler
    const form = formOverlay.querySelector('form');
    form.addEventListener('submit', function(e) {
        e.preventDefault();
        const formData = new FormData(form);
        const whatsappMessage = `Booking Request for ${itemName}\n` +
            `Name: ${formData.get('Name')}\n` +
            `Phone: ${formData.get('Phone')}\n` +
            `Email: ${formData.get('Email')}\n` +
            `Date: ${formData.get('Date')}\n` +
            `Additional Details: ${formData.get('Additional Requirements')}`;
        
        // Redirect to WhatsApp with pre-filled message
        window.open(`https://wa.me/7869809022?text=${encodeURIComponent(whatsappMessage)}`, '_blank');
    });

    // Add form styles
    const style = document.createElement('style');
    style.textContent = `
.booking-form-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.9); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 20px; box-sizing: border-box; } .booking-form-content { background: rgba(255, 255, 255, 0.1); border-radius: 15px; max-width: 500px; width: 100%; padding: 30px; color: var(--text-light); backdrop-filter: blur(10px); border: 1px solid rgba(255, 215, 0, 0.2); } .booking-form-content h2 { text-align: center; margin-bottom: 20px; color: var(--primary-color); } #bookingForm { display: flex; flex-direction: column; gap: 15px; } #bookingForm input, #bookingForm textarea { padding: 10px; background: rgba(255, 255, 255, 0.1); border: 1px solid rgba(255, 215, 0, 0.2); color: var(--text-light); border-radius: 5px; } #bookingForm button[type="submit"] { background: var(--primary-color); color: black; border: none; padding: 12px; border-radius: 5px; cursor: pointer; transition: background 0.3s ease; } .close-form { width: 100%; margin-top: 15px; padding: 10px; background: rgba(255, 0, 0, 0.1); color: var(--text-light); border: none; border-radius: 5px; cursor: pointer; }
    `;
    document.head.appendChild(style);
}

function closeBookingForm() {
    const formOverlay = document.getElementById('bookingFormOverlay');
    if (formOverlay) {
        formOverlay.remove();
    }
}

function showItemDetails(encodedItem) {
    const item = JSON.parse(decodeURIComponent(encodedItem));
    const detailsOverlay = document.createElement('div');
    detailsOverlay.id = 'itemDetailsOverlay';
    detailsOverlay.className = 'item-details-overlay';
    
    // Function to format details in a more readable way
    const formatDetails = (details) => {
        return Object.entries(details).map(([key, value]) => {
            // Convert arrays to comma-separated strings
            const formattedValue = Array.isArray(value) 
                ? value.join(', ') 
                : (typeof value === 'object' ? JSON.stringify(value) : value);
            
            // Convert camelCase to Title Case
            const formattedKey = key
                .replace(/([A-Z])/g, ' $1')
                .replace(/^./, str => str.toUpperCase());
            
            return `
                <div class="detail-item">
                    <strong>${formattedKey}:</strong> 
                    <span>${formattedValue}</span>
                </div>
            `;
        }).join('');
    };

    detailsOverlay.innerHTML = `
        <div class="item-details-content">
            <div class="details-header">
                <h2>${item.name}</h2>
                <button class="close-btn" onclick="this.closest('.item-details-overlay').remove()">
                    &times;
                </button>
            </div>
            
            <div class="details-image-container">
                <img src="${item.image}" alt="${item.name}" class="details-image">
            </div>
            
            <div class="details-info">
                <div class="details-section description">
                    <h3>Description</h3>
                    <p>${item.description}</p>
                </div>
                
                <div class="details-section price">
                    <h3>Price Range</h3>
                    <p>${item.price}</p>
                </div>
                
                <div class="details-section additional-details">
                    <h3>Additional Details</h3>
                    <div class="details-grid">
                        ${formatDetails(item.details)}
                    </div>
                </div>
                
                <div class="details-section contact">
                    <h3>Contact</h3>
                    <button class="whatsapp-btn" onclick="openWhatsApp('${item.whatsappNumber}')">
                        <i class="fab fa-whatsapp"></i> Chat on WhatsApp
                    </button>
                </div>
            </div>
        </div>
    `;

    // Add inline styles for improved presentation
    const style = document.createElement('style');
    style.textContent = `
.item-details-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.9); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 20px; box-sizing: border-box; overflow: auto; } .item-details-content { background: rgba(255, 255, 255, 0.1); border-radius: 15px; max-width: 800px; width: 100%; max-height: 90vh; overflow-y: auto; color: var(--text-light); position: relative; backdrop-filter: blur(10px); border: 1px solid rgba(255, 215, 0, 0.2); } .details-header { display: flex; justify-content: space-between; align-items: center; padding: 20px; border-bottom: 1px solid rgba(255, 215, 0, 0.2); } .details-header h2 { margin: 0; font-size: 24px; } .close-btn { background: none; border: none; color: var(--text-light); font-size: 36px; cursor: pointer; line-height: 1; } .details-image-container { width: 100%; height: 400px; overflow: hidden; } .details-image { width: 100%; height: 100%; object-fit: cover; } .details-info { padding: 20px; } .details-section { margin-bottom: 20px; } .details-section h3 { border-bottom: 2px solid var(--primary-color); padding-bottom: 10px; margin-bottom: 15px; } .details-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 15px; } .detail-item { background: rgba(255, 215, 0, 0.1); padding: 15px; border-radius: 8px; } .detail-item strong { display: block; margin-bottom: 5px; color: var(--primary-color); } .whatsapp-btn { background: #25D366; color: white; border: none; padding: 10px 15px; border-radius: 5px; display: flex; align-items: center; gap: 10px; cursor: pointer; } .whatsapp-btn i { font-size: 20px; } @media (max-width: 768px) { .details-image-container { height: 250px; } .details-header h2 { font-size: 20px; } .details-grid { grid-template-columns: 1fr; } }
    `;
    document.head.appendChild(style);
    document.body.appendChild(detailsOverlay);
}

function openWhatsApp(number) {
    window.open(`https://wa.me/${number}`, '_blank');
}

// Ensure search functionality is setup when reserve page is accessed
document.addEventListener('DOMContentLoaded', function() {
    const reserveNavItem = document.querySelector('.nav-item[data-page="reserve"]');
    if (reserveNavItem) {
        reserveNavItem.addEventListener('click', function(e) {
            e.preventDefault();
            showReserveOverlay();
        });
    }
});

function submitBookingViaWhatsApp() { const name = document.getElementById('nameInput').value.trim(); const contact = document.getElementById('contactInput').value.trim(); const address = document.getElementById('addressInput').value.trim(); const pincode = document.getElementById('pincodeInput').value.trim(); const date = document.getElementById('dateInput').value.trim(); const time = document.getElementById('timeInput').value.trim(); const bookingForm = document.getElementById('bookingForm'); const categoryId = bookingForm.dataset.categoryId || 'Not Specified'; const serviceName = bookingForm.dataset.serviceName || 'Not Specified'; const packageType = bookingForm.dataset.packageType || 'Not Specified'; if (!name || !contact) { showAlert('Please fill in Name and Contact Number', 'error'); return; } const message = `New Booking Request:\n📋 Category: ${categoryId}\n🛠 Service: ${serviceName}\n💎 Package: ${packageType}\n👤 Name: ${name}\n📞 Contact: ${contact}\n🏠 Address: ${address || 'Not Provided'}\n📍 Pincode: ${pincode || 'Not Provided'}\n📅 Date: ${date}\n⏰ Time: ${time}\n\nPlease confirm and process this booking.`; const encodedMessage = encodeURIComponent(message); const whatsappNumber = '78698 09022'; const whatsappUrl = `https://wa.me/${whatsappNumber.replace(/\s/g, '')}?text=${encodedMessage}`; window.open(whatsappUrl, '_blank'); showBookingConfirmation(); } function showBookingConfirmation() { const confirmationModal = document.createElement('div'); confirmationModal.id = 'bookingConfirmation'; confirmationModal.className = 'modal-overlay'; confirmationModal.innerHTML = `<div class="modal-content"><div class="confirmation-icon"><i class="fas fa-check-circle"></i></div><h2>Booking Submitted!</h2><p>Your booking request has been sent to WhatsApp. Our team will contact you shortly.</p><button onclick="closeConfirmation()" class="action-button">Close</button></div>`; document.body.appendChild(confirmationModal); document.getElementById('bookingForm').reset(); } function closeConfirmation() { const confirmationModal = document.getElementById('bookingConfirmation'); if (confirmationModal) { confirmationModal.remove(); } } document.addEventListener('DOMContentLoaded', function() { const submitBookingButton = document.getElementById('submitBooking'); if (submitBookingButton) { submitBookingButton.addEventListener('click', submitBookingViaWhatsApp); } }); document.addEventListener('DOMContentLoaded', function() { const locationNav = document.querySelector('.nav-item[data-page="location"]'); if (locationNav) { locationNav.addEventListener('click', function(e) { e.preventDefault(); showCitiesOverlay(); }); } }); function showAlert(message, type = 'success') { const alert = document.createElement('div'); alert.className = `alert alert-${type}`; alert.textContent = message; document.body.appendChild(alert); setTimeout(() => { alert.remove(); }, 3000); } function setupEventListeners() { window.addEventListener('click', function(event) { if (event.target.classList.contains('modal-overlay')) { closePackageInfo(); closeBookingForm(); closeConfirmation(); } }); }
                            </script>
