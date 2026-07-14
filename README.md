<div align="center">
  <img src="docs/hero-title-card.png" alt="Agentic Automation on iOS — built on Claude" width="720"/>

# Agentic Automation on iOS

**A voice-controlled Claude agent that drives a real iPhone — built so blind and low-vision users can operate any iOS app, not just Apple-native ones.**

🏆 3rd Place — Claude Builder Hackathon @ UCLA &nbsp;·&nbsp; 🔬 Continued as research at USC ISI (HUMANS Lab)

</div>

---

## Where's the code?

The full implementation is **private for now, on purpose**. This work is continuing as an open research effort at USC's Information Sciences Institute, and I'm still evaluating publication and research-sponsorship paths — until those decisions are made, the codebase stays closed so no option gets foreclosed.

Everything else lives here: the architecture, the measured benchmarks, the demos, and the design decisions. **If you're a recruiter, engineer, or researcher and want a code walkthrough, reach out — I'm happy to do one live.** [bryanram.com](https://bryanram.com) · [LinkedIn](https://www.linkedin.com/in/bryanrg22)

---

## What it does

Hold the iPhone's Action Button, speak a task — *"Open my most recent recruiter message on LinkedIn"* — and Claude takes over the physical phone: looking at the screen, tapping, typing, scrolling, and swiping through **~22 tool primitives**, while `AVSpeechSynthesizer` narrates every step out loud. A Node.js agent drives the device over USB through an XCTest bridge; a Swift companion app paints live progress into the Dynamic Island.

<div align="center">
  <img src="docs/holding-action-button.jpg" alt="Holding the Action Button to speak a task" width="260"/>
  &nbsp;&nbsp;
  <img src="docs/dynamic-island.jpg" alt="Dynamic Island live agent progress" width="340"/>
  &nbsp;&nbsp;
  <img src="docs/human-in-the-loop.jpg" alt="Human-in-the-loop — Claude asks before sending" width="340"/>
</div>

**Demos:** [full task end-to-end](docs/agentic_ios_demo.mp4) · [first autonomous multi-step drag on a physical iPhone](docs/agentic_ios_drag.mp4)

---

## The results

Six instrumented runs of the same task, one root-cause fix per run: **83 seconds and failing → 23.9 seconds, 4 steps — the theoretical minimum.**

<img src="docs/optimization.svg" alt="Six-run benchmark: 83s failed to 23.9s clean success" width="900"/>

The composed optimizations behind it — prompt caching, rolling summaries, direct XCTest HTTP, 50× JPEG screenshot compression, semantic stuck detection, and CoALA-style memory:

<div align="center">
  <img src="docs/speedup-benchmark.png" alt="Speedup benchmark" width="440"/>
  <img src="docs/time-breakdown.png" alt="Where the time goes" width="440"/>
</div>
<div align="center">
  <img src="docs/token-usage-benchmark.png" alt="Token usage benchmark" width="720"/>
</div>

---

## How it works

<img src="docs/architecture-diagram.png" alt="Architecture — iPhone (Action Button, XCTest Runner, Companion App, Dynamic Island) and MacBook (server, agent, Maestro bridge)" width="900"/>

That's the Mac-hosted path. The same agent also runs **entirely on the phone** — the loop, in Swift, driving the on-device runner directly. The Mac only launches and holds the XCTest runner; after that, the *only* thing that leaves the device is the LLM call:

<img src="docs/architecture-diagram-ondevice.svg" alt="On-device mode — the entire agent loop runs on the iPhone; the Mac only launches and holds the XCTest runner over USB (then unplug, session held over Wi-Fi), and only the LLM API call leaves the device" width="900"/>

The per-step loop — screenshot + UI hierarchy in, one tool call out, auto-captured result back:

<img src="docs/tool-calling-loop.png" alt="Tool-calling loop" width="900"/>

And the memory system that keeps a long task inside a small context window — three memory stores (semantic, episodic, procedural — the CoALA taxonomy), an XML-tagged prompt-cached system prompt, and a rolling summary that compresses history so each step carries a single screenshot:

<img src="docs/technique_memory.svg" alt="CoALA-style memory system" width="900"/>

---

## The research

This project became the foundation of **iOS-Agent**, an open research platform for physical mobile GUI agents at USC ISI's HUMANS Lab — including a novel continuous-trajectory drag primitive (the first autonomous multi-step drag on a physical iPhone, on tasks where Mobile-Agent, AppAgent, and Mobile-Agent-v2 score near zero).

<div align="center">
  <img src="docs/team-on-stage.jpeg" alt="3rd place — Claude Builder Hackathon @ UCLA" width="560"/>
</div>

---

<div align="center">

**Bryan Ramirez-Gonzalez** · USC CS '28 · [bryanram.com](https://bryanram.com) · [LinkedIn](https://www.linkedin.com/in/bryanrg22) · [GitHub](https://github.com/bryanrg22)

</div>
