# Position: PII Redaction Claims Must Specify the Release Mechanism

This artifact supports an anonymous, single-author position paper. It includes a documented protocol analysis, a bounded application to published numerical aggregates, mathematical arguments, and a separate hypothetical audit. No redaction model was run for these analyses.

## Contents

- `manuscript/main.tex` and three BibTeX files: editable paper.
- `manuscript/IEEEtran.cls` and `IEEEtran.bst`: unchanged third-party template files with their license notices.
- `manuscript/figures/release_changes.pdf`: main vector figure.
- `analysis/Release_Conditional_Risk_Illustrations.ipynb`: 18 executed code cells for stipulated populations, audit counts, and handcrafted wrapper checks.
- `analysis/illustration_outputs/`: 16 hypothetical-result CSV tables, JSON summaries, and main and supplementary figures.
- `analysis/Public_Protocol_Application.ipynb`: six executed code cells analyzing actual published LeakageBench Table 3 aggregates.
- `analysis/public_protocol_inputs/`: 14 transcribed rows and source provenance, version, retrieval date, and checksums.
- `analysis/public_protocol_outputs/`: five CSV result tables, JSON results and input checksums, and the execution log.
- `analysis/ILLUSTRATION_NOTES.md` and `PUBLIC_PROTOCOL_APPLICATION_NOTES.md`: reproduction details, assumptions, results, and interpretation for the respective notebooks.
- `analysis/PROTOCOL_SOURCES.md` and `ASSURANCE_SOURCES.md`: primary-source locators and comparisons with prior work.
- `analysis/evidence_reuse_record.json`: completed hypothetical record for the baseline, mask expansion, and separate gate-restriction comparison, with conditions for continued applicability and targeted reassessment triggers.
- `new_insights.txt`: proposed position-paper submission text.
- `requirements.txt`: dependency versions for the original illustration notebook.
- `SHA256SUMS.txt`: checksums for all other delivered files.

## Compile

From `manuscript/`, run `latexmk -pdf main.tex`. Alternatively run `pdflatex main.tex`, `bibtex main`, then `pdflatex main.tex` twice. The paper uses the standard IEEE conference class and common LaTeX packages, with default 10-point font and geometry.

The LLM-use disclosure states the assistance and checks performed. A source comment marks where the author-specific inspection confirmation should be added after the human author has completed that inspection; the separate submission notes provide the wording.

## Reproduce the analyses

Run each notebook from `analysis/`. No GPU, model, credentials, paid API, or private dataset is needed.

The illustration notebook uses NumPy, SciPy, and Matplotlib. Its counts are stipulated and its software fixtures are handcrafted. Its latest revision changes explanatory markdown only; all 18 numerical code cells and their successful saved outputs are unchanged. It writes `illustration_outputs/`. Copy `figure3_gate_and_denominator.pdf` from that folder to `manuscript/figures/release_changes.pdf` before recompiling to use the regenerated main figure. Figures 1 and 2 remain supplementary.

The public-protocol notebook uses only Python's standard library and works offline from embedded, checksum-verified inputs. The same input rows are supplied as CSV. Its six cells executed successfully; its source parser verified all 14 rows against the retrieved primary HTML. The manuscript and notebook markdown give the rounding constraints and a worked count example; this explanatory revision leaves all executable cells and stored numerical outputs unchanged. Remote source verification is optional and disabled by default. Outputs are written to `public_protocol_outputs/`.

The full source paper and inspected histogram are not redistributed. Their source locators and checksums support verification. A changed upstream HTML hash should trigger source review, rather than silently changing the analyzed version.

## Interpretation

The two analyses use different evidence. The first is hypothetical and reports no measured performance. Its mask-expansion claim inherits one baseline confidence event; its transferred and directly recomputed retained-sample bounds are separate procedures. The gate restriction leaves the target unsupported because fewer relevant audit observations remain, not because increased true risk was observed.

The second derives deterministic ranges from actual published validity and critical DocLeak marginals under explicit reporting and common-applicability assumptions. It includes rounding and own-row sensitivities, compatible count completions, and a conditional exact recount if all 500 pages are separately known to be applicable. The completions are mathematical witnesses, not observed per-page predictions. These fixed-table matching ranges are neither confidence intervals nor claims about deployment risk or rendered-mask concealment.

Reference names and IEEE copyright notices identify prior work and third-party template authors. The manuscript and authored artifact text omit the submitting author's identity.

## Reusing the example record

The record documents a hypothetical justification; filling its fields does not establish its assumptions. Check its population, release unit, policy and label coverage, delivered channels, acceptance, and mask-preservation conditions before extending a claim. Its reassessment entries distinguish changes outside the original scope from evidence that an original audit assumption was false. Structural arguments or a recount from adequate retained labels can suffice; a new full audit is not automatic.
