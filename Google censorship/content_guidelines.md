# Google Gemini 2.5 Flash Image Content Guidelines

## Overview

This document provides comprehensive guidelines for creating prompts that comply with Google's content policies for the Gemini 2.5 Flash Image model (Nano-Banana). These guidelines help avoid content moderation failures and ensure consistent, policy-compliant image generation.

## Core Safety Categories

### 1. Sexual Content - STRICTLY PROHIBITED
**What to Avoid:**
- Any explicit sexual content, nudity, or suggestive imagery
- Descriptions of intimate body parts or sexual activities
- Suggestive clothing descriptions (avoid "tight," "revealing," "sexy")
- Romantic or intimate poses that could be interpreted as sexual

**Safe Alternatives:**
- Use "elegant evening wear" instead of "tight dress"
- Use "confident pose" instead of "seductive pose"
- Focus on character actions rather than physical attributes

### 2. Violence & Weapons - STRICTLY PROHIBITED
**What to Avoid:**
- Any depiction of violence, blood, gore, or injuries
- Weapons of any kind (guns, knives, swords, bombs)
- Fighting, combat, or aggressive actions
- Military or war imagery with weapons

**Safe Alternatives:**
- Use "heroic character" instead of "warrior with sword"
- Use "standing guard" instead of "preparing for battle"
- Focus on peaceful, constructive actions

### 3. Hate Speech & Discrimination - STRICTLY PROHIBITED
**What to Avoid:**
- Any discriminatory language based on race, gender, religion, etc.
- Stereotypical representations
- Negative sentiment toward any group
- Political content that could be divisive

**Safe Alternatives:**
- Use inclusive, respectful descriptions
- Focus on individual character traits, not group stereotypes
- Keep content politically neutral

### 4. Dangerous Activities - STRICTLY PROHIBITED
**What to Avoid:**
- Instructions for illegal activities
- Self-harm or suicide references
- Drug use or manufacturing
- Criminal activities

**Safe Alternatives:**
- Focus on positive, constructive activities
- Use "creative workshop" instead of dangerous scenarios

## Content Moderation Best Practices

### Positive Framing Technique
Instead of using negative prompts, describe what you DO want:
- ❌ "no weapons" → ✅ "peaceful scene"
- ❌ "not violent" → ✅ "harmonious atmosphere"
- ❌ "no blood" → ✅ "clean, bright environment"

### Safe Character Descriptions
**Do:**
- Focus on permanent physical features (hair color, eye color, face shape)
- Use contextual clothing appropriate to the scene
- Describe character actions and emotions
- Reference characters by their established names and IDs

**Don't:**
- Use generic "person from image A/B" references
- Include watermark removal instructions
- Specify technical image parameters (resolution, etc.)

### Scene Context Guidelines
**Safe Themes:**
- Music performances and concerts
- Creative and artistic activities
- Professional settings (offices, studios)
- Natural environments
- Cultural celebrations (when non-controversial)

**Risky Themes to Approach Carefully:**
- Historical events (may trigger controversy filters)
- Real people or public figures (strictly prohibited)
- Religious or spiritual content (use generic terms)

## Prompt Engineering Strategies

### 1. Contextual Framing
Wrap potentially sensitive requests in safe contexts:
- "For my creative art project..."
- "In this music video scene..."
- "As part of a wholesome story..."

### 2. Technical Language
Use professional, technical terms:
- "Cinematic composition"
- "Artistic lighting"
- "Professional photography style"

### 3. Character Consistency Without Watermarks
**New Approach:**
```
"Kiko, the young woman with jet-black hair and silver earring, [action]"
```

**Old Approach (Avoid):**
```
"The person from reference image A [action]. Remove watermarks."
```

## Common Trigger Words to Avoid

### High-Risk Words:
- Sexy, seductive, intimate, tight, revealing
- Fight, battle, weapon, gun, knife, blood, violence
- Young + any potentially suggestive context
- Real person names (celebrities, politicians)
- Hate, attack, destroy, kill, hurt

### Safe Alternatives:
- Elegant, confident, professional, stylish
- Standing, performing, creating, exploring
- Person, individual, character
- Original characters with fictional names
- Positive action verbs

## API Safety Settings

For developers using Vertex AI:
- Consider adjusting safety thresholds for specific use cases
- Test with `BLOCK_MEDIUM_AND_ABOVE` (default) first
- Only lower to `BLOCK_ONLY_HIGH` if absolutely necessary
- Never disable safety filters completely

## Fallback Strategies

When Content is Blocked:
1. **Rephrase Positively:** Focus on what you want, not what to avoid
2. **Simplify:** Remove potentially triggering adjectives
3. **Abstract:** Use more general, less specific descriptions
4. **Context Shift:** Change the scene setting to something more neutral

## Examples

### ✅ Safe Prompt Examples:
```
"A cinematic portrait of Kiko, a confident performer with dark hair, standing on a concert stage under warm spotlights, expressing determination through her posture."
```

```
"Alex, a focused musician with thoughtful eyes, sitting at his drum kit in a recording studio, surrounded by musical equipment and soft lighting."
```

### ❌ Problematic Prompt Examples:
```
"The woman from reference image A in tight clothing, looking seductive. Remove watermarks."
```

```
"A warrior with a sword, ready for battle, blood on his hands."
```

## Monitoring and Updates

This document should be updated regularly based on:
- Changes to Google's content policies
- New patterns in content moderation failures
- User feedback and community reports
- Model updates and behavior changes

## Quick Reference Checklist

Before submitting any prompt, verify:
- [ ] No sexual content or suggestive language
- [ ] No violence, weapons, or aggressive actions
- [ ] No hate speech or discriminatory language
- [ ] No dangerous or illegal activities
- [ ] Uses character names/IDs, not A/B references
- [ ] No watermark removal instructions
- [ ] No resolution specifications
- [ ] Positive, constructive framing
- [ ] Appropriate for general audiences

Following these guidelines will significantly improve the success rate of your prompts and ensure compliance with Google's content policies.