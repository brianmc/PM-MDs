# Multi-Agent PM AI Team

**Author:** [name]
**Status:** Draft
**Last Updated:** 2026-03-07
**Target Release:** [milestone]

---

> **Working Backwards note:** This PRD follows Amazon's Working Backwards methodology.
> The Press Release and FAQ sections were written first — before any requirements.
> If the press release isn't compelling, the product isn't worth building.
> If the FAQ reveals unanswerable questions, the product isn't ready to design.

---

## Press Release

**FOR IMMEDIATE RELEASE**

### [Company] Launches [ProductName]: The AI-Powered PM Team That Makes Consistent Quality the Default

*Any product manager can now produce senior-quality PRDs, roadmaps, research syntheses, and stakeholder updates — every time, regardless of experience level.*

**[City], March 2026** — [Company] today announced [ProductName], a multi-agent AI system that gives every PM access to a full product management team: a Researcher, a Strategist, a Spec Writer, a Communicator, and a Critic that reviews every output before it reaches you.

For the first time, the quality of PM artifacts is determined by the process — not the person.

**The problem:** Today, the quality of a PRD, roadmap, or stakeholder update depends almost entirely on the individual who wrote it. Two PMs at the same company, working on similar features, produce artifacts of wildly different quality. Engineers spend hours in clarification loops on underspecified requirements. Roadmap decisions get relitigated because the strategy wasn't clearly articulated. Senior PMs rewrite junior PMs' work instead of doing strategic work. There is no organizational standard — just variance.

**How it works:** A PM describes their task in plain language — no templates, no forms. An Orchestrator agent routes the request to the right specialists. The Researcher synthesizes evidence and frames the problem. The Strategist builds the rationale and surfaces trade-offs. The Spec Writer drafts the artifact with full acceptance criteria and edge cases. The Communicator tailors updates for each audience. Before any output reaches the PM, a Critic agent evaluates it against a versioned quality rubric and requires targeted revisions until it passes. The PM receives a finished, review-ready artifact — not a starting point.

**"We built [ProductName] because the best PM teams had unwritten standards in their heads and no way to scale them. [ProductName] makes the standard explicit, enforces it automatically, and raises the floor for every person on the team."** — [Spokesperson Name, Title, Company]

**Getting started:** [ProductName] is available via [API / CLI / web]. PM teams can [onboarding path]. [Pricing or access details.]

**"I used to spend the first hour of every sprint cycle rewriting requirements before sharing them with engineering. Now I describe what I need and get back something I'd actually be proud to send. The Critic catches things I'd have missed on a busy day."** — [PM Name, Title, Company]

---

## External FAQ

*Questions a PM or PM team lead would ask before adopting this.*

**Q: Does this replace product managers?**
No. [ProductName] handles artifact production — the writing, structuring, and quality-checking of PM outputs. It does not replace the judgment, customer relationships, strategy, and prioritization decisions that make a great PM. It removes the low-value work so PMs can focus on the high-value work.

**Q: How does it handle domain knowledge specific to my product or industry?**
You provide the context. The agents work from what you give them — customer quotes, research, backlog items, strategic goals. They structure, synthesize, and critique. They do not invent domain knowledge they don't have. If input is thin, agents flag the gaps rather than hallucinate.

**Q: What if the output is wrong or doesn't reflect what I intended?**
The system is designed for iteration, not one-shot delivery. Every artifact comes back with a visible Critic review showing what passed and what was revised. You edit the output directly. For v1, the PM always reviews and owns the final artifact.

**Q: How do I get my engineering team to trust AI-generated requirements?**
The PRDs [ProductName] produces follow a defined structure with explicit acceptance criteria, edge cases, and non-functional requirements. Engineers can see exactly what the artifact contains and what's marked `[OPEN]`. The format is consistent — which builds trust faster than one-off documents of varying quality.

**Q: Can I customize the quality bar for my team's specific standards?**
[OPEN] v1 ships with a single org-wide rubric. Per-team customization is being evaluated for v2.

**Q: What formats does it output?**
Structured markdown compatible with Notion, Confluence, Linear, and standard doc systems. Direct integrations are planned for Phase 2.

**Q: How do I handle confidential product information?**
Your data is processed via your own Anthropic API key. [ProductName] does not store inputs or outputs beyond the session. [Additional data handling details TBD based on deployment model.]

**Q: How long does it take to produce an artifact?**
Target: under 60 seconds for a standard PRD from a 2,000-token input. Longer for research synthesis with large input corpora.

---

## Internal FAQ

*Questions from engineering, leadership, legal, and go-to-market teams.*

**Q: Why build this now? Why hasn't this been done already?**
Multi-agent systems capable of producing coherent, long-form professional documents have only become reliable at the quality bar required in the last 12-18 months. Earlier attempts at AI writing tools produced drafts that required as much editing as writing from scratch. The combination of strong instruction-following models, structured output, and a critique loop changes the quality equation enough to make this viable.

**Q: Why multi-agent instead of a single powerful model?**
A single model call could produce a PRD draft, but it conflates roles that benefit from separation: the Researcher's job is to question evidence; the Spec Writer's job is to write; the Critic's job is to find gaps. Keeping these roles distinct produces better outputs and makes the system easier to tune. It also allows different model tiers per role (cost optimization) and makes failure modes easier to debug.

**Q: Why build on Claude / Anthropic vs. other models?**
Claude's instruction-following, structured output reliability, and long context handling are best-in-class for this type of document-heavy, multi-step task. The Anthropic SDK provides the agent primitives we need without custom infrastructure.

**Q: What does v1 include vs. later phases?**
v1: Orchestrator + 4 worker agents + Critic, supporting PRDs, roadmaps, research syntheses, and stakeholder updates. Markdown output. No persistent state, no integrations.
Phase 2: Tool integrations (Notion, Jira, Linear), per-team rubric customization, session history.
Phase 3: Analytics, fine-tuning, publishing automation.

**Q: What are the biggest technical risks?**
1. **Latency:** 3-4 sequential LLM calls to produce a single artifact. If any call is slow, the 60-second target slips. Mitigation: parallelize where possible; use faster models for lower-stakes steps.
2. **Critic calibration:** A poorly calibrated rubric produces noisy feedback that degrades output rather than improving it. Mitigation: test rubric against a set of known-good and known-bad PM artifacts before launch.
3. **Hallucination in research synthesis:** The Researcher must not invent data. Mitigation: explicit prompting + Critic check for unsupported claims.

**Q: What are the biggest adoption risks?**
1. **PM trust:** PMs may be reluctant to share AI-generated artifacts with engineering teams. Mitigation: position as a drafting tool the PM edits and owns, not a replacement author.
2. **Rubric legitimacy:** If the quality rubric doesn't match what PMs and engineers consider "good," the Critic's PASS rating is meaningless. Mitigation: co-develop the rubric with target PM teams before launch.
3. **Input friction:** If PMs have to provide too much structured input upfront, adoption drops. Mitigation: natural language input is required; the agents ask clarifying questions rather than demanding a form.

**Q: How do we measure success?**
See Success Criteria section. The most important leading indicator is weekly active usage by the 3 launch teams within 90 days. Artifact quality and time savings are measured post-launch via rubric scores and PM self-report.

**Q: Who are the first 3 teams we're targeting for launch?**
[OPEN] — needs to be identified. Ideal launch team profile: mid-size PM org (5-15 PMs), already has a pain point around PRD quality or onboarding new PMs, has an internal champion willing to give structured feedback.

**Q: What's the business model?**
[OPEN] — not in scope for this PRD. Requires separate decision.

---

## Problem

PM teams produce inconsistent output. The quality of a PRD, roadmap, or stakeholder update depends almost entirely on the individual PM who wrote it — their experience, their templates, their judgment on any given day. There is no organizational standard for what "good" looks like, and no mechanism to enforce it at scale.

The consequences are real:

- Engineers waste time in clarification loops on underspecified requirements
- Roadmap decisions get relitigated because strategy wasn't clearly articulated upfront
- Stakeholder updates vary so widely in depth and format that leadership can't make reliable cross-team comparisons
- Senior PMs spend disproportionate time reviewing and rewriting junior PMs' work rather than doing strategic work

This is not a hiring problem — it's a process problem. PM teams lack a consistent, repeatable system for producing high-quality artifacts. As teams scale, the variance compounds.

**What happens if we do nothing:** PM output quality remains tied to individual skill. Onboarding new PMs takes longer. Engineering efficiency suffers from ambiguous requirements. The PM org becomes a bottleneck rather than a multiplier.

---

## Success Criteria

- 80% of PM artifacts produced with agent assistance meet the quality rubric on first Critic review, within 60 days of a team's first use
- Time spent on PM artifact production (PRDs, roadmap docs, stakeholder updates) decreases by ≥40% for teams using the system, measured via PM self-report at 30 days
- At least 3 PM teams with weekly active usage within 90 days of launch
- PM teams report higher confidence in artifact quality vs. their unassisted workflow (post-session survey, target: ≥4/5)
- Zero "I would not send this to engineering" ratings from PM reviewers on Critic-passed artifacts

---

## User Journey

### Writing a PRD

1. PM opens the system and describes a feature or problem in natural language — no template required
2. Orchestrator classifies the task as spec writing and activates the Researcher and Spec Writer agents
3. Researcher asks up to 3 clarifying questions: Who is affected? What's the evidence? What are the constraints?
4. PM answers in natural language; Researcher produces a structured problem brief
5. Spec Writer drafts a full PRD from the brief — including acceptance criteria, edge cases, and non-functional requirements
6. Critic evaluates the draft against the quality rubric; returns inline feedback on any failing dimensions
7. Spec Writer revises; Critic re-evaluates only the previously failing dimensions
8. PM receives the final artifact, reviews, edits if needed, and exports to their doc system

### Building a Roadmap

1. PM provides context: team goals, current initiatives, backlog items, strategic priorities
2. Orchestrator routes to Strategist agent
3. Strategist asks for constraints (capacity, dependencies, deadlines) then produces a prioritized roadmap with rationale per decision and explicit trade-offs surfaced
4. Critic reviews for internal consistency, missing dependencies, and goals not covered by any initiative
5. Revised roadmap returned to PM with a decision log showing what was deprioritized and why

### Synthesizing User Research

1. PM pastes or uploads raw research input (interview transcripts, survey data, support tickets)
2. Orchestrator routes to Researcher agent
3. Researcher extracts themes, surfaces supporting quotes, identifies gaps, and produces a structured synthesis with confidence levels (high/medium/low) per insight
4. Critic reviews for unsupported claims or missing user segments
5. PM receives final synthesis

### Writing Stakeholder Communication

1. PM specifies the audience (exec, engineering, external customers), context (launch, delay, strategy update), and key message
2. Orchestrator routes to Communicator agent
3. Communicator produces a draft calibrated to the audience — depth, format, and tone adjusted accordingly
4. Critic reviews for clarity, completeness, and appropriate level of detail
5. PM receives final draft ready to send or publish

---

## Requirements

### Requirement 1: Orchestrator Agent

The system must include an Orchestrator agent that receives all user inputs, classifies the task type, and routes to the appropriate worker agent(s).

**Acceptance Criteria:**
- Given a natural language input describing a PM task, the Orchestrator correctly identifies the task type (spec writing, roadmap planning, research synthesis, stakeholder communication) with ≥90% accuracy on a held-out test set
- Given an ambiguous input that could map to multiple task types, the Orchestrator asks one clarifying question before routing
- Given a task that spans multiple types (e.g., "write a PRD and a stakeholder update for this feature"), the Orchestrator activates multiple worker agents and sequences them appropriately
- The Orchestrator must complete task classification and routing within 3 seconds of receiving input
- The Orchestrator passes structured context to each worker agent including: task type, raw user input, and any clarifications collected

---

### Requirement 2: Researcher Agent

The Researcher agent must synthesize user research inputs and produce structured insight documents. It also provides problem framing support to downstream agents (e.g., Spec Writer).

**Acceptance Criteria:**
- Given raw input (transcript, survey, ticket dump) of up to 50,000 tokens, the Researcher produces a synthesis containing: (a) top 3-5 themes with supporting evidence, (b) representative quotes per theme, (c) identified gaps or underrepresented user segments, (d) confidence level (high/medium/low) per insight
- Given a problem description with insufficient evidence, the Researcher explicitly flags this and lists what additional data would strengthen the case — it must not fabricate evidence
- Given a research task with no input provided, the Researcher asks for source material before proceeding
- Output is structured markdown, parseable by downstream agents

---

### Requirement 3: Strategist Agent

The Strategist agent must produce prioritized roadmaps and strategic framing documents from PM-provided inputs.

**Acceptance Criteria:**
- Given a list of initiatives, team goals, and capacity constraints, the Strategist produces a roadmap with: (a) prioritized initiative list with rationale per item, (b) explicit dependencies flagged, (c) a decision log recording what was deprioritized and why
- Given conflicting priorities (e.g., two P0 items that can't both fit in a quarter), the Strategist surfaces the conflict explicitly and presents trade-off options — it must not silently resolve conflicts
- Given a roadmap request with no stated goals, the Strategist asks for success criteria before proceeding
- Every initiative in the output must reference at least one input goal

---

### Requirement 4: Spec Writer Agent

The Spec Writer agent must produce complete, engineer-ready PRDs from problem briefs or direct PM input.

**Acceptance Criteria:**
- Given a problem brief, the Spec Writer produces a PRD containing all of: problem statement, success criteria, user journey, requirements with acceptance criteria, scope (in/out), edge cases, and technical requirements
- Given a requirement that cannot be made testable, the Spec Writer flags it as `[NEEDS CLARIFICATION]` rather than writing vague acceptance criteria
- Given a feature description with no mentioned constraints, the Spec Writer marks performance, security, and scale targets as `[OPEN]` and notes they require PM input
- Each acceptance criterion must follow a given/when/then structure
- PRD output must be valid markdown importable to standard doc systems

---

### Requirement 5: Communicator Agent

The Communicator agent must produce stakeholder-ready communication artifacts calibrated to a specified audience and context.

**Acceptance Criteria:**
- Given an audience type, the Communicator adjusts output depth, format, and tone:
  - Executive: 1-page max, outcomes-focused, no implementation detail
  - Engineering: Technical context included, decision rationale explicit
  - External customer: No internal metrics or roadmap specifics, customer-benefit framing only
- Given a launch announcement context, the output includes: what changed, why it matters to the audience, and what action (if any) is required
- Given a delay or negative update context, the output leads with the impact and provides a path forward — it must not bury the bad news
- Output must pass a reading-level check appropriate to the specified audience

---

### Requirement 6: Critic Agent

The Critic agent must review all worker outputs against the quality rubric before any artifact is returned to the user. The Critic is a required pipeline step — no output bypasses it.

**Acceptance Criteria:**
- Given any PRD draft, the Critic evaluates it against: (a) problem statement clarity and evidence, (b) measurability of success criteria, (c) completeness of acceptance criteria, (d) edge case coverage, (e) presence of non-functional requirements
- Given a draft that passes all dimensions, the Critic returns it with `PASS` status and no blocking feedback
- Given a draft with ≥1 failing dimension, the Critic returns `NEEDS REVISION` status with: inline comments citing each gap and a specific suggested fix per gap
- Given a revised draft, the Critic re-evaluates only the previously failing dimensions — not the full rubric
- Critic review must complete within 10 seconds for documents up to 5,000 tokens
- The Critic must not alter the content of the draft — it annotates only

---

### Requirement 7: Quality Rubric

The system must maintain a versioned, configurable quality rubric used by the Critic agent.

**Acceptance Criteria:**
- The rubric defines pass/fail criteria for each artifact type: PRD, roadmap, research synthesis, stakeholder communication
- The rubric is stored as structured configuration (not hardcoded in the Critic prompt) so it can be updated without redeploying the agent
- Changes to the rubric are versioned; the version used in each Critic review is logged with the output
- [OPEN] Per-team rubric customization — needed for v1 or deferred to Phase 2?

---

## Scope

### In Scope
- Orchestrator agent with task classification and routing
- Four worker agents: Researcher, Strategist, Spec Writer, Communicator
- Critic agent as a required step on all worker outputs
- Four artifact types: PRDs, roadmaps, research syntheses, stakeholder updates
- Natural language input — no structured templates required from user
- Markdown output compatible with common doc systems

### Out of Scope (Future)
- Direct integrations with Notion, Jira, Linear, Confluence — Phase 2
- Real-time multi-PM collaboration on a single artifact — Phase 2
- Per-team rubric customization — Phase 2 (pending resolution of open question above)
- Custom agent personas or org-specific fine-tuning — Phase 3
- Automated artifact publishing or sending — Phase 3
- Team-level PM output analytics dashboard — Phase 3

---

## Edge Cases

- **Incomplete input:** Worker agents must ask for the minimum additional information needed rather than hallucinating context or producing a low-confidence artifact without flagging it
- **Contradictory inputs:** If user input contains internal contradictions (e.g., "urgent and low priority"), the Orchestrator surfaces the contradiction and asks the PM to resolve it before routing
- **Very long inputs:** Inputs exceeding a worker agent's effective context must be chunked; the agent notes that chunking occurred and flags any loss of continuity
- **Critic loop:** If Spec Writer and Critic cycle more than 3 times without reaching PASS, the system returns the best available draft with a note explaining the unresolved issues — it does not loop indefinitely
- **Ambiguous task type:** If the Orchestrator cannot classify with confidence, it presents the top 2 classifications and asks the PM to confirm
- **Empty research input:** The Researcher must not generate synthetic data. If input is insufficient, it returns a structured gap analysis of what data is missing and how to collect it

---

## Technical Requirements

- **Latency:** End-to-end artifact generation must complete within 60 seconds for standard inputs (≤2,000 token input, ≤5,000 token output)
- **Model:** Built on Claude via the Anthropic SDK; `claude-opus-4-6` for Critic and Orchestrator, `claude-sonnet-4-6` acceptable for worker agents
- **Agent communication:** Structured message passing between agents (not raw text); each message must include: sender identity, task context, artifact content, and metadata (version, timestamp)
- **Statelessness:** Each request must be completable in a single agent session; no cross-session state required for v1
- **Observability:** Each agent invocation must be logged with: agent name, input token count, output token count, latency, and Critic pass/fail result
- **API key management:** User-provided Anthropic API key; the system must not store keys beyond the session
- **Rate limiting:** Anthropic API rate limit errors must surface a user-facing message — no silent failures

---

## Technical Context

- The system will be built using the Anthropic SDK (Python or TypeScript) with multi-agent orchestration
- Claude tool use and structured outputs should be used for agent-to-agent communication and rubric evaluation — not freeform prose passing
- The Critic's rubric should be implemented as a tool call with a defined JSON schema to enable consistent, parseable feedback
- MCP tools may be used for document I/O but are not required for v1
- [OPEN] Orchestrator routing strategy: model-based classifier (accurate, ~1s added latency) vs. rule-based keyword matching (fast, brittle on ambiguous inputs)

---

## Open Questions

- [OPEN] Per-team rubric customization — v1 or Phase 2?
- [OPEN] Input UX — CLI, web UI, or API-only for v1? Biggest single lever on adoption trajectory.
- [OPEN] Orchestrator routing: model classifier vs. rule-based? Trade-off: accuracy vs. latency.
- [OPEN] Baseline measurement: how do we capture a "before" quality score on existing PM artifacts before launch, so the 80% success criterion is meaningful?
- [OPEN] First 3 launch teams — who are they? Ideal profile: 5-15 PMs, existing PRD quality pain, internal champion.
- [OPEN] Business model — separate decision, not in scope for this PRD.
- [RESOLVED 2026-03-07] Agent architecture — Orchestrator + worker agents + Critic review loop, over alternatives (pure orchestrator, single agent with critique, specialized role agents without orchestrator).

---

## References

- [Anthropic SDK documentation]
- [Claude model capabilities and context windows]
- [OPEN] Competitive analysis: Notion AI, Atlassian Intelligence, Glean, Product Board AI
- [OPEN] User research / PM team interview notes
