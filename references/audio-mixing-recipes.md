# Audio Mixing and Pacing Recipes (Barty-Bart + ElevenLabs)

This document contains formulas, FFmpeg filter chains, and mixing rules to ensure speech narration sounds articulate, energetic, and clean without awkward dead air or rushed delivery.

---

## ElevenLabs Credit Protection Protocol

1. **The Script Lock Principle**:
   - Calling the ElevenLabs API consumes paid character credits.
   - Script and layout revisions happen frequently during early storyboard and test shot stages (Steps 3 through 7).
   - **Hard Rule**: Hold ElevenLabs API generation until Step 7 (Test Frame Evaluation) is approved by the user.
   - Build the HyperFrames timeline in Steps 5 and 6 using the mathematical reading duration formula ($\text{words}/3.5 + 2\text{s}$) or local scratch audio. You avoid burning character quota if sentences change.
2. **One-Shot Execution**:
   - Once the user approves the test frames in Step 7, lock the script text.
   - Send all script beats to ElevenLabs in a single batch session.

---

## 1. Barty-Bart Sound Principles

The core rule from the Barty-Bart repository:
> *"Never synthesize music or sound effects in code (no oscillator or sine beeps). Use real recorded files. Mix to -16 LUFS with true peaks below -1.5 dBFS."*

### Three Audio Hierarchy Layers:
1. **Layer 1: Voiceover (Top Priority)**
   - Target Integrated Loudness: `-16.0 LUFS`
   - Maximum True Peak: `-1.5 dBFS`
   - High-pass filter at $80\text{Hz}$ to eliminate desk rumble and microphone boom vibrations.
2. **Layer 2: Background Music Bed**
   - Level during speech: `-20 dBFS` (ducked automatically to prevent masking human voice frequencies between $1\text{kHz}$ and $4\text{kHz}$).
   - Level during beat transitions: `-14 dBFS` (smooth 400ms fade-in).
3. **Layer 3: Organic Sound Effects**
   - *Whoosh*: Placed on camera move starts (fading out before cards land).
   - *Card Impact / Thud*: Placed on the exact frame a card lands ($t_0$).
   - *Click / Tick*: Soft volume ($-22\text{ dBFS}$) for animated numbers and counter text.

---

## 2. Pacing Management and Silence Trimming

ElevenLabs naturally produces pauses between sentences of $0.6$ to $1.2$ seconds. In vertical short-form video (Reels/TikTok), these long silences cause viewers to drop off. However, speeding up the entire audio track past $1.25\text{x}$ makes the narrator sound panicked and hard to follow.

### Two-Stage Pacing Fix:

#### Stage A: Trim Dead Silence (Preserving Natural Speech Speed)
Use FFmpeg to detect pauses longer than $0.35$ seconds and trim them down to natural breath pauses of roughly $0.15$ seconds:
```bash
ffmpeg -y -i raw_elevenlabs.mp3 \
  -af "silenceremove=start_periods=1:start_duration=0.05:start_threshold=-45dB:detection=peak,\
silenceremove=stop_periods=-1:stop_duration=0.35:stop_threshold=-42dB" \
  trimmed_voice.mp3
```

#### Stage B: Gentle Speed Adjustment (1.10x to 1.12x)
If the narration slightly exceeds the scene duration, apply a modest `atempo` adjustment:
```bash
# 1.12x saves 12% duration without distorting natural vocal pitch or clarity
ffmpeg -y -i trimmed_voice.mp3 -filter:a "atempo=1.12" balanced_voice.mp3
```

---

## 3. Full Master Audio Mix FFmpeg Script

Combine voice narration, ducked background music, and sound effects into a broadcast-standard master file:

```bash
ffmpeg -y \
  -i balanced_voice.mp3 \
  -i background_music.mp3 \
  -filter_complex \
  "[1:a]volume=0.10[music_ducked]; \
   [0:a][music_ducked]amix=inputs=2:duration=first:dropout_transition=2[mixed]; \
   [mixed]loudnorm=I=-16:TP=-1.5:LRA=11[master]" \
  -map "[master]" -c:a mp3 -b:a 192k full_mix.mp3
```

---

## 4. Script Word Count Formula

Lock script word counts during drafting so you never have to force unnatural audio speedups:

$$\text{Maximum Word Count} = \text{Target Duration (seconds)} \times 2.1$$

- **60-Second Target**: $60 \times 2.1 \approx \mathbf{125\text{ to }135\text{ words}}$.
- **75-Second Target**: $75 \times 2.1 \approx \mathbf{155\text{ to }165\text{ words}}$.
- **90-Second Target**: $90 \times 2.1 \approx \mathbf{185\text{ to }195\text{ words}}$.
