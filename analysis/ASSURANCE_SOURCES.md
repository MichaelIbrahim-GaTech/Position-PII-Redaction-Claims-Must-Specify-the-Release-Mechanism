# Assurance maintenance: sources and scope

General principles for maintaining assurance after system changes predate this manuscript. The sources below establish that lineage. The manuscript applies these principles to redaction losses, risk denominators, and recipient-visible channels; it does not introduce assurance maintenance or change-impact analysis.

## Transfer Assurance for Machine Learning in Autonomous Systems

**Authors:** Chiara Picardi, Richard Hawkins, Colin Paterson, Ibrahim Habli  
**Publication:** SafeAI 2023, CEUR Workshop Proceedings, volume 3381  
**Bibliography key:** `picardi2023`

- Primary full text: https://ceur-ws.org/Vol-3381/26.pdf
- Proceedings metadata: https://ceur-ws.org/Vol-3381/
- Institutional full-text copy: https://eprints.whiterose.ac.uk/id/document/2773866

**Relevant passages:** Sections 3.1–3.3. The process identifies changes affecting an assurance case, assesses their impact, and determines which assurance activities must be repeated. Section 3.2 explicitly considers operational restrictions without a model update and changing the assurance scope accordingly. Section 3.3 retains unaffected assurance while reconsidering affected evidence and assumptions.

**Relationship to this manuscript:** The general requirement to justify reuse after an update is prior work. The present analysis specifies particular consequences for redaction: restriction can preserve joint failed-release probability while changing conditional risk, and changes in protection policy or observable channels can alter the loss itself.

## Automating Safety Argument Change Impact Analysis for Machine Learning Components

**Authors:** Carmen Cârlan, Lydia Gauerhof, Barbara Gallina, Simon Burton  
**Publication:** 2022 IEEE 27th Pacific Rim International Symposium on Dependable Computing  
**DOI:** https://doi.org/10.1109/PRDC55274.2022.00019  
**Bibliography key:** `carlan2022`

- Primary full text: https://www.es.mdu.se/pdf_publications/6543.pdf
- Institutional metadata and BibTeX: https://www.es.mdu.se/publications/6543-Automating_Safety_Argument_Change_Impact_Analysis_for_Machine_Learning_Components

**Relevant passages:** Sections V–VI. The approach links changes in assurance artifacts to affected argument elements, whether those elements require rechecking, and recommended responses. Its application examines operating-domain changes and data-sufficiency arguments for an ML component.

**Relationship to this manuscript:** A table relating changes to validity and reassessment is an application of established change-impact analysis. The redaction-specific contribution is the explicit relation needed for each proposed reuse, including containment, acceptance decisions, and population assumptions.

## Provably Safe Model Updates

**Authors:** Leo Elmecker-Plakolm, Pierre Fasterling, Philip Sosnin, Calvin Tsay, Matthew Wicker  
**Version examined:** arXiv:2512.01899v2, 18 March 2026  
**Publication status:** The primary record identifies acceptance at IEEE SaTML 2026. The bibliography cites the verified arXiv version with that status.  
**Bibliography key:** `elmecker2026`

- Primary full text: https://arxiv.org/html/2512.01899v2
- Primary metadata: https://arxiv.org/abs/2512.01899v2

**Relevant passages:** Sections III–IV. Locally invariant parameter domains constrain model updates to preserve a stated performance specification. This provides a concrete preservation method, beyond a general recommendation to reassess updates.

**Relationship to this manuscript:** The redaction paper does not introduce certified preservation across parameter updates. Its scope includes changes to policies, gates, evaluation eligibility, and output representations, which require identifying the intended claim before applying an appropriate preservation or audit method.

## Verification boundary

Bibliographic titles, authors, years, venues or status, and the included DOI were checked against the primary or institutional records above. Unverified page ranges and proceedings identifiers were omitted. The comparisons concern these sources' stated methods and scope; no claim is made that they studied the manuscript's redaction scenarios.
