# OmniHuman 1.5 Research Summary: Audio + Image to Video for Singing/Rapping

**Last Updated:** 2025-11-12
**Research Focus:** Using OmniHuman 1.5 for generating singing/rapping videos with multiple character support

---

## Table of Contents

1. [Overview](#overview)
2. [Audio + Image to Video Workflow](#audio--image-to-video-workflow)
3. [Multiple Humans & Character Selection](#multiple-humans--character-selection)
4. [Audio-Driven Animation Features](#audio-driven-animation-features)
5. [Best Practices for Singing/Rapping Videos](#best-practices-for-singingrapping-videos)
6. [Technical Specifications](#technical-specifications)
7. [API Integration](#api-integration)
8. [Limitations & Constraints](#limitations--constraints)
9. [Use Cases for Music Videos](#use-cases-for-music-videos)
10. [Implementation Checklist](#implementation-checklist)

---

## Overview

OmniHuman 1.5 is a **film-grade digital human model** that generates video from:
- **Single image + audio + text prompt (optional)**

### Key Capabilities for Singing/Rapping

✅ **Audio-driven animation** automatically syncs lip movements, emotions, and rhythm
✅ **Multi-person support** with ability to select specific characters to sing/rap
✅ **Emotional performance** recognizes and performs emotions from audio
✅ **Rhythmic performance** includes natural pauses and rhythmic variations
✅ **Context awareness** aligns movements with words and intentions

### Quick Links

- **API Documentation:** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman1_5overview
- **Technical Report:** https://omnihuman-lab.github.io/v1_5/
- **Demo Platform:** https://dreamina.capcut.com/ai-tool/home?type=digitalHuman (Avatar Pro Mode)
- **Console:** https://console.byteplus.com/ai

---

## Audio + Image to Video Workflow

### Complete 3-Step Process

```mermaid
graph LR
    A[Image + Audio] --> B[Step 1: Subject Recognition<br/>Optional]
    B --> C[Step 2: Subject Detection<br/>Required for Multi-Person]
    C --> D[Step 3: Video Generation<br/>Required]
    D --> E[Generated Video]
```

### Step 1: Subject Recognition (Optional)

**Purpose:** Identifies if the image contains human/human-like subjects
**When to use:** Quality check before processing; can skip if you know image contains valid subjects

**API Endpoint:**
```
POST https://cv.byteplusapi.com/?Action=CVSubmitTask&Version=2024-06-06
```

**Request Parameters:**
```json
{
  "req_key": "realman_avatar_picture_create_role_omni_cv",
  "image_url": "https://your-image.jpg"
}
```

**Result Query:**
```
CVGetResult with task_id returns binary_data_base64 with recognition results
```

---

### Step 2: Subject Detection (Required for Multi-Person)

**Purpose:** Detects all subjects and returns mask URLs for each character
**Critical for:** Selecting specific characters to sing/rap in multi-person images

**API Endpoint:**
```
POST https://cv.byteplusapi.com/?Action=CVProcess&Version=2024-06-06
```

**Request Parameters:**
```json
{
  "req_key": "realman_avatar_object_detection_cv",
  "image_url": "https://your-image.jpg"
}
```

**Response Format:**
```json
{
  "code": 0,
  "object_detection_result": {
    "mask": {
      "url": [
        "https://mask1.png",
        "https://mask2.png",
        "https://mask3.png"
      ]
    }
  },
  "status": 1
}
```

**Key Details:**
- Mask URLs are **sorted by area** (largest subject first)
- Download masks as `.png` to preview which person each mask represents
- Use these mask URLs in Step 3 to select speakers

---

### Step 3: Video Generation (Required)

**Purpose:** Core generation combining image + audio + prompt

**API Endpoint:**
```
POST https://cv.byteplusapi.com/?Action=CVSubmitTask&Version=2024-06-06
```

**Request Parameters:**
```json
{
  "req_key": "realman_avatar_picture_omni15_cv",
  "image_url": "https://your-image.jpg",
  "audio_url": "https://your-audio.mp3",
  "prompt": "Optional text prompt describing actions",
  "mask_url": "https://mask1.png;https://mask2.png"
}
```

**Result Query:**
```
CVGetResult returns video URL, processing time, cost metrics
```

---

## Multiple Humans & Character Selection

### Support for Multiple Humans: ✅ YES

**How it Works:**

1. **Subject Detection** (Step 2) identifies all humans/subjects in the image
2. Returns **individual mask URLs** for each detected person
3. Pass specific mask_url(s) in video generation to designate speaker(s)

### Character Selection Methods

#### Single Speaker
```json
"mask_url": "https://mask-for-person-1.png"
```

#### Multiple Simultaneous Speakers
```json
"mask_url": "https://mask-person-1.png;https://mask-person-2.png"
```

#### Automatic Selection
Omit `mask_url` parameter - model will automatically choose based on audio semantics

### Best Practices for Multi-Person

- **Preview masks first:** Download and view mask images to identify which mask corresponds to which person
- **Mask order:** Masks are sorted by area size (largest first)
- **Describe interactions:** In prompts, focus on describing interaction modes between characters
- **Example prompt:** "Following the camera, the boy first quickened his pace forward to catch up with the girl and then started talking to her. The girl stopped, turned around with an impatient look, and stared at the boy."

---

## Audio-Driven Animation Features

### Core Capabilities

#### 1. Audio Comprehension
- Characters perform actions related to audio semantics
- **Example:** "swallow the blue pill" → character shows blue pill
- Camera can shift focus based on audio cues

#### 2. Emotional Performance
- Recognizes emotions in audio (anger, joy, sadness, etc.)
- Generates appropriate facial expressions and body language
- **No text prompt required** for basic emotional sync
- Covers full range of dramatic expressions

#### 3. Rhythmic Performance
- Movements synchronized to music rhythm
- **Not limited to lip-sync**—includes natural pauses and rhythmic variations
- Ideal for singer MV production, dance performances, musical drama

#### 4. Context Awareness
- Parses semantic context of audio
- Aligns character movements with words and intentions
- Creates cinematic performances
- Characters appear as if they have their own will

### Audio Understanding Without Prompts

The model can generate high-quality performances **even without text prompts** by:
- Analyzing emotional connotation of the audio
- Understanding semantic meaning
- Matching movements to audio rhythm and mood

---

## Best Practices for Singing/Rapping Videos

### Prompt Structure Template

```
[Camera movement] + [Emotion of speaking character] +
[Speaking state (talking/singing/rapping)] + [Specific actions] +
[Optional: Background events/Other character actions]
```

### Example Prompts for Singing

**Example 1: Emotional Singing Performance**
```
"The camera follows. The man turns around, looks at the camera and walks
forward, singing intoxicatedly. He first touches his collar with both hands,
then spreads his hands and raises his head, looking completely absorbed."
```

**Example 2: Dynamic Performance with Actions**
```
"The camera slowly moves from the side to a medium-front shot. The woman
is singing passionately while walking forward. She first raises her arms
dramatically, then closes her eyes and sways with the rhythm."
```

### Example Prompts for Rapping

**Example 1: Energetic Rap Performance**
```
"The man first makes hand gestures while rapping energetically to the camera.
Then he turns sideways, continues rapping, and points forward with confidence."
```

**Example 2: Multi-Person Rap Battle**
```
"Following the camera, the man on the left raps first, making rhythmic
gestures with his hands. The woman on the right watches him with attitude.
Then she steps forward and starts rapping back at him."
```

### Critical Guidelines

#### DO:

✅ **Always include "singing" or "rapping" in prompt** for better lip-sync performance
✅ Write prompts as natural storytelling (not keyword stacking)
✅ Describe action sequences using "first", "then", "finally"
✅ Control camera movements explicitly ("pan right", "zoom in", "focus on")
✅ Add environmental interactions ("wind blows hair", "lights flicker")
✅ Describe emotional states clearly
✅ Use specific rather than abstract descriptions

#### DON'T:

❌ Describe static features already in image (clothing, jewelry)
❌ Create contradictory instructions
❌ Use excessive negation ("not doing X")
❌ Make characters leave frame repeatedly (causes consistency issues)
❌ Use extreme postures or angles
❌ Describe features in ultra-long shots (face too small)

### Continuous Action Choreography

Use sequential words to describe complex choreography:

```
"The singer first stands still and starts singing softly. Then she gradually
raises her voice and lifts her arms. Finally, she spreads her hands wide and
tilts her head back, fully immersed in the performance."
```

### Camera Movement Control

**Techniques:**
- Describe camera state at the end of movement
- Use words like "rapidly" to improve responsiveness
- Example: "Pan the camera to the right in a circular motion and focus on the man's front face"

**Common Camera Movements:**
- **Pan:** "Pan left/right"
- **Zoom:** "Zoom in/out", "Push in", "Pull back"
- **Follow:** "Camera follows"
- **Focus:** "Focus on [subject]"
- **Circular:** "Pan in a circular motion"

---

## Technical Specifications

### Input Requirements

#### Image

| Specification | Requirement |
|--------------|-------------|
| **Formats** | JPG (recommended), PNG, JFIF |
| **Size** | < 5 MB |
| **Resolution** | < 4096×4096 pixels |
| **Quality** | Higher clarity = better results |
| **Subjects** | Humans, animals, cartoon characters, anthropomorphic figures |

#### Audio

| Specification | Requirement |
|--------------|-------------|
| **Duration** | **< 35 seconds** (hard limit—exceeding causes generation failure) |
| **Formats** | MP3, WAV |
| **Quality** | Clear audio improves emotional recognition |
| **Best Results** | Within 15 seconds for optimal structural stability |

#### Prompt (Optional)

| Specification | Requirement |
|--------------|-------------|
| **Languages** | Chinese, English, Japanese, Korean, Spanish (Mexican), Indonesian |
| **Purpose** | Control scene, movements, camera work |
| **Style** | Natural storytelling, not keyword stacking |

### Output

| Specification | Details |
|--------------|---------|
| **Format** | MP4 |
| **Resolution** | 1080P |
| **Generation Time** | RTF35 (35-second video benchmark; may increase during congestion) |
| **Quality** | Structural stability best within 15 seconds; may decline after |

---

## API Integration

### Authentication

1. Access console: https://console.byteplus.com/ai
2. Generate Access Key/Secret Key (AK/SK)
3. Activate OmniHuman 1.5 in Model Plaza

### Request Headers

```
Service: cv
Region: ap-singapore-1
Authentication: Standard BytePlus authentication with AK/SK
```

### Integration Methods

1. **SDK Integration:** Pre-built SDKs for simplified development
2. **HTTP Direct Access:** Raw API calls for custom implementations

### Workflow Implementation

```python
# Pseudocode for implementation

# Step 1: (Optional) Subject Recognition
recognition_result = submit_task(
    req_key="realman_avatar_picture_create_role_omni_cv",
    image_url="https://your-image.jpg"
)

# Step 2: (Required for multi-person) Subject Detection
detection_result = cv_process(
    req_key="realman_avatar_object_detection_cv",
    image_url="https://your-image.jpg"
)

# Get mask URLs for character selection
mask_urls = detection_result["object_detection_result"]["mask"]["url"]
# mask_urls[0] = largest subject, mask_urls[1] = second largest, etc.

# Step 3: Video Generation
video_task = submit_task(
    req_key="realman_avatar_picture_omni15_cv",
    image_url="https://your-image.jpg",
    audio_url="https://your-audio.mp3",
    prompt="The man sings passionately while walking forward...",
    mask_url=mask_urls[0]  # Select first person to sing
)

# Poll for result
video_result = get_result(task_id=video_task["task_id"])
video_url = video_result["video_url"]
```

### Pricing

**Global Pricing:** $0.12 USD per second of generated video

**Concurrency:** 1 simultaneous request by default (contact BytePlus to increase)

---

## Limitations & Constraints

### Timing Constraints

| Constraint | Details |
|------------|---------|
| **Audio Duration** | **< 35 seconds** (hard limit) |
| **Best Quality Window** | Within 15 seconds |
| **Structural Stability** | May decline after 15 seconds |

### Visual Constraints

| Issue | Solution |
|-------|----------|
| **Faces too small** | Avoid extreme long shots; faces may have non-moving mouths |
| **Extreme postures/angles** | May not generate well; use moderate angles |
| **Characters leaving frame** | Avoid repeated exits/re-entries; causes consistency issues |
| **Consistency changes** | Character off-screen for too long may reappear differently |

### Technical Constraints

| Constraint | Details |
|------------|---------|
| **Audio limit** | Exceeding 35 seconds = generation failure |
| **Randomness** | Single-generation has randomness; complex scenarios may require multiple attempts |
| **Prompt effectiveness** | If prompts don't work well, try omitting them |

---

## Use Cases for Music Videos

### AI MV & Music Content

**Capabilities:**
- **Singer MV production:** Emotional digital singers from single image + song
- **Music video creation:** Dynamic camera movements, performance tension
- **Dance performance display:** Rhythm-driven actions
- **Musical drama segments:** Plot-specific performances

**Supported Styles:**
- Solo lyrical performances
- Concert-style performances
- Lively energetic performances
- Rap and hip-hop videos
- Duets and group performances

### Fantasy Vlog Production

**Use Cases:**
- Time-travel interviews
- Animal selfie live-streaming
- Self-made documentaries
- Celebrity resurrection content

**Features:**
- Rich and realistic picture special effects
- Speed dynamics close to reality
- Fully controllable picture events
- High-naturalness presentation

### UGC Creative Content

**Supported Types:**
- Pixel-style characters (chicks, penguins, etc.)
- Cartoon and anime characters
- Animal performances
- Anthropomorphic characters
- Creative virtual avatars

**Creative Applications:**
- Personal creative videos
- Fun interactive content
- Animal image creation
- Pixel-style content

---

## Implementation Checklist

### Pre-Production

- [ ] **Image Preparation**
  - [ ] High-clarity image (< 5 MB, < 4096×4096)
  - [ ] Clear view of faces (avoid extreme long shots)
  - [ ] Appropriate posture/angle (avoid extremes)
  - [ ] JPG format preferred

- [ ] **Audio Preparation**
  - [ ] Duration < 35 seconds (ideally < 15 seconds for best results)
  - [ ] Clear audio quality
  - [ ] MP3 or WAV format

- [ ] **Prompt Planning** (Optional but Recommended)
  - [ ] Write natural storytelling prompt
  - [ ] Include "singing" or "rapping" keyword
  - [ ] Describe sequential actions ("first", "then", "finally")
  - [ ] Plan camera movements
  - [ ] Describe emotional states

### API Setup

- [ ] **Authentication**
  - [ ] Access BytePlus Vision AI console
  - [ ] Generate AK/SK credentials
  - [ ] Activate OmniHuman 1.5 service

- [ ] **Multi-Person Setup** (if needed)
  - [ ] Run Subject Detection (Step 2)
  - [ ] Download and preview mask images
  - [ ] Identify which mask corresponds to which person
  - [ ] Select appropriate mask URL(s) for speakers

### Generation

- [ ] **Single-Person Generation**
  - [ ] Upload image and audio
  - [ ] Add optional prompt
  - [ ] Submit generation request (Step 3)
  - [ ] Poll for results

- [ ] **Multi-Person Generation**
  - [ ] Run Subject Detection first (Step 2)
  - [ ] Select mask URL(s) for speaker(s)
  - [ ] Upload image, audio, prompt, and mask_url(s)
  - [ ] Submit generation request (Step 3)
  - [ ] Poll for results

### Post-Production

- [ ] **Quality Check**
  - [ ] Verify lip-sync accuracy
  - [ ] Check emotional performance
  - [ ] Review camera movements
  - [ ] Assess overall quality

- [ ] **Iteration** (if needed)
  - [ ] Generate multiple variations for complex scenarios
  - [ ] Try different prompts
  - [ ] Experiment with/without prompts
  - [ ] Adjust audio duration if quality declines

### Troubleshooting

- [ ] **Common Issues**
  - [ ] Face too small → Use closer shots
  - [ ] Mouth not moving → Enlarge face in frame, include "singing/rapping" in prompt
  - [ ] Consistency issues → Avoid character leaving/re-entering frame
  - [ ] Poor quality after 15s → Consider splitting into shorter segments
  - [ ] Generation failure → Check audio duration (must be < 35s)

---

## Key Takeaways for Singing/Rapping Implementation

### Essential Requirements

1. ✅ **Always use "singing" or "rapping" in prompt** for optimal performance
2. ✅ **Multi-person selection requires Step 2 (Subject Detection)**
3. ✅ **Mask URLs enable precise character control** for who sings/raps
4. ✅ **Audio-driven animation is automatic**—model understands rhythm and emotion
5. ✅ **Best results:** Clear audio + high-quality image + descriptive prompt

### Workflow Summary

```
Image → (Optional: Subject Detection for multi-person) →
Audio + Prompt → Video Generation → Download Result
```

### Quick Reference: Character Selection

| Scenario | mask_url Parameter |
|----------|-------------------|
| **Single person (no specification)** | Omit parameter; auto-select |
| **Single person (specify)** | Pass one mask URL |
| **Multiple simultaneous speakers** | Pass multiple mask URLs separated by `;` |
| **Multi-person with one speaker** | Pass mask URL of designated speaker |

### Performance Optimization

| Goal | Strategy |
|------|----------|
| **Better lip-sync** | Include "singing/rapping" in prompt |
| **Better emotions** | Describe emotional states in prompt |
| **Better actions** | Use sequential descriptions ("first", "then") |
| **Better camera work** | Specify camera movements and focus |
| **Better consistency** | Avoid character leaving frame repeatedly |
| **Best quality** | Keep audio < 15 seconds when possible |

---

## Additional Resources

### Official Documentation

- **Overview:** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman1_5overview
- **Step 1 (Recognition):** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_recognition
- **Step 2 (Detection):** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_detection
- **Step 3 (Generation):** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-video_generation

### Support

- **Console:** https://console.byteplus.com/ai
- **Free Trial:** Contact BytePlus account manager
- **Increase Concurrency:** Contact BytePlus representatives
- **Content Filter:** Contact BytePlus representatives

---

**Document Version:** 1.0
**Research Completed:** 2025-11-12
**Next Steps:** Implement proof-of-concept for singing/rapping video generation
