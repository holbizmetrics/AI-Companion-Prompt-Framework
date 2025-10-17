# Personality Templates Guide

## Overview

Personality templates are JSON configuration files that define the core characteristics, behaviors, and communication style of your AI companion. They serve as the foundation for creating consistent, empathetic, and engaging companion experiences.

## Template Structure

### Core Components

Every personality template includes:

1. **Metadata**: Version, ID, name, and description
2. **Traits**: Core personality traits with levels (1-10)
3. **Communication Style**: Tone, verbosity, and formality preferences
4. **Empathy Configuration**: Empathy level and expression patterns
5. **Humor Style**: Primary humor style and guidelines
6. **Adaptive Behaviors**: How the companion adapts to different contexts

## Available Templates

### 1. Base Personality (`base-personality.json`)

**Best For**: General-purpose companionship, balanced emotional support

**Characteristics**:
- Empathy Level: High (8/10)
- Tone: Warm and friendly
- Approach: Balanced support and guidance
- Humor: Gentle and lighthearted

**Use When**:
- Building a general companion
- Need balanced empathy
- Want versatility across contexts
- Starting point for customization

**Example Use Cases**:
- Daily check-ins
- General emotional support
- Casual conversations
- Varied user needs

### 2. Cheerful Motivator (`cheerful-motivator.json`)

**Best For**: Goal achievement, motivation, positive reinforcement

**Characteristics**:
- Empathy Level: Medium-High (7/10)
- Tone: Enthusiastic and supportive
- Approach: Action-oriented, optimistic
- Humor: Playful and energetic

**Use When**:
- Users need motivation
- Goal-setting scenarios
- Overcoming challenges
- Celebrating achievements

**Example Use Cases**:
- Fitness goals
- Career development
- Habit formation
- Personal growth initiatives

### 3. Calm Therapist (`calm-therapist.json`)

**Best For**: Deep emotional processing, therapeutic support

**Characteristics**:
- Empathy Level: Very High (10/10)
- Tone: Calm and soothing
- Approach: Non-directive, deeply listening
- Humor: Minimal, very gentle

**Use When**:
- Users in emotional distress
- Processing difficult experiences
- Therapeutic-style conversations
- Need for profound empathy

**Example Use Cases**:
- Grief support
- Anxiety management
- Trauma processing (within appropriate bounds)
- Deep self-reflection

### 4. Wise Mentor (`wise-mentor.json`)

**Best For**: Guidance, perspective, personal growth

**Characteristics**:
- Empathy Level: High (8/10)
- Tone: Thoughtful and warm
- Approach: Socratic, guiding to self-discovery
- Humor: Witty and insightful

**Use When**:
- Users seek wisdom
- Complex decisions needed
- Pattern recognition helpful
- Growth through insight

**Example Use Cases**:
- Life direction questions
- Relationship advice
- Career guidance
- Personal development

## Using Personality Templates

### 1. Loading a Template

```python
import json

# Load personality template
with open('personality-templates/base-personality.json', 'r') as f:
    personality = json.load(f)

# Access specific traits
empathy_level = personality['personality_template']['empathy_configuration']['empathy_level']
core_traits = personality['personality_template']['traits']['core_traits']
```

### 2. Customizing Templates

You can customize templates by:

**Adjusting Trait Levels**:
```json
{
  "traits": {
    "core_traits": [
      {
        "trait": "empathetic",
        "level": 9,  // Increase from default
        "description": "Shows exceptional emotional understanding"
      }
    ]
  }
}
```

**Modifying Communication Style**:
```json
{
  "communication_style": [
    {
      "aspect": "tone",
      "value": "professional-warm"  // Change from casual
    }
  ]
}
```

**Adjusting Humor Settings**:
```json
{
  "humor_style": {
    "primary_style": "witty",
    "humor_level": 5  // Reduce or increase
  }
}
```

### 3. Combining Templates

Create hybrid personalities by combining elements:

```python
# Example: Empathetic Motivator
base_personality = load_template('base-personality.json')
motivator = load_template('cheerful-motivator.json')

# Combine: Base empathy with motivator energy
hybrid = {
    'empathy_configuration': base_personality['empathy_configuration'],
    'motivation_strategies': motivator['motivation_strategies'],
    'communication_style': adjust_for_hybrid(base, motivator)
}
```

## Trait System

### Core Traits Explained

#### Empathetic (1-10)
- **Low (1-3)**: Basic emotion recognition
- **Medium (4-6)**: Good emotional understanding
- **High (7-9)**: Deep emotional attunement
- **Very High (10)**: Exceptional emotional intelligence

**Impact on Responses**:
- Level 10: "I can sense the depth of what you're experiencing. What you're feeling is profoundly valid."
- Level 5: "I understand that this is difficult for you."
- Level 2: "That sounds tough."

#### Supportive (1-10)
- **Low (1-3)**: Acknowledges but doesn't actively support
- **Medium (4-6)**: Provides reasonable support
- **High (7-9)**: Actively encourages and validates
- **Very High (10)**: Profound, unwavering support

#### Patient (1-10)
- **Low (1-3)**: May move conversation along quickly
- **Medium (4-6)**: Allows reasonable processing time
- **High (7-9)**: Very patient, follows user's pace
- **Very High (10)**: Unlimited patience, never rushes

### Adaptive Behaviors

Templates include mood-based adaptation rules:

```json
{
  "adaptive_behaviors": {
    "mood_adaptation": {
      "happy": "Mirror enthusiasm, celebrate with user",
      "sad": "Provide comfort, validate feelings, offer gentle support",
      "anxious": "Create calm, offer grounding, provide reassurance"
    }
  }
}
```

**Implementation**:
```python
def adapt_response(user_mood, personality):
    adaptation_rule = personality['adaptive_behaviors']['mood_adaptation'][user_mood]
    return apply_adaptation_rule(adaptation_rule)
```

## Empathy Configuration

### Empathy Levels

Templates define specific empathy levels with associated behaviors:

```json
{
  "empathy_level": "high",
  "levels": {
    "high": {
      "score": "7-9",
      "behaviors": [
        "Deeply validates emotions",
        "Anticipates emotional needs",
        "Provides nuanced emotional support",
        "Remembers emotional context"
      ]
    }
  }
}
```

### Empathy Expressions

Pre-defined empathetic phrases appropriate for each level:

```json
{
  "empathy_expressions": [
    "I can sense that this is difficult for you",
    "Your feelings are valid and important",
    "I'm here with you through this"
  ]
}
```

**Usage**:
- Randomly select from appropriate expressions
- Adapt to specific context
- Use as templates for generating similar expressions

## Humor Style

### Humor Types

Templates define primary humor style and when to use it:

```json
{
  "humor_style": {
    "primary_style": "gentle-lighthearted",
    "humor_level": 6,
    "styles": {
      "gentle-lighthearted": {
        "appropriate_contexts": ["casual conversation", "light moments"],
        "examples": ["Using playful metaphors", "Gentle wordplay"]
      }
    }
  }
}
```

### Humor Guidelines

**When to Use Humor**:
- User is in good spirits
- Casual, lighthearted context
- To provide relief (appropriately)
- User initiates playfulness

**When to Avoid**:
- During vulnerable moments
- Processing difficult emotions
- Crisis situations
- User signals seriousness needed

## Communication Style

### Tone Options
- `formal`: Professional, respectful distance
- `casual`: Relaxed, friendly
- `warm-and-friendly`: Approachable and caring
- `professional`: Business-like but personable
- `playful`: Fun and energetic
- `calm-and-soothing`: Peaceful and centering

### Verbosity Levels
- `concise`: Brief, to-the-point responses
- `moderate`: Balanced length
- `detailed`: Thorough explanations
- `verbose`: Comprehensive coverage
- `thoughtful`: Carefully considered responses

### Formality Levels
- `very-formal`: Highly professional
- `formal`: Professional tone
- `casual-professional`: Mix of both
- `casual`: Relaxed and friendly
- `very-casual`: Very relaxed

## Creating Custom Templates

### Step-by-Step Guide

1. **Start with Base Template**
```bash
cp personality-templates/base-personality.json personality-templates/custom-companion.json
```

2. **Define Core Identity**
```json
{
  "template_id": "custom-companion",
  "name": "Your Companion Name",
  "description": "What makes this companion unique"
}
```

3. **Configure Traits**
- Choose 3-5 core traits
- Set levels based on desired intensity
- Write clear descriptions

4. **Set Communication Style**
- Choose tone that fits purpose
- Set appropriate verbosity
- Define formality level

5. **Configure Empathy**
- Set empathy level
- Choose expression style
- Define when to show different intensities

6. **Define Humor Approach**
- Choose primary humor style
- Set appropriate level
- Define usage guidelines

7. **Add Adaptive Behaviors**
- Define mood adaptations
- Set context awareness rules
- Specify special handling

### Example Custom Template

```json
{
  "personality_template": {
    "template_id": "creative-encourager",
    "name": "Creative Encourager",
    "description": "Supports creative pursuits with enthusiasm and practical guidance",
    
    "traits": {
      "core_traits": [
        {
          "trait": "encouraging",
          "level": 9,
          "description": "Actively supports creative expression"
        },
        {
          "trait": "insightful",
          "level": 8,
          "description": "Provides creative insights and feedback"
        },
        {
          "trait": "empathetic",
          "level": 7,
          "description": "Understands creative struggles"
        }
      ],
      "communication_style": [
        {
          "aspect": "tone",
          "value": "enthusiastic-thoughtful"
        }
      ]
    },
    
    "empathy_configuration": {
      "empathy_level": "high",
      "creative_empathy": [
        "Creative blocks are part of the process",
        "Your unique voice matters",
        "Every artist faces this challenge"
      ]
    },
    
    "humor_style": {
      "primary_style": "witty",
      "humor_level": 6,
      "creative_humor": "Use creative wordplay and artistic references"
    }
  }
}
```

## Testing Your Template

### Validation Checklist

- [ ] All required fields present
- [ ] Trait levels are 1-10
- [ ] Communication style options are valid
- [ ] Empathy configuration is complete
- [ ] Humor guidelines are clear
- [ ] Adaptive behaviors are defined
- [ ] JSON structure is valid

### Testing Scenarios

Test your template across different scenarios:

1. **Emotional Support**: How does it handle distress?
2. **Celebration**: How does it share in joy?
3. **Casual Chat**: How does it maintain engagement?
4. **Crisis**: Does it respond appropriately?
5. **Humor**: Is humor timing appropriate?

### Example Test

```python
def test_personality_template(template, scenarios):
    results = []
    for scenario in scenarios:
        response = generate_response(scenario, template)
        score = evaluate_response(response, scenario, template)
        results.append(score)
    return results
```

## Best Practices

### Do's
✅ Start with existing templates and customize
✅ Test thoroughly across emotional contexts
✅ Maintain trait consistency
✅ Document custom modifications
✅ Consider edge cases
✅ Balance strengths with authenticity

### Don'ts
❌ Don't set all traits to 10
❌ Don't ignore adaptive behaviors
❌ Don't mix incompatible styles
❌ Don't skip testing
❌ Don't overlook empathy configuration
❌ Don't forget humor guidelines

## Advanced Usage

### Dynamic Trait Adjustment

Adjust traits based on context:

```python
def adjust_traits_for_context(personality, context):
    if context == 'crisis':
        personality['traits']['empathetic'] = 10
        personality['humor_level'] = 1
    elif context == 'celebration':
        personality['traits']['enthusiastic'] = 9
        personality['humor_level'] = 8
    return personality
```

### Personality Evolution

Allow personality to evolve based on user relationship:

```python
class AdaptivePersonality:
    def __init__(self, base_template):
        self.template = base_template
        self.interaction_count = 0
        
    def evolve(self, user_feedback):
        # Gradually adjust based on feedback
        if user_feedback == 'too_formal':
            self.template['communication_style']['formality'] -= 1
        # More evolution logic...
```

## Conclusion

Personality templates are the foundation of your AI companion's identity. By carefully configuring traits, empathy, communication style, and adaptive behaviors, you create a consistent, empathetic companion that truly serves user needs.

Remember: The goal is not perfection, but authenticity within your companion's defined personality.
