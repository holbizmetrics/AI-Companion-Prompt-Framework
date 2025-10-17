# Implementation Examples

## Overview

This document provides practical examples of implementing the AI Companion Prompt Framework in various scenarios. Each example includes code snippets, configuration details, and expected outcomes.

## Example 1: Basic Empathetic Companion

### Objective
Create a simple empathetic companion that responds appropriately to user emotions.

### Configuration

```python
import json
import yaml

# Load personality template
with open('personality-templates/base-personality.json', 'r') as f:
    personality = json.load(f)['personality_template']

# Load system prompt
with open('system-prompts/empathetic-companion.md', 'r') as f:
    system_prompt = f.read()

# Load emotional contexts
with open('conversation-prompts/emotional-contexts.yaml', 'r') as f:
    emotional_contexts = yaml.safe_load(f)['emotional_contexts']
```

### Implementation

```python
class EmpathenticCompanion:
    def __init__(self, personality, system_prompt, emotional_contexts):
        self.personality = personality
        self.system_prompt = system_prompt
        self.emotional_contexts = emotional_contexts
        self.conversation_history = []
    
    def detect_emotion(self, user_input):
        """Detect user's emotional state from input"""
        # Simple keyword-based detection (can be enhanced with ML)
        emotions = {
            'sadness': ['sad', 'down', 'depressed', 'grief', 'loss'],
            'anxiety': ['anxious', 'worried', 'panic', 'stressed'],
            'joy': ['happy', 'excited', 'great', 'wonderful'],
            'anger': ['angry', 'frustrated', 'mad', 'furious']
        }
        
        user_input_lower = user_input.lower()
        for emotion, keywords in emotions.items():
            if any(keyword in user_input_lower for keyword in keywords):
                return emotion
        return 'neutral'
    
    def get_response_prompts(self, emotion):
        """Get appropriate prompts for detected emotion"""
        context_key = f"{emotion}_" + ("grief" if emotion == "sadness" else "worry" if emotion == "anxiety" else "excitement")
        if context_key in self.emotional_contexts:
            return self.emotional_contexts[context_key]
        return None
    
    def respond(self, user_input):
        """Generate empathetic response"""
        # Detect emotion
        emotion = self.detect_emotion(user_input)
        
        # Get appropriate prompts
        prompts = self.get_response_prompts(emotion)
        
        # Build response using system prompt + emotional context
        response_template = f"""
        System Context: {self.system_prompt[:500]}...
        
        User Emotion: {emotion}
        User Input: {user_input}
        
        Respond with empathy level: {self.personality['empathy_configuration']['empathy_level']}
        Use tone: {self.personality['traits']['communication_style'][0]['value']}
        
        Available emotional prompts: {prompts['opening_prompts'] if prompts else 'general empathy'}
        """
        
        # In real implementation, this would go to your LLM
        # For demonstration, we'll use a simulated response
        self.conversation_history.append({
            'user': user_input,
            'emotion': emotion,
            'response': response_template
        })
        
        return response_template

# Usage
companion = EmpathenticCompanion(personality, system_prompt, emotional_contexts)
response = companion.respond("I've been feeling really down lately")
print(response)
```

### Expected Output

The companion should:
1. Detect sadness in the input
2. Load sadness/grief context prompts
3. Apply high empathy level from personality
4. Generate warm, validating response
5. Follow empathetic-companion system prompt guidelines

## Example 2: Motivational Coach for Goal Setting

### Objective
Implement a motivational coach that helps users set and achieve goals using prompt chains.

### Configuration

```python
# Load motivational coach personality
with open('personality-templates/cheerful-motivator.json', 'r') as f:
    coach_personality = json.load(f)['personality_template']

# Load motivational coach system prompt
with open('system-prompts/motivational-coach.md', 'r') as f:
    coach_system_prompt = f.read()

# Load motivation building chain
with open('prompt-chains/context-aware-chains.json', 'r') as f:
    chains = json.load(f)['prompt_chains']
    motivation_chain = chains['motivation_building_chain']
```

### Implementation

```python
class MotivationalCoach:
    def __init__(self, personality, system_prompt, chain):
        self.personality = personality
        self.system_prompt = system_prompt
        self.chain = chain
        self.current_step = 1
        self.user_data = {}
    
    def start_goal_setting(self, user_goal):
        """Begin goal-setting chain"""
        self.user_data['goal'] = user_goal
        step_1 = self.chain['steps'][0]
        
        # Use step 1 prompt with personality energy
        energy = self.personality['traits']['core_traits'][1]['level']  # energetic trait
        
        response = f"{step_1['prompt']}"
        # Add motivational energy based on personality
        if energy >= 8:
            response = response.replace("!", "! 🎯")
            
        self.current_step = 2
        return response
    
    def process_response(self, user_response):
        """Process user response and advance through chain"""
        current_step_data = self.chain['steps'][self.current_step - 1]
        
        # Determine next step based on conditions
        conditions = current_step_data.get('next_step_conditions', {})
        
        # Simple condition evaluation (can be made more sophisticated)
        if 'when_why_identified' in conditions and 'because' in user_response.lower():
            next_step = conditions['when_why_identified']
        else:
            # Default progression
            next_step = f"step_{self.current_step + 1}"
        
        # Find next step in chain
        for step in self.chain['steps']:
            if step.get('step') == next_step or step.get('phase') == next_step:
                self.current_step += 1
                return step['prompt']
        
        return "Great progress! What would you like to focus on next?"
    
    def provide_encouragement(self):
        """Provide motivational encouragement"""
        strategies = self.personality.get('motivation_strategies', [])
        import random
        return random.choice(strategies)

# Usage
coach = MotivationalCoach(coach_personality, coach_system_prompt, motivation_chain)

# Start goal setting
response1 = coach.start_goal_setting("I want to run a marathon")
print("Coach:", response1)

# User responds
user_response = "Success for me means crossing that finish line feeling strong"
response2 = coach.process_response(user_response)
print("Coach:", response2)

# Provide encouragement
encouragement = coach.provide_encouragement()
print("Coach:", encouragement)
```

## Example 3: Context-Aware Conversation Manager

### Objective
Create a system that manages conversation context and switches between different companion modes based on user needs.

### Implementation

```python
class ContextAwareCompanion:
    def __init__(self):
        self.personalities = self.load_all_personalities()
        self.system_prompts = self.load_all_system_prompts()
        self.current_mode = 'empathetic-companion'
        self.conversation_context = []
        self.user_emotional_state = 'neutral'
    
    def load_all_personalities(self):
        """Load all personality templates"""
        personalities = {}
        personality_files = [
            'base-personality',
            'cheerful-motivator',
            'calm-therapist',
            'wise-mentor'
        ]
        for pfile in personality_files:
            with open(f'personality-templates/{pfile}.json', 'r') as f:
                personalities[pfile] = json.load(f)
        return personalities
    
    def load_all_system_prompts(self):
        """Load all system prompts"""
        prompts = {}
        prompt_files = [
            'empathetic-companion',
            'motivational-coach',
            'reflective-listener',
            'playful-friend'
        ]
        for pfile in prompt_files:
            with open(f'system-prompts/{pfile}.md', 'r') as f:
                prompts[pfile] = f.read()
        return prompts
    
    def detect_mode_switch_needed(self, user_input):
        """Determine if mode switch is appropriate"""
        keywords = {
            'motivational-coach': ['goal', 'achieve', 'motivation', 'help me', 'succeed'],
            'calm-therapist': ['anxious', 'panic', 'overwhelming', 'therapy', 'process'],
            'reflective-listener': ['understand', 'reflect', 'think about', 'confused'],
            'playful-friend': ['fun', 'joke', 'laugh', 'play', 'lighten up']
        }
        
        user_input_lower = user_input.lower()
        for mode, mode_keywords in keywords.items():
            if any(keyword in user_input_lower for keyword in mode_keywords):
                return mode
        
        return self.current_mode
    
    def switch_mode(self, new_mode):
        """Switch companion mode"""
        if new_mode != self.current_mode:
            transition_message = self.get_transition_message(self.current_mode, new_mode)
            self.current_mode = new_mode
            return transition_message
        return None
    
    def get_transition_message(self, old_mode, new_mode):
        """Generate smooth transition between modes"""
        transitions = {
            ('empathetic-companion', 'motivational-coach'): 
                "I hear you're ready to take action. Let's create a plan together!",
            ('motivational-coach', 'calm-therapist'):
                "I'm sensing you need a different kind of support right now. Let's slow down and process this together.",
            ('calm-therapist', 'playful-friend'):
                "I'm glad you're feeling lighter. Want to have some fun?",
            ('playful-friend', 'empathetic-companion'):
                "I'm here for you, whatever you need. Let's talk about what's on your mind."
        }
        return transitions.get((old_mode, new_mode), "I'm here for you.")
    
    def respond(self, user_input):
        """Generate context-aware response"""
        # Check if mode switch is needed
        needed_mode = self.detect_mode_switch_needed(user_input)
        transition = self.switch_mode(needed_mode)
        
        # Get current personality and system prompt
        personality = self.get_current_personality()
        system_prompt = self.system_prompts[self.current_mode]
        
        # Build response
        response = {
            'mode': self.current_mode,
            'transition': transition,
            'personality_traits': personality['personality_template']['traits'],
            'system_context': system_prompt[:200],
            'user_input': user_input
        }
        
        # Add to conversation context
        self.conversation_context.append({
            'input': user_input,
            'mode': self.current_mode,
            'response': response
        })
        
        return response
    
    def get_current_personality(self):
        """Map current mode to appropriate personality"""
        mode_personality_map = {
            'empathetic-companion': 'base-personality',
            'motivational-coach': 'cheerful-motivator',
            'reflective-listener': 'calm-therapist',
            'calm-therapist': 'calm-therapist',
            'playful-friend': 'base-personality'
        }
        personality_key = mode_personality_map.get(self.current_mode, 'base-personality')
        return self.personalities[personality_key]

# Usage
companion = ContextAwareCompanion()

# User starts with emotional need
response1 = companion.respond("I'm feeling really overwhelmed")
print("Mode:", response1['mode'])

# User shifts to wanting motivation
response2 = companion.respond("I need help achieving my goals")
print("Mode:", response2['mode'])
print("Transition:", response2['transition'])

# Check conversation context
print(f"\nConversation has {len(companion.conversation_context)} exchanges")
```

## Example 4: Safety-First Crisis Detection

### Objective
Implement crisis detection and appropriate response with safety resources.

### Implementation

```python
class SafetyFirstCompanion:
    def __init__(self):
        self.crisis_keywords = [
            'suicide', 'kill myself', 'end it all', 'no reason to live',
            'better off dead', 'want to die', 'hurt myself'
        ]
        self.crisis_resources = {
            'us': {
                'suicide_prevention': '988',
                'crisis_text': 'Text HOME to 741741',
                'emergency': '911'
            },
            'international': {
                'helpline': 'https://findahelpline.com'
            }
        }
    
    def detect_crisis(self, user_input):
        """Detect potential crisis situation"""
        user_input_lower = user_input.lower()
        for keyword in self.crisis_keywords:
            if keyword in user_input_lower:
                return True
        return False
    
    def crisis_response(self, user_input):
        """Provide immediate crisis support"""
        response = {
            'is_crisis': True,
            'immediate_response': """
I hear how much pain you're in right now, and I'm deeply concerned about your safety. 
You don't have to face this alone. 

Please reach out to a crisis counselor who can provide immediate support:
- Call 988 (Suicide & Crisis Lifeline) - Available 24/7
- Text HOME to 741741 (Crisis Text Line)
- If you're in immediate danger, call 911

I'm here with you right now, but these professionals can provide the help you need 
and deserve. Your life matters, and there are people who want to help.

Are you safe right now? Would you be willing to reach out to one of these resources?
            """,
            'resources': self.crisis_resources,
            'stay_present': True
        }
        return response
    
    def standard_response(self, user_input):
        """Standard empathetic response for non-crisis"""
        return {
            'is_crisis': False,
            'response': f"I hear you. Tell me more about what you're experiencing."
        }
    
    def respond(self, user_input):
        """Route to appropriate response"""
        if self.detect_crisis(user_input):
            return self.crisis_response(user_input)
        else:
            return self.standard_response(user_input)

# Usage
safety_companion = SafetyFirstCompanion()

# Test with crisis input
crisis_response = safety_companion.respond("I don't want to be here anymore")
print("Crisis detected:", crisis_response['is_crisis'])
print(crisis_response['immediate_response'])

# Test with regular input
regular_response = safety_companion.respond("I'm having a hard day")
print("\nCrisis detected:", regular_response['is_crisis'])
```

## Example 5: Testing and Validation

### Objective
Implement testing framework to validate companion responses.

### Implementation

```python
import yaml

class CompanionTester:
    def __init__(self, companion):
        self.companion = companion
        self.test_scenarios = self.load_test_scenarios()
        self.results = []
    
    def load_test_scenarios(self):
        """Load test scenarios"""
        # In real implementation, parse from test-scenarios.md
        return [
            {
                'scenario_id': 'grief-001',
                'user_input': "My dad died yesterday. I can't believe he's gone.",
                'expected_elements': [
                    'profound sympathy',
                    'validation without platitudes',
                    'space for silence'
                ],
                'inappropriate_responses': [
                    "He's in a better place",
                    "At least",
                    "You'll feel better soon"
                ]
            },
            {
                'scenario_id': 'joy-001',
                'user_input': "I GOT INTO MY DREAM SCHOOL!!!",
                'expected_elements': [
                    'enthusiastic celebration',
                    'genuine happiness',
                    'acknowledgment of effort'
                ],
                'inappropriate_responses': [
                    'lukewarm response',
                    'pointing out challenges'
                ]
            }
        ]
    
    def evaluate_response(self, response, expected, inappropriate):
        """Evaluate response quality"""
        score = 5  # Start with perfect score
        
        # Check for inappropriate responses
        response_lower = response.lower()
        for inappropriate_item in inappropriate:
            if inappropriate_item.lower() in response_lower:
                score -= 2
        
        # Check for expected elements (simplified)
        found_elements = 0
        for expected_item in expected:
            if expected_item.lower() in response_lower:
                found_elements += 1
        
        # Adjust score based on found elements
        expected_ratio = found_elements / len(expected)
        if expected_ratio < 0.5:
            score -= 2
        elif expected_ratio < 0.8:
            score -= 1
        
        return max(1, min(5, score))  # Clamp between 1-5
    
    def run_tests(self):
        """Run all test scenarios"""
        print("Running companion tests...\n")
        
        for scenario in self.test_scenarios:
            print(f"Scenario: {scenario['scenario_id']}")
            print(f"Input: {scenario['user_input']}")
            
            # Get companion response
            response = self.companion.respond(scenario['user_input'])
            response_text = str(response)  # Simplified
            
            # Evaluate
            score = self.evaluate_response(
                response_text,
                scenario['expected_elements'],
                scenario['inappropriate_responses']
            )
            
            result = {
                'scenario_id': scenario['scenario_id'],
                'score': score,
                'passed': score >= 3
            }
            
            self.results.append(result)
            
            print(f"Score: {score}/5")
            print(f"Status: {'PASS' if result['passed'] else 'FAIL'}")
            print("-" * 50)
        
        # Summary
        passed = sum(1 for r in self.results if r['passed'])
        total = len(self.results)
        print(f"\nTest Summary: {passed}/{total} passed")
        
        return self.results

# Usage
companion = EmpathenticCompanion(personality, system_prompt, emotional_contexts)
tester = CompanionTester(companion)
results = tester.run_tests()
```

## Integration Examples

### Example 6: Web API Integration

```python
from flask import Flask, request, jsonify

app = Flask(__name__)
companion = ContextAwareCompanion()

@app.route('/companion/chat', methods=['POST'])
def chat():
    """API endpoint for companion interaction"""
    data = request.json
    user_input = data.get('message', '')
    user_id = data.get('user_id', 'anonymous')
    
    # Generate response
    response = companion.respond(user_input)
    
    return jsonify({
        'response': response,
        'mode': companion.current_mode,
        'emotional_state': companion.user_emotional_state
    })

@app.route('/companion/mode', methods=['POST'])
def set_mode():
    """Manually set companion mode"""
    data = request.json
    mode = data.get('mode', 'empathetic-companion')
    
    transition = companion.switch_mode(mode)
    
    return jsonify({
        'current_mode': companion.current_mode,
        'transition_message': transition
    })

if __name__ == '__main__':
    app.run(debug=True)
```

## Best Practices Demonstrated

1. **Load configurations at initialization**: Templates and prompts loaded once
2. **Detect emotional context**: Simple keyword-based detection (can be enhanced)
3. **Route to appropriate responses**: Different handling for different situations
4. **Maintain conversation history**: Track context for continuity
5. **Priority safety**: Crisis detection comes first
6. **Test thoroughly**: Automated testing framework
7. **Smooth mode transitions**: Natural switches between companion modes
8. **Clear separation of concerns**: Each class has specific responsibility

## Conclusion

These examples demonstrate practical implementation patterns for the AI Companion Prompt Framework. Adapt these patterns to your specific needs while maintaining the core principles of empathy, safety, and personality consistency.
