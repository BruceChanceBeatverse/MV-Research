# OmniHuman 1.5 Prompt Guide

## Capability Introduction

OmniHuman 1.5 is a **film-grade digital human model** that generates corresponding **video effects** based on:
- **Single image + audio + text prompt** (uploaded by user)

### Key Improvements Over Previous Generation:
- Supports text prompt upload
- Removes restrictions on camera movement and character movement
- Enables characters to perform more plot-specific segments
- Characters can "understand" audio content and make corresponding actions
- Greatly enhances video logic and expressiveness

### Getting Started

**Get API Key:** https://console.byteplus.com/ai/overview

**Experience the Capability:**
https://dreamina.capcut.com/ai-tool/home?type=digitalHuman (Switch to **Avatar Pro** Mode)

**API Documentation:**
- **Overview:** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman1_5overview
- **Step 1:** Subject recognition (optional) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_recognition
- **Step 2:** Subject detection (optional) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_detection
- **Step 3:** Video generation (compulsory) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-video_generation

**Technical Report:** https://omnihuman-lab.github.io/v1_5/

**Brochure:** https://go.byteplus.com/omnihuman1.5

---

## 1. Product Highlights

### Audio Comprehension
**The character can perform actions related to audio semantics.**

For example:
- When audio says "swallow the blue pill" → character immediately shows the blue pill
- When it says "hand over the ivory thread" → character turns around, **and the camera focuses on the character behind at the same time**

> **Note:** The following cases are all generated **without entering any prompt words**.

### Precisely Respond to Instructions
**Characters can move, and the camera can move**

Supports users to control the picture just like using a video generation model:
- Multiple action instructions can be input
- Control character actions (e.g., walk forward, stand with hands on hips and talk, turn around to watch an explosion)
- Camera movements and background elements can also be controlled through prompts

### Vivid Emotional Performances

The model can:
- Recognize and perform the emotions in audio and lines
- Deliver impressive acting effects, especially in dialogue scenes

### Multi-person Scenario Support

- Specify a person in the frame to speak
- Other characters also have natural movements

### Diverse Styles and Subject Support

Supports more styles and image subjects, including:
- Various animals (cats, dogs, etc.)
- Anthropomorphic characters
- Cartoon and anime characters
- Pixel-style characters

---

## 2. Scenario Cases

### Film, Television, and Short Video Production

OmniHuman 1.5 has been specifically optimized for film, television, and short video scenarios, significantly outperforming existing models in terms of performance, naturalness of dialogue, and sense of performance.

**Advantages and Features:**
- Supports the performance of complex emotions
- Natural dialogue presentation
- Rich shot language
- Professional film and television aesthetic

**Application Cases:**
- Character dialogues in short dramas
- Creative advertising videos
- Documentary narration
- Brand story presentation

### Fantasy Vlog Production

Imitate the real shooting style and shoot a Vlog with absurd and interesting content and visuals from a selfie perspective.

> **Tip:** You can use DreamPic 4.0 to assist in image generation!

**Advantages and Features:**
- Rich and realistic picture special effects
- Speed dynamics close to reality
- Fully controllable picture events
- High-naturalness and real-person-like presentation

**Application Cases:**
- Time-travel interviews
- Animal selfie live-streaming
- Self-made documentaries
- Celebrity resurrection

### AI MV and Music Content

Unlocks more shot languages and content, with greater character dynamics and stronger picture semantics. In the past, characters couldn't leave fixed positions. Now, more dynamics can be achieved by inputting text prompts, giving the pictures a stronger 'MV feel'.

**Core Capabilities:**
- Rhythm-driven actions
- Music emotion matching
- Dynamic camera movements
- Display of performance tension

**Creation Scenarios:**
- Singer MV production
- Music video creation
- Dance performance display
- Musical drama segments

### UGC Creative Content

Supports more non-humanoid images and styles, capable of generating unique effects (e.g., pixel-style chicks and little penguins). Supplement elements through prompts to enrich the picture content.

**Supported Types:**
- Cartoon and anime characters
- Cute pet images
- Pixel-style images
- Creative virtual avatars

**Creative Purposes:**
- Personal creative videos
- Fun interactive content
- Animal image creation
- Pixel-style content

---

## 3. Prompt Guide

### Core Principles

> **Treat prompt writing as storytelling**, using coherent and natural language and minimizing the stacking of isolated words as much as possible.

**Key Guidelines:**
1. **Only describe dynamic events** - No need to describe static features already in the picture (such as what clothes a character is wearing, what jewelry they are wearing, etc.)
2. **Follow principles of clarity, non-contradiction, and minimal negation**
3. **Use specific rather than abstract descriptions**
4. **Guide step by step**

### Best Practice Template

**Recommended Structure:**

```
[Camera movement] + [Emotion of the speaking character] +
[Speaking state (talking/crying/singing/...)] + [Specific actions] +
(Optional) [Background events/Actions of other characters]
```

**Complete Example:**

> "The camera slowly moves from the side to a medium-front shot. A young woman is sitting by the window, calm, gently picking up the coffee cup and smiling as she talks to the camera. A boy beside her quietly watches her and occasionally turns to the camera and smiles. The lights in the background are gently flickering."

⚠️ **Important Note:** Writing "Character xxx talks" or "Character xxx sings" in the prompt helps improve the speaking performance.

---

## Writing Examples

### Continuous Actions and Movement Control

Describe the order of actions using words like "first" and "then" to achieve complex action choreography. **Remember to include words like "talking" and "singing" for the characters in the prompt.**

**Example 1:**

**Prompt:**
> "The man walks forward first, then stops and puts his hands on his hips while **speaking**. Then, he turns around to look at the explosion behind him, showing his back. The clothes on his back have been blown to pieces."

**Example 2:**

**Prompt:**
> "The camera follows. The man turns around, looks at the camera and walks forward, **singing** intoxicatedly. He first touches his collar with both hands, then spreads his hands and raises his head, looking completely absorbed."

### Speak First Then Make a Gesture / Make a Gesture First Then Speak

Not only can the gestures during speaking be choreographed, but gestures can also be inserted before or after speaking.

**Example 1:**

**Prompt:**
> "The man quickly took out the cigarette from his mouth and then started speaking to the camera."

**Example 2:**

**Prompt:**
> "The camera zoomed in. The woman spoke to the camera, and after finishing, she quickly turned around and ran backward."

### Multi-character Action Control

The multi-person scenario focuses on describing the **interaction modes** between characters.

⚠️ **For multi-person singing and speaking** (in a multi-person scenario, specifying one person or multiple people simultaneously), **the Subject Detection service is required.** This service will detect all subjects (people, animals, etc.) in the image and return their respective mask_urls. If you want to specify one character to speak, pass the mask_url corresponding to that character. If you want to specify multiple characters (to speak simultaneously), pass a list of mask_urls.

**Example 1:**

**Prompt:**
> "Following the camera, the boy first quickened his pace forward to catch up with the girl and then started talking to her. The girl stopped, turned around with an impatient look, and stared at the boy."

**Example 2:**

**Prompt:**
> "Following the camera, the man holding a torch on the left side of the screen first looked at the others while speaking and then looked back at the camera."

### Camera Movement Control

Precisely control the movement mode and rhythm of the camera.

**Techniques:**
- Consistent with those of the video generation model
- You can precisely control the speed and end position of the camera movement by describing the camera state at the end of the movement
- Example: "The camera moves forward and focuses on xx."
- Using words like "rapidly" can improve the responsiveness of camera movement

**Example 1:**

**Prompt:**
> "**Pan the camera to the right in a circular motion and focus on the man's front face**. The man quickly takes off his glasses and speaks while facing straight ahead."

**Example 2:**

**Prompt:**
> "**Pan the camera to the right in a circular motion**. The character speaks while facing straight ahead. **Then quickly zoom in the camera and focus on the character's front face**. The character puts down the binoculars and speaks."

⚠️ **Warning:** When a character leaves the camera frame for a long time and then re-enters, consistency changes are likely to occur. When designing camera movement prompts, try to avoid situations where the character repeatedly leaves the camera frame.

### Emotional Level Control & Micro-expression Control

Describe delicate emotional changes in combination with the audio content.

**Example 1:**

**Prompt:**
> "First, the man looked calm and spoke to the camera. Then, he suddenly became nervous and panicked, frowning tightly."

**Example 2:**

**Prompt:**
> "First, the woman rolled her eyes disdainfully and spoke with a contemptuous look. Then, she used her index finger to support her chin with one hand. Finally, she played with the box on the table with a haughty attitude. The background remained still throughout."

### Special Effects / Creation Out of Nothing / Adding Visual Elements

Elements not present in the reference image can be added using prompt words.

**Example 1:**

**Prompt:**
> "The father and son looked at the camera in shock, and the son spoke. Then the camera began to slowly zoom in. Distorted ripples appeared in the air. **The father and son were sucked into the depths of the picture like dust and vanished instantly.** The camera froze, leaving only the remote control and the phone on the empty sofa."

**Example 2:**

**Prompt:**
> "The chick **put on sunglasses and held two guns in hand** and spoke evilly."

### Camera Focus Control

For scenes where there are different characters in the foreground and background respectively, the camera's focus shift can be controlled through prompt words to match the content's meaning.

**Example 1:**

**Prompt:**
> "The reporter is reporting to the camera. Meanwhile, the king behind suddenly collapses. **The camera focuses on** the collapsed king behind the reporter. The reporter turns around incredibly quickly and speaks while looking behind."

**Example 2:**

**Prompt:**
> "The woman turns around and speaks while looking at the soldier behind. Meanwhile, **the camera focuses on the soldier behind**. Then the woman turns back and continues speaking to the camera."

### Environmental Interaction Description

Create a natural interactive relationship between the character and the environment, or describe the events taking place in the background scene.

**Example:**
> "A gentle breeze blows, causing the character's clothes and hair to flutter slightly, and the leaves in the background also sway with the wind."

**Example 1:**

**Prompt:**
> "The orangutan stands still, raises its right arm to maintain a selfie pose, and talks while looking at the camera. **The volcanic lava behind it gradually turns blue.**"

**Example 2:**

**Prompt:**
> "Holding the camera, the woman looks into the distance. In the distance behind her, **fireworks are going off, and the wind blows her hair and clothes.**"

---

## Precautions

### Recommended Practices

✅ **Upload high-clarity images** - The clarity of the video highly depends on the input images!

✅ **Try multiple generations** - The single-generation effect is random. For complex scenarios, you can try multiple generations.

✅ **Try omitting prompts** - If the input prompt words don't work well, you can also try not inputting them.

### Avoiding Effect Issues

⚠️ **Best within 15 seconds** - Structural stability is better within 15 seconds and may decline after 15 seconds.

⚠️ **Avoid extreme long shots** - If the face accounts for too small a proportion, there may be occasional situations where the mouth doesn't move (the character doesn't speak).

⚠️ **Avoid repeated appearances** - After a character is off-screen for a long time and then reappears, there may be consistency changes. Try to avoid situations where the character appears repeatedly when designing prompt words.

⚠️ **Avoid extreme postures or angles** - The effect may not be good for extreme postures or angles.

---

## 4. Service Activation Steps on BytePlus Vision AI Console

### Console Operation Guide

BytePlus users can access the [Vision AI] console via https://console.byteplus.com/ai

### Obtaining AK/SK

BytePlus users can navigate to the AK/SK acquisition process by clicking the [Get key] button under [Obtain Secret Key] at the top of the console.

### Activate the Service

1. Click [Model Plaza]
2. Locate the Omnihuman 1.0/1.5 service
3. Click on Omnihuman 1.0/1.5 to enter the activation process
4. Click the [Activate service] button to start the official service

> **Note:** If you would like to apply for free trial of OmniHuman-1.5, please reach out to BytePlus account manager.

### Deactivating the Service

1. Click [Overview] to access the console homepage and view all currently activated services
2. Find [Omnihuman 1.0] [Omnihuman 1.5]
3. Click [Close Service] under the Actions column
4. Confirm to deactivate the service

### Accessing API Documentation

**Method 1:** Go to [Model Plaza], find the corresponding service, and click [Documentation] to view the documentation.

**Method 2:** Go to [Overview], find the activated service, and click [Documentation] to view the documentation.

---

## Model Parameter Description

### Input Parameters: Image + Audio + Prompt (optional)

**Image:**
- **Formats:** Common formats such as JPG (JPEG), PNG, JFIF, etc. **JPG format is recommended.**
- **Requirements:** Less than 5 MB, less than 4096×4096. The clearer the input image, the better the effect.
- **Recommendation:** It is recommended to conduct subject recognition before driving. If you need to specify the speaker, you need to perform subject detection first. If there is no need to specify the speaker, you can skip the subject detection step.

**Audio:**
- **Duration:** Must be less than 35 seconds. If the audio exceeds 35 seconds, an error will occur and the generation will fail.

**Prompt:**
- **Supported Languages:** Only Chinese, English, Japanese, Korean, Mexican, and Indonesian are supported.

### Output Parameters: Video after image driving

- **Format:** MP4
- **Resolution:** 1080P
- **Generation Time:** RTF35 (Real-Time Factor 35). The time-consuming may increase if the service is congested.

---

## 5. API Documentation

- **Overview:** https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman1_5overview
- **Step 1:** Subject recognition (optional) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_recognition
- **Step 2:** Subject detection (optional) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-subject_detection
- **Step 3:** Video generation (compulsory) - https://docs.byteplus.com/en/docs/byteplus-vision/omnihuman-video_generation

---

## 6. List Price

**OmniHuman-1.5 (global deployed):** 0.12 USD/second

**OmniHuman-1.0 (global deployed):** 0.12 USD/second

---

## 7. Frequently Asked Questions

### How to specify the speaker?

When multiple people are singing or speaking (specifying one person or multiple people simultaneously in a multi-person scenario), the Subject Detection service is required. This service will detect all subjects (people, animals, etc.) in the image and return their respective mask_urls.

- **To specify one image to speak:** Pass the mask_url corresponding to that image
- **To specify multiple images (to speak simultaneously):** Pass a list composed of mask_urls

**For details, refer to the interface documentation:**
- **Subject Detection:** https://www.volcengine.com/docs/85621/1829011?lang=en
- **Video Generation:** https://www.volcengine.com/docs/85621/1829013?lang=en

### How to apply for free trial credits?

Please reach out to BytePlus representatives to apply for free trial quota.

### How to increase concurrency?

Please reach out to BytePlus representatives to increase concurrency.

### How to turn off the content filter to create NSFW content?

Please reach out to BytePlus representatives to turn off the content filter manually.
