---
title: "Yi-Wei Lien · 連奕維"
---

# Yi-Wei Lien · 連奕維

### **Backend & AI Engineer · NTU CS MS · MIRLab · MSLab**

I'm a CS master's student at National Taiwan University, split between two labs: [MIRLab](https://mirlab.org) (Multimedia Information Retrieval Lab, where I work on quantitative trading strategies for TWSE) and MSLab (Machine Discovery and Social Network Mining, where I research knowledge conflicts and memory in large language models). Before grad school I spent 1.5 years building backend systems at Shopback and Cmoney — real products, real traffic, real fire drills.

I care about two things that often get siloed: making backend systems that *actually don't fall over*, and understanding AI at a research level. On the industry side I'm drawn to AI engineering and infrastructure — building the systems that make LLM products reliable at scale. On the research side I'm fascinated by agentic AI, RAG systems, and applying deep learning to quantitative trading. First year of the MS, thesis direction still open.

If you want to know more about the work: browse [Posts](/posts), check [Projects](/projects), or just [email me](mailto:ted20030214@gmail.com).

---

## 🏢 Work Experience

### Software Engineer Intern — **Shopback** *(May 2025 – Nov 2025)*
*Taipei · Hybrid · TypeScript · Kubernetes Experience · SQL · Monorepo*

Shopback is Southeast Asia's leading cashback and rewards platform with tens of millions of users. I worked on the backend team inside a TypeScript monorepo, touching customer support infrastructure, watchlist features, and travel notification systems.

Key contributions:
- **Migrated 10+ cross-service API calls to direct function calls** as part of a mono-repo consolidation. The driver was uneven container load — separate static resource containers were causing traffic spikes and unstable scaling, so they were merged into a shared monorepo structure to balance load. A side effect: each cross-service call dropped from ~100ms to under 10ms once the network hop was removed.
- **Connected a recommendation system to the search page**, handling 1M+ API requests/month. The integration drove a +30% increase in user access behaviour post-deployment.
- **Resolved 8+ production backend issues**, achieving 10–15% error rate reduction on the affected APIs. Most were noise-reduction work — downgrading alert severity on known benign patterns and refactoring the error handling workflow around them. Two involved investigating non-deterministic behaviour from third-party API integrations and updating the handling logic accordingly.

---

### Backend Development Engineer Intern — **Cmoney** *(Jul 2024 – Apr 2025)*
*New Taipei City · Hybrid · C# · ASP.NET · MongoDB · Microservice*

Cmoney is Taiwan's leading financial platform with 7M+ app downloads. I joined during an active microservice refactor of their core backend.

Key contributions:
- **Implemented microservice refactors for 9 backend APIs** in the flagship product. This meant extracting shared business logic out of a monolith, defining clean service interfaces, and migrating data access patterns from direct DB calls to versioned service contracts.
- Worked primarily in C# and ASP.NET with MongoDB as the document store. Got deeply comfortable with the kinds of high-concurrency edge cases that only appear when you have millions of real users reading financial data at market open.
- Learned OOP architecture patterns the hard way — not from textbooks, but from inheriting code that didn't use them.

> Refactoring someone else's production system is the best education in software architecture I've had. You learn what "good design" actually buys you when the tests are green but the code is unmaintainable.

---

### Research Assistant (Undergraduate) — **Academia Sinica, Citi Lab** *(Sep 2023 – Jun 2024)*
*Nangang · Remote · Python · Cybersecurity · Synthetic Data · GAN*

Worked in the Citi (Cyber Intelligence) lab under Academia Sinica. The research goal was improving intrusion detection by generating realistic synthetic Linux audit logs to augment training data for GAN-based detection models.

Key contributions:
- **Built a data pipeline processing 200M+ Linux audit log records** — parsing, normalizing, and structuring raw audit data into sequences suitable for sequence-to-sequence GAN training.
- Navigated the challenge of synthetic data quality for security: a GAN that generates "realistic" logs is only useful if the synthetic attacks fool a detector trained on real data. We spent significant time on evaluation methodology to avoid optimistic benchmarks.
- First real exposure to the gap between academic benchmarks and operational reality in security ML.

---

### Teaching Assistant — **NTU Dept. of CS, Data Structures & Algorithms** *(Feb 2023 – Jun 2023)*
*Taipei · On-site*

TA for NTU's undergraduate DSA course. Ran weekly lab sessions, answered questions on office hours, graded assignments and exams. Learned that explaining something clearly is harder than understanding it yourself.

---

## 🔬 Research & Projects

### ICC Mem/Gen — Knowledge Conflicts in LLMs *(Mar 2026 – Jun 2026)*
*MSLab · 4-person team · LoRA · PEFT · Activation Steering*

**The question**: When a language model has been fine-tuned to believe X, but its pretraining says Y, which wins — and can we steer that without retraining?

- Built a **cross-format data augmentation pipeline** for ConflictQA and ConflictBank, generating semantically equivalent question variants to stress-test knowledge conflict handling across paraphrase and format shifts.
- Used **activation steering** to extract fine-tuned behavioral vectors and apply them to a frozen base model. Key finding: the behavioral direction is surprisingly transferable — the steering vector from a LoRA-trained model can push a base model's completions in the expected direction even without any weight updates.
- The broader implication: fine-tuning injects directional bias into the residual stream in a way that's partially separable from task knowledge. This matters for alignment — if you can steer behavior without weights, you can also potentially *undo* fine-tuning signals.

📄 *Paper under review, Neurips 2026.*

---

### Algorithm Trading — TWSE Daily Strategy *(Jan 2026 – Jun 2026)*
*MIRLab · 3-person team · Python · TWSE · Quant*

Developing systematic daily trading strategies for the Taiwan Stock Exchange. The project involves signal generation, backtesting infrastructure, and live paper-trading evaluation.

Work so far:

- Researching momentum-based and mean-reversion signals on daily OHLCV data. The TWSE is a retail-dominated market with different microstructure than US equities — many patterns from Western quant literature don't transfer cleanly.
- Currently evaluating ML-based signal combination.

---

### Bubblo — 2D Platformer Game Demo *(Apr 2025 – Jun 2025)*
*6-person team · Project Lead · Unity · C# · OOP · Design Patterns · 600+ commits*

Led a 6-person team to build a complete 2D platformer game demo in Unity over one semester. 600+ commits across the team.

What I actually did as project lead:
- Designed the component architecture before a single line of game code was written. We used Observer, State Machine, and Command patterns explicitly — because with 6 people touching the same codebase, implicit coupling kills velocity.
- Ran weekly standups and maintained a task board. Learned that project management in a student team is 80% unblocking people and 20% writing code yourself.
- The final demo runs at stable 60fps, has 5 playable levels, full audio, and a proper game loop with lives and scoring.

🎮 [Play the demo](https://lien0214.itch.io) *(coming soon)*

---

### E-MoTchi — Emoji-to-Mandarin NLP *(Nov 2024 – Dec 2024)*
*5-person team · Taiwan-LLaMA · Google mT5 · LoRA · RLHF*

Fine-tuned multilingual LLMs for the task of translating emoji sequences into natural Mandarin expressions. Designed the dataset from scratch.

- Built the training dataset by crowdsourcing emoji-to-Mandarin mappings and then applying rule-based augmentation. The key challenge: emoji meaning is deeply contextual and varies by platform, age group, and Taiwan-specific internet culture.
- Fine-tuned **Taiwan-LLaMA** and **Google mT5** using LoRA (low-rank adaptation) with RLHF-style preference optimization. mT5 outperformed LLaMA on this task due to its explicit multilingual pretraining.
- The most interesting result: models that scored well on BLEU performed noticeably worse on human evaluation for "feels natural" — a clean example of reward hacking where the metric diverges from the goal.

---

## 🛠 Technical Skills

### Languages
`TypeScript` `Python` `C#` `Go (learning)` `SQL`

### Backend & Infrastructure
`ASP.NET Core` `Node.js` `Kubernetes` `Docker` `MongoDB` `PostgreSQL` `Redis` `Microservices` `REST APIs` `CI/CD`

### ML & AI
`PyTorch` `Transformers (HuggingFace)` `LoRA / PEFT` `Activation Steering` `RAG` `RLHF` `Fine-tuning` `Taiwan-LLaMA` `mT5`

### Tools & Workflow
`Git` `GitHub Actions` `Unity` `Linux` `Vim` `Grafana`

---

## 📚 What I'm Reading / Thinking About

> This section updates manually — things I'm actively digesting.

**Currently reading:**
- *Representation Engineering* (Zou et al., 2023) — the theoretical backbone for the activation steering work in ICC Mem/Gen
- *Flash Attention 2* implementation details — trying to understand what makes attention fast at the hardware level
- Taiwan quant literature on TWSE microstructure — most papers are in Chinese and surprisingly rigorous

**Currently thinking about:**
- Whether behavioral vectors from activation steering are actually causal or just correlational — the experiments we ran are suggestive but not conclusive
- How you'd build a trading system that's genuinely live-trading safe in Taiwan (regulatory constraints, broker APIs, latency)
- The right way to evaluate "knowledge" in an LLM — existing benchmarks feel too static

---

## 📬 Contact

I'm open to research collaboration, job conversations (targeting AI engineer roles, ~2027 graduation), or just nerdy conversations about LLMs and quant finance.

**Email**: [ted20030214@gmail.com](mailto:ted20030214@gmail.com)
**GitHub**: [github.com/lien0214](https://github.com/lien0214)
**LinkedIn**: [linkedin.com/in/yi-wei-lien](https://www.linkedin.com/in/yi-wei-lien/)

<form action="https://formspree.io/f/YOUR_FORM_ID" method="POST" style="max-width: 520px; margin: 24px 0; padding: 24px; border: 1px solid var(--border); border-radius: 8px; background: var(--code-bg);">
  <input type="text" name="_gotcha" style="display:none" />
  <div style="margin-bottom: 14px;">
    <label style="display:block; margin-bottom:6px; font-size:0.9em;">Name</label>
    <input type="text" name="name" required style="width:100%; padding:9px 12px; border:1px solid var(--border); border-radius:4px; background:var(--theme); color:var(--content); font-size:0.95em;">
  </div>
  <div style="margin-bottom: 14px;">
    <label style="display:block; margin-bottom:6px; font-size:0.9em;">Email</label>
    <input type="email" name="_replyto" required style="width:100%; padding:9px 12px; border:1px solid var(--border); border-radius:4px; background:var(--theme); color:var(--content); font-size:0.95em;">
  </div>
  <div style="margin-bottom: 14px;">
    <label style="display:block; margin-bottom:6px; font-size:0.9em;">Message</label>
    <textarea name="message" required rows="5" style="width:100%; padding:9px 12px; border:1px solid var(--border); border-radius:4px; background:var(--theme); color:var(--content); font-size:0.95em; resize:vertical;"></textarea>
  </div>
  <button type="submit" style="background:var(--primary); color:var(--theme); padding:10px 24px; border:none; border-radius:4px; cursor:pointer; font-weight:600; font-size:0.95em;">Send Message</button>
</form>
