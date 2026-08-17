# Roadmap — AI-OS Experiment

## Phase 0 — Foundation (Current)
- [x] Create repository and document vision
- [x] Collect practical rooted-emulator setup methods
- [ ] Automate root verification scripts
- [ ] Choose primary development emulator (recommend: Android Studio AVD + rootAVD/Magisk)

## Phase 1 — Observation Layer
Build the sensing side of an AI OS:
- Continuous collection of process list, CPU/memory/IO stats from `/proc` and `/sys`
- Structured logging / metrics store
- Simple LLM or rule-based agent that can answer "what is the system doing right now?"

## Phase 2 — First Decision Loop (AI Process Supervisor)
- Agent receives natural-language goals ("keep browser responsive", "prioritize compilation", "kill anything using >2 GB that isn't the main app")
- Agent proposes actions (nice, kill, renice, cgroup moves)
- Human-in-the-loop approval at first, then limited auto-execution under root

## Phase 3 — Service & Init Experiments
- Wrap or replace selected Android services with AI-mediated versions
- Experiment with Magisk modules that inject AI logic
- Explore eBPF + AI for more fine-grained control without full kernel rewrite

## Phase 4 — Conversational Interface
- Primary user interface becomes chat with the AI OS
- Intent → system action translation
- Long-term memory of user preferences and system history

## Phase 5+ — Deep Systems Work (Long Horizon)
- Custom kernel modules or patches
- Unikernel / microkernel experiments designed for AI mediation
- Research into formal verification of AI OS decisions
- Eventually: can we boot something that is no longer recognizably Linux?

---

**Principle**: Every phase must leave a working, demonstrable artifact and clear documentation. We prefer small working experiments over grand unfinished designs.
