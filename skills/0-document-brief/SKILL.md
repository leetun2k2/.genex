---
name: 0-document-brief
description: Condense existing documents into concise, source-faithful briefs for quick understanding. Use when asked to brief a document, extract its key points, or make one or more documents faster to read and understand. Do not apply to authoring or revising source documents, conducting new analysis or reviews, or implementing their recommendations.
---

# Document Brief

## Purpose and output

Produce a standalone Markdown brief that helps the intended reader quickly understand what a document says, why it matters, and what needs attention. Preserve meaning while reducing reading effort. Decisions and actions matter when present; do not force them onto informational material.

Optimize for a **15–30 second first read**. Default to **50–120 words**, ideally one screen. These are flexible heuristics, never quotas: do not pad a small brief or suppress essential meaning to meet a target. Success means less reading effort without losing information that could change understanding, judgment, or action.

## Inputs and independence

Read the existing document or documents supplied by the user, such as reports, proposals, policies, guides, meeting notes, research, or technical specifications. Input may be a file, an accessible document, or supplied text; no specific filename or producing skill is required.

Work from these sources independently. Do not redo the underlying research, design, or review, or require a newly authored source. Read enough to find the main message and important qualifications, including conclusions outside the opening sections. Infer the audience from the request and sources; do not assume a developer audience. If essential source content is unavailable, identify the missing input rather than inventing a brief of unseen material. Reconstruct context from the request and sources, not hidden session state.

## Meaning and priority

Select and order information by what the reader needs to understand, **not by source document order**. Include only applicable items:

| Priority | Question to answer |
|---|---|
| **Main message** | What is this about, and what does it say? |
| **Key points** | Which findings, rules, ideas, or choices matter most? |
| **Meaning** | Why does this matter to the intended reader? |
| **Attention** | What conditions, exceptions, uncertainties, or risks change the meaning? |
| **Action** | What decisions, next steps, owners, deadlines, or blockers are stated? |

Lead with the main message, unless an urgent action or critical risk dominates. Keep enough context for someone who has not read the source. Preserve evidence, definitions, scope, dates, and exceptions when they change interpretation. For findings and reports, distinguish observed outcomes, suspected causes, and proposed actions.

Remove by default:

- Background explanations, repetition, and common knowledge the audience already has.
- Low-value detail and lengthy walkthroughs; retain steps or sequence when essential to understanding a procedure.
- Rejected alternatives with no ongoing relevance.
- Long justifications for settled decisions.
- Information that does not change understanding, judgment, or action.

Prefer the central message over a section-by-section recap, outcomes over process narration, and material caveats over exhaustive detail. Use direct language and real domain terms from the source; explain unfamiliar terms briefly when needed by the audience. Retain a short rationale, example, or tradeoff when omitting it would change understanding.

## Fidelity and authority

- Derive every factual claim, requirement, decision, guarantee, and assumption from the source. Do not add conclusions or recommendations that exist only in the brief.
- Preserve conditions, uncertainty, approval status, and implementation readiness. Never turn an assumption into a fact, a proposal into an approved decision, or a suspected cause into a confirmed finding.
- Never silently remove essential qualifications, unresolved decisions, blockers, high-impact risks, or material tradeoffs. If a source is incomplete or contradictory, flag the gap and link to it; do not resolve it with new analysis or recommendations.
- The brief is a **read-only derived view**. Never modify, overwrite, rename, reformat, or summarize the source in place. Write only the brief artifact; do not change source documents even when review feedback has been accepted.
- The source remains authoritative. If it is a raw specification, implementation follows raw, not the brief. If source and brief disagree, reconcile the brief from the source.
- Accepted feedback must be recorded in the authoritative source before it can appear as an adopted decision in a regenerated brief. Unaccepted feedback remains distinct from settled decisions. Updating the source belongs to the calling task, not this skill; this skill does not initiate implementation.
- Source changes make an existing brief potentially stale. Regenerate from current sources when asked to refresh; if freshness is uncertain, do not present the brief as current without checking.

## Presentation and traceability

Make the brief independently readable: use compact prose or a few focused bullets, with only applicable labels. Avoid fixed templates, empty sections, or a separate summary of the brief. Use a table for useful comparisons or a simple Mermaid diagram for relationships or flows when it communicates faster than prose; do not duplicate its contents in narration.

List source links compactly and link key points, qualifications, and actions to the relevant source sections. Prefer relative Markdown links for local files and direct section links for accessible documents. For supplied text without a stable link, identify its title or message and section explicitly; do not invent a URL or require saving a new source document.

If one screen cannot safely cover the essential meaning, put the first-read overview first and add concise, linked details below. Keep essential qualifications, unresolved decisions, blockers, high-impact risks, and tradeoffs visible rather than hiding them behind a generic “details” link. A longer brief is better than a misleading one.

## File placement

Follow the requested path or repository conventions. Otherwise place `<source-name>.brief.md` beside a local source, or use `docs/<topic>.brief.md` for supplied text or remote sources. For `<name>.raw.md`, use `<name>.brief.md`. Multiple related sources may feed one brief that links to all of them. Never rename sources to fit this convention, and never use a source path as the output path.

Finish with a link to the brief. No templates, cross-skill imports, or particular coding harness are required.

## Before finishing

- **First read:** Can the intended reader quickly grasp the main message, key points, and what needs attention without opening the source? Remove repetition and low-value detail rather than important conditions.
- **Fidelity:** Is each claim traceable to the source? Are scope, exceptions, status, assumptions, uncertainty, and dates preserved where material?
- **Coverage:** Did any essential qualification, unresolved decision, blocker, high-impact risk, or material tradeoff disappear during compression? Keep it visible and linked, even if that exceeds the first-screen target.
- **Traceability:** Do source links and section references resolve? For multiple sources, are conflicts or differences in authority explicit rather than silently reconciled?
- **Source preservation:** Verify source files are unchanged, for example by comparing bytes or content hashes before and after brief generation. Do not claim synchronization or source preservation without checking it.
