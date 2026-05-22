<div align="center">

```
███╗   ██╗███████╗██╗   ██╗██████╗  ██████╗       ██╗     ██╗███╗   ██╗██╗  ██╗
████╗  ██║██╔════╝██║   ██║██╔══██╗██╔═══██╗      ██║     ██║████╗  ██║██║ ██╔╝
██╔██╗ ██║█████╗  ██║   ██║██████╔╝██║   ██║      ██║     ██║██╔██╗ ██║█████╔╝ 
██║╚██╗██║██╔══╝  ██║   ██║██╔══██╗██║   ██║      ██║     ██║██║╚██╗██║██╔═██╗ 
██║ ╚████║███████╗╚██████╔╝██║  ██║╚██████╔╝      ███████╗██║██║ ╚████║██║  ██╗
╚═╝  ╚═══╝╚══════╝ ╚═════╝ ╚═╝  ╚═╝ ╚═════╝       ╚══════╝╚═╝╚═╝  ╚═══╝╚═╝  ╚═╝
```

**Cyberpunk Gesture Control Interface**

*Jack in. Take control. The grid is yours.*

[![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/HTML)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-0097A7?style=for-the-badge&logo=google&logoColor=white)](https://mediapipe.dev/)
[![Web Audio API](https://img.shields.io/badge/Web%20Audio%20API-FF5722?style=for-the-badge&logo=googlechrome&logoColor=white)](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
[![No Dependencies](https://img.shields.io/badge/Zero%20Dependencies-00ff88?style=for-the-badge)](https://github.com)

</div>

---

## 📡 System Overview

**NEURO-LINK** is a real-time, browser-based cyberpunk interface that transforms your hands into a control device. Using Google's MediaPipe Hands, it tracks hand gestures via your webcam and drives a multi-layered canvas rendering engine — complete with a warping neural grid, particle effects, procedural audio synthesis, and a reactive glitch system.

No installations. No frameworks. Open the HTML file. Jack in.

---

## 🎮 Feature Matrix

| System | Description |
|--------|-------------|
| 🖐️ **Hand Tracking** | Real-time dual-hand skeleton tracking via MediaPipe Hands (21 landmarks per hand) |
| 🔲 **Neural Grid** | Dynamic warping grid that bends toward your hand position in real-time |
| 🎨 **Multi-Mode Engine** | 4 distinct visual interaction modes, each with its own aesthetic and behavior |
| ⚡ **Particle System** | Physics-driven glow particles with gravity, decay, and velocity |
| 🔊 **Procedural Audio** | Web Audio API synthesizer — drone oscillators, bit-crush SFX, white noise |
| 💥 **Glitch System** | Gesture-triggered scanline glitches and pixel corruption effects |
| 📺 **CRT Overlay** | Scanline filter layered over the full interface for authentic retro-future aesthetic |
| 🎭 **Boot Sequence** | Animated terminal-style startup with a progress loader |

---

## 🕹️ The Four Modes

### ⚡ HACK Mode *(Default)*
> *Neon lime. Full control.*

- Full 21-point hand skeleton rendered in `#adff00` (neon lime) with glow shadows
- **HUD reticles** appear at each fingertip
- **Pinch gesture** (thumb ↔ index finger) triggers:
  - Burst of 30 lime-colored glow particles
  - Bit-crush audio SFX (descending square wave)
  - Glitch intensity spike

### 🎨 CYBER-ART Mode
> *Hot pink. Creative chaos.*

- **V-sign** or **Rock-on gesture** spawns floating 3D wireframe shapes
  - Wireframe cubes (double-offset for depth)
  - Wireframe tori (crossed ellipses)
- Shapes float upward with rotation and fade decay

### 🌀 OVERRIDE Mode
> *Cyan. Leave your mark.*

- **Hand movement** paints luminous cyan motion trails
- Trails render with velocity-based line width tapering
- **Shake gesture** (fast wrist movement > 0.15 threshold) clears all strokes + fires:
  - White noise burst
  - Full-intensity glitch

### ⬛ SOKKO-VOID Mode
> *Platinum white. The void speaks.*

- Aesthetic shift to a platinum/silver (`#E5E4E2`) color palette
- Named after Jasmine Sokko's stage visual aesthetics

---

## 🖐️ Gesture Reference

| Gesture | Detection Method | Effect |
|---------|-----------------|--------|
| **Pinch** | Thumb tip ↔ Index tip distance < 0.05 | Particles + glitch (HACK) |
| **V-Sign** ✌️ | Index + Middle extended, Ring + Pinky folded | Spawn shape (CYBER-ART) |
| **Rock-On** 🤘 | Index + Pinky extended, Middle + Ring folded | Spawn shape (CYBER-ART) |
| **Hand Move** | Wrist landmark tracking | Paint stroke (OVERRIDE) |
| **Shake** | Wrist velocity > 0.15 (normalized) | Clear canvas + noise (OVERRIDE) |

---

## 🏗️ Architecture

The rendering pipeline is built on **5 stacked HTML5 canvases**, each handling a distinct visual layer:

```
z-index: 4  │  glitchOverlay  │  Pixel corruption & scanline slices
z-index: 3  │  uiCanvas       │  HUD reticles & fingertip markers
z-index: 2  │  effectCanvas   │  Particles, shapes, strokes, skeleton
z-index: 1  │  webcamCanvas   │  Mirrored webcam feed + dithering
z-index: 0  │  neuralGrid     │  Animated warping grid background
```

> All canvases are dynamically resized on window resize events.

### State Machine

```js
state = {
  mode: 'hack',           // Active interaction mode
  hands: [],              // MediaPipe landmark arrays
  particles: [],          // Active physics particles
  shapes: [],             // Floating wireframe shapes
  strokes: [],            // OVERRIDE mode paint strokes
  glitchIntensity: 0,     // Decay-based glitch trigger
  audioCtx: null,         // Web Audio context
  droneOsc: null,         // Continuous sawtooth drone oscillator
}
```

---

## 🔊 Audio Engine

Built entirely on the **Web Audio API** with no external sound files:

| Sound | Method | Details |
|-------|--------|---------|
| **Drone** | Sawtooth oscillator → lowpass filter | Pitch shifts with hand Y position (50–200 Hz) |
| **Bit Crush** | Square wave with exponential frequency sweep | 800 Hz → 100 Hz in 150ms |
| **Static Burst** | Procedural white noise buffer | 0.3 second noise burst at 40% gain |

---

## 🚀 Quick Start

No build step. No npm. No config.

```bash
# Clone or download the repo
git clone https://github.com/your-username/neuro-link.git

# Open the file in your browser
# → Just double-click neuro-link.html
# → Or open with Live Server in VS Code
```

**Browser Requirements:**
- ✅ Chrome 90+ (recommended)
- ✅ Firefox 88+
- ✅ Edge 90+
- ⚠️ Requires webcam access permission
- ⚠️ Requires microphone permission (for AudioContext on some browsers)
- ❌ Safari has limited WebRTC & AudioContext support

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `1` | Switch to HACK mode |
| `2` | Switch to CYBER-ART mode |
| `3` | Switch to OVERRIDE mode |

Mode buttons are also clickable via the HUD at the bottom of the screen.

---

## 📁 Project Structure

```
cyberpunktry1/
└── neuro-link.html      # Complete single-file application
```

All logic, styles, and assets are self-contained in a single HTML file — no external dependencies beyond CDN-loaded libraries.

**CDN Dependencies:**
- `@mediapipe/hands` — Hand landmark detection model
- `Google Fonts` — Orbitron (display) + JetBrains Mono (mono)

---

## 🛠️ Tech Stack

| Technology | Role |
|-----------|------|
| **HTML5 Canvas API** | Multi-layer rendering pipeline |
| **MediaPipe Hands** | Real-time hand landmark tracking (21 points) |
| **Web Audio API** | Procedural audio synthesis |
| **CSS Animations** | Scan bar sweep, CRT scanlines, HUD effects |
| **Orbitron Font** | Cyberpunk display typography |
| **JetBrains Mono** | HUD monospace data readouts |
| **Vanilla JavaScript** | Zero-framework logic and state management |

---

## 🔬 Performance Notes

- MediaPipe runs on a **16ms polling loop** (≈60 FPS target)
- The neural grid warp is computed per-frame using hand landmark position
- Glitch intensity **decays automatically** to prevent sustained performance hits
- Particles use **object pooling via splice** for efficient garbage management
- FPS counter and hand count are displayed live in the bottom-right HUD

---

## 🌐 Inspiration

> *"The sky above the port was the color of television, tuned to a dead channel."*
> — William Gibson, *Neuromancer*

NEURO-LINK draws from:
- **Cyberpunk 2077** — HUD aesthetics and color language
- **Ghost in the Shell** — Neural interface concepts
- **Jasmine Sokko** — Stage visual aesthetics (SOKKO-VOID mode)
- **Vaporwave/Dark Synthwave** — Color palette and CRT nostalgia

---

## 🤝 Contributing

Pull requests welcome. If you're adding a new gesture mode:

1. Add color constant to the `COLORS` object
2. Define the mode button in `#modeSelector`
3. Add CSS classes for `.mode-<name>` and `.btn-<name>`
4. Implement gesture detection in the `update()` → `frame()` loop
5. Wire mode switch in `setMode()`

---

## 📜 License

```
MIT License — Use it, hack it, make it yours.
```

---

<div align="center">

**[ NEURAL LINK ESTABLISHED ]**

*Built with neon, noise, and no npm.*

</div>
