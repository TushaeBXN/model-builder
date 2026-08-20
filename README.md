# model/builder

**Vision AI model spec tool — from problem definition to production-ready spec in a single session.**

Built by [Anthos Intelligence](https://github.com/TushaeBXN) · Powered by Claude · Zero backend · Single HTML file

---

## What it is

Most vision AI projects fail before a model is ever trained — because the spec was wrong.

Wrong label definitions. Wrong edge cases. Wrong architecture for the latency requirement. Wrong output format for the downstream system.

**model/builder** fixes that. It's a structured AI interview tool that asks the right questions, generates a complete technical specification at each stage, and — if you want — immediately turns around and tries to break that spec as a skeptical senior ML engineer would.

The output is a model design document you can hand to an engineering team, drop into a client proposal, or export as a PDF on the spot.

---

## Quick start

```bash
git clone https://github.com/TushaeBXN/model-builder
cd model-builder
open model_builder_v2.html
```

No npm install. No build step. No server. Open the file in a browser and you're running.

### Running outside claude.ai

The tool calls the Anthropic API directly from the browser. To run it standalone, add your API key to the two `fetch('https://api.anthropic.com/v1/messages', ...)` calls in the script:

```javascript
headers: {
  "Content-Type": "application/json",
  "x-api-key": "sk-ant-...",
  "anthropic-version": "2023-06-01",
  "anthropic-dangerous-direct-browser-ipc": "true"
}
```

> ⚠️ Never commit API keys. For anything beyond local use, put a lightweight proxy in front of the API and call that instead.

**Recommended local server:**

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

Then open `http://localhost:8080/model_builder_v2.html`.

---

## How it works

```
You describe the problem
        ↓
Claude interviews you — one or two questions at a time
        ↓
Spec sections are generated and built in the background
        ↓
Adversarial mode tears each section apart before you commit
        ↓
Export PDF or TXT — ready to share
```

### Two modes

**Interview (default)** — Claude leads. Describe your detection problem in plain language and Claude pulls out what it needs through conversation. When it has enough context for a section, it writes the spec directly into the chat.

**Form** — You lead. Five structured steps, each with targeted fields and a generate button. Better when you already know your inputs and just want fast output.

Switch between them anytime with the toggle in the header — state is preserved.

---

## Features

### ⚔️ Adversarial Mode

The differentiator. Toggle it on and every spec Claude generates is immediately followed by a critique — Claude playing the role of a skeptical senior ML engineer trying to find the gaps.

- Specific failure modes
- Missing edge cases
- Annotation ambiguities
- Deployment risks

Not generic advice — pointed critique grounded in what you actually described. Most spec tools tell you what to build. This one also tells you why it might fail.

### 📎 Image Upload — Grounded to Your Footage

Drop a real frame from your camera feed. Claude analyzes the specific scene: lighting conditions, camera angle, what a false positive would literally look like in this footage. The spec it generates references what it actually sees, not generic descriptions of your use case.

### 📋 Scenario Library — 11 Production Use Cases

Pre-built scenarios that auto-fill all fields instantly. Drawn from real production deployments.

| Industry | Scenarios |
|----------|-----------|
| Smart Cities & Traffic | Flood detection · Traffic obstruction · Road construction · Yield-to-pedestrian |
| Manufacturing & Industrial | PPE compliance · Product defects · Spill / hazard |
| Retail & Public Safety | Crowd density · Slip hazard |
| Agriculture | Crop disease · Irrigation leak |

### 💰 Inference Cost Estimator

Real-time cost calculator. Input your daily image volume, model type, and cloud provider — get cost-per-image, daily, monthly, and annual estimates with a compute/storage breakdown and GPU recommendation.

| Model Type | Typical Cost/Image | Recommended GPU |
|------------|-------------------|-----------------|
| VLM Large (8B+) | ~$0.0018 | A100 / H100 |
| VLM Small (3B) | ~$0.0006 | A10G / L4 |
| Two-stage pipeline | ~$0.0009 | A10G + H100 |
| Object Detector | ~$0.00008 | T4 / V100 |
| Classifier only | ~$0.000015 | T4 / CPU |

Self-hosted GPU multiplier: **0.35×** cloud cost.

### 📄 Export

- **PDF** — formatted multi-page report with cover page, cost summary, and one page per spec section. Client-ready.
- **TXT** — plain text dump for Notion, Confluence, Linear, or wherever your team lives.

---

## What gets generated

At each stage Claude produces a focused, technical output — not summaries, but the actual working artifact:

**01 · Problem Specification**
Exact positive/negative case definitions. Annotation ambiguities to resolve before labeling. Output format with specific label names. Top false-positive risks for this specific scenario.

**02 · Data Strategy**
Minimum labeled data required and the reasoning behind it. Full annotation schema. Class balancing approach. Edge case sourcing plan. Train/val/test split recommendation.

**03 · Model Architecture**
Specific model names — not categories. Two-stage screening decision with rationale. VLM prompt engineering strategy if applicable. Key tradeoffs. Inference cost estimate per image.

**04 · Training Plan**
Fine-tuning method (SFT, LoRA, full fine-tune, or prompt-only). Data techniques including cross-check rephrasing and class balancing. Evaluation benchmark design with correct rejection cases built in. Target accuracy range.

**05 · Deployment Plan**
Inference pipeline architecture. Production monitoring metrics. Model drift detection approach. Retraining trigger criteria. Alert and escalation logic. Cost optimization at your specific volume.

---

## Who it's for

**ML Engineers** — Get the spec right before touching any code. Generate evaluation benchmarks with correct rejection cases baked in. Sanity-check architecture choices against your actual constraints.

**AI Product Managers** — Translate "we want to detect flooding on roads" into a spec an engineering team can execute. Produce client-ready PDF proposals. Understand tradeoffs in plain language.

**Consultants & Integrators** — Scope vision AI projects faster. Start from production-tested scenarios. Walk out of a client meeting with a draft spec.

**Smart City & Infrastructure Teams** — The scenario library is built from this world. Two-stage screening pattern, graded output scales, and adversarial review reflect what actually works in production camera deployments.

---

## Stack

| Layer | Technology |
|-------|-----------|
| UI | Vanilla HTML/CSS/JS — zero dependencies, runs anywhere, shareable as a single file |
| AI | Claude Sonnet 4.6 via Anthropic API — vision-language reasoning with image input support |
| PDF | jsPDF via CDN — fully client-side, no server required |
| Fonts | IBM Plex Mono + Inter via Google Fonts |

---

## Roadmap

- **Prompt playground** — test the VLM prompts Claude recommends directly against your uploaded frames
- **Annotation template export** — generate Label Studio / CVAT / Roboflow config from the annotation schema
- **Benchmark builder** — auto-generate a structured test set (detections + rejections) from the spec
- **Multi-architecture comparison** — run the same scenario through VLM vs detector vs hybrid side by side
- **Team workspace** — save, version, and share specs across a team
- **Training job integration** — push the spec directly to SageMaker / Vertex AI / Azure ML as a starter config

---

## Background

The two-stage screening pattern, scenario library, and training techniques (class balancing, cross-check rephrasing, graded output scales over binary yes/no) are informed by production VLM deployments in smart city infrastructure — specifically the approach demonstrated by Linker Vision using NVIDIA Cosmos 3 post-training, where detection accuracy on flooded roadways improved from 0.64 → 0.98 (+53% relative gain) using these methods.

---

## License

MIT — use it, fork it, ship it.

---

*Anthos Intelligence · Intelligence for the physical world.*
