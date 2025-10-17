# Quick Start Guide

## Get Started in 5 Minutes

This guide will help you quickly implement your first AI companion using this framework.

## Step 1: Choose Your Companion Type (1 minute)

Pick the personality that best fits your use case:

| Companion Type | Best For | Empathy Level | Energy Level |
|---------------|----------|---------------|--------------|
| **Base Companion** | General purpose, balanced support | High (8/10) | Moderate |
| **Cheerful Motivator** | Goal achievement, motivation | Medium-High (7/10) | High |
| **Calm Therapist** | Emotional processing, deep support | Very High (10/10) | Calm |
| **Wise Mentor** | Guidance, life decisions | High (8/10) | Thoughtful |

**Recommendation for beginners**: Start with **Base Companion** - it's versatile and works well across most situations.

## Step 2: Load Your Configuration (2 minutes)

### Python Implementation

```python
import json
import yaml

# 1. Load personality template
with open('personality-templates/base-personality.json', 'r') as f:
    personality = json.load(f)

# 2. Load system prompt
with open('system-prompts/empathetic-companion.md', 'r') as f:
    system_prompt = f.read()

# 3. Load conversation prompts
with open('conversation-prompts/emotional-contexts.yaml', 'r') as f:
    conversation_prompts = yaml.safe_load(f)

print("✅ Configuration loaded successfully!")
```

### JavaScript Implementation

```javascript
const fs = require('fs');
const yaml = require('js-yaml');

// 1. Load personality template
const personality = JSON.parse(
    fs.readFileSync('personality-templates/base-personality.json', 'utf8')
);

// 2. Load system prompt
const systemPrompt = fs.readFileSync(
    'system-prompts/empathetic-companion.md', 
    'utf8'
);

// 3. Load conversation prompts
const conversationPrompts = yaml.load(
    fs.readFileSync('conversation-prompts/emotional-contexts.yaml', 'utf8')
);

console.log("✅ Configuration loaded successfully!");
```

## Step 3: Create Your First Companion (2 minutes)

### Minimal Implementation

```python
class MyFirstCompanion:
    def __init__(self, personality, system_prompt, conversation_prompts):
        self.personality = personality
        self.system_prompt = system_prompt
        self.conversation_prompts = conversation_prompts
    
    def respond(self, user_input):
        """Generate empathetic response"""
        # Build the complete prompt for your LLM
        full_prompt = f"""
{self.system_prompt}

User says: {user_input}

Respond with empathy level: {self.personality['personality_template']['empathy_configuration']['empathy_level']}

Your response:"""
        
        # Send to your LLM (OpenAI, Anthropic, etc.)
        # response = your_llm_api(full_prompt)
        
        return full_prompt

# Create your companion
companion = MyFirstCompanion(personality, system_prompt, conversation_prompts)

# Test it
response = companion.respond("I'm feeling really down today")
print(response)
```

### Integration with OpenAI

```python
from openai import OpenAI

class AICompanion:
    def __init__(self, personality, system_prompt):
        self.client = OpenAI()  # Uses OPENAI_API_KEY env var
        self.personality = personality
        self.system_prompt = system_prompt
        self.conversation_history = []
    
    def respond(self, user_input):
        """Generate response using OpenAI"""
        # Add user message to history
        self.conversation_history.append({
            "role": "user",
            "content": user_input
        })
        
        # Create messages with system prompt
        messages = [
            {"role": "system", "content": self.system_prompt}
        ] + self.conversation_history
        
        # Get response
        response = self.client.chat.completions.create(
            model="gpt-4",
            messages=messages,
            temperature=0.7
        )
        
        assistant_message = response.choices[0].message.content
        
        # Add to history
        self.conversation_history.append({
            "role": "assistant",
            "content": assistant_message
        })
        
        return assistant_message

# Usage
companion = AICompanion(personality, system_prompt)
response = companion.respond("I need someone to talk to")
print(response)
```

## Step 4: Test Your Companion

```python
# Test different emotional contexts
test_inputs = [
    "I'm so happy! I got the job!",
    "I'm really anxious about my presentation tomorrow",
    "I don't know what to do with my life anymore",
    "Just wanted to say hi!"
]

for test_input in test_inputs:
    print(f"\nUser: {test_input}")
    response = companion.respond(test_input)
    print(f"Companion: {response}\n")
    print("-" * 50)
```

## Next Steps

### Enhance Your Companion

1. **Add Emotion Detection**
   ```python
   def detect_emotion(self, user_input):
       # Use keywords, sentiment analysis, or ML
       if any(word in user_input.lower() for word in ['sad', 'down', 'depressed']):
           return 'sadness'
       # More detection logic...
   ```

2. **Implement Prompt Chains**
   ```python
   # Load a chain
   with open('prompt-chains/context-aware-chains.json', 'r') as f:
       chains = json.load(f)
   
   # Use emotional support chain
   support_chain = chains['prompt_chains']['emotional_support_chain']
   ```

3. **Add Safety Checks**
   ```python
   def check_crisis(self, user_input):
       crisis_keywords = ['suicide', 'kill myself', 'end it all']
       if any(keyword in user_input.lower() for keyword in crisis_keywords):
           return self.crisis_response()
   ```

4. **Store Conversation Context**
   ```python
   # Remember previous conversations
   self.user_profile = {
       'name': None,
       'recent_topics': [],
       'emotional_patterns': []
   }
   ```

### Explore Advanced Features

- **Multiple Personality Modes**: Switch between companion types
- **Context Awareness**: Remember and reference previous conversations
- **Emotional Intelligence**: Detect and respond to complex emotions
- **Safety Protocols**: Implement crisis detection and resources

### Read Full Documentation

- [Personality Templates Guide](personality-guide.md)
- [System Prompts Guide](system-prompts-guide.md)
- [Emotional Guidelines Guide](emotional-guidelines-guide.md)
- [Prompt Chains Guide](prompt-chains-guide.md)
- [Testing Guide](testing-guide.md)

## Common Issues & Solutions

### Issue: Responses lack empathy
**Solution**: Check empathy level in personality template and ensure system prompt is being used correctly.

### Issue: Personality inconsistent
**Solution**: Make sure you're including personality traits in your prompts to the LLM.

### Issue: Not handling emotions well
**Solution**: Use conversation prompts from emotional-contexts.yaml for specific emotional situations.

### Issue: Safety concerns
**Solution**: Implement crisis detection using testing-scenarios examples.

## Example: Complete Simple Companion

Here's a complete, working example:

```python
import json
from openai import OpenAI

class SimpleCompanion:
    def __init__(self):
        # Load configuration
        with open('personality-templates/base-personality.json', 'r') as f:
            self.personality = json.load(f)['personality_template']
        
        with open('system-prompts/empathetic-companion.md', 'r') as f:
            self.system_prompt = f.read()
        
        self.client = OpenAI()
        self.history = []
    
    def chat(self, message):
        """Simple chat interface"""
        self.history.append({"role": "user", "content": message})
        
        messages = [{"role": "system", "content": self.system_prompt}] + self.history
        
        response = self.client.chat.completions.create(
            model="gpt-4",
            messages=messages,
            temperature=0.7
        )
        
        reply = response.choices[0].message.content
        self.history.append({"role": "assistant", "content": reply})
        
        return reply

# Use it
companion = SimpleCompanion()
print(companion.chat("Hi, I need someone to talk to"))
print(companion.chat("I've been feeling really anxious lately"))
```

## Getting Help

- **Check Examples**: See `examples/implementation-examples.md`
- **Review Tests**: See `testing-scenarios/test-scenarios.md`
- **Read Guides**: See all guides in `/docs/`

## You're Ready! 🎉

You now have a working AI companion! Start experimenting, test with different scenarios, and customize to your needs.

Remember: The goal is creating genuine, helpful, empathetic interactions. Use the testing scenarios to ensure your companion is performing well.

Happy building! 💙
