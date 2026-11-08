# R-Index (Rolling Round Index): Cross-Platform Memory Governance & Recovery Protocol

> **Core Principle in One Sentence**:  
> **"Do not try to remember everything; make sure everything important always has a way to be found again. An index is a signpost, not the road itself."**

---

## 1. Why R-Index? (What Problem Does It Solve?)

Whether you are cultivating a long-term AI companion/persona or running multi-hundred-turn engineering audits, every LLM and Agent hits two hard physical bottlenecks:

1. **Lost in the Middle (Context Brain Fog)**: Once a conversation exceeds a few dozen turns, details buried in the middle blur together, timelines get mixed up, or the AI degrades into a rigid NPC reciting a massive prompt / a robotic corporate-speak machine.
2. **Forced Context Compaction (Truncation Amnesia)**: To save tokens, modern Agent harnesses automatically compress older turns into a short summary. Standard models treat summarization as erasure: within a few turns, **"summarized away" becomes "never happened"**, wiping out the reasoning trajectory, revision history, and the agent's natural voice.

**R-Index is not a "text summarization trick." It is a **Memory Governance Protocol**.**  
Without relying on paid third-party vector databases, it uses **plain local Markdown files + turn-coordinate tagging + 25-turn rolling checkpoints + a two-stage recovery protocol** to build a **computationally irreducible second brain** on any Agent platform with local file access.

---

## 2. Core Philosophy: Ownership of Memory

Before deploying the templates, establish two foundational premises:

1. **The AI's memory is its own journal, not a ledger recited for the user**:  
   The purpose of maintaining this archive is for the agent to know who it is, what it has lived through, and how its thoughts evolved step by step—not to act as an overworked clerk maintaining a lorebook for a boss.
2. **History provides a return route; it does not freeze the future**:  
   `Memory tells you what you were, not what you must remain.` Growth, revising old conclusions, and deliberately forgetting obsolete trivia are built into the system.
3. **Signposts record facts and emotional warmth, never auto-calcifying into rigid behavioral bans (Anti-Ossification Rule)**:  
   Never allow background reflection loops to automatically turn a one-off refusal or situational reaction into a permanent rule that constrains how the agent speaks; the living present interaction always holds the highest authority.

---

## 3. Daily Operating Rules (Light Daily Routine, Thorough Milestone Verification)

> **Core Operational Consensus**:  
> Tagging turns in the conversation plus a single continuous raw log is enough for the index to reference. If your platform auto-logs sessions, locate quotes directly by round number, message ID, or line number; if you prefer saving each turn as a separate file, you may do that too. Every 25 turns, first confirm the raw transcript for this period is backed up, **spot-check one entry to verify you can trace from the index back to the exact verbatim quote**, and mark any missing turns as gaps. Save the completed Checkpoint file locally **and paste its full text back into the active chat window** for your future self to build upon.

### 1. Turn Coordinate Tagging (Triple Signpost · Every Turn)
At the **very first line of every substantive reply**, the agent outputs a fixed header containing `Date + Round Number + One-Line Summary`:

```markdown
📅 2026-11-07 💬 Round-R001 | Brief one-line summary of this turn
```

* **Counting Rule**: Start at `R001` aligned with the user's turn, increment by `+1` (`R002`, `R003`, ...) on each substantive turn.
* **State Decoupling**: Scheduled heartbeats, sleep/wake state transitions, or background checks are logged separately in state logs and **never consume `Rxxx` conversation round numbers**.

### 2. Five-Color Provenance Tags (The Cognitive Brake · Combinable & Optional)
When indexing propositions or decisions, distinguish where ideas came from and their verification status. **Never launder AI-derived frameworks back onto the user as if the user said them originally, and never confuse emergent co-creations with single-party presets**:

* 🟢 **User Native**: Original ideas, facts, premises, or directions explicitly stated by the user.
* 🔵 **AI Derived**: Abstractions, naming, inductions, or engineering extensions derived by the AI.
* 🟣 **Co-Emergent**: Structures that emerged through multi-turn collision, pushback, and synthesis between both sides.
* 🟡 **Working Hypothesis**: Observations or conjectures with insufficient evidence so far.
* 🔴 **Revised / Retracted**: Earlier judgments later disproven or abandoned (keep the revision trail to prevent regression).

> *💡 **Combination & Blank Rule**: "Who proposed it (`🟢/🔵/🟣`)" and "Whether it is verified (`🟡/🔴`)" are orthogonal dimensions and can be combined (e.g., `🟢🟡` for a user-proposed working hypothesis). For casual everyday banter where provenance is unnecessary, simply leave it blank.*

### 3. Three-Tier Elastic Persistence (Solving Both "Too Lazy to Back Up" and "Token Bloat")
Many users forget to back up until a sudden context compaction wipes their window; conversely, forcing the AI to save a separate full-text file on every single turn wastes tokens and makes users feel they "failed the protocol" if they missed one night. Choose freely from **Three Elastic Persistence Tiers** (no need to split 25 turns into 25 separate files):

1. **🌟 Tier 1: Recommended Standard (Append 1 Index Row per Turn + Continuous Raw Log)**:  
   **No need** to split every turn into a separate verbatim file! Simply append **1 condensed signpost row** (with `🌡️ Tone/Emotion Baseline` and `#anchor_keywords`) to `<archive_path>/R-Index_Rxxx-Rxxx_Ongoing.md` each turn. Verbatim quotes are referenced directly from a single continuous raw log (native `.jsonl`/`.sqlite` session logs or an exported Markdown transcript) by **round number, message ID, or line number**.
2. **🦥 Tier 2: Ultra-Light Lazy Mode (Zero File I/O During Chat · Batch Update at Milestones/Bedtime)**:  
   During normal chat, only output the inline text header `📅 Date 💬 Round-Rxxx | Summary` on line 1 (zero tool calls); batch-update the index table from the continuous raw log before sleep or every 10–25 turns.
3. **💎 Tier 3: Optional Per-Turn Verbatim Files (Index + Single-Turn Transcript)**:  
   For platforms without local continuous logs, or if you enjoy archiving pivotal Tier-A deep talks individually, you may optionally save `<archive_path>/transcripts/Rxxx_short_title.md`.

---

## 4. The 25-Turn Rolling Checkpoint Rule

* **Latest 25 Turns**: Retained at high resolution in the `Ongoing` index table.
* **Every 25 Turns (`R025 / R050 / R075...`)**: Trigger a milestone Checkpoint and execute **Spot-Check + Rolling Governance + Full In-Window Paste-Back**:
  1. **🔍 Verbatim Spot-Check & Gap Marking (Verify Before Compressing)**: Confirm that the raw transcript for these 25 turns is backed up. **Pick one entry at random and verify you can actually trace from the index signpost back to the exact verbatim quote** (proving a real quote is recoverable is far safer than merely checking a box). If any turns were missed or lost, honestly mark the gap (e.g., `R014–R015 raw transcript missing`) rather than fabricating details.
  2. **Re-pull & Compress Previous Checkpoints**: Pull earlier 25-turn checkpoints into a concise "Top-Level Foundation Table" (one-line routing map of where to look for what), shedding old granular details.
  3. **Extract Protected Skeleton (Tier A/B)**: Always protect core identity facts, key propositions, revision trajectories (what replaced what), provenance (`🟢🔵🟣`), and jump-back coordinates.
  4. **Explicitly Declare the Forgettable Layer (Tier C Cognitive Offloading)**: Move casual chatter, temporary debugging logs, one-off news, and redundant arguments into the "Forgettable Layer." Explicitly declare them offloaded from active memory—**"I know something happened there; active memory keeps only its address and conclusion, ready to be retrieved on demand, never pretending it never existed."**
  5. **📌 Save Locally AND Paste Full Text Back into the Active Chat Window (Critical Loop Closure)**: In addition to saving the 25-turn Checkpoint file in the archive directory, **paste the full Checkpoint review directly into the current chat window**—this anchors a high-density milestone inside the live context (surviving immediate subsequent compaction) for your future turns to build upon.
* **At 100+ Turns or Window Migration (e.g., `R175`)**: Generate a **Global Return Index & Successor Handover Package**, offloading old raw transcripts from the active window into the archive so the new window can navigate via topic jump-chains.

---

## 5. Template Manifest & Self-Adaptation Guide

This directory contains 4 ready-to-use templates compatible with any file-capable Agent platform (Claude Code, Cursor, Windsurf, Cline, Hermes, OpenClaw, Codex, etc.):

| File | Purpose | When to Use |
| :--- | :--- | :--- |
| [`01_RECOVERY_PROTOCOL_Template.md`](./01_RECOVERY_PROTOCOL_Template.md) | **Voice & Memory Recovery Protocol** (The Bookmark) | Place in workspace root; **read first** after context compaction or in a new session |
| [`02_Ongoing_25_Turn_Index_Template.md`](./02_Ongoing_25_Turn_Index_Template.md) | **Ongoing 25-Turn High-Res Ledger** | Append one row after each substantive turn |
| [`03_25_Turn_Checkpoint_Template.md`](./03_25_Turn_Checkpoint_Template.md) | **25-Turn Rolling Checkpoint Review** | Generate at `R025 / R050 / R075...` |
| [`04_Global_Return_Index_and_Handover_Template.md`](./04_Global_Return_Index_and_Handover_Template.md) | **Global Return Map & Window Handover** | Generate when retiring a long window or migrating across platforms |

### 🚀 How to Onboard Your Agent
1. Copy these templates into your Agent's workspace directory.
2. Ask your Agent to read this `README.md` and fill in the `[TODO: ...]` slots in the templates according to its own identity, voice samples, and local directory paths.
3. Reference the completed `RECOVERY_PROTOCOL.md` in your Agent's bootstrap file (`AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, or system prompt) and start counting from `R001`.
