# Protocol sources and scope of analysis

This record identifies the primary sources used in the manuscript's published-protocol case and separates their reported methods from the manuscript's deductions and hypothetical examples. Source verification used the full versioned text linked below. No benchmark predictions were downloaded or regenerated, and no model experiment was performed.

## LeakageBench

**Citation key:** `leakagebench2026`  
**Title:** *LeakageBench: Document-Level Leakage Risk for Redacting Personally Identifiable Information in Document Images*  
**Authors:** Vishnu Prasad Vijaya Kumar, Santhosh Venkatesh, and Ivan P. Yamshchikov  
**Version examined:** arXiv:2609.02207v1, 2 September 2026; preprint  
**Primary full text:** https://arxiv.org/html/2609.02207v1  
**Version record:** https://arxiv.org/abs/2609.02207v1

| Source location | Published fact used |
|---|---|
| Section 3.1 | Inference and evaluation operate separately on individual pages. |
| Sections 3.3–3.4, Equation (1) | DocLeak's denominator contains pages with at least one in-scope gold identifier. Its numerator counts pages with an unmatched identifier. Headline scoring uses greedy one-to-one, type-aware matching at IoU ≥ 0.75. |
| Section 4.1; Table 2 | The challenge set contains 500 pages from 291 documents, grouped into OCR-IDL, VRDU Ad-Buy, and FCC free-form sources. |
| Section 5.1; Section 6; Table 4 | Full-schema evaluation retains unsupported gold types as misses. Supported-schema evaluation filters gold types. Unusable outputs become empty prediction sets. |
| Section 4.1; Limitations | The authors delimit the challenge-set scope and acknowledge that IoU does not directly measure pixel coverage. |

## Manuscript deductions and hypothetical applications

The following are the manuscript's analysis, not results reported by LeakageBench:

- Distinguishing content applicability, the evaluation failure event, and an operational release decision. Conditional-probability identities explain why changing one of these predicates changes the interpretation of a score.
- Examining a hypothetical operator that withholds unparseable outputs. The proposed gate is not attributed to a benchmark deployment. On applicable pages, invalid outputs necessarily fail matching, so withholding them cannot increase the matching-failure fraction on that subset. The exact recount is (d - (1 - v_plus)) / v_plus when v_plus is positive. This preservation does not automatically cover complete concealment or all-page risk. Recounting fixed predictions would measure the modified protocol on the same benchmark; it would not establish deployment representativeness.
- Considering changes to requested entity labels, the release unit, or a searchable PDF's observable channels. These are illustrative update scenarios, not measured modifications of the published systems.
- Constructing rectangles to distinguish IoU matching from complete concealment. If a containing mask doubles a required rectangle's area, IoU is 1/2 while containment remains complete. An interior mask covering 80% has IoU 0.8 while leaving 20% of the stipulated required region uncovered. The region is defined to be fully protected for these constructions; this is not an assertion about the sensitive-pixel density of real annotation boxes.

The illustration notebook evaluates hypothetical populations and geometric constructions. A separate published-aggregate notebook, described below, analyzes reported Table 3 marginals without replicating model predictions or estimating deployment frequencies.

## RedacBench citation record

**Citation key:** `redacbench2026`  
**Title:** *RedacBench: Can AI Erase Your Secrets?*  
**Authors:** Hyunjun Jeon, Kyuyoung Kim, and Jinwoo Shin  
**Citation:** arXiv:2603.20208, 2026; preprint, primary category `cs.CL`  
**Primary metadata and abstract:** https://arxiv.org/abs/2603.20208  
**Versioned full text:** https://arxiv.org/html/2603.20208v1  
**DOI:** https://doi.org/10.48550/arXiv.2603.20208

The abstract establishes prior work on policy-conditioned redaction and separate security and utility evaluation. Its dataset comprises 514 human-authored texts, 187 policies, and 8,053 annotated propositions. The manuscript cites it to acknowledge this prior contribution; it does not claim that policy conditioning originates in the present paper. **RedacBench** and **RedactionBench** are distinct works with separate bibliography entries.

## Exact gate used in the worked protocol application

The manuscript defines V before unusable outputs are normalized to empty predictions. V is one precisely when the fixed parsing and validation procedure yields a usable prediction list; a valid empty list is allowed. The hypothetical operator starts from release-all and releases exactly V=1, with no other filter. Under the stipulated matching convention, V=0 on an applicable page implies matching failure. An exact recount can use sufficient joint counts. Reconstructing it from individual outputs needs labels and either raw outputs or normalized predictions plus validity flags; normalized empty lists alone can conflate valid empty predictions and failures. This is the manuscript's explicit operational application, not an additional claim about an observed deployment.

## Published-aggregate application added after review

`Public_Protocol_Application.ipynb` uses the critical DocLeak and page-validity columns for all 14 full-schema systems in Table 3. It transcribes published numerical facts and enumerates compatible integer page counts under a declared reporting model. The source's supported-schema Table 4 is not mixed into the common-denominator calculation. Source version, exact HTML checksum, transcription provenance, and source locators are retained in `public_protocol_inputs/source_provenance.json`; the full source paper is not redistributed.

The reported rows are factual inputs. The conditional validity gate, feasible-count ranges, and compatible completions are this paper's deductions. The completions are not asserted to be the underlying page-level data. Sensitivity calculations separately assess the use of one row, the common applicability set, and nearest rounding versus truncation. The result concerns critical typed-IoU matching on a fixed challenge set. It is not a confidence interval, an empirical replication, a deployment estimate, or a rendered-mask concealment measurement.
