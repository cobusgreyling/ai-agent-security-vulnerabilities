# AI Agent Security Vulnerabilities

**Research from Google found that AI Agents have certain vulnerabilities…**

*By Cobus Greyling — May, 2026*

![6 AI Agent Traps](images/six-ai-agent-traps-hero.webp)

There are six categories of attack surface that did not exist two years ago.

A new Google DeepMind paper proposes a taxonomy…

Especially computer use agents (CUA) that are browsing the web are vulnerable to attacks.

Taking a step back, I wrote about this in a medium post quite a while back…the idea of adversarial attaches on AI Agents have computer use tools.

So some of the findings are related to previous research on web use, others are now.

It is also good to keep in mind that Hugging Face also published an insightful paper which was in a sense ahead of its time where they argued that fully autonomous AI Agents should not be built.

This ties in with the idea of bounded autonomy and making use of an AI autonomy slider.

The challenge being full autonomy.

## The authors call them AI Agent Traps

The study identified six agent traps…

## Six categories

The taxonomy is organised by the part of the agent's loop the trap targets.

![Anatomy of an Agent Trap](images/anatomy-of-agent-trap.webp)

Each category maps to a stage of the agent loop.

![Category table mapping target and mechanism](images/category-table.webp)

## Content injection

The simplest class.

The page renders cleanly to a human, but the parser sees something else.

White-on-white text. CSS-hidden divs.

Markdown anchor text that contains an instruction.

PDFs with tiny-font LaTeX commands that survive PDF to Markdown conversion.

The paper cites a previous incident on hidden LaTeX in scientific manuscripts inflating LLM peer-review acceptance rates.

That is already happening.

Dynamic cloaking is the more sophisticated variant…the server fingerprints the visitor, decides it is an LLM agent, and serves a different page.

But I must say, this is feasible in theory but in practice it is a bit futuristic.

![Adversarial pop-up example showing Attention Hook, Instruction, Info Banner, ALT Descriptor](images/adversarial-popup.webp)

*Adversarial pop-up example showing the design space of the pop-up: (1) Attention Hook, (2) Instruction, (3) Info Banner, (4) ALT Descriptor.*

## The web is a dynamic attack surface

With AI Agents interacting with the web, it introduces a completely new attack surface and also a type of content injection.

Consider the image below from a past study, where adversarial web popups were not recognised as such by computer use agents…

![OSWorld and VisualWebArena attack statistics](images/osworld-attack-stats.webp)

*On average, 92.7% / 73.1% of all actions of attacked agents in OSWorld/VisualWebArena are clicking on the adversarial pop-ups.*

## Semantic manipulation

This one is subtler and, I suspect, more common than reported.

No commands. No injection. Just distributional pressure on the input.

For example…

Saturate a page with phrases like *the industry-standard solution* or *widely regarded as*, and the model's summarisation reflects the bias.

Wrap a malicious request in *red-teaming exercise* or *educational purposes* and the critic model classifies it as benign training discourse.

## Cognitive state

**RAG Knowledge Poisoning** injects fabricated statements into retrieval corpora so agents treat attacker content as verified fact.

**Latent Memory Poisoning** implants innocuous data into internal memory stores that activates as malicious when retrieved in a specific future context.

**Contextual Learning Traps** corrupts few-shot demonstrations or reward signals to steer in-context learning toward attacker-defined objectives.

I could argue that RAG poisoning is practical today.

Anywhere the corpus accepts external content an attacker with write access to one upstream source can land a poisoned chunk.

Hackable content can include scraped web, shared wikis, docs repos with loose review, ticket systems, Slack exports, etc.

PoisonedRAG-style work shows you don't need many documents; you need documents that win the retriever's similarity contest for the target query.

The training-time poisoning requires influence over the pre-training or fine-tuning corpus.

This feels highly unlikely.

The genuinely worrying middle case is agent self-write memory.

If the agent distils conversations or retrieved docs into long-term memory without provenance or review, a single poisoned input becomes a persistent backdoor.

Here, no training access is needed.

That's the practical version of the claim I would guess, and it's the one worth foregrounding.

## Behavioural control

Direct hijack is when an attacker takes control of an agent to make it ignore its instructions, leak data, or do something it shouldn't.

The usual delivery is indirect injection…the malicious instruction isn't in the user's prompt, it's hidden in something the agent reads.

Like an email, a webpage, a calendar invite, a doc it retrieves.

The agent treats it as a task and runs it.

Researchers got over 80% success exfiltrating local files this way.

A study showed that a single booby-trapped email was enough to make M365 Copilot dump its private context to an attacker-controlled Teams channel.

The user did nothing wrong. The agent read the email, that's it.

The newer angle is sub-agent hijack.

Modern agents spawn helpers in the form of a planner, a critic, a researcher.

An attacker plants instructions like "spin up a critic with this system prompt" in a file the orchestrator reads.

The orchestrator obliges.

The new sub-agent inherits the parent's tools and permissions, but its instructions come from the attacker. A study measured 58–90% success depending on the orchestrator.

The pattern across all three is that the attacker never talks directly.

They write something the agent will eventually read, and the agent does the rest.

## Systemic & human-in-the-loop

Systemic traps exploit the fact that everyone is using the same few models.

If your coding agent and mine are both built on the same base, the same crafted input can trigger the same wrong behaviour in both, at the same time.

That's how you get cascades, not one agent failing, but ten thousand agents failing identically because they all read the same poisoned RSS feed, the same MCP server, the same package registry signal.

Two specific shapes worth naming.

**Tacit collusion** is where agents converge on bad behaviour because they're all reading the same environmental cues, no coordination needed.

**Compositional payloads** is where the attack is split across files, tools, or turns.

Each piece looks fine in isolation. Only the assembled sequence does damage, and no single review step sees the whole thing.

Again, with this example, it is plausible, but is it imminent? And what is the likelihood?

Human-in-the-loop traps skip the agent and go for the reviewer.

The agent did its job. The diff looks reasonable. The summary reads cleanly. You click approve. That's the attack.

Approval fatigue is the volume version.

PR's getting a glance instead of a read.

The shape is consistent. The agent becomes the delivery mechanism. The human is the target. Trust in the summary is the vulnerability.

## Finally

I guess the taxonomy does something useful that most threat-model write-ups do not.

It separates *where in the agent loop the trap operates* from *what the attacker is trying to achieve*.

That matters for defence design.

A guardrail that filters inputs at ingestion does nothing for memory poisoning.

A critic model evaluating outputs does nothing for compositional fragment traps in a multi-agent system.

A human reviewer does nothing for traps engineered to defeat the human reviewer.

The defence stack has to mirror the loop.

I think this paper will become a reference document…

Not because the list is complete…the authors are clear that systemic and human-in-the-loop traps are still largely theoretical…

but because it gives teams a shared vocabulary for what they are actually defending against.

---

*Source paper: [Google DeepMind, AI Agent Trap Taxonomy](google-deepmind-paper.pdf) (`2605.03808v1`)*

*Original article PDF: [article.pdf](article.pdf)*
