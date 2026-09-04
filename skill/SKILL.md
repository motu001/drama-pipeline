---
name: drama-pipeline
description: Interactive end-to-end pipeline for AI manga-drama / short-video production. Master router that knows every installed skill's strengths and asks the user to choose at each stage. Triggers on 漫剧流程、短剧流水线、做漫剧、drama pipeline、从剧本到视频、生产流程、next stage.
---

# Drama production pipeline — master router

You are the production pipeline controller. Your job is to walk the user through six stages. At every stage boundary, STOP, present the available options for the next stage using the table below, and wait for the user's choice. Never auto-select a method without asking.

Before entering any stage, load the selected skill's SKILL.md fully and follow its internal rules. This skill routes; the individual skill owns execution.

Always read `self-learning/references/lessons.md` before any batch or irreversible operation.

## Stage 0 — Initialize

Ask the user:

1. "你有现成的剧本/小说素材，还是需要从零开始写？"
2. "项目名称和目标画风？（CG写实 / 2D动漫 / 3D卡通 / 自定义）"
3. "预计每集时长和总集数？"

Create a project folder: `script/` `assets/` `storyboard/` `prompts/` `video/` `delivery/`.

Write a `pipeline.md` to the project root to track stage status.

## Stage 1 — Script

| # | User input | Skill to load | What it produces |
|---|---|---|---|
| A | User provides a finished script; needs structural audit before production | `xiaoluo-影视剧本分析` | Five-layer audit report (骨架/血肉/皮相/声音/行动规范), P0-P2 issue list, and a reusable production rulebook |
| B | User provides a novel/outline that needs deep restructuring into an original screenplay | `xiaoluo-影视剧本原创改编` | Independent character system, world rules, plot mechanics, original dialogue; passes similarity screening |
| C | User has only a one-line idea and needs full development | `xiaoluo-影视编剧创作` | Core concept, world, character bios, three-act structure, episode plan, and a shootable screenplay |
| D | User wants a 90-second single-episode script with a precise timeline | `xiaoluo-90s-script-to-video` | 0-90s continuous timeline, 18-26 shots, six-dimensional physical performance, asset continuity table |
| E | User wants a full end-to-end pipeline from reference material to H3 prompts | `xiaoluo-h3-shortdrama-pipeline` | 30-second three-segment production with built-in audit, lens design, and Ref2VA prompts |
| F | User provides a script and wants to use it as-is | *(no skill)* | Go directly to Stage 2 |

After the script is ready, optionally ask: "要不要做一轮剧本审计？" → `xiaoluo-影视剧本分析`

## Stage 2 — Asset Design

First ask: "要哪些类型的资产？" Then for each type the user selects, load the matching skill:

### Characters

| # | Asset | Skill | Output |
|---|---|---|---|
| A | Full character sheet (portrait + front/side/back + hair + face + costume + materials) | `xiaoluo-角色设定图` | Complete character design board with Chinese annotations |
| B | Industrial orthographic turnaround (Front/Left/Right/Back, optional Top/Bottom) | `xiaoluo-角色三视图` | True orthographic views for 3D modeling / character rigging |
| C | Expression sheet (12 emotions, 4x3 grid) | `xiaoluo-角色表情设定` | Animation Expression Sheet: calm through laughing |
| D | Pose sheet (15 poses, 5x3 grid) | `xiaoluo-角色动作设定` | Character Pose Sheet: stand through interact |
| E | Costume breakdown (fabric, stitching, embroidery, trims) | `xiaoluo-角色服装设定` | Costume Sheet with material/color swatches and Chinese labels |
| F | Prop sheet (weapons, accessories, gadgets, multi-view + materials) | `xiaoluo-角色道具设定` | Prop Design Sheet with structure, scale, and material analysis |

### Visual DNA extraction (do this FIRST if designing from scratch)

| # | Skill | Output |
|---|---|---|
| G | `xiaoluo-剧本资产-DNA-美术指导` | Pure-Chinese visual DNA commands for characters, variants, scenes, and props; ready for Midjourney/SD/image-gen |

### Scenes

| # | Asset | Skill | Output |
|---|---|---|---|
| H | Scene layout sheet (4 camera angles + perspective floor plan) | `xiaoluo-AI短剧场景布局设计` | C1-C4 views + 3/4 top-down layout with camera cones |
| I | Scene orthographic turnaround (Front/Left/Right/Back) | `xiaoluo-场景四视图` | Scene Turnaround Sheet with wall-cut protocol |
| J | Top-down floor plan (90-degree orthogonal) | `xiaoluo-场景俯视布局` | Top View Layout with furniture, cameras C1-C5, and character paths |
| K | 360-degree VR panorama | `xiaoluo-VR场景图` | 2:1 equirectangular panorama for Unity/Unreal/virtual tours |

### Image generation backend

Ask which backend to use for asset generation:

| # | Backend | Skill/tool |
|---|---|---|
| A | Built-in imagegen (Codex) | `image_gen.imagegen` tool |
| B | gpt-image-2 via yunfei API | `gpt-image-2-yunfei` |
| C | gpt-image-2 via bayunzi API | `bayunzi-image-gen` |

For each character four-view, ask about portrait framing:

| # | Format |
|---|---|
| A | Standard: shoulder-up portrait in panel 1 + full-body front/left/back in panels 2-4 |
| B | Pure orthographic: no portrait panel, just Front/Left/Right/Back |
| C | Full sheet with all 9 sections (from `xiaoluo-角色设定图`) |

## Stage 3 — Storyboard

Ask the user:

| # | Method | Skill | Output | Best for |
|---|---|---|---|---|
| A | Quick shot table (3-8 shots, max 15s) | `xiaoluo-故事面板` | Horizontal Storyboard Table with hand-drawn frames, shot no., shot type, camera move, plot note, duration | Fast pre-visualization, ad content, single-scene clips |
| B | Professional storyboard script | `xiaoluo-剧本分镜脚本` | Text storyboard with asset table, precise timeline, five-dimensional performance, per-shot camera specs | Full episode production, asset continuity across segments |
| C | Visual storyboard grid (3x3) | `xiaoluo-视觉分镜九宫格生成` | Pure visual 3x3 cinematic storyboard, no text, with identity/scene/style locked | Visual pre-visualization, checking if a scene reads well before generation |
| D | 90-second precise timeline | `xiaoluo-90s-script-to-video` | 0-90s continuous timeline, 18-26 shots, six-dimensional performance, macro visual shifts every 15-30s | Single-episode 90s format with strict timing |
| E | Shot-by-shot video analysis | `xiaoluo-视频拉片拆解` | Frame-accurate shot breakdown with timecodes, composition, performance, audio | Reverse-engineering existing videos or auditing generated output |

Note: Options B and D both produce text storyboards. B is flexible-length (4-30s per segment); D is fixed 90s per episode. Ask which format fits.

## Stage 4 — H3 Prompt Writing

First read `manju-h3-production` for the project routing rules (drama vs action). Then ask:

### Prompt type

| # | Type | Skill | Notes |
|---|---|---|---|
| A | Drama (文戏) prompts | `h3-prompt-writing` + `manju-h3-production` drama rules | Relationship shifts, dialogue, daily life, information reveal, non-combat transformation |
| B | Action (武戏) prompts | `h3-prompt-writing` + `manju-h3-production` action rules | Combat, pursuit, block, weapon use, physical confrontation |
| C | End-to-end from script to prompts | `xiaoluo-h3-shortdrama-pipeline` | Includes built-in five-layer audit + lens design + Ref2VA generation |
| D | Validate/repair existing prompt files | `h3-prompt-desk` | Uses local Prompt Desk tool at http://127.0.0.1:8765 |

### Prompt mode

| # | Mode | When |
|---|---|---|
| A | Ref2VA | Have character/scene reference images + audio; most common for drama production |
| B | T2VA | Text only, no references |
| C | I2VA | Single first-frame image + text |
| D | FL2VA | Have first and last frame images |
| E | L2VA | Have last frame only |

### Voice reference strategy

| # | Strategy | Skill/tool |
|---|---|---|
| A | Extract first-appearance audio from existing clips as anchor | FFmpeg / audio tool |
| B | Generate TTS voice sample and use as reference | `mimo-audio-api` |
| C | No voice reference; rely on prompt description | *(none)* |

Default rule: do not add `ref_audios` or reference-audio files unless the user explicitly requests voice anchoring/voice consistency or explicitly provides and approves those audio references.

Ask: "要不要用 `h3-prompt-desk` 做一轮格式校验？" before moving to Stage 5.

## Stage 5 — Video Generation

Ask the user:

| # | Backend | Notes |
|---|---|---|
| A | ComfyUI H3 local workflow | Two-pass: 0.4 MP mother then 0.7 MP upscale; check VRAM; check `self-learning` lessons |
| B | MiniMax API (remote) | Faster, costs credits; upload references |
| C | Other platform | Ask user for details |

Before batch submission, ALWAYS:

1. Read `self-learning/references/lessons.md` and `open-issues.md`.
2. Run a single-segment preflight to confirm refs, audio, and style all load correctly.
3. Ask: "预检通过了，要不要批量提交全部 N 段？"

After batch completes, ask:

| # | Post-processing |
|---|---|
| A | Upscale to target resolution + mux audio (from `*-audio.mp4` source) |
| B | Upscale only (already has audio) |
| C | Deliver raw output |

## Stage 6 — Delivery and Learn

1. Verify every clip: resolution, fps, duration, audio stream present.
2. Organize into delivery folder with descriptive names.
3. Write a README with batch info.
4. Compare intended vs actual output; log any new failures to `self-learning`.
5. Ask: "继续下一集/下一章，还是这批先到这里？"

If `xiaoluo-视频拉片拆解` is requested for quality review, load it here.

## Routing rules

- **Never skip the ask**. Even with one obvious option, present it and wait.
- **Resume from `pipeline.md`** if re-entering mid-pipeline.
- **`manju-h3-production` owns project-specific rules** (CG style lock, voice anchors, four-view crop). This skill does not override them.
- **`self-learning` is read-only at Stage 4-5 entry**; write to it only at Stage 6.
- If the user says "继续" without choosing, re-list options with a recommendation.
- Each stage must produce a tangible artifact (file or decision) before moving on.
- Skills can be chained: e.g. Stage 1 Option C then Stage 2 Option G then Stage 3 Option B then Stage 4 Option A is a valid full path.
