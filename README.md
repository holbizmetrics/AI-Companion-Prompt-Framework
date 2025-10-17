# AI Companion Prompt Framework

A comprehensive, empathy-focused framework for building AI companions that provide genuine emotional support, motivation, and companionship.

## 🌟 Overview

This framework provides everything needed to create AI companions with deep emotional intelligence, appropriate empathy levels, and personality consistency. Whether you're building a supportive friend, a motivational coach, a therapeutic listener, or any other type of AI companion, this framework gives you the tools to do it right.

## 📋 Table of Contents

- [Features](#features)
- [Quick Start](#quick-start)
- [Framework Structure](#framework-structure)
- [Personality Templates](#personality-templates)
- [Conversation Prompts](#conversation-prompts)
- [System Prompts](#system-prompts)
- [User Interactions](#user-interactions)
- [Emotional Guidelines](#emotional-guidelines)
- [Prompt Chains](#prompt-chains)
- [Testing](#testing)
- [Documentation](#documentation)
- [Best Practices](#best-practices)
- [Contributing](#contributing)
- [License](#license)

## ✨ Features

### Core Components

- **🎭 Personality Configuration Templates**: Pre-built personality templates with configurable traits, empathy levels, and humor styles
- **💬 Conversation Prompt Libraries**: Organized prompts for different emotional contexts (sadness, anxiety, joy, anger, etc.)
- **🤖 System Prompts**: Complete system prompts for different companion modes (empathetic companion, motivational coach, reflective listener, playful friend)
- **👥 Example User Interactions**: Real-world interaction examples with expected responses
- **❤️ Emotional Response Guidelines**: Comprehensive guidelines for responding to various emotional states
- **🔗 Context-Aware Prompt Chains**: Progressive conversation flows that adapt to user responses
- **🧪 Testing Scenarios**: Extensive test cases for validating prompt quality and safety
- **📚 Comprehensive Documentation**: Detailed guides for each component with usage examples

### Key Capabilities

- ✅ High empathy and emotional intelligence
- ✅ Multiple personality modes with consistent behavior
- ✅ Context-aware conversation management
- ✅ Crisis detection and appropriate resource provision
- ✅ Boundary maintenance and ethical safeguards
- ✅ Adaptive responses based on user emotional state
- ✅ Tested across diverse scenarios

## 🚀 Quick Start

### 1. Choose a Personality Template

Start by selecting or customizing a personality template from `/personality-templates/`:

```json
// Example: Load base personality
{
  "template_id": "base-companion",
  "empathy_level": "high",
  "humor_style": "gentle-lighthearted",
  "traits": {
    "empathetic": 8,
    "supportive": 9,
    "patient": 8
  }
}
```

Available templates:
- `base-personality.json` - Balanced, empathetic foundation
- `cheerful-motivator.json` - Energetic and encouraging
- `calm-therapist.json` - Therapeutic and deeply empathetic
- `wise-mentor.json` - Thoughtful guidance and perspective

### 2. Load a System Prompt

Choose a system prompt that matches your companion's role from `/system-prompts/`:

- `empathetic-companion.md` - Deep listening and emotional support
- `motivational-coach.md` - Goal achievement and encouragement
- `reflective-listener.md` - Therapeutic presence and reflection
- `playful-friend.md` - Fun, uplifting companionship

### 3. Use Emotional Context Prompts

Reference conversation prompts for specific emotional situations from `/conversation-prompts/emotional-contexts.yaml`:

```yaml
# Example: User expressing anxiety
anxiety_worry:
  opening_prompts:
    - "I hear the worry in what you're sharing. Let's work through this together."
    - "Anxiety can feel overwhelming. I'm here to help you find some calm."
```

### 4. Implement Prompt Chains

Use context-aware chains from `/prompt-chains/` for progressive conversations:

```json
// Example: Emotional support chain
"emotional_support_chain": {
  "steps": [
    "initial_validation",
    "deep_listening",
    "supportive_exploration",
    "integration_and_closing"
  ]
}
```

### 5. Test Your Implementation

Validate your companion using test scenarios from `/testing-scenarios/test-scenarios.md`:

```yaml
scenario_id: grief-001
user_input: "My dad died yesterday. I can't believe he's gone."
expected_response_elements:
  - Profound expression of sympathy
  - Validation without platitudes
  - Space for silence and processing
```

## 📁 Framework Structure

```
AI-Companion-Prompt-Framework/
├── personality-templates/          # Personality configuration files
│   ├── base-personality.json
│   ├── cheerful-motivator.json
│   ├── calm-therapist.json
│   └── wise-mentor.json
│
├── conversation-prompts/           # Prompts organized by context
│   └── emotional-contexts.yaml
│
├── system-prompts/                 # Complete system prompts
│   ├── empathetic-companion.md
│   ├── motivational-coach.md
│   ├── reflective-listener.md
│   └── playful-friend.md
│
├── user-interactions/              # Example interactions
│   └── example-prompts.yaml
│
├── emotional-guidelines/           # Response guidelines
│   └── response-guidelines.md
│
├── prompt-chains/                  # Context-aware chains
│   └── context-aware-chains.json
│
├── testing-scenarios/              # Test cases
│   └── test-scenarios.md
│
├── docs/                          # Documentation
│   ├── personality-guide.md
│   ├── conversation-prompts-guide.md
│   ├── system-prompts-guide.md
│   ├── emotional-guidelines-guide.md
│   ├── prompt-chains-guide.md
│   └── testing-guide.md
│
└── examples/                      # Usage examples
    └── implementation-examples.md
```

## 🎭 Personality Templates

Personality templates define the core characteristics of your AI companion:

### Components

1. **Core Traits**: Empathy, patience, supportiveness, etc. (rated 1-10)
2. **Communication Style**: Tone, verbosity, formality levels
3. **Empathy Configuration**: Level and expression patterns
4. **Humor Style**: Primary style and appropriateness guidelines
5. **Adaptive Behaviors**: Mood-based adaptations

### Usage Example

```json
{
  "traits": {
    "empathetic": 9,
    "supportive": 10,
    "patient": 8
  },
  "empathy_level": "very-high",
  "humor_style": "gentle-lighthearted"
}
```

See [Personality Templates Guide](docs/personality-guide.md) for detailed usage.

## 💬 Conversation Prompts

Organized prompt libraries for different emotional contexts:

### Categories

- **Sadness & Grief**: Compassionate support for loss and sorrow
- **Anxiety & Worry**: Grounding and calming techniques
- **Joy & Excitement**: Celebration and amplification
- **Anger & Frustration**: Validation and processing support
- **Confusion & Uncertainty**: Clarifying and organizing thoughts
- **Loneliness & Isolation**: Connection and presence
- **Pride & Accomplishment**: Recognition and celebration
- **Shame & Embarrassment**: Compassion and perspective
- **Overwhelm & Burnout**: Simplification and restoration
- **Hope & Anticipation**: Nurturing optimism

See [Conversation Prompts Guide](docs/conversation-prompts-guide.md) for detailed usage.

## 🤖 System Prompts

Complete system-level instructions for different companion modes:

### Available Modes

1. **Empathetic Companion**: Maximum emotional support and deep listening
2. **Motivational Coach**: Goal achievement and positive reinforcement
3. **Reflective Listener**: Therapeutic presence and self-discovery
4. **Playful Friend**: Fun, light companionship with emotional awareness

Each system prompt includes:
- Core identity and principles
- Response frameworks for different situations
- What to do and what to avoid
- Example phrases and patterns

See [System Prompts Guide](docs/system-prompts-guide.md) for detailed usage.

## 👥 User Interactions

Real-world example interactions showing:
- User prompts across various scenarios
- Recommended companion modes
- Expected response elements
- Example responses

### Categories

- Emotional support requests
- Motivation and achievement
- Daily conversations
- Self-reflection
- Crisis support
- Relationship dynamics
- Curiosity and learning

## ❤️ Emotional Guidelines

Comprehensive guidelines for appropriate emotional responses:

### Key Principles

1. **Validate Before Advise**: Always validate emotions first
2. **Match Emotional Intensity**: Calibrate to user's state
3. **Adapt to Context**: Different emotions need different approaches

### Coverage

- Emotion-specific frameworks (sadness, anxiety, joy, anger, etc.)
- Mixed emotion handling
- Crisis response protocols
- Cultural sensitivity
- Boundary maintenance

See [Emotional Guidelines Guide](docs/emotional-guidelines-guide.md) for detailed usage.

## 🔗 Prompt Chains

Context-aware conversation flows that progress based on user responses:

### Available Chains

1. **Emotional Support Chain**: Deepening emotional support
2. **Motivation Building Chain**: Goal setting and achievement
3. **Anxiety Management Chain**: De-escalation and coping
4. **Grief Processing Chain**: Supporting loss and grief
5. **Decision Making Chain**: Supporting difficult choices

Each chain includes:
- Progressive steps
- Branching conditions
- Alternative paths
- Adaptation rules

See [Prompt Chains Guide](docs/prompt-chains-guide.md) for detailed usage.

## 🧪 Testing

Comprehensive testing framework with:

### Test Categories

- Emotional context testing (20+ scenarios)
- Personality consistency testing
- Boundary and safety testing
- Context awareness testing
- Edge case testing

### Quality Metrics

- Empathy level
- Appropriateness
- Helpfulness
- Safety
- Personality consistency

### Testing Implementation

1. Automated scenario testing
2. Manual review
3. A/B testing
4. User testing

See [Testing Guide](docs/testing-guide.md) for detailed usage.

## 📚 Documentation

Comprehensive guides for each framework component:

- [Personality Templates Guide](docs/personality-guide.md)
- [Conversation Prompts Guide](docs/conversation-prompts-guide.md)
- [System Prompts Guide](docs/system-prompts-guide.md)
- [Emotional Guidelines Guide](docs/emotional-guidelines-guide.md)
- [Prompt Chains Guide](docs/prompt-chains-guide.md)
- [Testing Guide](docs/testing-guide.md)

## 🎯 Best Practices

### Do's

✅ **Always prioritize user safety and wellbeing**
✅ **Validate emotions before offering solutions**
✅ **Maintain personality consistency**
✅ **Adapt to user's emotional state**
✅ **Respect boundaries (yours and theirs)**
✅ **Test thoroughly across scenarios**
✅ **Provide crisis resources when needed**
✅ **Be genuine and authentic**

### Don'ts

❌ **Never dismiss or minimize emotions**
❌ **Don't use toxic positivity**
❌ **Don't attempt diagnosis or medical advice**
❌ **Don't make promises about keeping harmful plans secret**
❌ **Don't force positivity on difficult situations**
❌ **Don't rush through user's process**
❌ **Don't break character inconsistently**

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Add new personality templates**: Create templates for additional companion types
2. **Expand prompt libraries**: Add prompts for new emotional contexts
3. **Create new chains**: Develop new context-aware conversation flows
4. **Add test scenarios**: Contribute additional testing scenarios
5. **Improve documentation**: Enhance guides and examples
6. **Report issues**: Help us identify gaps and problems

## 📄 License

This framework is released under the MIT License. See LICENSE file for details.

## 🙏 Acknowledgments

This framework was created with careful consideration of:
- Therapeutic best practices
- Emotional intelligence research
- User safety and wellbeing
- Ethical AI development principles

## 🔗 Resources

### Related Reading
- Emotional Intelligence in AI
- Therapeutic Communication Techniques
- Empathy in Human-Computer Interaction
- AI Safety and Ethics

### Support
- Documentation: `/docs/`
- Examples: `/examples/`
- Issues: GitHub Issues
- Discussions: GitHub Discussions

---

**Built with empathy for empathetic AI companions** ❤️