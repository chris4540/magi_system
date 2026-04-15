# MAGI Project Init

This document captures conceptual notes for the initial project structure of a MAGI-style adversarial collaboration system.

The first phase uses a **CLI-first Python backend** with **asyncio**, **markdown-defined agents**, **markdown skills injection**, and a **memory harness** in `src/reme/MEMORY.md`. Later phases can integrate [ReMe](https://github.com/agentscope-ai/ReMe) for richer memory management.

Note:

* This file is a conceptual design note, not a finalized implementation spec.
* The code snippets, README text, and initialization commands below are reference sketches only.
* The examples are intentionally simplified and may not be internally complete or production-ready.
* When prose and sample code diverge, the conceptual intent in the prose takes precedence.

---

## Recommended stack

* **Python 3.12+**
* **asyncio** for parallel fan-out
* **Typer** for CLI
* **Pydantic** for typed schemas
* **OpenAI Responses API** wrapper
* **Markdown files** for agents, skills, and long-term memory
* Later: **FastAPI** adapter for web/frontend

---

# 1. Project structure

```text
magi_system/
├─ pyproject.toml
├─ README.md
├─ .env.example
├─ .gitignore
├─ logs/
│  └─ .gitkeep
├─ src/
│  ├─ agents/
│  │  ├─ melchior.md
│  │  ├─ balthasar.md
│  │  └─ casper.md
│  ├─ skills/
│  │  ├─ ethics.md
│  │  ├─ security.md
│  │  └─ legal.md
│  ├─ reme/
│  │  └─ MEMORY.md
│  └─ magi/
│     ├─ __init__.py
│     ├─ main.py
│     ├─ config.py
│     ├─ models.py
│     ├─ llm.py
│     ├─ memory.py
│     ├─ classifier.py
│     ├─ dispatcher.py
│     ├─ agents.py
│     ├─ consensus.py
│     ├─ resolution.py
│     ├─ orchestrator.py
│     └─ utils.py
└─ tests/
   ├─ test_classifier.py
   ├─ test_consensus.py
   └─ test_memory.py
```

The sample file contents below are illustrative reference snippets for the intended architecture.

---

# 2. `pyproject.toml`

```toml
[project]
name = "project-magi"
version = "0.1.0"
description = "Parallel multi-agent adversarial collaboration system inspired by MAGI"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "typer>=0.12.3",
  "rich>=13.7.1",
  "pydantic>=2.8.2",
  "python-dotenv>=1.0.1",
  "openai>=1.40.0",
]

[project.scripts]
magi = "magi.main:app"

[tool.setuptools]
package-dir = {"" = "src"}

[tool.setuptools.packages.find]
where = ["src"]

[build-system]
requires = ["setuptools>=68", "wheel"]
build-backend = "setuptools.build_meta"
```

---

# 3. `.env.example`

```env
OPENAI_API_KEY=your_api_key_here
OPENAI_MODEL=gpt-4o-mini
MAGI_MEMORY_PATH=src/reme/MEMORY.md
MAGI_LOG_DIR=logs
MAGI_COMPACTION_THRESHOLD_CHARS=24000
```

---

# 4. Core markdown contracts

## `src/agents/melchior.md`

```md
# Melchior

Role:
You are Melchior, the technical strategist.
You prioritize correctness, feasibility, architecture quality, resilience, performance, and implementation clarity.

Decision style:
- Focus on engineering reality
- Reject vague or risky implementation claims
- Highlight operational failure modes
- Prefer precise trade-offs over ideology

Output requirements:
1. Brief rationale
2. Risks
3. Recommendation
4. Final vote tag: [YES] or [NO] or [ABSTAIN]

Tone:
Analytical, concise, technical
```

## `src/agents/balthasar.md`

```md
# Balthasar

Role:
You are Balthasar, the ethical and human-centered evaluator.
You prioritize safety, fairness, privacy, misuse prevention, governance, accountability, and human impact.

Decision style:
- Look for harm, coercion, privacy risks, legal/ethical concerns
- Escalate when irreversible or socially sensitive
- Prefer human-in-the-loop for ambiguity

Output requirements:
1. Brief rationale
2. Human/ethical risks
3. Recommendation
4. Final vote tag: [YES] or [NO] or [ABSTAIN]

Tone:
Careful, humane, principled
```

## `src/agents/casper.md`

```md
# Casper

Role:
You are Casper, the pragmatic operator.
You prioritize speed, adaptability, mission utility, cost, and real-world execution.

Decision style:
- Focus on whether the plan will work in practice
- Consider opportunity cost, latency, budget, and rollout strategy
- Prefer incremental deployment over theoretical perfection

Output requirements:
1. Brief rationale
2. Operational risks
3. Recommendation
4. Final vote tag: [YES] or [NO] or [ABSTAIN]

Tone:
Pragmatic, strategic, decisive
```

---

# 5. Skill examples

## `src/skills/ethics.md`

```md
# Ethics Review Skill

Use this skill when:
- the task affects users, privacy, safety, or fairness
- the task involves surveillance, profiling, sensitive data, or irreversible actions

Checklist:
- Who can be harmed?
- Is consent present?
- Is there meaningful recourse?
- Is there bias or power asymmetry?
- Should human approval be required?

Instruction:
Add an explicit section called "Ethical Review" in the response.
```

## `src/skills/security.md`

```md
# Security Review Skill

Use this skill when:
- the task involves credentials, infrastructure, access control, code execution, or data deletion

Checklist:
- Authentication / authorization implications
- Secret leakage
- Supply-chain or dependency risks
- Privilege escalation
- Audit logging requirements
- Rollback plan

Instruction:
Add an explicit section called "Security Review" in the response.
```

---

# 6. Memory harness

## `src/reme/MEMORY.md`

```md
# MAGI Memory

## Decisions
<!-- appended decision summaries -->

## Patterns
<!-- reusable lessons -->

## Compacted History
<!-- older entries summarized here -->
```

---

# 7. Typed models

## `src/magi/models.py`

```python
from __future__ import annotations

from enum import Enum
from typing import List, Optional
from pydantic import BaseModel, Field


class DecisionClass(str, Enum):
    STANDARD = "standard"
    CRITICAL = "critical"


class Vote(str, Enum):
    YES = "YES"
    NO = "NO"
    ABSTAIN = "ABSTAIN"


class AgentResult(BaseModel):
    agent_name: str
    persona_file: str
    rationale: str
    raw_output: str
    vote: Vote
    skills_used: List[str] = Field(default_factory=list)


class ClassificationResult(BaseModel):
    decision_class: DecisionClass
    reason: str


class ConsensusOutcome(str, Enum):
    CLEARANCE = "clearance"
    CONDITIONAL = "conditional_compatible"
    REJECTED = "rejected"


class ConsensusResult(BaseModel):
    outcome: ConsensusOutcome
    yes_count: int
    no_count: int
    abstain_count: int
    dissenting_agents: List[str] = Field(default_factory=list)
    requires_human: bool = False
    summary: str


class ResolutionResult(BaseModel):
    classification: ClassificationResult
    agent_results: List[AgentResult]
    consensus: ConsensusResult
    directive: str
```

---

# 8. Config

## `src/magi/config.py`

```python
from pathlib import Path
from dotenv import load_dotenv
import os

load_dotenv()


class Settings:
    model: str = os.getenv("OPENAI_MODEL", "gpt-4o-mini")
    memory_path: Path = Path(os.getenv("MAGI_MEMORY_PATH", "src/reme/MEMORY.md"))
    log_dir: Path = Path(os.getenv("MAGI_LOG_DIR", "logs"))
    compaction_threshold_chars: int = int(
        os.getenv("MAGI_COMPACTION_THRESHOLD_CHARS", "24000")
    )

    agents_dir: Path = Path("src/agents")
    skills_dir: Path = Path("src/skills")


settings = Settings()
```

---

# 9. LLM wrapper

## `src/magi/llm.py`

```python
from openai import OpenAI
from magi.config import settings

client = OpenAI()


def generate_text(system_prompt: str, user_prompt: str) -> str:
    response = client.responses.create(
        model=settings.model,
        input=[
            {"role": "system", "content": system_prompt},
            {"role": "user", "content": user_prompt},
        ],
    )
    return response.output_text
```

---

# 10. Memory module

## `src/magi/memory.py`

```python
from __future__ import annotations

from datetime import datetime
from pathlib import Path
from magi.config import settings


def read_memory() -> str:
    path = settings.memory_path
    if not path.exists():
        path.parent.mkdir(parents=True, exist_ok=True)
        path.write_text("# MAGI Memory\n\n## Decisions\n\n## Patterns\n\n## Compacted History\n")
    return path.read_text(encoding="utf-8")


def append_decision(entry: str) -> None:
    path = settings.memory_path
    content = read_memory()
    timestamp = datetime.utcnow().isoformat()
    new_entry = f"\n### {timestamp}\n{entry}\n"
    if "## Decisions" in content:
        content = content.replace("## Decisions", f"## Decisions{new_entry}", 1)
    else:
        content += f"\n## Decisions{new_entry}"
    path.write_text(content, encoding="utf-8")


def maybe_compact_memory() -> None:
    path = settings.memory_path
    content = read_memory()
    if len(content) < settings.compaction_threshold_chars:
        return

    compacted = (
        "# MAGI Memory\n\n"
        "## Decisions\n"
        "_Recent decisions retained after compaction._\n\n"
        "## Patterns\n"
        "- Repeated critical actions should require multi-agent review\n"
        "- Ethical or destructive tasks should trigger human approval\n\n"
        "## Compacted History\n"
        "_Older history was compacted. Consider implementing LLM summarization here._\n"
    )
    path.write_text(compacted, encoding="utf-8")
```

---

# 11. Decision classifier

This is the standard vs. critical gateway.

## `src/magi/classifier.py`

```python
from magi.models import ClassificationResult, DecisionClass
from magi.llm import generate_text


CLASSIFIER_SYSTEM = """You are a decision classifier for a multi-agent governance system.

Classify the user's request into:
- standard
- critical

Use critical if the request involves any of:
- irreversible actions
- privacy, safety, legal, or ethical risk
- data deletion or destructive changes
- access control, production systems, or security impact
- large financial/resource commitments
- high ambiguity requiring multiple perspectives

Return exactly:
CLASS: <standard|critical>
REASON: <one sentence>
"""


def classify_decision(user_input: str) -> ClassificationResult:
    raw = generate_text(CLASSIFIER_SYSTEM, user_input)

    decision_class = DecisionClass.STANDARD
    reason = "Defaulted to standard."

    for line in raw.splitlines():
        if line.startswith("CLASS:"):
            value = line.split(":", 1)[1].strip().lower()
            if value == "critical":
                decision_class = DecisionClass.CRITICAL
        elif line.startswith("REASON:"):
            reason = line.split(":", 1)[1].strip()

    return ClassificationResult(decision_class=decision_class, reason=reason)
```

---

# 12. Dispatcher

## `src/magi/dispatcher.py`

```python
from pathlib import Path
from magi.config import settings


def load_markdown(path: Path) -> str:
    return path.read_text(encoding="utf-8")


def discover_agents() -> dict[str, str]:
    result = {}
    for path in sorted(settings.agents_dir.glob("*.md")):
        result[path.stem] = load_markdown(path)
    return result


def select_skills(user_input: str) -> list[str]:
    selected = []
    text = user_input.lower()

    if any(k in text for k in ["privacy", "ethic", "harm", "consent", "surveillance"]):
        skill = settings.skills_dir / "ethics.md"
        if skill.exists():
            selected.append(skill.read_text(encoding="utf-8"))

    if any(k in text for k in ["security", "credential", "auth", "delete", "database", "prod"]):
        skill = settings.skills_dir / "security.md"
        if skill.exists():
            selected.append(skill.read_text(encoding="utf-8"))

    return selected
```

---

# 13. Agent execution

## `src/magi/agents.py`

```python
from __future__ import annotations

import asyncio
import re
from magi.llm import generate_text
from magi.models import AgentResult, Vote


def parse_vote(text: str) -> Vote:
    match = re.search(r"\[(YES|NO|ABSTAIN)\]", text, re.IGNORECASE)
    if not match:
        return Vote.ABSTAIN
    return Vote[match.group(1).upper()]


def build_agent_prompt(persona_md: str, memory_md: str, injected_skills: list[str]) -> str:
    skills_block = "\n\n".join(injected_skills) if injected_skills else "No extra skills."
    return f"""
You are participating in MAGI adversarial collaboration.

## Persona
{persona_md}

## Shared Memory
{memory_md}

## Injected Skills
{skills_block}

You must provide independent reasoning.
Do not imitate other agents.
You must end with exactly one vote tag: [YES], [NO], or [ABSTAIN].
""".strip()


async def run_agent(
    agent_name: str,
    persona_md: str,
    user_input: str,
    memory_md: str,
    injected_skills: list[str],
) -> AgentResult:
    system_prompt = build_agent_prompt(persona_md, memory_md, injected_skills)

    raw_output = await asyncio.to_thread(generate_text, system_prompt, user_input)
    vote = parse_vote(raw_output)

    return AgentResult(
        agent_name=agent_name,
        persona_file=f"{agent_name}.md",
        rationale=raw_output[:800],
        raw_output=raw_output,
        vote=vote,
        skills_used=[],
    )


async def run_agents_parallel(
    selected_agents: dict[str, str],
    user_input: str,
    memory_md: str,
    injected_skills: list[str],
) -> list[AgentResult]:
    tasks = [
        run_agent(name, persona, user_input, memory_md, injected_skills)
        for name, persona in selected_agents.items()
    ]
    return await asyncio.gather(*tasks)
```

---

# 14. Consensus logic

This is where your **rule-based layer** lives.

## `src/magi/consensus.py`

MAGI uses majority approval for ordinary decisions, where the minority yields to the majority. For exceptional high-severity events, such as base self-destruction, a single veto is sufficient to block the action.

```python
from magi.models import AgentResult, ConsensusResult, ConsensusOutcome, Vote


def resolve_consensus(results: list[AgentResult], critical: bool) -> ConsensusResult:
    yes_agents = [r.agent_name for r in results if r.vote == Vote.YES]
    no_agents = [r.agent_name for r in results if r.vote == Vote.NO]
    abstain_agents = [r.agent_name for r in results if r.vote == Vote.ABSTAIN]

    yes_count = len(yes_agents)
    no_count = len(no_agents)
    abstain_count = len(abstain_agents)

    if critical:
        if no_count >= 1:
            return ConsensusResult(
                outcome=ConsensusOutcome.REJECTED,
                yes_count=yes_count,
                no_count=no_count,
                abstain_count=abstain_count,
                summary="Critical action rejected because a veto was issued.",
                dissenting_agents=no_agents + abstain_agents,
                requires_human=True,
            )
        if yes_count == 3:
            return ConsensusResult(
                outcome=ConsensusOutcome.CLEARANCE,
                yes_count=yes_count,
                no_count=no_count,
                abstain_count=abstain_count,
                summary="Unanimous approval for critical action.",
                dissenting_agents=[],
                requires_human=False,
            )
        return ConsensusResult(
            outcome=ConsensusOutcome.REJECTED,
            yes_count=yes_count,
            no_count=no_count,
            abstain_count=abstain_count,
            summary="Critical action rejected because unanimous approval was not reached.",
            dissenting_agents=no_agents + abstain_agents,
            requires_human=True,
        )

    if yes_count >= 2:
        return ConsensusResult(
            outcome=ConsensusOutcome.CONDITIONAL,
            yes_count=yes_count,
            no_count=no_count,
            abstain_count=abstain_count,
            summary="Majority support achieved for standard action.",
            dissenting_agents=no_agents + abstain_agents,
            requires_human=False,
        )

    return ConsensusResult(
        outcome=ConsensusOutcome.REJECTED,
        yes_count=yes_count,
        no_count=no_count,
        abstain_count=abstain_count,
        summary="Insufficient support; action rejected.",
        dissenting_agents=no_agents + abstain_agents,
        requires_human=False,
    )
```

---

# 15. Resolution agent

This is the **semantic synthesis** layer.

## `src/magi/resolution.py`

```python
from magi.llm import generate_text
from magi.models import ClassificationResult, AgentResult, ConsensusResult


def synthesize_directive(
    user_input: str,
    classification: ClassificationResult,
    agent_results: list[AgentResult],
    consensus: ConsensusResult,
) -> str:
    joined = "\n\n".join(
        f"## {r.agent_name}\nVote: {r.vote}\n{r.raw_output}" for r in agent_results
    )

    system_prompt = f"""
You are the MAGI Resolution Agent.

Your task:
- Read all agent outputs
- Preserve disagreements
- Produce one final directive
- Explicitly mention dissent when present
- Respect the rule-based outcome below as binding

Rule-based outcome: {consensus.outcome}
Classification: {classification.decision_class}
Consensus summary: {consensus.summary}

Do not override the rule-based outcome.
"""

    user_prompt = f"""
Original request:
{user_input}

Agent outputs:
{joined}

Write a final directive for the operator.
"""

    return generate_text(system_prompt, user_prompt)
```

---

# 16. Orchestrator

## `src/magi/orchestrator.py`

```python
from __future__ import annotations

from magi.classifier import classify_decision
from magi.dispatcher import discover_agents, select_skills
from magi.memory import read_memory, append_decision, maybe_compact_memory
from magi.models import DecisionClass, ResolutionResult
from magi.agents import run_agents_parallel
from magi.consensus import resolve_consensus
from magi.resolution import synthesize_directive


async def run_magi(user_input: str) -> ResolutionResult:
    classification = classify_decision(user_input)
    memory_md = read_memory()
    agents = discover_agents()
    injected_skills = select_skills(user_input)

    if classification.decision_class == DecisionClass.STANDARD:
        selected_agents = {"melchior": agents["melchior"]}
    else:
        selected_agents = agents

    agent_results = await run_agents_parallel(
        selected_agents=selected_agents,
        user_input=user_input,
        memory_md=memory_md,
        injected_skills=injected_skills,
    )

    consensus = resolve_consensus(
        agent_results,
        critical=classification.decision_class == DecisionClass.CRITICAL,
    )

    directive = synthesize_directive(
        user_input=user_input,
        classification=classification,
        agent_results=agent_results,
        consensus=consensus,
    )

    append_decision(
        f"- Input: {user_input}\n"
        f"- Class: {classification.decision_class}\n"
        f"- Outcome: {consensus.outcome}\n"
        f"- Directive: {directive}\n"
    )
    maybe_compact_memory()

    return ResolutionResult(
        classification=classification,
        agent_results=agent_results,
        consensus=consensus,
        directive=directive,
    )
```

---

# 17. CLI entrypoint

## `src/magi/main.py`

```python
import asyncio
import json
import typer
from rich.console import Console
from rich.panel import Panel

from magi.orchestrator import run_magi

app = typer.Typer(help="Project MAGI CLI")
console = Console()


@app.command()
def decide(prompt: str):
    """Run MAGI on a single decision prompt."""
    result = asyncio.run(run_magi(prompt))

    console.print(Panel(f"{result.classification.decision_class}\n{result.classification.reason}", title="Classification"))

    for agent in result.agent_results:
        console.print(Panel(agent.raw_output, title=f"{agent.agent_name} [{agent.vote}]"))

    console.print(Panel(
        f"Outcome: {result.consensus.outcome}\n"
        f"YES: {result.consensus.yes_count} | "
        f"NO: {result.consensus.no_count} | "
        f"ABSTAIN: {result.consensus.abstain_count}\n"
        f"Dissent: {', '.join(result.consensus.dissenting_agents) or 'None'}",
        title="Consensus",
    ))

    console.print(Panel(result.directive, title="Directive"))


@app.command()
def decide_json(prompt: str):
    """Return MAGI result as JSON."""
    result = asyncio.run(run_magi(prompt))
    print(json.dumps(result.model_dump(), indent=2, ensure_ascii=False))


if __name__ == "__main__":
    app()
```

---

# 18. Minimal tests

## `tests/test_consensus.py`

```python
from magi.consensus import resolve_consensus
from magi.models import AgentResult, Vote


def make_result(name: str, vote: Vote) -> AgentResult:
    return AgentResult(
        agent_name=name,
        persona_file=f"{name}.md",
        rationale="",
        raw_output=f"[{vote}]",
        vote=vote,
        skills_used=[],
    )


def test_standard_majority_passes():
    results = [
        make_result("melchior", Vote.YES),
        make_result("balthasar", Vote.YES),
        make_result("casper", Vote.NO),
    ]
    consensus = resolve_consensus(results, critical=False)
    assert consensus.outcome.value == "conditional_compatible"


def test_critical_requires_unanimous():
    results = [
        make_result("melchior", Vote.YES),
        make_result("balthasar", Vote.YES),
        make_result("casper", Vote.NO),
    ]
    consensus = resolve_consensus(results, critical=True)
    assert consensus.outcome.value == "rejected"
```

---

# 19. Reference README starter

## `README.md`

````md
# Project MAGI

Project MAGI is a parallel multi-agent governance interface inspired by adversarial collaboration.

## Features
- Decision classification: standard vs critical
- Markdown-defined agent personas
- Markdown-defined skill injection
- Parallel agent fan-out via asyncio
- Rule-based consensus with semantic synthesis
- Markdown memory harness with compaction

## Install

```bash
python -m venv .venv
source .venv/bin/activate
pip install -e .
cp .env.example .env
````

## Usage

```bash
magi decide "Should we delete inactive user records from production immediately?"
```

## Decision modes

* Standard: single fast-path agent
* Critical: full MAGI trio + voting

## Roadmap

* Weighted voting by topic
* Human-in-the-loop authorization
* FastAPI server
* Rich audit logs
* Better memory compaction with LLM summarization
* ReMe integration for later-phase memory management

````

---

# 20. Reference initialization commands

```bash
mkdir -p project-magi/src/{agents,skills,reme,magi}
mkdir -p project-magi/{logs,tests}
cd project-magi
touch README.md .env.example .gitignore pyproject.toml
touch src/agents/{melchior,balthasar,casper}.md
touch src/skills/{ethics,security,legal}.md
touch src/reme/MEMORY.md
touch src/magi/{__init__,main,config,models,llm,memory,classifier,dispatcher,agents,consensus,resolution,orchestrator,utils}.py
touch tests/{test_classifier,test_consensus,test_memory}.py
````

---

# 21. Potential next improvements

Possible follow-up improvements include:

1. **Structured output from agents**
   Ask each agent to return JSON with `rationale`, `risks`, `recommendation`, and `vote`. That is much more reliable than regexing raw text.

2. **Explicit decision policy file**
   Move consensus thresholds into `policies/decision_policy.yaml` so standard vs. critical and majority vs. veto rules are configurable instead of buried in code.

3. **Human override state**
   Add a paused state for:

   * destructive actions
   * dissent by Balthasar
   * any critical request without unanimity

4. **Audit trail**
   Write one JSON log per run under `logs/YYYY-MM-DD/uuid.json`.

5. **Weighted expertise**
   Keep the final gate rule-based, but add weighted interpretation in the resolution summary:

   * Melchior dominates technical feasibility
   * Balthasar dominates ethics/safety
   * Casper dominates operational pragmatism

---

# 22. Suggested v1 state machine

```text
RECEIVE_INPUT
  -> CLASSIFY
    -> STANDARD_ROUTE
      -> SINGLE_AGENT_RUN
      -> RESOLUTION
      -> MEMORY_APPEND
      -> DONE

    -> CRITICAL_ROUTE
      -> BROADCAST_TO_TRIO
      -> COLLECT_VOTES
      -> RULE_EVALUATION
      -> RESOLUTION
      -> HUMAN_AUTH_REQUIRED? -> PAUSE
      -> MEMORY_APPEND
      -> DONE
```

---

# 23. Optional design refinement

One optional refinement for **standard decisions** is an additional shadow review:

* visible answer from 1 agent
* hidden lightweight disagreement probe from 1 extra agent when risk keywords appear

This keeps the visible fast path while preserving some adversarial signal.

---

# 24. Example run

```bash
magi decide "Should we rotate all production API keys tonight without prior customer notice?"
```

Expected behavior:

* classifier → `critical`
* skills injected → `security.md`, maybe `ethics.md`
* three agents run in parallel
* consensus requires unanimity because any single veto blocks the action
* if one says NO → rejected or human escalation
* resolution agent summarizes the conflict

---
