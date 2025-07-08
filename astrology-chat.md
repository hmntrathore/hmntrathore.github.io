---
title: "Astrology Chat App"
description: "AI-powered astrology chat application with OpenAI integration"
layout: default
---

# Astrology Chat App

Welcome to the AI-powered Astrology Chat App! Ask questions about your zodiac sign, horoscope, or any astrology-related topics, and get personalized insights from our AI astrology expert.

<div id="chat-container">
  <div id="chat-header">
    <h3>🌟 Astrology AI Assistant</h3>
    <p>Ask me anything about astrology, horoscopes, or zodiac signs!</p>
  </div>
  
  <div id="chat-messages">
    <div class="message ai-message">
      <div class="message-content">
        <strong>AI Astrologer:</strong> Hello! I'm your AI astrology assistant. I can help you with:
        <ul>
          <li>Daily horoscopes</li>
          <li>Zodiac sign compatibility</li>
          <li>Birth chart insights</li>
          <li>Astrological guidance</li>
        </ul>
        What would you like to know about astrology today?
      </div>
    </div>
  </div>
  
  <div id="chat-input-area">
    <div id="input-container">
      <input type="text" id="user-input" placeholder="Ask about your zodiac sign or any astrology question..." />
      <button id="send-button">Send</button>
    </div>
    <div id="api-key-container">
      <input type="password" id="api-key-input" placeholder="Enter your OpenAI API key (optional - for enhanced responses)" />
      <button id="save-api-key">Save Key</button>
    </div>
  </div>
</div>

<div id="loading" style="display: none;">
  <div class="spinner"></div>
  <p>Getting astrological insights...</p>
</div>

<style>
#chat-container {
  max-width: 800px;
  margin: 20px auto;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

#chat-header {
  background: rgba(255, 255, 255, 0.1);
  padding: 20px;
  text-align: center;
  color: white;
  border-bottom: 1px solid rgba(255, 255, 255, 0.2);
}

#chat-header h3 {
  margin: 0 0 10px 0;
  font-size: 24px;
}

#chat-header p {
  margin: 0;
  opacity: 0.9;
}

#chat-messages {
  height: 400px;
  overflow-y: auto;
  padding: 20px;
  background: white;
  background-image: 
    radial-gradient(circle at 20% 50%, rgba(102, 126, 234, 0.05) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(118, 75, 162, 0.05) 0%, transparent 50%);
}

.message {
  margin-bottom: 20px;
  padding: 15px;
  border-radius: 15px;
  max-width: 80%;
  word-wrap: break-word;
}

.user-message {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  margin-left: auto;
  text-align: right;
}

.ai-message {
  background: #f8f9fa;
  border: 1px solid #e9ecef;
  color: #333;
  margin-right: auto;
}

.message-content {
  line-height: 1.6;
}

.message-content strong {
  color: #667eea;
}

.message-content ul {
  margin: 10px 0;
  padding-left: 20px;
}

.message-content li {
  margin: 5px 0;
}

#chat-input-area {
  padding: 20px;
  background: rgba(255, 255, 255, 0.1);
  border-top: 1px solid rgba(255, 255, 255, 0.2);
}

#input-container {
  display: flex;
  gap: 10px;
  margin-bottom: 10px;
}

#user-input {
  flex: 1;
  padding: 12px 15px;
  border: none;
  border-radius: 25px;
  font-size: 16px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  outline: none;
}

#user-input:focus {
  box-shadow: 0 2px 8px rgba(102, 126, 234, 0.3);
}

#send-button {
  padding: 12px 24px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 25px;
  cursor: pointer;
  font-weight: bold;
  transition: transform 0.2s;
}

#send-button:hover {
  transform: translateY(-1px);
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

#send-button:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
}

#api-key-container {
  display: flex;
  gap: 10px;
  font-size: 14px;
}

#api-key-input {
  flex: 1;
  padding: 8px 12px;
  border: none;
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.9);
  font-size: 14px;
  outline: none;
}

#save-api-key {
  padding: 8px 16px;
  background: rgba(255, 255, 255, 0.2);
  color: white;
  border: 1px solid rgba(255, 255, 255, 0.3);
  border-radius: 20px;
  cursor: pointer;
  font-size: 14px;
  transition: background 0.2s;
}

#save-api-key:hover {
  background: rgba(255, 255, 255, 0.3);
}

#loading {
  text-align: center;
  padding: 20px;
  color: #667eea;
}

.spinner {
  border: 3px solid #f3f3f3;
  border-top: 3px solid #667eea;
  border-radius: 50%;
  width: 40px;
  height: 40px;
  animation: spin 1s linear infinite;
  margin: 0 auto 10px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

/* Mobile responsiveness */
@media (max-width: 768px) {
  #chat-container {
    margin: 10px;
    border-radius: 5px;
  }
  
  #chat-messages {
    height: 300px;
  }
  
  .message {
    max-width: 90%;
  }
  
  #input-container {
    flex-direction: column;
  }
  
  #api-key-container {
    flex-direction: column;
  }
}
</style>

<script>
class AstrologyChat {
  constructor() {
    this.apiKey = localStorage.getItem('openai_api_key') || '';
    this.initializeEventListeners();
    this.setupAPIKeyInput();
  }

  initializeEventListeners() {
    const sendButton = document.getElementById('send-button');
    const userInput = document.getElementById('user-input');
    const saveApiKeyButton = document.getElementById('save-api-key');

    sendButton.addEventListener('click', () => this.sendMessage());
    userInput.addEventListener('keypress', (e) => {
      if (e.key === 'Enter') {
        this.sendMessage();
      }
    });
    saveApiKeyButton.addEventListener('click', () => this.saveApiKey());
  }

  setupAPIKeyInput() {
    const apiKeyInput = document.getElementById('api-key-input');
    if (this.apiKey) {
      apiKeyInput.value = this.apiKey;
    }
  }

  saveApiKey() {
    const apiKeyInput = document.getElementById('api-key-input');
    this.apiKey = apiKeyInput.value.trim();
    if (this.apiKey) {
      localStorage.setItem('openai_api_key', this.apiKey);
      this.showMessage('API key saved successfully!', 'ai-message');
    }
  }

  async sendMessage() {
    const userInput = document.getElementById('user-input');
    const message = userInput.value.trim();
    
    if (!message) return;

    // Show user message
    this.showMessage(message, 'user-message');
    userInput.value = '';

    // Show loading
    this.showLoading(true);

    try {
      const response = await this.getAstrologyResponse(message);
      this.showMessage(response, 'ai-message');
    } catch (error) {
      this.showMessage('Sorry, I encountered an error. Please try again later.', 'ai-message');
      console.error('Error:', error);
    } finally {
      this.showLoading(false);
    }
  }

  async getAstrologyResponse(message) {
    // If API key is available, use OpenAI
    if (this.apiKey) {
      return await this.getOpenAIResponse(message);
    } else {
      // Fallback to local astrology responses
      return this.getLocalAstrologyResponse(message);
    }
  }

  async getOpenAIResponse(message) {
    const response = await fetch('https://api.openai.com/v1/chat/completions', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${this.apiKey}`
      },
      body: JSON.stringify({
        model: 'gpt-3.5-turbo',
        messages: [
          {
            role: 'system',
            content: 'You are a knowledgeable and mystical astrologer. Provide insightful, personalized astrology readings and advice. Be warm, intuitive, and use astrological terminology appropriately. Always maintain a mystical yet helpful tone.'
          },
          {
            role: 'user',
            content: message
          }
        ],
        max_tokens: 500,
        temperature: 0.7
      })
    });

    if (!response.ok) {
      throw new Error('OpenAI API request failed');
    }

    const data = await response.json();
    return data.choices[0].message.content;
  }

  getLocalAstrologyResponse(message) {
    // Local fallback responses for common astrology queries
    const responses = {
      aries: "♈ Aries (March 21 - April 19): You're a natural leader with fiery energy! Your ruling planet Mars gives you courage and determination. Focus on channeling your passion constructively.",
      taurus: "♉ Taurus (April 20 - May 20): Your earth sign nature makes you practical and reliable. Venus brings beauty and harmony to your life. Trust your steady approach to achieve your goals.",
      gemini: "♊ Gemini (May 21 - June 20): Your curious and adaptable nature is your strength! Mercury enhances your communication skills. Embrace variety and new experiences.",
      cancer: "♋ Cancer (June 21 - July 22): Your emotional intelligence and intuition are powerful gifts. The Moon influences your moods - honor your feelings and nurture others.",
      leo: "♌ Leo (July 23 - August 22): Your radiant personality and creativity shine bright! The Sun empowers your natural leadership. Share your talents with confidence.",
      virgo: "♍ Virgo (August 23 - September 22): Your attention to detail and service-oriented nature are admirable. Mercury helps you analyze and improve everything around you.",
      libra: "♎ Libra (September 23 - October 22): Your sense of balance and harmony brings peace to others. Venus blesses you with charm and diplomatic skills.",
      scorpio: "♏ Scorpio (October 23 - November 21): Your intensity and transformative power are remarkable. Pluto gives you insight into hidden truths and deep mysteries.",
      sagittarius: "♐ Sagittarius (November 22 - December 21): Your adventurous spirit and wisdom-seeking nature inspire others. Jupiter expands your horizons and opportunities.",
      capricorn: "♑ Capricorn (December 22 - January 19): Your ambition and practical approach lead to success. Saturn teaches you patience and rewards your hard work.",
      aquarius: "♒ Aquarius (January 20 - February 18): Your innovative and humanitarian nature makes you unique. Uranus brings unexpected opportunities and original ideas.",
      pisces: "♓ Pisces (February 19 - March 20): Your compassionate and intuitive nature is a gift to the world. Neptune enhances your creativity and spiritual connection."
    };

    const lowerMessage = message.toLowerCase();
    
    // Check for zodiac signs
    for (const [sign, response] of Object.entries(responses)) {
      if (lowerMessage.includes(sign)) {
        return response;
      }
    }

    // General astrology responses
    if (lowerMessage.includes('horoscope') || lowerMessage.includes('today')) {
      return "🌟 The stars are aligned for new opportunities today! Focus on your intuition and trust the cosmic energy guiding you. Remember, every day is a chance for growth and transformation.";
    }

    if (lowerMessage.includes('compatibility') || lowerMessage.includes('relationship')) {
      return "💕 Astrological compatibility depends on many factors beyond just sun signs. Consider your moon signs, Venus placements, and Mars positions for deeper relationship insights. Communication and understanding are key to any successful partnership.";
    }

    if (lowerMessage.includes('birth chart') || lowerMessage.includes('natal')) {
      return "🗺️ Your birth chart is a cosmic map of your soul's journey! It shows the positions of planets at your birth moment. For a detailed reading, you'd need your exact birth time, date, and location. Each placement reveals different aspects of your personality and life path.";
    }

    // Default response
    return "🌙 The cosmos holds many mysteries! I sense you're seeking guidance about your astrological path. Could you tell me your zodiac sign or ask about a specific astrological topic? I'm here to help you understand the celestial influences in your life.";
  }

  showMessage(message, className) {
    const chatMessages = document.getElementById('chat-messages');
    const messageDiv = document.createElement('div');
    messageDiv.className = `message ${className}`;
    
    const contentDiv = document.createElement('div');
    contentDiv.className = 'message-content';
    
    if (className === 'ai-message') {
      contentDiv.innerHTML = `<strong>AI Astrologer:</strong> ${message}`;
    } else {
      contentDiv.innerHTML = `<strong>You:</strong> ${message}`;
    }
    
    messageDiv.appendChild(contentDiv);
    chatMessages.appendChild(messageDiv);
    chatMessages.scrollTop = chatMessages.scrollHeight;
  }

  showLoading(show) {
    const loading = document.getElementById('loading');
    const sendButton = document.getElementById('send-button');
    
    if (show) {
      loading.style.display = 'block';
      sendButton.disabled = true;
    } else {
      loading.style.display = 'none';
      sendButton.disabled = false;
    }
  }
}

// Initialize the chat when the page loads
document.addEventListener('DOMContentLoaded', () => {
  new AstrologyChat();
});
</script>

---

### Features:
- **AI-Powered Responses**: Uses OpenAI's GPT for personalized astrology insights
- **Fallback System**: Local astrology knowledge when API key is not provided
- **Responsive Design**: Works on desktop and mobile devices
- **Secure**: API key is stored locally and never transmitted to our servers
- **Interactive Chat**: Real-time conversation with the AI astrologer

### How to Use:
1. **Optional**: Enter your OpenAI API key for enhanced responses
2. Type your astrology questions in the chat
3. Ask about zodiac signs, horoscopes, compatibility, or birth charts
4. Enjoy personalized astrological guidance!

---

### [Home](https://hmntrathore.github.io) | [Back to Blog](https://hmntrathore.github.io)