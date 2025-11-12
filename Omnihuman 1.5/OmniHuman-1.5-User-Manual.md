# OmniHuman 1.5 User Manual

## 1. Product Introduction

OmniHuman 1.5 generates videos corresponding to the **input image + audio + text prompt** uploaded by the user.

### Core Functions

- Supports input of **images** containing any subject (humans, pets, animations, etc.)
- Generates high-quality lip-synced videos by combining with **audio**
- Strong correlation between the subject's emotions, movements, and the audio
- Allows adjusting the scene, character movements, and camera movements through **text prompts**
- Can specify the speaker/character

### Iterative Advantages

Compared with the previous generation model, OmniHuman 1.5 has:
- Significantly improved motion naturalness and structural stability
- Better movement expressiveness of the subject
- Higher image quality

### Scenario Adaptability

Widely applied in scenarios such as:
- Content expression
- Singing performances
- Plot dialogues
- Multi-person dialogues/duets
- Product interactions
- Animation drama production

Compared with other general video models, it has **outstanding advantages in the plot performance effect of the subject**.

**Resources:**
- **Technical report:** https://omnihuman-lab.github.io/v1_5/
- **Brochure:** https://go.byteplus.com/omnihuman1.5

---

## 2. Product Upgrades Compared with OmniHuman-1

OmniHuman 1.5 introduces several key upgrades:

1. **Text prompt input feature added** - Supporting combined control via user action prompts and audio
2. **Resolution enhanced** - Generated videos in **1080p**
3. **Camera movement plugin added** - Responds to camera movement commands while being controlled by audio and text
4. **Designated speaker selection** - Supports picking of specific characters from multiple objects

---

## 3. Product Advantages

### Rhythmic Performance

The flexibility of the model extends to the music field:
- Algorithm can create emotional digital singers using only a single image and a song
- Movements are not limited to simple lip-sync—they also include natural pauses and rhythmic variations
- Enables flexible adaptation to various styles, from solo lyrical songs to lively concert performances

### Emotional Performance

Using only a single image and a piece of audio, the model can bring digital actors "to life":
- Analyzes the emotional connotation of the audio (no text prompt required)
- Generates engaging, cinematic performances
- Covers a full range of dramatic expressions, from intense anger to sincere confessions

### Context Awareness

By parsing the semantic context of the audio, our model goes beyond simple lip-sync and repetitive movements:
- Enables characters to exhibit authentic emotional changes
- Aligns their movements with their words and intentions
- Characters appear as if they have their own will

### Text-Guided Multimodal Performance

The carefully designed algorithm framework accepts text prompts and demonstrates excellent prompt-following capabilities:
- Precise control over object generation
- Camera movements control
- Specific actions control
- Maintains perfect audio synchronization

### Single/Multi-Person Scene Performance

The model framework extends to complex multi-person scenarios:
- Generates dynamic performances by routing separate audio tracks to the correct character in a single frame
- Supports both the specification of a speaker and joint performances

### Support for Diverse Subjects

The model exhibits excellent robustness and covers an incredibly wide range of subjects:
- Real animals
- Anthropomorphic characters
- Stylized cartoons

> **Note:** The model's generation results have a certain degree of randomness, and not every generation effect may meet your expectations.

---

## 4. Demo Showcases

### Continuous Actions and Movement Control

Describe the order of actions using words like "first" and "then" to achieve complex action choreography. **Remember to include words like "talking" and "singing" for the characters in the prompt.**

**Example 1:**

**Prompt:**
> "The man walks forward first, then stops and puts his hands on his hips while **speaking**. Then, he turns around to look at the explosion behind him, showing his back. The clothes on his back have been blown to pieces."

**Example 2:**

**Prompt:**
> "The camera follows. The man turns around, looks at the camera and walks forward, **singing** intoxicatedly. He first touches his collar with both hands, then spreads his hands and raises his head, looking completely absorbed."

---

### Speak First Then Make a Gesture / Make a Gesture First Then Speak

Not only can the gestures during speaking be choreographed, but gestures can also be inserted before or after speaking.

**Example 1:**

**Prompt:**
> "The man quickly took out the cigarette from his mouth and then started speaking to the camera."

**Example 2:**

**Prompt:**
> "The camera zoomed in. The woman spoke to the camera, and after finishing, she quickly turned around and ran backward."

---

### Multi-character Action Control

The multi-person scenario focuses on describing the **interaction modes** between characters.

⚠️ **For multi-person singing and speaking** (in a multi-person scenario, specifying one person or multiple people simultaneously), **the Subject Detection service is required.** This service will detect all subjects (people, animals, etc.) in the image and return their respective mask_urls. If you want to specify one character to speak, pass the mask_url corresponding to that character. If you want to specify multiple characters (to speak simultaneously), pass a list of mask_urls.

**Example 1:**

**Prompt:**
> "Following the camera, the boy first quickened his pace forward to catch up with the girl and then started talking to her. The girl stopped, turned around with an impatient look, and stared at the boy."

**Example 2:**

**Prompt:**
> "Following the camera, the man holding a torch on the left side of the screen first looked at the others while speaking and then looked back at the camera."

---

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

---

### Emotional Level Control & Micro-expression Control

Describe delicate emotional changes in combination with audio content.

**Example 1:**

**Prompt:**
> "First, the man looked calm and spoke to the camera. Then, he suddenly became nervous and panicked, frowning tightly."

**Example 2:**

**Prompt:**
> "First, the woman rolled her eyes disdainfully and spoke with a contemptuous look. Then, she used her index finger to support her chin with one hand. Finally, she played with the box on the table with a haughty attitude. The background remained still throughout."

---

### Special Effects / Creation Out of Nothing / Adding Visual Elements

Elements not present in the reference image can be added using prompt words.

**Example 1:**

**Prompt:**
> "The father and son looked at the camera in shock, and the son spoke. Then the camera began to slowly zoom in. Distorted ripples appeared in the air. **The father and son were sucked into the depths of the picture like dust and vanished instantly.** The camera froze, leaving only the remote control and the phone on the empty sofa."

**Example 2:**

**Prompt:**
> "The chick **put on sunglasses and held two guns in hand** and spoke evilly."

---

### Camera Focus Control

For scenes where there are different characters in the foreground and background respectively, the camera's focus shift can be controlled through prompt words to match the content's meaning.

**Example 1:**

**Prompt:**
> "The reporter is reporting to the camera. Meanwhile, the king behind suddenly collapses. **The camera focuses on** the collapsed king behind the reporter. The reporter turns around incredibly quickly and speaks while looking behind."

**Example 2:**

**Prompt:**
> "The woman turns around and speaks while looking at the soldier behind. Meanwhile, **the camera focuses on the soldier behind**. Then the woman turns back and continues speaking to the camera."

---

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

## 5. Input and Output Parameters

### Input Parameters

| Input | Specific Requirements & Description |
|-------|-------------------------------------|
| **Image** | **Format:** Common formats such as JPG (JPEG), PNG, JFIF; **JPG is recommended as the preferred format**<br><br>**Size:** Less than 5 MB, resolution less than 4096×4096; the clearer the input image, the better the generation effect.<br><br>**Additional Recommendation:** If a speaker needs to be specified, **subject detection** must be performed first; if not, subject detection can be skipped. |
| **Audio** | **Duration:** The duration must be less than 35 seconds. If the audio exceeds 35 seconds, an error will be generated and the generation will fail; excessive duration may lead to degraded effect) |
| **Prompt (Optional)** | **Supported Languages:** Only Chinese, English, Japanese, Korean, Spanish (Mexican), Indonesian<br><br>**Note:** Optional, used for precise control of the scene, movements, camera movements, etc. |

### Output Parameters

| Output | Description |
|--------|-------------|
| **Video** | **Format:** MP4<br><br>**Resolution:** **1080P**<br><br>**Generation Time:** Defaults to RTF35 (a benchmark for generating 35-second videos). If there is service congestion, the generation time may increase. |

---

## 6. Service Activation Steps on BytePlus Vision AI Console

### Console Operation Guide

BytePlus users can access the [Vision AI] console via https://console.byteplus.com/ai

### Obtaining AK/SK

BytePlus users can navigate to the AK/SK acquisition process by clicking the [Get key] button under [Obtain Secret Key] at the top of the console.

### Activate the Service

1. Click [Model Plaza]
2. Locate the Omnihuman 1.0/1.5 service
3. Click on Omnihuman 1.0/1.5 to enter the activation process
4. Click [Activate Service] to enable the service automatically

### Deactivating the Service

1. Click [Overview] to access the console homepage and view all currently activated services
2. Find [Omnihuman 1.0] [Omnihuman 1.5]
3. Click [Close Service] under the Actions column
4. Confirm to deactivate the service

### Accessing API Documentation

**Method 1:** Go to [Model Plaza], find the corresponding service, and click [Documentation] to view the documentation.

**Method 2:** Go to [Overview], find the activated service, and click [Documentation] to view the documentation.

---

## 7. API Documentation

### BytePlus API Documentation (English)

- [Step 1: Subject Recognition (Optional)](https://bytedance.larkoffice.com/docx/ZKUFddPRGo3CHRxdRvtc5mtBnfb)
- [Step 2: Subject Detection (Optional)](https://bytedance.larkoffice.com/docx/N2fwdARIIoz75sx7Nkrce6nkn0d)
- [Step 3: Video Generation (Required)](https://bytedance.larkoffice.com/docx/EytDdI79WonixmxlgtHckYi4nfg)

### Volcano Engine Official Documentation (Chinese)

**Product Introduction:**
- 中文产品介绍: https://www.volcengine.com/docs/85621/1834143?lang=zh

**Interface Documentation (Chinese):**
- 主体识别: https://www.volcengine.com/docs/85621/1828975?lang=zh
- 主体检测: https://www.volcengine.com/docs/85621/1829011?lang=zh
- 视频生成: https://www.volcengine.com/docs/85621/1829013?lang=zh

**Prompt Guide:**
- [OmniHuman-1.5 Prompt Guide](https://bytedance.larkoffice.com/docx/FdZldIXpdo3a3bxLdYicneYWnOg)

---

## 8. List Price

**Pricing:** 1.2 RMB/second (approximately 0.17 USD/second)

> **Note:** For global deployment pricing, refer to the official BytePlus pricing page or contact BytePlus representatives.
