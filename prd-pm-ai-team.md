# Multi-Agent PM AI Team

**Author:** [name]
**Status:** Draft
**Last Updated:** 2026-03-07
**Target Release:** [milestone]

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

- 80% of PM artifacts produced with agent assistance meet a defined quality bar on first review (vs. a baseline to be established pre-launch)
- Time spent on PM artifact production (PRDs, roadmap docs, stakeholder updates) decreases by ≥40% for teams using the system
- At least 3 PM teams actively using the system within 90 days of launch, with weekly active usage
- PM teams report higher confidence in artifact quality compared to their unassisted workflow (measured via post-session survey, target: ≥4/5)

---

## User Journey

### Writing a PRD

1. PM opens the system and describes a feature or problem in natural language (no template required)
2. Orchestrator receives the input, identifies it as a spec-writing task, and activates the Researcher and Spec Writer agents
3. Researcher agent asks clarifying questions: Who is affected? What's the evidence? What are the constraints?
4. PM answers in natural language; Researcher synthesizes a problem brief
5. Spec Writer agent drafts a full PRD using the problem brief as input — including acceptance criteria, edge cases, and non-functional requirements
6. Critic agent reviews the draft against a defined quality rubric and produces structured feedback inline
7. Spec Writer revises based on Critic feedback; result is returned to the PM
8. PM reviews the output, makes any edits, and exports or saves to their doc system

### Building a Roadmap

1. PM provides context: team goals, current initiatives, backlog items, strategic priorities
2. Orchestrator routes to Strategist agent
3. Strategist asks for constraints (capacity, dependencies, deadlines) and produces a prioritized roadmap with rationale for each decision
4. Critic agent reviews the roadmap for internal consistency, missing dependencies, and unstated assumptions
5. Revised roadmap returned to PM with a decision log

### Synthesizing User Research

1. PM pastes or uploads raw research input (interview transcripts, survey data, support tickets, feedback threads)
2. Orchestrator routes to Researcher agent
3. Researcher extracts themes, surfaces quotes, identifies gaps in the data, and produces a structured research synthesis document
4. Critic agent reviews for unsupported claims or missing segments
5. PM receives synthesis with confidence levels per insight

### Writing Stakeholder Communication

1. PM describes the audience (exec, engineering, external customers), the context (launch, delay, strategy update), and the key message
2. Orchestrator routes to Communicator agent
3. Communicator produces a draft update calibrated to the audience — depth, format, and tone adjusted accordingly
4. Critic agent reviews for clarity, completeness, and appropriate level of detail for the audience
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
- The Orchestrator passes structured context to each worker agent, including: task type, raw user input, and any clarifications collected

---

### Requirement 2: Researcher Agent

The Researcher agent must synthesize user research inputs and produce structured insight documents. It also provides problem framing support to other agents (e.g., the Spec Writer).

**Acceptance Criteria:**
- Given raw input (transcript, survey, ticket dump) of up to 50,000 tokens, the Researcher produces a synthesis document containing: (a) top 3-5 themes with supporting evidence, (b) representative quotes per theme, (c) identified gaps or underrepresented user segments, (d) confidence level (high/medium/low) per insight
- Given a problem description with insufficient evidence, the Researcher explicitly flags this and lists what additional data would strengthen the case — rather than fabricating evidence
- Given a research synthesis task with no input provided, the Researcher asks for source material before proceeding
- Output format is structured markdown, parseable by downstream agents

---

### Requirement 3: Strategist Agent

The Strategist agent must produce prioritized roadmaps and strategic framing documents from provided inputs.

**Acceptance Criteria:**
- Given a list of initiatives, team goals, and capacity constraints, the Strategist produces a roadmap with: (a) prioritized initiative list with rationale per item, (b) explicit dependencies flagged, (c) a decision log recording what was deprioritized and why
- Given conflicting priorities (e.g., two P0 items that can't both fit in a quarter), the Strategist surfaces the conflict explicitly and presents trade-off options rather than silently resolving it
- Given a roadmap request with no stated goals, the Strategist asks for success criteria before proceeding
- Roadmap output must reference the input goals and show how each initiative maps to at least one goal

---

### Requirement 4: Spec Writer Agent

The Spec Writer agent must produce complete, engineer-ready PRDs from problem briefs or user-provided context.

**Acceptance Criteria:**
- Given a problem brief (from Researcher or directly from user), the Spec Writer produces a PRD containing all of: problem statement, success criteria, user journey, requirements with acceptance criteria, scope (in/out), edge cases, and technical requirements
- Given a requirement that cannot be made testable (too vague), the Spec Writer flags it as `[NEEDS CLARIFICATION]` rather than writing vague acceptance criteria
- Given a feature description with no mentioned constraints, the Spec Writer explicitly notes that performance, security, and scale targets are `[OPEN]` and prompts the PM to fill them in
- Each acceptance criterion must follow a given/when/then structure
- PRD output must be valid markdown and importable to standard doc systems

---

### Requirement 5: Communicator Agent

The Communicator agent must produce stakeholder-ready communication artifacts calibrated to a specified audience and context.

**Acceptance Criteria:**
- Given an audience type (executive, engineering, external customer), the Communicator adjusts output depth, format, and tone accordingly:
  - Executive: 1-page max, outcomes-focused, no implementation detail
  - Engineering: Technical context included, decision rationale explicit
  - External customer: No internal metrics or roadmap specifics, customer-benefit framing only
- Given a launch announcement context, the Communicator produces a draft that includes: what changed, why it matters to the audience, and what action (if any) is required
- Given a delay or negative update context, the Communicator produces a draft that does not obscure the bad news — it leads with the impact and provides a path forward
- Output must pass a reading-level check appropriate to the specified audience

---

### Requirement 6: Critic Agent

The Critic agent must review all worker agent outputs against a defined quality rubric before returning results to the user. The Critic operates as a required step in the pipeline — no output reaches the user without Critic review.

**Acceptance Criteria:**
- Given any PRD draft, the Critic evaluates it against the following dimensions and produces structured inline feedback: (a) problem statement clarity and evidence, (b) measurability of success criteria, (c) completeness of acceptance criteria, (d) coverage of edge cases, (e) presence of non-functional requirements
- Given a PRD draft that passes all rubric dimensions, the Critic returns it with a `PASS` status and no blocking feedback
- Given a PRD draft with ≥1 failing dimension, the Critic returns it with a `NEEDS REVISION` status, inline comments citing the specific gap, and a suggested fix per gap
- Given a revised draft, the Critic must re-evaluate only the previously failing dimensions (not re-run the full rubric) to avoid infinite loops
- The Critic must complete review within 10 seconds for documents up to 5,000 tokens
- The Critic must not alter the content of the draft — it annotates only

---

### Requirement 7: Quality Rubric

The system must maintain a versioned, configurable quality rubric used by the Critic agent.

**Acceptance Criteria:**
- The rubric defines pass/fail criteria for each artifact type: PRD, roadmap, research synthesis, stakeholder communication
- The rubric is stored as a structured configuration (not hardcoded in the Critic prompt) so it can be updated without redeploying the agent
- Changes to the rubric are versioned; the version used for each Critic review is logged with the output
- [OPEN] Should PM teams be able to customize the rubric per team, or is a single org-wide rubric sufficient?

---

## Scope

### In Scope
- Orchestrator agent with task classification and routing
- Four worker agents: Researcher, Strategist, Spec Writer, Communicator
- Critic agent applied to all worker outputs before delivery
- Support for four artifact types: PRDs, roadmaps, research syntheses, stakeholder updates
- Natural language input (no structured templates required from user)
- Markdown output compatible with common doc systems

### Out of Scope (Future)
- Direct integrations with specific tools (Notion, Jira, Linear, Confluence) — Phase 2
- Real-time collaboration (multiple PMs working on the same artifact simultaneously) — Phase 2
- Custom agent personas or org-specific agent fine-tuning — Phase 3
- Automated artifact publishing or sending — Phase 3
- Analytics dashboard showing team-level PM output metrics — Phase 3

---

## Edge Cases

- **Incomplete input:** If a PM provides insufficient context to generate a useful artifact, the relevant worker agent must ask for the minimum additional information needed — it must not hallucinate context or produce a low-confidence artifact without flagging it
- **Contradictory inputs:** If user input contains internal contradictions (e.g., "this is urgent and low priority"), the Orchestrator surfaces the contradiction and asks the PM to resolve it before routing
- **Very long inputs:** Inputs exceeding the context window of a worker agent must be chunked; the agent must note that chunking occurred and flag any loss of continuity
- **Critic loop:** If the Spec Writer and Critic cycle more than 3 times without reaching a PASS, the system returns the best available draft with a note explaining the unresolved issues — it does not loop indefinitely
- **Ambiguous task type:** If the Orchestrator cannot classify a task with confidence, it presents the top 2 classifications and asks the PM to confirm before proceeding
- **Empty or near-empty research input:** Researcher agent must not generate synthetic research data. If input is insufficient, it must return a structured gap analysis explaining what data is missing and how to collect it

---

## Technical Requirements

- **Latency:** End-to-end artifact generation (input to final revised output) must complete within 60 seconds for standard-length inputs (≤2,000 token input, ≤5,000 token output)
- **Model:** Built on Claude via the Anthropic SDK; use the most capable available model at build time (currently `claude-opus-4-6` for Critic and Orchestrator, `claude-sonnet-4-6` acceptable for worker agents)
- **Agent communication:** Agent-to-agent communication via structured message passing (not raw text); each message must include: sender identity, task context, artifact content, and metadata (version, timestamp)
- **Statelessness:** Each request must be completable in a single agent session; no cross-session state is required for v1
- **Observability:** Each agent invocation must be logged with: agent name, input token count, output token count, latency, and Critic pass/fail result
- **API key management:** User-provided Anthropic API key; the system must not store keys beyond the session
- **Rate limiting:** The system must handle Anthropic API rate limit errors gracefully — surface a user-facing message rather than failing silently

---

## Technical Context

- The system will be built using the Anthropic SDK (Python or TypeScript) with multi-agent orchestration
- Claude supports tool use and structured outputs, which should be used for agent-to-agent communication and rubric evaluation
- The Critic's quality rubric should be implemented as a tool call or structured prompt with a defined schema — not freeform prose — to enable consistent, parseable feedback
- MCP (Model Context Protocol) tools may be used for document I/O, but are not required for v1
- [OPEN] Should the Orchestrator use a classifier model call to route tasks, or rule-based routing on keywords? Classifier is more robust but adds latency.

---

## Open Questions

- [OPEN] Should PM teams be able to customize the Critic rubric per team, or is a single org-wide rubric sufficient for v1?
- [OPEN] What is the right input UX? CLI, web UI, or API-only for v1? This affects adoption trajectory significantly.
- [OPEN] Should the Orchestrator use a model-based classifier or rule-based routing? Trade-off: accuracy vs. latency.
- [OPEN] How do we measure "quality bar" at baseline (before agent assistance) to make the 80% success criterion meaningful?
- [RESOLVED 2026-03-07] Agent architecture — Orchestrator + worker agents + Critic review loop selected over alternatives (pure orchestrator, specialized role agents without orchestrator, single agent with critique).

---

## References

- [Anthropic SDK documentation]
- [Claude model capabilities and context windows]
- [OPEN] Link to competitive analysis (Notion AI, Atlassian Intelligence, etc.)
- [OPEN] Link to user research / PM team interviews if available
