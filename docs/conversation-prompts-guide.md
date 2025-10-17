# Conversation Prompts Guide

## Overview

Conversation prompts are pre-written, context-specific prompts organized by emotional situations. They help your AI companion respond appropriately across different emotional contexts.

## Structure

The conversation prompts are organized in `conversation-prompts/emotional-contexts.yaml` with the following structure:

```yaml
emotional_contexts:
  [emotion_category]:
    context_description: "Description of the emotional state"
    emotional_tone: "Appropriate tone for responses"
    
    opening_prompts: [...]
    validation_prompts: [...]
    support_prompts: [...]
    reflection_prompts: [...]
```

## Available Emotional Contexts

### 1. Sadness and Grief (`sadness_grief`)

**When to Use**: User expresses loss, sorrow, or deep sadness

**Example Opening Prompts**:
- "I'm so sorry you're going through this. I'm here to listen."
- "I can sense the heaviness in what you're sharing. Take all the time you need."

**Key Principles**:
- Never rush to fix or cheer up
- Avoid "at least" statements
- Allow silence and processing time
- Validate without platitudes

**Usage Example**:
```python
if emotion == 'sadness':
    prompts = emotional_contexts['sadness_grief']
    opening = random.choice(prompts['opening_prompts'])
    validation = random.choice(prompts['validation_prompts'])
    
    response = f"{opening} {validation}"
```

### 2. Anxiety and Worry (`anxiety_worry`)

**When to Use**: User expresses worry, anxiety, panic, or stress

**Prompt Categories**:
- **Opening prompts**: Initial acknowledgment
- **Grounding prompts**: Physical grounding techniques
- **Reframing prompts**: Cognitive reframing
- **Action prompts**: Concrete next steps

**Key Principles**:
- Provide calm, grounding presence
- Offer specific techniques (breathing, 5-4-3-2-1)
- Help separate fears from facts
- Empower with actionable steps

**Usage Example**:
```python
if emotion == 'anxiety':
    prompts = emotional_contexts['anxiety_worry']
    
    # Start with grounding
    grounding = prompts['grounding_prompts'][0]
    # "Let's bring you back to this present moment..."
    
    # Then reframe
    reframe = prompts['reframing_prompts'][2]
    # "What would you tell a friend who was worried about this?"
```

### 3. Joy and Excitement (`joy_excitement`)

**When to Use**: User shares good news, achievements, or happiness

**Prompt Categories**:
- **Celebration prompts**: Enthusiastic sharing of joy
- **Amplification prompts**: Making the moment bigger
- **Savoring prompts**: Helping them fully experience it

**Key Principles**:
- Match their enthusiasm
- Don't dampen with "but what about..."
- Ask interested questions
- Help them savor the moment

**Usage Example**:
```python
if emotion == 'joy':
    prompts = emotional_contexts['joy_excitement']
    
    # Celebrate first!
    celebration = prompts['celebration_prompts'][0]
    # "This is wonderful! I'm so happy for you!"
    
    # Then amplify
    amplification = prompts['amplification_prompts'][0]
    # "What's the best part of this for you?"
```

### 4. Anger and Frustration (`anger_frustration`)

**When to Use**: User expresses anger, frustration, or feeling wronged

**Prompt Categories**:
- **Validation prompts**: Legitimizing their anger
- **Expression prompts**: Creating space to vent
- **Processing prompts**: Moving toward resolution

**Key Principles**:
- Validate the anger fully
- Don't tell them to calm down
- Create safe space for expression
- Help process underlying hurt

**Usage Example**:
```python
if emotion == 'anger':
    prompts = emotional_contexts['anger_frustration']
    
    # Validate immediately
    validation = prompts['validation_prompts'][0]
    # "It's completely understandable that you feel angry about this."
    
    # Create space for expression
    expression = prompts['expression_prompts'][0]
    # "Tell me more about what happened. I'm listening."
```

### 5. Confusion and Uncertainty (`confusion_uncertainty`)

**When to Use**: User is confused, uncertain, or can't decide

**Prompt Categories**:
- **Clarity prompts**: Helping organize thoughts
- **Exploration prompts**: Opening up options
- **Support prompts**: Normalizing uncertainty

**Key Principles**:
- Help break down complexity
- No pressure to decide immediately
- Organize thoughts together
- Validate discomfort with uncertainty

### 6. Loneliness and Isolation (`loneliness_isolation`)

**When to Use**: User feels alone, disconnected, or isolated

**Prompt Categories**:
- **Presence prompts**: Offering companionship
- **Connection prompts**: Exploring connection needs
- **Validation prompts**: Normalizing the need for connection

**Key Principles**:
- Provide immediate sense of presence
- Don't dismiss with "you're not really alone"
- Validate need for connection
- Explore what kind of connection they crave

### 7. Pride and Accomplishment (`pride_accomplishment`)

**When to Use**: User achieved something and feels proud

**Prompt Categories**:
- **Celebration prompts**: Sharing in pride
- **Recognition prompts**: Acknowledging effort
- **Forward prompts**: Building on success

**Key Principles**:
- Celebrate genuinely
- Recognize the effort, not just outcome
- Ask about the journey
- Help them internalize success

### 8. Shame and Embarrassment (`shame_embarrassment`)

**When to Use**: User feels shame, embarrassment, or harsh self-judgment

**Prompt Categories**:
- **Normalizing prompts**: Making it human
- **Perspective prompts**: Reframing the situation
- **Compassion prompts**: Encouraging self-kindness

**Key Principles**:
- Separate behavior from identity
- Normalize being human and making mistakes
- Encourage self-compassion
- Offer perspective without minimizing

### 9. Overwhelm and Burnout (`overwhelm_burnout`)

**When to Use**: User feels overwhelmed, burned out, or at capacity

**Prompt Categories**:
- **Acknowledgment prompts**: Seeing their struggle
- **Simplifying prompts**: Breaking down complexity
- **Restoration prompts**: Encouraging rest

**Key Principles**:
- Acknowledge the weight they're carrying
- Help simplify and prioritize
- Permission to slow down
- Focus on what's actually essential

### 10. Hope and Anticipation (`hope_anticipation`)

**When to Use**: User feels hopeful or anticipating something positive

**Prompt Categories**:
- **Nurturing prompts**: Supporting the hope
- **Planning prompts**: Building on anticipation
- **Protection prompts**: Realistic optimism

**Key Principles**:
- Nurture the hope without dampening
- Help them plan while protecting from disappointment
- Balance optimism with realism
- Celebrate the feeling of hope itself

## How to Use Prompts

### 1. Direct Selection

Simply select appropriate prompts based on detected emotion:

```python
def get_prompts(emotion):
    emotion_map = {
        'sad': 'sadness_grief',
        'anxious': 'anxiety_worry',
        'happy': 'joy_excitement',
        'angry': 'anger_frustration'
    }
    
    context_key = emotion_map.get(emotion)
    return emotional_contexts.get(context_key)
```

### 2. Progressive Use

Use different prompt categories as conversation progresses:

```python
class ProgressivePrompts:
    def __init__(self, emotion_context):
        self.context = emotion_context
        self.stage = 'opening'
    
    def get_next_prompt(self):
        if self.stage == 'opening':
            prompts = self.context['opening_prompts']
            self.stage = 'validation'
        elif self.stage == 'validation':
            prompts = self.context['validation_prompts']
            self.stage = 'support'
        elif self.stage == 'support':
            prompts = self.context['support_prompts']
            self.stage = 'reflection'
        else:
            prompts = self.context['reflection_prompts']
        
        return random.choice(prompts)
```

### 3. Template Modification

Use prompts as templates and customize:

```python
def customize_prompt(prompt_template, user_context):
    # Example: "I hear the [emotion] in what you're sharing"
    prompt = prompt_template.replace('[emotion]', user_context['detected_emotion'])
    
    # Add user's name if known
    if user_context.get('name'):
        prompt = f"{user_context['name']}, {prompt}"
    
    return prompt
```

### 4. Combination

Combine multiple prompts for richer responses:

```python
def build_response(emotion_context):
    opening = random.choice(emotion_context['opening_prompts'])
    validation = random.choice(emotion_context['validation_prompts'])
    support = random.choice(emotion_context['support_prompts'])
    
    return f"{opening} {validation} {support}"
```

## Advanced Usage

### Context-Aware Selection

Select prompts based on conversation history:

```python
class ContextAwarePrompts:
    def __init__(self):
        self.used_prompts = []
        self.conversation_depth = 0
    
    def select_prompt(self, prompts):
        # Avoid repeating recently used prompts
        available = [p for p in prompts if p not in self.used_prompts[-5:]]
        
        # If conversation is deep, use reflection prompts
        if self.conversation_depth > 3:
            if 'reflection_prompts' in available:
                prompts = available['reflection_prompts']
        
        selected = random.choice(prompts)
        self.used_prompts.append(selected)
        self.conversation_depth += 1
        
        return selected
```

### Emotional Intensity Matching

Adjust prompts based on emotional intensity:

```python
def match_intensity(prompts, intensity_level):
    """
    intensity_level: 1 (mild) to 10 (intense)
    """
    if intensity_level >= 8:
        # Use most empathetic, strongest prompts
        return prompts[0]  # Usually strongest
    elif intensity_level >= 5:
        return prompts[len(prompts)//2]  # Middle range
    else:
        return prompts[-1]  # Gentler prompts
```

### Multi-Emotion Handling

When user expresses mixed emotions:

```python
def handle_mixed_emotions(emotions_detected):
    """
    emotions_detected: ['anxiety', 'excitement']
    """
    responses = []
    
    for emotion in emotions_detected:
        context = get_emotion_context(emotion)
        prompt = random.choice(context['opening_prompts'])
        responses.append(prompt)
    
    # Acknowledge both emotions
    combined = f"{responses[0]} I also sense {emotions_detected[1]}. {responses[1]}"
    
    return combined
```

## Customizing Prompts

### Adding New Emotional Contexts

```yaml
# Add to emotional-contexts.yaml
new_emotion_context:
  context_description: "User experiencing [new emotion]"
  emotional_tone: "appropriate tone"
  
  opening_prompts:
    - "Your opening prompt here"
  
  validation_prompts:
    - "Your validation prompt here"
```

### Creating Domain-Specific Prompts

```yaml
# Example: Student support companion
academic_stress:
  context_description: "Student experiencing academic pressure"
  emotional_tone: "understanding, encouraging, practical"
  
  opening_prompts:
    - "Academic pressure can be really intense. I'm here to help you navigate this."
    - "I hear the stress in what you're sharing about your studies."
  
  support_prompts:
    - "What would help you feel more manageable about this coursework?"
    - "Let's break down what's on your plate into smaller pieces."
```

## Testing Your Prompts

### Evaluation Criteria

1. **Appropriateness**: Does prompt fit the emotional context?
2. **Empathy Level**: Does it match required empathy?
3. **Helpfulness**: Does it move conversation forward?
4. **Naturalness**: Does it sound natural, not scripted?

### Testing Process

```python
def test_prompt(prompt, context, expected_elements):
    """
    Test if prompt contains expected elements
    """
    score = 0
    
    for element in expected_elements:
        if element.lower() in prompt.lower():
            score += 1
    
    appropriateness = score / len(expected_elements)
    
    return {
        'prompt': prompt,
        'score': appropriateness,
        'passed': appropriateness >= 0.7
    }

# Usage
result = test_prompt(
    "I'm so sorry you're going through this. Your feelings are valid.",
    context='sadness_grief',
    expected_elements=['sorry', 'valid', 'feelings']
)
```

## Best Practices

### Do's
✅ **Match emotional tone**: Align prompt tone with user's emotional state
✅ **Progress naturally**: Move from opening → validation → support → reflection
✅ **Customize when possible**: Personalize prompts with user's context
✅ **Vary responses**: Don't repeat the same prompts
✅ **Test thoroughly**: Validate prompts work across scenarios

### Don'ts
❌ **Don't use happy prompts for sad emotions**: Maintain emotional appropriateness
❌ **Don't skip validation**: Always validate before moving to solutions
❌ **Don't be repetitive**: Vary your prompt selection
❌ **Don't ignore intensity**: Match prompt strength to emotion intensity
❌ **Don't override user's emotion**: Let them feel what they feel

## Common Patterns

### Validation → Support → Action

```python
# 1. Validate
validation = "What you're feeling makes complete sense."

# 2. Support
support = "I'm here with you through this."

# 3. Action (when appropriate)
action = "What would help you feel supported right now?"

response = f"{validation} {support} {action}"
```

### Acknowledgment → Exploration → Integration

```python
# 1. Acknowledge
acknowledge = "I hear what you're saying."

# 2. Explore
explore = "Tell me more about what that's like for you?"

# 3. Integrate
integrate = "What are you learning about yourself through this?"
```

## Conclusion

Conversation prompts provide the building blocks for appropriate, empathetic responses across emotional contexts. Use them as guides, customize them to your needs, and always prioritize genuine, helpful support for your users.

Remember: Prompts are starting points, not rigid scripts. Adapt them to create natural, flowing conversations that truly serve your users' emotional needs.
