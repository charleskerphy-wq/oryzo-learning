# Oryzo Learning Recreation — Architecture

**Project Goal**  
Educational rebuild of the *spirit* and *systems* behind oryzo.ai (Lusion).  
Premium + playful tone. Original content, original assets. Not a 1:1 clone.

**Chosen Stack**  
- Astro 5 + React islands  
- Three.js (latest)  
- TypeScript  
- Custom virtual scroll + Scene Director  

---

## 1. Design Principles (Locked)

1. **Product at the center** — the coaster is always the hero.
2. **Restraint** — minimal typefaces, limited color palette, quiet UI.
3. **Seamless continuity** — no hard cuts between scenes.
4. **Premium craft + dry humor** — serious presentation of a trivial object.
5. **Learning-first** — every system should be readable and documented.

---

## 2. High-Level Architecture

```
┌────────────────────────────────────────────────────────────┐
│                        Browser                               │
│  ┌───────────────────┐     ┌───────────────────────────┐  │
│  │   DOM / React UI   │◄────│   Scroll Progress (0→1)    │  │
│  │  (Nav, Cards, Text) │     │   + velocity               │  │
│  └───────────────────┘     └───────────────────────────┘  │
│                                           │                 │
│  ┌───────────────────┐                   │                 │
│  │  WebGL Canvas      │◄──────────────────┘                 │
│  │  (Three.js)        │     SceneDirector                   │
│  │  - Coaster model   │     (maps progress → camera,        │
│  │  - Lighting        │      objects, materials, UI states) │
│  │  - Environment     │                                     │
│  └───────────────────┘                                     │
└────────────────────────────────────────────────────────────┘
```

### Core Systems

| System            | Responsibility                                      | Location          |
|-------------------|-----------------------------------------------------|-------------------|
| ScrollManager     | Virtual height, input handling, smoothed progress   | `src/core/`       |
| SceneDirector     | Timeline of keyframes, interpolation                | `src/core/`       |
| AssetLoader       | Priority loading + progress reporting               | `src/core/`       |
| WebGL Runtime     | Renderer, resize, render loop                       | `src/components/canvas/` |
| UI Overlay        | React components driven by progress                 | `src/components/ui/` |
| Scenes            | Individual 3D compositions                          | `src/scenes/`     |

---

## 3. Scroll System (Critical)

- Document is given a large artificial height (e.g. 40 000 px).
- Native scroll is intercepted / observed.
- A smoothed `progress` value (0 → 1) is the single source of truth.
- All camera moves, object transforms, material changes, and UI visibility are driven by this value.
- Velocity is also tracked for secondary effects (parallax intensity, etc.).

This is the most important architectural decision. Everything else hangs off `progress`.

---

## 4. Scene Director Pattern

```ts
interface Keyframe {
  progress: number;          // 0–1
  camera: {
    position: [x, y, z];
    target: [x, y, z];
    fov?: number;
  };
  objects?: Record<string, Partial<ObjectState>>;
  ui?: Record<string, UIState>;
}
```

The director:
1. Receives current `progress`.
2. Finds the two surrounding keyframes.
3. Interpolates (linear or eased).
4. Applies results to the Three.js scene and notifies the UI layer.

---

## 5. Content Structure (Original)

| Section          | Progress Range | Purpose                                      |
|------------------|----------------|----------------------------------------------|
| Preloader        | —              | Geometric / tool aesthetic loader            |
| Hero             | 0.00 – 0.18    | Coaster on desk, strong statement            |
| Manifesto        | 0.18 – 0.32    | Short intentional-design statement           |
| Features         | 0.32 – 0.55    | 3–4 “serious” benefits of the coaster        |
| Interaction Beat | 0.55 – 0.68    | Playful interaction (hand / hover gag)       |
| Model Release    | 0.68 – 0.85    | Satirical open-weight 3D model section       |
| Closing          | 0.85 – 1.00    | Quiet end + credits                          |

All copy is newly written. Tone: premium + playful.

---

## 6. Color & Type System

```css
--ink:        #0c0b09;
--paper:      #f3eee4;
--cork:       #c98972;
--mat-green:  #3d4a3a;
--accent:     #e25a45;   /* sparingly */
--muted:      #8e8b84;
```

Typography: one primary sans family. Hierarchy through size + tracking.

---

## 7. Implementation Phases

| Phase | Deliverable                                      | Status |
|-------|--------------------------------------------------|--------|
| 0     | Architecture + project scaffolding               | Done   |
| 1     | ScrollManager + basic camera driven by progress  | Next   |
| 2     | Hero scene with real coaster model + lighting    | —      |
| 3     | React UI overlay system                          | —      |
| 4     | Multi-scene director + transitions               | —      |
| 5     | Features, interaction beat, model section        | —      |
| 6     | Polish, loader, responsiveness, performance      | —      |

---

## 8. File Ownership Rules

- `src/core/` → pure TypeScript, no React, no DOM
- `src/scenes/` → Three.js scene setup only
- `src/components/ui/` → React only
- `src/components/canvas/` → bridge between React and Three

This keeps the mental model clean.
