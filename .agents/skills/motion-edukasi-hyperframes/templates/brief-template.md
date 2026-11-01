# One-Page Brief and Storyboard Template (v1.3.0)

Follow this template in order. **Do not call the ElevenLabs API until visual test frames and script lines are approved at Step 7 (Script Lock).**

---

## Phase 1: Script and Storyboard

### 1. One-Page Brief
- **Core Takeaway (Exactly One Sentence)**:
  > *Note: If the core takeaway cannot be summarized in one sentence, the brief is incomplete.*
  > `[Write exactly one sentence summarizing the core takeaway]`
- **Target Duration**: [Select: 60 seconds / 75 seconds]
- **Target Script Length**: [125 to 135 words for 60s; 155 to 165 words for 75s]
- **Top Branding Bar**:
  - **Top-Left**: `[Series title or short topic tag, e.g., AI INFRASTRUCTURE]`
  - **Top-Right**: `[Creator account handle or studio, e.g., @yourhandle]`
- **Target Audience**: [e.g., General public, tech enthusiasts, students]
- **Tone**: [3 adjectives, e.g., reflective, authoritative, calm]
- **Character Usage**:
  - [ ] **NO CHARACTER** (Focus on schematic diagrams, product cutouts, typography, and data cards)
  - [ ] **WITH CHARACTER**
    - *Upload Status*: [Uploaded / Pending]
    - *File Location*: `assets/images/character.png`
- **Negative Constraints**: [e.g., No generic glossy 3D renders, no neon palette]
- **Key Real-World Objects**: [e.g., Substation transformer, silicon chip wafer, four-year calendar]

---

### 2. Research and Source Verification
1. Primary Metric 1: [Real number + cited report or paper]
2. Primary Metric 2: [Real number + contextual bounds]
3. Citations / Historical References (if applicable):
   - Reference text: `[...]`
   - Source book, chapter, or verse: `[...]`
   - Label: `"Summary of Meaning"`
   - Contextual bounds: `[...]`

---

### 3. Storyboard and Reading-Time Math
> *Reading Duration Formula: Display Duration $\ge (\text{Word Count} / 3.5) + 2.0\text{ seconds}$*

| Beat # | Time Window | Spoken Narration | On-Screen Display Text | Concrete Visual Elements |
|---|---|---|---|---|
| **Beat 1 (Hook)** | 0.0s to 10.0s | [Spoken script...] | [Display text $\le 10$ words] | [Concrete visual objects...] |
| **Beat 2 (Problem)** | 10.0s to 25.0s | [Spoken script...] | [Display text $\le 14$ words] | [Concrete visual objects...] |
| **Beat 3 (Bottleneck)** | 25.0s to 42.0s | [Spoken script...] | [Display text $\le 15$ words] | [Concrete visual objects...] |
| **Beat 4 (Real Data)** | 42.0s to 58.0s | [Spoken script...] | [Display text $\le 14$ words] | [Concrete visual objects...] |
| **Beat 5 (Analogy)** | 58.0s to 70.0s | [Spoken script...] | [Display text $\le 12$ words] | [Concrete visual objects...] |
| **Beat 6 (Outro)** | 70.0s to 75.0s | [Spoken script...] | [Actionable closing takeaway] | [Branding signature stamp] |

---

## Phase 2: Visual Style and Assets

### 4. Style, Voice, and Character
- **Chosen Visual Style**: [Collage Cartoon / Visual Journalism / Minimalist Catalog / Vintage Engraving / Vector Route / Stage Puppet / Kinetic Typography]
- **ElevenLabs Voice Preference**: [e.g., Adam / Roger / Rachel / British / Custom Voice ID] *(API call held until Step 7)*
- **Character Asset**: [Confirm cutout is isolated with transparent background]

### 5. Estimated Timing and Sound Beds
- Total script word count: `[...]` words
- Background music selected: `assets/audio/music/[track].mp3`
- Sound effects cataloged: whoosh, click, card impact, paper slide

---

## Phase 3: Composition, Approval, and Render

### 6. HyperFrames Setup
- [ ] Safe margins respected: horizontal $\ge 110\text{px}$, top $\ge 160\text{px}$, bottom $\ge 260\text{px}$
- [ ] Top bar implemented: left title, right handle
- [ ] Rotated elements center-anchored (`left: 50%`, `transform: translateX(-50%)`)

### 7. Test Frame Evaluation (Human Gate)
- [ ] 3 to 5 PNG screenshots rendered and compiled into a contact sheet
- [ ] User review approved without script changes

### 8. Voice Synthesis and Final Render
- [ ] ElevenLabs API called for approved script lines
- [ ] Silence trimmed with FFmpeg `silenceremove`
- [ ] Master audio mixed to `-16 LUFS` (peaks below `-1.5 dBFS`, music ducked to `-20 dB`)
- [ ] Full MP4 rendered at 30 fps
