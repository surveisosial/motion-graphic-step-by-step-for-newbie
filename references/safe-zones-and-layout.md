# Safe Zones and Layout Protection Guide (9:16 Mobile Motion)

This guide documents exact margin constraints, center-anchoring formulas, and header bar specifications to prevent text clipping, layout collisions, and off-screen elements in 1080x1920 vertical video (Instagram Reels, TikTok, and YouTube Shorts).

---

## 1. 1080x1920 Mobile Safe Zone Map

```
+-------------------------------------------------------+  Y = 0px
|                     TOP DANGER ZONE                   |
|        (Phone camera notch, dynamic island, status)   |
|                   Height: 160px                       |
+-------------------------------------------------------+  Y = 160px
| [TOP-LEFT: TOPIC TITLE]        [TOP-RIGHT: @HANDLE]   |  Top Bar (Y = 160px)
|   |                                               |   |
| L |                                               | R |
| E |                                               | I |
| F |               USABLE SAFE ZONE                | G |
| T |                 Width: 860px                  | H |
|   |                Height: 1500px                 | T |
| M |                                               |   |
| A |                                               | M |
| R |                                               | A |
| G |                                               | R |
| I |                                               | G |
| N |                                               | I |
|   |                                               | N |
| 1 |                                               | 1 |
| 1 |                                               | 1 |
| 0 |                                               | 0 |
| p |                                               | p |
| x |                                               | x |
|   |                                               |   |
+-------------------------------------------------------+  Y = 1660px
|                   BOTTOM DANGER ZONE                  |
|    (Caption box, audio track title, like/share icons) |
|                   Height: 260px                       |
+-------------------------------------------------------+  Y = 1920px
```

---

## 2. Top Branding Bar Specifications

Every vertical video places an identity bar along the upper safe line ($Y = 160\text{px}$):

### HTML Structure:
```html
<header class="top-bar">
  <div class="top-title">AI INFRASTRUCTURE // VOL. 01</div>
  <div class="top-credit">@yourhandle</div>
</header>
```

### CSS Rules:
```css
.top-bar {
  position: absolute;
  top: 160px; /* Aligned to upper safe boundary */
  left: 110px;
  right: 110px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-family: 'IBM Plex Mono', monospace;
  font-size: 22px;
  letter-spacing: 0.10em;
  text-transform: uppercase;
  z-index: 100;
  pointer-events: none;
}
.top-title {
  font-weight: 700;
  color: var(--accent, #c2410c);
  display: flex;
  align-items: center;
  gap: 8px;
}
.top-credit {
  font-weight: 600;
  color: var(--ink, #1c1917);
  opacity: 0.75;
}
```

---

## 3. Center-Anchoring Formula for Rotated Badges and Cards

When CSS rotation (`transform: rotate(-4deg)`) is applied to an element, its bounding box expands according to:
$$\Delta W \approx W \cos(\theta) + H \sin(\theta)$$

If a $600\text{px}$ card rotates by $5^\circ$ while positioned near the edge with a fixed `left: 80%`, the rotated corner pokes outside the $1080\text{px}$ canvas boundary.

### Standard CSS Solution:
```css
/* Center-anchor floating badges and rotated cards */
.stamp, .floating-badge {
  position: absolute;
  left: 50%;
  bottom: 120px;
  transform: translateX(-50%) rotate(-4deg);
  max-width: calc(100% - 240px); /* Guarantees 120px margins on both sides */
  transform-origin: center center;
  box-sizing: border-box;
}
```

When building a sliding camera deck where the canvas moves horizontally across beats:
```css
/* For Beat N at a discrete horizontal offset */
.beat-stamp {
  position: absolute;
  /* Center point of beat N: (N - 0.5) * 1080px */
  left: 540px; /* Beat 1: 540px, Beat 2: 1620px, Beat 3: 2700px */
  transform: translateX(-50%) rotate(3deg);
  max-width: 840px;
}
```

---

## 4. Pre-Render Geometry Checklist
1. [ ] Is the **Top Bar** placed at $Y = 160\text{px}$ with $110\text{px}$ left and right padding?
2. [ ] Does every text block maintain at least $110\text{px}$ clearance from both canvas edges?
3. [ ] Does the first headline sit below $Y = 220\text{px}$ (clearing the top bar)?
4. [ ] Does the lowest call-to-action or citation sit above $Y = 1660\text{px}$?
5. [ ] Do text blocks use `text-wrap: balance` or explicit `max-width` to prevent stray dangling words?
