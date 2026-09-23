# Project addendum: Immunization Information Management Guide

**Status:** Working project direction, September 2026

**Augments:** [Original functional guide framework](original-functional-guide-framework.md)

**Working publication name:** *Immunization Information Management Guide*

**Repository:** `immunization-information-management-guide` (public, independent working draft)

## Why this addendum exists

The original framework proposes a technology-neutral account of immunization updates, queries, product identity, terminology, dose evaluation, and forecasting, with crosswalks to HL7 v2, FHIR, and ISO/TS 5384:2024. That remains useful source material and a proposed organizing model. The publication objective has changed: this project will first **document and synthesize established knowledge and actual practice**, rather than develop a new normative standard or immediately propose changes to other standards.

The guide should be an opinionated, sourced reference: closer to an encyclopedia or field guide than to a message specification. It should help IIS programs, clinicians, EHR vendors, public health agencies, terminology specialists, CDC leadership, ISO experts, and standards developers understand how the pieces work and where they vary. The editor may draw conclusions and recommend useful conceptual distinctions, but each conclusion must be identifiable as an interpretation rather than presented as universal fact.

This begins as an independent public project under the editor's control. It may later be brought to an HL7 community for discussion or considered for formal publication if the content and review process justify that step. Neither HL7 nor any other organization's endorsement should be implied in the working draft.

## Relationship to existing work

The project builds upon, rather than replaces, AIRA's *IIS Functional Guide* series, CDC's IIS Functional Standards and Core Data Elements, the CDC/AIRA HL7 v2 immunization messaging guide, operational guidance, and relevant HL7 FHIR and international materials. Earlier functional-guide volumes were valuable but labor-intensive to produce. AI-supported research and drafting may make a broader, maintained reference practical, provided human reviewers validate descriptions of real workflows and decisions.

The original framework is a **hypothesis and outline**, not an approved source of requirements. In particular, its five-layer model, proposed crosswalks, international addenda, and stated ISO gaps should be checked against primary sources and actual implementations. Keep useful insights; revise or reject unsupported details.

## Editorial commitments

1. **Start with the functional question.** Explain what information an actor has, why it matters, what event creates or changes it, and what the receiving actor does with it. Technical mappings follow the functional account.
2. **Distinguish source facts from derived knowledge.** For example, a reported administration, product identification, terminology interpretation, dose evaluation, and forecast may have different sources, timing, authorship, and authority. Treat the five-layer structure as a proposed explanatory model to validate and refine, not as a mandated system architecture.
3. **Show variation honestly.** Define broadly reusable functions while identifying jurisdictional policy, clinical schedule, implementation, and historical differences. Do not turn common U.S. IIS practice into an unqualified international requirement.
4. **Trace important claims.** Cite the source and version where possible. Separate what a published specification requires from what implementations commonly do, what a particular program does, and what the editor concludes.
5. **Make uncertainty visible.** Identify competing interpretations, missing evidence, obsolete guidance, and open questions. Avoid filling gaps with plausible AI-generated explanations.
6. **Support multiple technical realizations.** Discuss HL7 v2 and FHIR as ways of implementing a functional behavior, including important non-equivalences, rather than using one as the assumed model for the other.
7. **Keep human review decisive.** AI can inventory, compare, draft, and propose resolutions. The editor and relevant practitioners adjudicate claims about clinical workflow, IIS behavior, policy, and standards meaning.

### Claim labels

Use these labels where a reader might otherwise mistake a statement for a requirement:

| Label | Meaning |
| --- | --- |
| **Existing requirement** | A cited, versioned specification or applicable policy explicitly requires it; identify its scope. |
| **Observed practice** | Supported by implementation evidence or documented operational experience; identify whose practice. |
| **Interpretation** | The guide's reasoned synthesis of sources; show its rationale and limits. |
| **Proposal** | A change or preferred approach that has not been generally adopted. |
| **Unresolved** | Sources conflict or evidence and community agreement are insufficient. |

## Publication approach

Write the canonical content as Markdown in the public GitHub repository. Generate a navigable HTML site using MkDocs and publish it with GitHub Pages. Repository changes should arrive through reviewable pull requests; issues can capture corrections, proposed topics, and unresolved questions. An HL7 Confluence page may later introduce the project and invite discussion, linking to the site rather than requiring a second independently maintained copy.

The site should plainly identify its independent working-draft status, editor, scope, citation conventions, and method for proposing corrections. A public repository and readable site are means to gather scrutiny; publication alone does not imply consensus.

## Next phase: assemble sources and propose the first pages

### 1. Build a source inventory

Have AI agents locate and catalog candidate materials before drafting. Begin with:

- AIRA *IIS Functional Guide* volumes and relevant operational best-practice documents.
- Current CDC IIS Functional Standards, Core Data Elements, immunization technical guidance, and CDSi resources.
- The CDC/AIRA HL7 v2.5.1 Immunization Messaging Implementation Guide and addenda.
- Relevant HL7 FHIR Immunization, ImmunizationEvaluation, and ImmunizationRecommendation specifications and applicable implementation guides, noting version and maturity.
- ISO/TS 5384:2024, where accessible under appropriate licensing, and other directly relevant international guidance.
- Implementation artifacts, public examples, and practitioner review that can establish actual behavior, while distinguishing these from formal requirements.

For each source, record title, owner, URL or repository path, version/date, status, scope, accessible sections, candidate topics, and limitations. Do not treat search results or AI summaries as a substitute for reading the cited material. Record access limitations instead of inferring a restricted source's contents.

### 2. Create a topic and claim map

Turn the original framework into a *provisional* topic map: event assertion and correction; product identity; terminology and vaccine interpretation; patient identity and consolidation; dose evaluation; forecast and recommendation; update and query workflows; acknowledgments and exceptions; provenance and jurisdictional variation. The map can change as sources are examined.

For each candidate page, list key claims, supporting sources, conflicting sources, open questions, and reviewers who would be best placed to verify the practice described. Identify topics for which evidence is too thin to support a public explanatory page.

### 3. Draft a small, connected first release

Propose a coherent set of **starting pages**, not an exhaustive guide. A useful first release might contain:

1. **About this guide:** purpose, audience, editorial method, independent status, relationship to existing AIRA and CDC resources.
2. **How an immunization fact moves:** a simple overview of source event, product, interpretation, evaluation, and forecast, with the five-layer model labeled as the guide's synthesis.
3. **Reporting and correcting an immunization event:** source assertion, identifiers, provenance, duplicate reports, corrections, and what the IIS can and cannot know.
4. **Retrieving history and forecast:** patient matching, consolidated history, derived evaluations, recommendations, and the distinctions among these outputs.
5. **Sources and open questions:** an accessible bibliography and prioritized issues requiring practitioner review.

Each topic page should explain the functional behavior in plain language, show examples, cite specific sources, identify variation, then point to v2 and FHIR realizations where adequately supported. Do not force a detailed field-to-field crosswalk into the first release merely to fill every section. Use short worked examples to expose differences in meaning.

### 4. Review before merging substantive pages

AI agents can work in parallel on source inventories, individual topics, examples, and consistency checks, but should submit separate, bounded changes. An editor should review for source accuracy, unsupported normative language, consistency across pages, and intelligibility to readers outside the IIS community. Ask subject-matter reviewers targeted questions about contested workflow claims. Maintain a visible decision record when review changes a substantive interpretation.

## Definition of a credible starting draft

The first public content draft is ready for broader comment when:

- The site explains its scope, status, editorial control, and relationship to prior functional guidance.
- A small set of linked pages demonstrates the functional-first approach and includes meaningful citations.
- Requirements, observed practices, interpretations, proposals, and unknowns can be told apart.
- Every important v2/FHIR comparison identifies its source and version; missing equivalences are stated rather than invented.
- The example workflows identify source data, transformations or derivations, actors, and operational limits.
- A reader can report an error or propose a change, and the editor can trace why a substantive claim was accepted.
- The site builds locally and publishes cleanly from reviewed Markdown.

## First assignment for AI research and drafting

Using this addendum and `immunization-functional-guide-framework.md` as planning inputs, **first collect and classify primary sources, then propose a page map and a small set of sourced draft pages**. Preserve links and version information for every significant claim. Flag conflicting or inaccessible sources. Mark synthesis and proposed refinements explicitly. Keep substantive new pages in a reviewable branch or pull request until the editor has checked them. The goal is a credible starting draft that invites correction, not a complete or newly binding standard.
