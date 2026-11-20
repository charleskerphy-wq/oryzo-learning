# oryzo-learning

Educational recreation of the **systems and spirit** behind [oryzo.ai](https://oryzo.ai) by Lusion.

> Premium + playful product storytelling with WebGL, virtual scroll, and a Scene Director architecture.

This is **not** a 1:1 clone.  
We rebuild the architecture, interaction patterns, and design principles with original content and assets for learning purposes.

## Goals

- Understand how high-end WebGL product experiences are structured
- Learn virtual scroll + progress-driven camera/object systems
- Practice restrained, premium design language with dry humor
- Keep every system readable and well-documented

## Stack

- **Astro** + React islands
- **Three.js**
- TypeScript
- Custom virtual scroll + Scene Director

## Architecture

See [`docs/ARCHITECTURE.md`](./docs/ARCHITECTURE.md) for the full plan.

### Core Ideas

1. **Single source of truth**: a smoothed `progress` value (0 → 1)
2. **SceneDirector** maps progress → camera, objects, materials, and UI states
3. **UI is a thin overlay** — never competes with the 3D
4. **Seamless continuity** — no hard cuts between scenes
5. **Product at the center** at all times

## Status

- [x] Architecture locked
- [ ] Phase 1 — ScrollManager + progress-driven camera
- [ ] Phase 2 — Hero scene with real coaster model
- [ ] Phase 3 — React UI overlay system
- [ ] Phase 4 — Multi-scene director + transitions
- [ ] Phase 5 — Features, interaction beat, model section
- [ ] Phase 6 — Polish, loader, performance

## License

For learning and personal study.  
Do not redistribute as a commercial product or claim it as an official Lusion work.
