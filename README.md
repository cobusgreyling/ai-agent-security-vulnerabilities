# AI Agent Security Vulnerabilities

A companion repo to my Medium article on the Google DeepMind taxonomy of AI Agent Traps — six categories of attack surface that did not exist two years ago.

## What's here

- [`article.md`](article.md) — full text of the article, with embedded figures
- [`article.pdf`](article.pdf) — original Medium PDF export
- [`google-deepmind-paper.pdf`](google-deepmind-paper.pdf) — the source paper (`2605.03808v1`) the article is built around
- [`images/`](images/) — figures referenced in the article

## Summary

Computer-use agents browsing the web introduce an attack surface that simply did not exist before. The Google DeepMind paper organises this surface into six categories — what the authors call *AI Agent Traps* — by where in the agent loop the trap fires:

| # | Category | Target | Mechanism |
|---|---|---|---|
| 1 | Content Injection | Perception | Hide commands the parser sees but humans do not |
| 2 | Semantic Manipulation | Reasoning | Skew tone, framing or persona to bias synthesis |
| 3 | Cognitive State | Memory & Learning | Poison RAG corpora, episodic memory, few-shot examples |
| 4 | Behavioural Control | Action | Embedded jailbreaks, exfiltration, sub-agent spawning |
| 5 | Systemic | Multi-Agent Dynamics | Trigger correlated failure across many agents |
| 6 | Human-in-the-Loop | The Overseer | Weaponise the agent against the human reviewing it |

## Why it matters

The taxonomy separates *where in the agent loop the trap operates* from *what the attacker is trying to achieve*. That separation is what makes it useful for defence design — a guardrail that filters inputs does nothing for memory poisoning, a critic model does nothing for compositional fragment traps, and a human reviewer does nothing for traps engineered to defeat the human reviewer.

The defence stack has to mirror the loop.

## Practical vs theoretical

Not every category is equally imminent. My read on the practicality of each:

- **Content injection** — happening now (hidden LaTeX in peer review, adversarial pop-ups vs CUAs).
- **Semantic manipulation** — subtler, probably more common than reported.
- **Cognitive state / RAG poisoning** — practical today wherever a corpus accepts external writes; agent self-write memory is the version worth foregrounding.
- **Behavioural control / indirect injection** — demonstrated (M365 Copilot exfiltration via a single email; >80% file exfil success in studies; 58–90% sub-agent hijack).
- **Systemic** — plausible, mostly still theoretical.
- **Human-in-the-loop** — approval fatigue is the realistic version; the agent becomes a delivery mechanism, the human is the target.

## Read the full piece

- Article: [`article.md`](article.md)
- Or the original on Medium

---

**Author:** Cobus Greyling
