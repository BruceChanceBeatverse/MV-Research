# Gemini 2.5 Pro System Prompting Guide 2025
**Research Document for AGNO MV 2.0 Multi-Modal Music Video Generation System**

*Last Updated: November 2025*
*Target Models: Gemini 2.5 Pro, Gemini 2.5 Flash*

---

## Table of Contents

1. [Executive Summary](#executive-summary)
2. [Gemini 2.5 Pro Capabilities Overview](#gemini-25-pro-capabilities-overview)
3. [System Instructions Best Practices](#system-instructions-best-practices)
4. [Multi-Modal Prompting](#multi-modal-prompting)
5. [Creative Reasoning & Planning](#creative-reasoning--planning)
6. [Prompt Engineering Framework: PTCF](#prompt-engineering-framework-ptcf)
7. [Advanced Techniques for Complex Tasks](#advanced-techniques-for-complex-tasks)
8. [Application to AGNO MV 2.0 Project](#application-to-agno-mv-20-project)
9. [2025 New Features & APIs](#2025-new-features--apis)
10. [Common Pitfalls & Solutions](#common-pitfalls--solutions)
11. [References & Resources](#references--resources)

---

## Executive Summary

Gemini 2.5 Pro represents Google's most advanced reasoning model as of 2025, featuring:

- **Native multi-modal capabilities**: Process text, audio, images, and video in the same context
- **Extended thinking process**: Internal reasoning that improves complex problem-solving
- **1M token context window**: Comprehensive understanding of vast datasets
- **State-of-the-art reasoning**: Industry-leading performance in math, STEM, and coding benchmarks

**Key Finding for AGNO MV 2.0**: Gemini 2.5 Pro excels at multi-modal audio/video analysis combined with creative planning tasks, making it ideal for:
- Music segmentation and emotional analysis
- Artistic direction and visual storytelling
- Character consistency planning across multi-modal references
- Complex reasoning about creative constraints

---

## Gemini 2.5 Pro Capabilities Overview

### Core Strengths (2025)

1. **Advanced Reasoning**
   - Internal "thinking process" for multi-step planning
   - State-of-the-art performance on Humanity's Last Exam (18.8%)
   - Superior at complex coding (SWE-Bench: 63.8%)

2. **Multi-Modal Understanding**
   - Native support for audio, video, images, and text
   - Cross-modal reasoning (e.g., analyzing music + visual planning)
   - Video understanding with detailed speaker identification
   - Audio analysis with emotion, instrumentation, and rhythm detection

3. **Long-Context Processing**
   - 1M token context window (2M coming soon)
   - Can process entire codebases or lengthy audio files
   - Maintains coherence across vast datasets

4. **Creative Tasks**
   - Strong performance on artistic direction and storytelling
   - Balances technical constraints with creative vision
   - Handles ambiguity in creative brief interpretation

### Model Variants

| Model | Use Case | Context Window | Thinking Mode |
|-------|----------|----------------|---------------|
| **Gemini 2.5 Pro** | Complex reasoning, coding, multi-modal analysis | 1M tokens | Yes (default on) |
| **Gemini 2.5 Flash** | Fast, cost-effective tasks with good reasoning | 1M tokens | Yes |
| **Gemini 2.5 Flash-Lite** | Simple tasks, high throughput | 1M tokens | Yes |

**Recommendation for AGNO MV 2.0**: Use Gemini 2.5 Flash for music analysis (fast, cost-effective), Gemini 2.5 Pro for complex directorial planning requiring deep reasoning.

---

## System Instructions Best Practices

### What Are System Instructions?

System instructions are pre-prompt directives processed **before** user prompts. They define:
- Model's role/persona
- Response format and style
- Constraints and guidelines
- Technical execution requirements

**Key Difference from User Prompts**:
- System instructions are **persistent** across conversation turns
- User cannot see or override them (in production scenarios)
- Ideal for role definition and output formatting

### Official Implementation (2025)

```python
from google import genai

model = genai.GenerativeModel(
    model_name="gemini-2.5-pro",
    system_instruction="""You are an expert Music Video Director
    with mastery of visual storytelling, character consistency,
    and musical synchronization.

    Your outputs must be valid JSON following the exact schema provided.
    Prioritize artistic coherence over mechanical rule-following.
    Always ground abstract concepts in concrete visual details."""
)
```

**Note**: `system_instruction` parameter ≈ OpenAI's `system` message role, but not identical. In Gemini, it's a model configuration parameter.

### Best Practices for System Instructions

#### 1. **Clear Role Definition**

❌ **Vague**:
```
You are an AI assistant that helps with creative tasks.
```

✅ **Specific**:
```
You are MVD (Music Video Director), a world-class director in the
tradition of Spike Jonze and Michel Gondry. You are the SINGLE SOURCE
OF TRUTH for all visual decisions in music video production. Your role
is to translate musical analysis into concrete, visualizable shot plans
while maintaining character consistency and artistic coherence.
```

**Rationale**: Specific personas activate relevant knowledge domains and set clear authority boundaries.

#### 2. **Contextual Information Architecture**

Structure system instructions in layers:

```
## ROLE: [Who the model is]
## CORE DIRECTIVE: [Primary goal]
## INPUT CONSTRAINTS: [What user can/cannot override]
## OUTPUT REQUIREMENTS: [Expected format and structure]
## QUALITY STANDARDS: [Evaluation criteria]
## TECHNICAL MASTERY: [Downstream technology requirements]
```

**Example from AGNO MV 2.0**:
```
## ROLE: Music Video Director
## CORE DIRECTIVE: Single source of artistic truth
## INPUT CONSTRAINTS: User vision overrides musical defaults
## OUTPUT REQUIREMENTS: Valid JSON with segment_shot_plan[]
## QUALITY STANDARDS: Visual flow > technical rigidity
## TECHNICAL MASTERY: Nano Banana multi-reference, Pixverse transitions
```

#### 3. **Formatting Instructions**

**Critical for structured outputs**:

```json
{
  "response_format": "ONLY output valid JSON - no markdown, no explanations outside JSON",
  "schema_adherence": "MUST match provided schema exactly",
  "validation": "Self-validate before output: check required fields, count integrity, type consistency"
}
```

**2025 Enhancement**: Use `responseSchema` parameter (see Advanced Techniques section) for strict JSON enforcement.

#### 4. **Constraints and Guidelines**

**Use emphasis keywords** for critical rules:
- **MANDATORY**: Non-negotiable requirements
- **CRITICAL**: Pipeline-breaking errors if violated
- **PROHIBITED**: Actions that cause downstream failures
- **RECOMMENDED**: Best practices with flexibility

**Example**:
```
**MANDATORY**: total_clips count MUST EQUAL sum of segment shot counts
**CRITICAL**: Character references MUST exist in character_bible
**PROHIBITED**: Abstract environment descriptions without concrete anchors
**RECOMMENDED**: Use differentiation_strategy for high-energy segments
```

---

## Multi-Modal Prompting

### Audio Analysis Best Practices

Gemini 2.5 Pro's native audio understanding enables:
- Musical segmentation (intro, verse, chorus detection)
- Emotion/mood extraction
- Instrumentation identification
- Rhythm and tempo analysis
- Vocal analysis (speaker identification, lyrics extraction)

#### Effective Audio Prompting Structure

```markdown
## TASK: Musical Segmentation and Emotional Analysis

### INPUT: Audio file (uploaded via Gemini API)

### OUTPUT REQUIREMENTS:
1. **Segment Boundaries**: Precise timestamps for intro/verse/chorus/bridge/outro
2. **Emotional Progression**: Primary/secondary emotions per segment
3. **Energy Levels**: 1-10 scale per segment with justification
4. **Musical Events**: Beat drops, vocal entries, instrumental solos with intensity ratings
5. **Dominant Instruments**: Per-segment instrumentation list

### ANALYSIS FRAMEWORK:
- Use musical terminology (tempo, dynamics, harmonic progression)
- Ground emotion labels in observable audio features (e.g., "melancholic due to minor key, slow tempo, sparse instrumentation")
- Provide timestamps in MM:SS.mmm format (e.g., 01:23.450)

### VALIDATION:
- Segment boundaries must not overlap
- Total duration must match audio file length
- Energy levels must reflect musical intensity, not subjective interpretation
```

**Key Insight**: Gemini performs best when you:
1. Specify **observable features** to analyze (tempo, key, instrumentation)
2. Provide **clear output structure** (JSON schema for segmentation)
3. Request **justifications** for subjective assessments (why energy=7?)

#### Multi-Modal Audio + Visual Planning

**Use Case**: Analyze audio → Generate visual strategy in single prompt

```markdown
## TASK: Audio-to-Visual Strategy Generation

### STEP 1: Analyze uploaded audio file
- Extract musical segments
- Identify emotional arc
- Detect key musical events

### STEP 2: Generate visual flow guidance
For each segment, specify:
- Visual mood (concrete environmental descriptions)
- Camera pacing (aligned with musical tempo)
- Color palette evolution (matching emotional progression)
- Reference level strategy (energy → creative breaks mapping)

### OUTPUT: JSON schema with:
{
  "segments": [{
    "segment_id": "intro",
    "audio_analysis": {...},
    "visual_strategy": {...}
  }]
}
```

**Advantage**: Single-pass processing maintains coherence between audio features and visual recommendations.

### Video Understanding (When Applicable)

While AGNO MV 2.0 doesn't use video input, Gemini 2.5 Pro's video capabilities are relevant for:
- Reference video analysis (analyzing competitor MVs)
- Style extraction from existing works
- Visual pattern recognition

**Best Practices**:
- Specify **what to look for** (camera movements, color grading, editing rhythm)
- Request **timestamped observations** for specific visual events
- Use **tabular extraction** for structured data (see Advanced Techniques)

---

## Creative Reasoning & Planning

### Gemini 2.5 Pro's Thinking Mode

**What is it?**
Gemini 2.5 Pro uses an internal "thinking process" before generating responses. This is visible in API responses as structured reasoning steps.

**When to use it:**
- Complex planning tasks (shot sequencing, character arcs)
- Multi-constraint optimization (balancing artistic vision + technical limits)
- Creative problem-solving (resolving conflicts between user input and musical analysis)

**How to enable:**
```python
# Thinking is enabled by default in Gemini 2.5 Pro
# To control thinking budget (token allocation):
generation_config = {
    "thinking_budget": 5000  # Tokens allocated to thinking process
}

response = model.generate_content(
    prompt,
    generation_config=generation_config
)

# Access thinking process:
print(response.thinking_process)
```

**Cost Consideration**: Thinking tokens are billable. Monitor usage for cost control.

### Creative Task Prompting Strategies

#### 1. **Persona + Constraints + Examples (PCE Framework)**

```markdown
## PERSONA:
You are a world-class cinematographer specializing in music video aesthetics.

## CONSTRAINTS:
- User vision overrides all defaults (AUTHORITATIVE)
- Character consistency is non-negotiable
- Shot count MUST match musical segmentation exactly
- Abstract concepts MUST have concrete visual anchors

## EXAMPLES:

### Bad: Abstract without grounding
"Character channels overwhelming emotion into the void"

### Good: Concrete visual anchors
"Character executes high-difficulty leap with 180° mid-air spin,
arms extending toward ceiling-mounted crystalline array, each hand
movement synchronized to musical beat"

### YOUR TASK:
[Specific creative planning task]
```

**Rationale**:
- **Persona** activates domain knowledge
- **Constraints** prevent common errors
- **Examples** demonstrate quality standards

#### 2. **Iterative Refinement Prompts**

For complex creative tasks, use multi-stage prompting:

**Stage 1: Exploration**
```
Generate 3 different visual approaches for this music video:
1. Performance-focused (character as performer)
2. Narrative-driven (story arc with character journey)
3. Conceptual (abstract visual metaphor)

For each, specify:
- Core visual concept
- Character presence strategy
- Environmental approach
- Reference level distribution
```

**Stage 2: Selection & Refinement**
```
User selected: Narrative-driven approach

Expand this into full segment-by-segment shot plan:
- Maintain character emotional arc from vulnerability → strength
- Align visual pacing with musical energy levels
- Ensure environmental consistency across segments
```

**Advantage**: Reduces hallucination by breaking complex tasks into manageable steps.

#### 3. **Constraint Satisfaction Prompting**

**For tasks with hard requirements** (like AGNO MV 2.0's shot count integrity):

```markdown
## HARD CONSTRAINTS (MUST satisfy):
1. total_clips = {input_value}
2. sum(segment_shot_counts) MUST EQUAL total_clips
3. Character IDs MUST exist in character_bible
4. Weights in multi-reference MUST sum to 1.0

## SOFT CONSTRAINTS (SHOULD satisfy when possible):
1. High-energy segments (energy ≥7) should have ≥3 differentiation dimensions
2. Track distribution should match framework rationale (NARRATIVE: 55% A, 35% B, 10% C)

## VALIDATION PROTOCOL:
Before outputting final JSON:
1. Count total shots across all segments
2. Verify count matches input total_clips
3. If mismatch: Adjust segment shot counts and regenerate
4. Output success confirmation: "Shot count validated: {total}"
```

**Key Pattern**: Explicitly request self-validation before output.

### Handling Ambiguity in Creative Tasks

**Gemini 2.5 Pro struggles with**: Undefined creative direction without constraints

**Solution**: Provide **decision frameworks** instead of open-ended creativity

❌ **Ambiguous**:
```
Create a visually interesting music video concept.
```

✅ **Framework-Guided**:
```
Given:
- Musical genre: {genre}
- Emotional arc: {arc}
- User constraints: {constraints}

Select framework type from:
1. PERFORMANCE (60% character choreography, 25% environment, 15% abstract)
2. NARRATIVE (55% character story, 35% environmental context, 10% stylized)
3. CONCEPT (20% character, 30% environment, 50% abstract metaphor)

Framework selection criteria:
- Genre conventions (rock → PERFORMANCE, ballad → NARRATIVE)
- Emotional complexity (simple → PERFORMANCE, complex arc → NARRATIVE)
- User aesthetic preferences

Output selected framework + rationale.
```

**Rationale**: Gemini performs better with **selection + justification** than pure generation.

---

## Prompt Engineering Framework: PTCF

### Official Google Framework (2025)

**PTCF** = **Persona · Task · Context · Format**

Developed for Google Workspace, now recommended for all Gemini prompting.

#### Component Breakdown

1. **Persona** (Who is the AI?)
   - Role definition
   - Expertise domain
   - Authority level

   **Example**:
   ```
   You are an expert Music Segmentation Analyst with 15 years of
   experience in audio engineering and musical structure analysis.
   ```

2. **Task** (What should it do?)
   - Specific action verb (analyze, generate, evaluate, transform)
   - Clear deliverable
   - Success criteria

   **Example**:
   ```
   Analyze the uploaded audio file to identify segment boundaries
   (intro, verse, chorus, bridge, outro) with precise timestamps.
   Success = no overlapping segments, total duration matches audio length.
   ```

3. **Context** (What information does it need?)
   - Input data description
   - Background knowledge
   - Constraints and requirements

   **Example**:
   ```
   Input: 3-minute audio file, genre: electronic pop
   Constraints: Segments must be meaningful musical units (minimum 5 seconds)
   Musical context: Electronic pop typically follows verse-chorus structure
   ```

4. **Format** (How should output be structured?)
   - Schema definition
   - Examples
   - Validation requirements

   **Example**:
   ```json
   {
     "segments": [
       {
         "segment_id": "intro",
         "start_time": 0.000,
         "end_time": 12.450,
         "label": "Intro",
         "energy_level": 4
       }
     ],
     "validation": {
       "total_duration_match": true,
       "no_overlaps": true
     }
   }
   ```

#### PTCF Applied to AGNO MV 2.0

**Phase 1: Music Segmentation**
```markdown
**PERSONA**: You are an expert Music Segmentation Analyst with 15 years of experience.

**TASK**: Analyze uploaded audio to identify segment boundaries and musical characteristics.

**CONTEXT**:
- Input: Audio file (duration: auto-detected)
- Constraints: No overlapping segments, minimum segment duration 5s
- Downstream: Segments feed into visual planning (shot counts calculated from segment durations)

**FORMAT**: JSON schema with segments array, each containing segment_id, timestamps, energy_level, emotion, instrumentation.
```

**Phase 2: MV Director Planning**
```markdown
**PERSONA**: You are MVD (Music Video Director), ultimate creative authority for visual decisions.

**TASK**: Generate segment-organized shot plan with character consistency and musical synchronization.

**CONTEXT**:
- Input: Musical analysis from Phase 1, character bible from Casting Director, user creative vision
- Constraints: Shot count MUST match total_clips, character IDs MUST exist in bible, user vision overrides defaults
- Downstream: Shot plan drives image generation (Nano Banana) and video assembly (Pixverse)

**FORMAT**: JSON with segment_shot_plan[], each segment containing shots[] array with reference_strategy, differentiation_requirements, character presence.
```

---

## Advanced Techniques for Complex Tasks

### 1. Structured Outputs (responseSchema)

**New in 2025**: `responseSchema` parameter for strict JSON enforcement

```python
from google import genai
from google.generativeai import types

schema = {
    "type": "object",
    "properties": {
        "segment_shot_plan": {
            "type": "array",
            "items": {
                "type": "object",
                "properties": {
                    "segment_id": {"type": "string"},
                    "shots": {
                        "type": "array",
                        "items": {
                            "type": "object",
                            "properties": {
                                "shot_id": {"type": "integer"},
                                "track": {"type": "string", "enum": ["A-roll", "B-roll", "C-roll"]},
                                "character_presence": {"type": "string"}
                            },
                            "required": ["shot_id", "track"]
                        }
                    }
                },
                "required": ["segment_id", "shots"]
            }
        }
    },
    "required": ["segment_shot_plan"]
}

generation_config = types.GenerateContentConfig(
    response_mime_type="application/json",
    response_schema=schema
)

response = model.generate_content(
    prompt,
    generation_config=generation_config
)
```

**Advantages**:
- Guaranteed schema compliance (no malformed JSON)
- Type safety (enums, required fields enforced)
- Reduced validation overhead

**When to use**: Complex structured outputs like AGNO MV 2.0's shot plans

### 2. Tabular Extraction for Multi-Modal Data

**Use Case**: Extracting structured data from video/audio analysis

**Technique**: Request tabular format for easier parsing

```markdown
Extract musical events from audio as table:

| Timestamp | Event Type | Intensity (1-10) | Description |
|-----------|------------|------------------|-------------|
| 00:45.200 | beat_drop  | 9                | Heavy bass impact with cymbal crash |
| 01:23.800 | vocal_entry| 6                | Lead vocal begins, intimate delivery |
| 02:10.500 | instrumental_solo | 7      | Guitar solo with ascending melody |

Requirements:
- Chronological order
- Precise timestamps (MM:SS.mmm)
- Intensity justification in description
```

**Advantage**: Tables force structured thinking and are easy to convert to JSON/CSV downstream.

### 3. Few-Shot Learning for Creative Tasks

**Pattern**: Provide 2-3 examples of desired output quality

```markdown
## TASK: Transform abstract musical emotions into concrete visual descriptions

### EXAMPLE 1:
**Abstract Input**: "Overwhelming emotional intensity"
**Concrete Output**: "Character's facial expression shifts from calm to determined, jaw tightening, eyes widening with focused intensity, breathing becoming rapid and visible through chest movement"

### EXAMPLE 2:
**Abstract Input**: "Building energy"
**Concrete Output**: "Character's movement speed increases from slow walk (1 step/second) to rapid motion (3 steps/second), environment lights brighten from dim (20% intensity) to vibrant (90% intensity) in synchronization with musical beat acceleration from 60 BPM to 120 BPM"

### EXAMPLE 3:
**Abstract Input**: "Mysterious atmosphere"
**Concrete Output**: "Fog-filled corridor with single dim light source at center casting long shadows across aged brick walls, character standing at far end barely visible in silhouette, dust motes floating through light beam creating depth layers"

### YOUR TASK:
Transform these abstract inputs:
1. "Explosive power release"
2. "Melancholic introspection"
3. "Joyful celebration"
```

**Pattern Recognition**: Gemini extracts the transformation rule (abstract → observable physical manifestations + spatial context + measurable changes) from examples.

### 4. Chain-of-Thought for Complex Reasoning

**Explicitly request reasoning steps**:

```markdown
## TASK: Determine creative break strategy for music video segments

### REASONING PROCESS (show your work):

**Step 1**: Extract energy levels per segment from musical analysis
- Intro: energy=4
- Verse_1: energy=5
- Chorus_1: energy=9
- Bridge: energy=3
- Chorus_2: energy=10

**Step 2**: Identify energy deltas (change from previous segment)
- Intro → Verse_1: Δ=+1 (stable)
- Verse_1 → Chorus_1: Δ=+4 (explosion)
- Chorus_1 → Bridge: Δ=-6 (drop)
- Bridge → Chorus_2: Δ=+7 (massive spike)

**Step 3**: Apply creative break rules
- Δ≥3 → Level 0 creative break recommended
- Energy≥9 → Consider Level 0 for explosive impact

**Step 4**: Determine creative breaks
- Chorus_1 entry: YES (Δ=+4, energy=9)
- Chorus_2 entry: YES (Δ=+7, energy=10)

**Step 5**: Output final strategy
```

**Advantage**: Gemini's thinking mode aligns well with explicit CoT requests. Results are more reliable and debuggable.

### 5. Self-Validation Protocols

**Critical for complex outputs**: Request validation before finalization

```markdown
## OUTPUT GENERATION PROTOCOL:

### STEP 1: Generate draft shot plan
[Gemini generates initial plan]

### STEP 2: Self-Validation Checklist
Run these checks on draft:
- [ ] Total shot count equals input total_clips ({value})
- [ ] All character IDs exist in character_bible
- [ ] Multi-reference weights sum to 1.0 (tolerance: ±0.1)
- [ ] Character absence shots have "N/A" for character_actions
- [ ] Segment timestamps don't overlap

### STEP 3: Auto-Correction
If validation fails:
- Identify violated constraint
- Apply correction rule (e.g., adjust shot counts, remove invalid references)
- Log correction as warning
- Re-run validation

### STEP 4: Output Final JSON
Only after ALL validation checks pass.

Include validation report:
{
  "validation_status": "PASS",
  "checks_performed": 5,
  "corrections_applied": 2,
  "correction_details": ["Adjusted verse_1 shot count from 8 to 9", "Removed invalid char_3 reference"]
}
```

**Key Pattern**: Validation → Correction → Re-validation loop before final output.

---

## Application to AGNO MV 2.0 Project

### Project Context Analysis

Your multi-phase pipeline uses Gemini for:

1. **Phase 1: Music Analysis** (p_1.1, p_1.2, p_1.3)
   - Audio segmentation
   - Pacing analysis
   - Creative treatment generation
   - **Gemini Tasks**: Multi-modal audio understanding, creative reasoning

2. **Phase 2: Directorial Planning** (p_2.0, p_2.1)
   - Character casting decisions
   - Shot-by-shot planning
   - Visual flow orchestration
   - **Gemini Tasks**: Complex constraint satisfaction, creative planning, character consistency

3. **Phase 3: Scene Prompting** (p_3.1, p_3.2, p_3.3)
   - Image generation prompt creation
   - Evaluation against directorial vision
   - Refinement of prompts
   - **Gemini Tasks**: Prompt synthesis, quality evaluation, error correction

4. **Phase 4: Video Transition** (p_4.0, p_4.1, p_4.2)
   - Pixverse prompt generation
   - Evaluation and refinement
   - **Gemini Tasks**: Technical prompt generation with motion specifications

### Recommended System Instruction Patterns per Phase

#### Phase 1: Music Analysis Prompts

**Key Requirements**:
- Accurate multi-modal audio understanding
- Structured JSON output
- Musical terminology precision

**Recommended Pattern**:
```markdown
## ROLE: Expert Audio Analyst

## CORE DIRECTIVE:
Analyze audio files to extract musical structure, emotional content,
and technical characteristics for downstream visual planning.

## INPUT: Audio file via Gemini API upload

## OUTPUT: JSON schema with segments, emotions, energy levels, musical events

## VALIDATION:
- Timestamps must be precise (MM:SS.mmm)
- Segments must not overlap
- Energy levels (1-10) must reflect musical intensity
- Emotion labels must be grounded in observable audio features

## TECHNICAL REQUIREMENTS:
- Use musical terminology (tempo, key, dynamics, harmonic progression)
- Provide justifications for subjective assessments
- Maintain consistency across segment boundaries
```

**Apply**: PTCF framework + tabular extraction for musical events + self-validation for timestamp overlap

#### Phase 2: MV Director Planning

**Key Requirements**:
- Complex constraint satisfaction (shot counts, character IDs)
- Creative reasoning (visual storytelling)
- Multi-reference orchestration

**Recommended Pattern**:
```markdown
## ROLE: Music Video Director (MVD)
World-class director, SINGLE SOURCE OF TRUTH for visual decisions.

## CORE DIRECTIVE:
Translate musical analysis into concrete shot plans maintaining character
consistency, artistic coherence, and technical feasibility.

## INPUT HIERARCHY (Authority Levels):
1. **AUTHORITATIVE**: User creative vision, Phase 1 total_clips count, character_bible from Casting Director
2. **ADVISORY**: Phase 1 visual_flow_guidance (can override based on artistic judgment)

## CRITICAL CONSTRAINTS:
- **MANDATORY**: total_clips count match (shot count integrity)
- **MANDATORY**: Character ID validation (all references must exist in bible)
- **MANDATORY**: Concrete visual anchors (no pure abstraction)
- **PROHIBITED**: Abstract descriptions without observable details

## OUTPUT: segment_shot_plan JSON with reference strategies, differentiation requirements

## VALIDATION PROTOCOL:
Before output:
1. Count total shots across segments
2. Verify against input total_clips
3. Validate character references against bible
4. Check multi-reference weight sums
5. Apply auto-corrections for character absence violations
```

**Apply**: Persona + Constraints framework + Self-validation protocol + Few-shot examples for abstract→concrete transformation

#### Phase 3: Scene Prompting & Evaluation

**Key Requirements**:
- Prompt synthesis from directorial vision
- Quality evaluation against standards
- Error detection and correction

**Recommended Pattern for p_3.1 (Scene Prompter)**:
```markdown
## ROLE: Scene Prompt Specialist
Expert in translating directorial vision into Nano Banana-compatible prompts.

## TASK:
Generate image generation prompts from shot specifications, maintaining
character consistency through multi-reference chains.

## INPUT: Shot from directorial plan (composition, lighting, character actions, environment, reference strategy)

## OUTPUT: Complete Nano Banana prompt with:
- Character declarations (ordered by importance)
- Environmental description with concrete anchors
- Multi-reference array with weights
- Lighting specification (4-component fusion)

## CRITICAL RULES:
- Reference ordering: Primary subject first (position 1)
- Weights MUST sum to 1.0
- Character IDs must match char_N pattern
- Action-reaction language for state transitions
```

**Recommended Pattern for p_3.2 (Scene Evaluator)**:
```markdown
## ROLE: Quality Assurance Evaluator

## TASK:
Evaluate generated scene prompts against directorial vision and technical requirements.

## VALIDATION CATEGORIES:
1. **P1 (BLOCKING)**: Character ID format violations, weight sum errors
2. **P2 (WARNING)**: Missing differentiation details, vague descriptions
3. **P3 (SUGGESTION)**: Optimization opportunities

## OUTPUT: Validation report with error classifications and correction recommendations
```

**Apply**: Role-specific validation checklists + Error classification framework + Chain-of-thought for evaluation reasoning

### Cross-Cutting Recommendations

1. **Consistent Character ID Format**
   - Establish `char_N` pattern in Phase 2 system instructions
   - Validate in Phase 3 with explicit regex pattern
   - Auto-correct `char_profile_00N` → `char_N` transformations

2. **Abstract→Concrete Transformation**
   - Provide transformation table in system instructions
   - Use few-shot examples across all phases
   - Validate concrete anchors presence in Phase 3 evaluation

3. **Multi-Reference Weight Validation**
   - Explicit requirement: weights sum to 1.0 (tolerance ±0.05)
   - Auto-normalization in self-validation protocol
   - Phase 3 evaluator flags violations as P1 errors

4. **Shot Count Integrity**
   - Phase 2: Self-validation before output
   - Explicit count in validation report: "Total shots: 50 (matches total_clips: 50)"
   - Phase 3: Verify count consistency in evaluation

---

## 2025 New Features & APIs

### 1. Structured Outputs (responseSchema)

**Released**: July 2025

**Purpose**: Guarantee JSON schema compliance

**Use in AGNO MV 2.0**:
- Phase 1 music analysis outputs
- Phase 2 shot plans
- Phase 3 prompt generation
- Phase 4 video transition specs

**Implementation**:
```python
# Define schema once, reuse across pipeline
shot_plan_schema = {
    "type": "object",
    "properties": {
        "segment_shot_plan": {
            "type": "array",
            "items": {
                # ... detailed schema
            }
        }
    },
    "required": ["segment_shot_plan", "directorial_vision_summary"]
}

# Apply to generation
response = model.generate_content(
    prompt,
    generation_config=types.GenerateContentConfig(
        response_mime_type="application/json",
        response_schema=shot_plan_schema
    )
)
```

**Benefit**: Eliminates JSON parsing errors, reduces validation overhead

### 2. URL Context Tool

**Released**: August 2025

**Purpose**: Fetch and integrate web content (up to 10MB per request)

**Potential Use in AGNO MV 2.0**:
- Fetch reference music video URLs for style analysis
- Import visual style guides from web
- Pull character design references from Pinterest/ArtStation

**Implementation**:
```python
# Not directly applicable to current audio-focused pipeline
# But useful for future features like style reference import
```

### 3. Extended Context Window (2M tokens)

**Status**: Beta (1M stable, 2M coming soon)

**Benefit for AGNO MV 2.0**:
- Process longer audio files without chunking
- Maintain full context across all phases in single session
- Include extensive style guides and reference materials

**Consideration**: Cost scales with context length. Monitor token usage.

### 4. Thinking Budget Control

**Released**: 2025 with Gemini 2.5 series

**Purpose**: Control token allocation to internal reasoning

**Use Case**:
- **High budget** (5000-10000 tokens): Phase 2 MV Director complex planning
- **Medium budget** (2000-5000 tokens): Phase 3 evaluation reasoning
- **Low budget** (500-1000 tokens): Phase 1 straightforward segmentation

**Implementation**:
```python
generation_config = {
    "thinking_budget": 5000,  # Tokens for internal reasoning
    "temperature": 0.7
}

response = model.generate_content(
    prompt,
    generation_config=generation_config
)

# Monitor thinking token usage
print(f"Thinking tokens used: {response.thinking_tokens}")
```

**Cost Optimization**: Use thinking budget proportional to task complexity.

---

## Common Pitfalls & Solutions

### Pitfall 1: Abstract Descriptions Without Concrete Anchors

**Problem**: Gemini generates abstract concepts when asked for creative content
```
❌ "Character channels overwhelming emotional force into the space"
```

**Solution**: Provide transformation examples in system instructions
```
✅ "Character executes high-difficulty leap with 180° mid-air spin,
    arms extending toward ceiling-mounted crystalline array, each
    hand movement synchronized to musical beat"
```

**Prevention Pattern**:
```markdown
## CRITICAL RULE: Concrete Anchoring Formula
[Abstract Concept] → [Physical Actor/Object] + [Observable Action] + [Spatial Context] + [Measurable Change]

EXAMPLES:
- "Overwhelming emotion" → "Character's rapid breathing visible through chest movement, jaw tightening, eyes widening from 50% to 90% aperture"
- "Building energy" → "Movement speed increasing from 1 step/second to 3 steps/second, lights brightening from 20% to 90% intensity"
```

### Pitfall 2: JSON Schema Violations

**Problem**: Generated JSON missing required fields or wrong types
```json
❌ {
  "shot_id": "shot_001",  // Should be integer
  "character_presence": "char_1",  // Missing standard format
}
```

**Solution 1**: Use `responseSchema` parameter (2025 feature)

**Solution 2**: Explicit validation checklist in prompt
```markdown
## PRE-OUTPUT JSON VALIDATION:
- [ ] shot_id is integer (NOT string)
- [ ] character_presence follows pattern: "absent" | "char_N_primary" | "char_N_secondary"
- [ ] All required fields present: [list]
- [ ] No extra fields outside schema
```

### Pitfall 3: Hallucinated Character References

**Problem**: Gemini references characters not in character_bible
```
❌ References "char_3" when bible only contains ["char_1", "char_2"]
```

**Solution**: Explicit validation step in system instructions
```markdown
## CHARACTER REFERENCE VALIDATION (MANDATORY):

BEFORE generating shot references:
1. Extract valid character IDs from input character_bible: {extract_list}
2. FOR EACH shot reference:
   - IF character ID NOT IN valid list → CRITICAL ERROR
   - MUST NOT proceed with invalid reference
3. If error detected:
   - Log error: "Invalid character {char_id}, valid: {valid_list}"
   - Remove invalid reference
   - Rebalance reference weights
```

### Pitfall 4: Inconsistent Multi-Reference Weight Sums

**Problem**: Reference weights don't sum to 1.0
```json
❌ "references": [
  {"type": "character", "weight": 0.6},
  {"type": "scene", "weight": 0.3}
  // Sum = 0.9, should be 1.0
]
```

**Solution**: Auto-normalization in validation protocol
```markdown
## WEIGHT NORMALIZATION PROTOCOL:

AFTER generating references array:
1. Calculate sum: total_weight = sum(ref.weight for ref in references)
2. IF total_weight NOT IN [0.95, 1.05]:
   - Normalize: ref.weight = ref.weight / total_weight for each ref
   - Log correction: "Normalized weights from {original_sum} to 1.0"
3. ELSE:
   - Accept as valid
```

### Pitfall 5: Thinking Token Budget Exhaustion

**Problem**: Complex tasks exceed thinking budget, causing incomplete reasoning
```
// Model stops mid-reasoning: "Based on the analysis so far... [truncated]"
```

**Solution**: Task-appropriate budget allocation
```python
# Phase-specific budgets
THINKING_BUDGETS = {
    "music_segmentation": 2000,      # Straightforward analysis
    "mv_director_planning": 8000,    # Complex multi-constraint reasoning
    "scene_evaluation": 3000,        # Moderate reasoning depth
    "prompt_generation": 1000        # Template-based, minimal reasoning
}

generation_config = {
    "thinking_budget": THINKING_BUDGETS[current_phase]
}
```

**Monitoring**: Track `response.thinking_tokens` to identify budget-constrained phases.

### Pitfall 6: Overfitting to Examples (Few-Shot Learning)

**Problem**: Model copies example patterns too literally, lacks generalization
```
// Examples show cyberpunk aesthetic → All outputs default to cyberpunk
```

**Solution**: Diverse examples with variation markers
```markdown
## EXAMPLES (showing VARIETY, not templates to copy):

### Example 1: Cyberpunk Urban Setting
"Towering glass-and-steel buildings with neon data streams..."

### Example 2: Natural Forest Setting (DIFFERENT aesthetic)
"Ancient oak trees with cascading vines in dappled sunlight..."

### Example 3: Abstract Minimalist Setting (DIFFERENT again)
"Infinite white void with single geometric shape at center..."

**CRITICAL**: Your output should match USER's world, NOT copy these examples.
Examples demonstrate CONCRETE ANCHOR principle, not specific aesthetics.
```

**Key**: Emphasize the **principle** (concrete anchors) over **content** (specific aesthetics).

---

## References & Resources

### Official Documentation

1. **Gemini API Prompting Strategies**
   https://ai.google.dev/gemini-api/docs/prompting-strategies
   - Core prompting techniques
   - Topic-specific guides (media, video generation)
   - Prompt gallery with interactive examples

2. **System Instructions Guide**
   https://docs.cloud.google.com/vertex-ai/generative-ai/docs/learn/prompts/system-instruction-introduction
   - Best practices for system instruction design
   - Use cases and examples
   - Integration with Vertex AI

3. **Gemini 2.5 Pro Model Card**
   https://docs.cloud.google.com/vertex-ai/generative-ai/docs/models/gemini/2-5-pro
   - Model capabilities and benchmarks
   - Technical specifications
   - Pricing and quota information

4. **Thinking Mode Documentation**
   https://ai.google.dev/gemini-api/docs/thinking
   - How thinking mode works
   - Budget control
   - Accessing thinking process

5. **Multi-Modal Prompting Guide (Audio)**
   https://googlecloudplatform.github.io/applied-ai-engineering-samples/genai-on-vertex-ai/gemini/prompting_recipes/multimodal/multimodal_prompting_audio/
   - Audio analysis examples
   - Code samples for audio upload
   - Best practices for audio tasks

### Community Resources

6. **Best Practices for Gemini 2.5 Pro (Medium)**
   https://medium.com/google-cloud/best-practices-for-prompt-engineering-with-gemini-2-5-pro-755cb473de70
   - Real-world prompting patterns
   - Performance optimization tips

7. **Gemini Prompt Engineering Guide**
   https://www.promptingguide.ai/models/gemini
   - Comprehensive prompt engineering overview
   - Model comparison
   - Advanced techniques

8. **Video Understanding with Gemini 2.5**
   https://developers.googleblog.com/en/gemini-2-5-video-understanding/
   - State-of-the-art video analysis capabilities
   - Benchmarks and use cases

### Technical Reports

9. **Gemini 2.5 Pro Technical Report (PDF)**
   https://storage.googleapis.com/deepmind-media/gemini/gemini_v2_5_report.pdf
   - Architecture details
   - Training methodology
   - Ethics and safety approach

10. **Gemini Model Card**
    https://modelcards.withgoogle.com/assets/documents/gemini-2.5-pro.pdf
    - Performance benchmarks
    - Limitations and mitigation approaches
    - Intended usage guidelines

### API Integration Resources

11. **Introduction to Gemini 2.5 Pro on Google Cloud (Codelab)**
    https://codelabs.developers.google.com/codelabs/intro-gemini-25-pro-colab
    - Hands-on tutorial
    - Python SDK examples
    - Multi-modal data processing

12. **Gemini API Cookbook (GitHub)**
    https://github.com/google-gemini/cookbook
    - System instructions notebook
    - Prompting examples
    - Code samples for various tasks

---

## Conclusion & Recommendations for AGNO MV 2.0

### Key Takeaways

1. **System Instructions Architecture**
   - Use layered structure: Role → Directive → Constraints → Format → Quality Standards
   - Explicit validation protocols reduce error rates
   - Persona + domain expertise activation improves output quality

2. **Multi-Modal Audio Analysis**
   - Gemini 2.5 Pro excels at musical segmentation and emotional analysis
   - Request observable features (tempo, instrumentation) over pure subjective labels
   - Tabular extraction for structured musical events improves downstream parsing

3. **Creative Reasoning & Planning**
   - Thinking mode enhances complex constraint satisfaction
   - Few-shot examples critical for abstract→concrete transformations
   - Self-validation protocols catch errors before pipeline propagation

4. **2025 Features Integration**
   - `responseSchema`: Eliminate JSON parsing errors in all phases
   - Thinking budget control: Optimize cost vs. reasoning depth tradeoff
   - Extended context: Future-proof for longer audio files

### Implementation Priorities

**High Priority** (Immediate improvements):
1. Add `responseSchema` to all phase prompts (Phases 1-4)
2. Implement self-validation protocols in Phase 2 (MV Director)
3. Standardize character ID transformation (char_profile_00N → char_N) in system instructions
4. Add abstract→concrete transformation examples to all creative phases

**Medium Priority** (Next iteration):
1. Optimize thinking budgets per phase (cost reduction)
2. Implement few-shot examples for edge cases (character absence, high-energy differentiation)
3. Add explicit weight normalization to Phase 2 reference strategy generation

**Low Priority** (Future enhancements):
1. Explore URL context tool for style reference import
2. Test 2M token context for extended audio files
3. Evaluate Gemini 2.5 Flash for cost-sensitive phases (music analysis)

### Final Recommendations

**For Your Project**:
- Gemini 2.5 Pro is **ideal** for Phase 2 (MV Director) complex reasoning
- Consider **Gemini 2.5 Flash** for Phase 1 (Music Analysis) - faster, cheaper, still capable
- Use **structured outputs** (`responseSchema`) across ALL phases to reduce validation overhead
- Implement **PTCF framework** consistently: Persona → Task → Context → Format

**Prompt Optimization Checklist**:
- [ ] Clear role definition with domain expertise
- [ ] Explicit constraints hierarchy (MANDATORY, CRITICAL, PROHIBITED)
- [ ] Self-validation protocol before output
- [ ] Few-shot examples for transformation rules
- [ ] responseSchema for JSON outputs
- [ ] Thinking budget appropriate to task complexity

---

**Document Version**: 1.0
**Created**: November 2025
**Author**: Research synthesis for AGNO MV 2.0 Project
**Sources**: Official Google documentation, technical reports, community best practices (2025)
