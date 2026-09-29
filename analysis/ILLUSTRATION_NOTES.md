# Reproducing the exact illustrations

Manuscript: *Position: PII Redaction Claims Must Specify the Release Mechanism*.
Author: Anonymous Author.

Every population and audit count in this notebook is stipulated. The outputs are analytical illustrations, not model evaluations, benchmark measurements, or observations from a deployment audit.

## Reproduction

Open `Release_Conditional_Risk_Illustrations.ipynb` in a Python environment with NumPy, SciPy, and Matplotlib, and run all cells in order. The notebook runs on CPU and does not download data or models. It writes figures, CSV tables, JSON summaries, and dependency versions under `illustration_outputs/` in the current working directory. The delivered notebook contains outputs from a complete successful execution of 18 code cells. Reproduction needs no separate Python script.

Tested versions: Python 3.12.14, NumPy 2.3.5, SciPy 1.17.0, Matplotlib 3.10.8. The notebook writes current versions on each execution. The numerical constructions and handcrafted wrapper checks are deterministic; PDF metadata can differ across runs.

## Main-paper illustration

`illustration_outputs/figure3_gate_and_denominator.pdf` is a vector figure sized 7.16 x 2.72 inches for a double-column layout. Its PNG copy supports inspection. It has two panels:

- **Gate tightening:** Among 1,000 inputs, an old gate releases 800 records and all eight failures. A stricter nested gate releases 80 and the same eight failures. Conditional risk increases from 1% to 10%, while failed releases divided by all inputs remain 0.8%. Outputs do not change.
- **Denominator dilution:** A fixed population has 100 records containing protected content, ten of which fail, and 900 records without protected content. Releasing the additional 900 records changes all-release risk from 10% to 1%. Risk among releases containing protected content remains 10%, and the same ten failures remain.

Suggested caption: **Exact hypothetical populations show two denominator effects. (a) A stricter nested gate retains all eight failures while reducing releases from 800 to 80; conditional failure increases although the joint failed-release frequency is unchanged. (b) Adding 900 releases without policy-required content reduces overall conditional failure while leaving failure among the 100 protected-content releases unchanged. No redacted output changes in either construction. These are arithmetic illustrations, not measured system performance.**

The previous `figure1_constructed_risks.pdf` and `figure2_zero_failure_audits.pdf` remain supplementary arithmetic illustrations.

## Added checks and exact results

### Keep/drop identity

In the 800-to-80 gate example, retention rho = 0.1, retained risk = 0.1, dropped risk = 0, and old risk = 0.01. The notebook verifies both identities:

- old risk = rho * retained risk + (1-rho) * dropped risk;
- new risk - old risk = (1-rho) * (retained risk - dropped risk).

### Same-score thresholding and group restriction

Four score bins have 1,000 records each, scores 0.01, 0.05, 0.10, and 0.20, and failure counts 10, 50, 100, and 200. Each bin is calibrated for whole-record failure. Releasing scores at most 0.01, 0.05, 0.10, or 0.20 gives risks 0.01, 0.03, 0.0533333333, and 0.09. Tightening the threshold decreases risk in this monotone conditional-risk construction.

A separate population assigns the constant calibrated score 0.01 to all 1,000 records, with ten failures. A subgroup of 100 contains all ten failures. Restricting release to this subgroup gives risk 0.10. This restriction uses group information beyond a threshold on the same constant score.

### Complete hypothetical audit readout

Stipulated counts: N = 5,000 inputs, n = 3,000 released records, k = 0 failures; 300 of the released records contain protected content and none fails. Empirical coverage is 60%, and protected-content prevalence among releases is 10%.

| Confidence design | Claim population | Applicable releases | Upper bound |
| --- | --- | ---: | ---: |
| Individual 95% | All released records | 3,000 | 0.0998079012% |
| Individual 95% | Releases with protected content | 300 | 0.9936081944% |
| Simultaneous 95%, delta = 0.025 per claim | All released records | 3,000 | 0.1228870804% |
| Simultaneous 95%, delta = 0.025 per claim | Releases with protected content | 300 | 1.2220974694% |

The individual all-release bound supports a 0.1% target under the stated hypothetical audit assumptions. Its subgroup counterpart does not. Neither bound supports that target when the two statements receive simultaneous 95% coverage by this allocation. The empirical prevalence is not treated as a known population constant.

### Jointly valid gate-retention transfer

A separate hypothetical old audit has 3,000 released units and zero failures. A frozen nested gate retains 1,500. Allocate total delta = 0.05 equally between old-risk and retention bounds:

- old-risk upper bound: 0.001228870803825512;
- one-sided retention lower bound: 0.48194876932698494;
- transferred risk upper bound: 0.0025497955011723815, or **0.2549795501%**.

The transferred bound is min(1, old-risk upper / retention lower). Joint validity uses the union bound and does not require the two estimates to be independent. It does require a common target population, a genuinely nested frozen gate, valid independent audit units, and pointwise no-greater new loss. If the retention lower bound were zero, the transferred bound would be one. The observed retention proportion 0.5 is not substituted for its unknown population value. Transfer is useful when an aggregate old-risk bound and valid retention evidence are available but losses linked to the retained units are unavailable. If those labels are available, outputs are unchanged, and the new gate was fixed independently of the audit, a direct bound can instead be recomputed from the existing retained audit labels without a fresh audit or relabeling.

## Audit-to-update worked wrapper

The added wrapper is explicit, but its deterministic span extractor `f` remains a mathematical parameter. No real checkpoint, trained weights, or observed model outputs support the hypothetical counts. Handcrafted software fixtures check Unicode code-point handling, UTF-8 rejection, valid and invalid offsets, labels and scores, overlapping masks, empty predictions, mask expansion, and the fixed length restriction. These fixtures are not the hypothetical audit units and produce no empirical redaction-performance claim.

The baseline gate accepts valid UTF-8 input with at most 4,096 Unicode code points only after extraction, all offset/label/score checks, and rendering succeed. Extractor results are a finite list of half-open integer intervals, labels PERSON/EMAIL/PHONE, and finite scores in [0,1]. Valid overlaps and an empty list are permitted. The union of intervals scoring at least 0.5 is replaced with one ASCII `*` per source code point. Only the resulting plain text is released; internal source text, offsets, scores, and masks are not included in the recipient-visible artifact.

Update 1, the positive control, expands the original mask by one code point at either end and clips at source boundaries. It preserves every baseline release decision and introduces no new extractor call or acceptance branch. Update 2 uses the prespecified gate `g2 = g0 and length <= 2048`, which additionally requires input length at most 2,048 code points and leaves the baseline redacted output unchanged.

Counts are stipulated: 5,000 input units, 3,000 baseline releases with zero failures, 300 baseline releases with protected content, and 1,500 releases retained by the restricted gate. No protected-content count under the restricted gate is stipulated.

| Procedure | Applicable releases | Upper bound | 0.1% target |
| --- | ---: | ---: | --- |
| Baseline, individual 95% | 3,000 | 0.0998079012% | Established under the hypothetical assumptions |
| Mask expansion, same confidence event | 3,000 | 0.0998079012% | Inherited by pointwise dominance |
| Restricted gate, joint transfer with 0.025 + 0.025 allocation | 1,500 | 0.2549795501% | Not established |
| Restricted gate, direct recomputation from existing labels, individual 95% | 1,500 | 0.1995161862% | Not established |

The direct calculation reuses the existing labels of the 1,500 retained units and their unchanged outputs. No new audit or relabeling is assumed. The smaller retained sample leaves the 0.1% target unsupported despite zero retained failures. These calculations demonstrate neither an increase in true risk nor a violation of the target. The table compares distinct confidence procedures and does not claim simultaneous 95% coverage across all rows. The earlier simultaneous all-release/protected-content example remains a separate confidence plan. The baseline and expansion share one event, while the transfer and direct procedures have their own stated coverage interpretations. Results are in `update_decisions.csv`.

One valid baseline confidence event supports every structurally proved pointwise no-greater-loss update with the same policy, population, and gate. No additional allocation is needed per such update because all conclusions follow on that same event. Empirical apparent improvements without a valid pointwise argument do not qualify for this inheritance.

## Supplementary numerical illustrations

Both mention-level systems have TP = 99,000, FN = 1,000, FP = 0, precision = 1, recall = 0.99, F1 = 0.9949748743718593, and F10 = 0.9900980295078721. Their bad-record proportions are 100% and 1%.

The fixed release population has 100,000 processed records and 10,000 releases including 1,000 failures. The joint failed-release frequency is 1%; conditional failure is 10%.

For a fixed pre-gate failure marginal q and coverage c > 0, the conditional-risk interval is [max(0, (q+c-1)/c), min(1, q/c)]. These standard intersection bounds carry no novelty claim.

| Target risk | Required zero-failure audits, H = 1 | H = 20 | H = 100 |
| --- | ---: | ---: | ---: |
| 0.01 | 299 | 597 | 757 |
| 0.001 | 2,995 | 5,989 | 7,598 |
| 0.0001 | 29,956 | 59,912 | 76,006 |

These are per-claim counts for at least 95% simultaneous coverage using delta = 0.05/H. They are neither measurements nor universal lower bounds for every possible assurance method.

In the candidate-restricted illustration, 200 of 100,000 released records contain protected content outside every candidate mask. Without broader masking or suppression, perfect candidate classification still leaves a 0.2% failure floor, exceeding a 0.1% target.

## Rectangle containment versus matching

The executable geometric illustration uses one gold rectangle of area 100 and the same correct type label in every case. A matching opaque mask has area 100, IoU 1, and complete concealment. A containing expanded mask has area 200, IoU 0.5, and complete concealment, but fails the 0.75 matching threshold. An inner mask has area 80 and IoU 0.8, so it passes matching while leaving 20% of the required region uncovered. These are exact hypothetical rectangles, not image predictions, and no one-to-one assignment ambiguity is involved. Results are in `geometry_containment_vs_matching.csv`.

## Invalid-output withholding under a matching protocol

When every invalid output is assigned empty predictions and every evaluated page contains required content, all invalid outputs fail the matching loss. Withholding only those outputs cannot increase that same loss among retained valid outputs. The exact illustration has 20 applicable pages, eight failures, and five invalid outputs: failure decreases from 8/20 = 40% to 3/15 = 20%. The notebook additionally checks all 230 feasible failure/invalid-count pairs with at least one valid output in a 20-page population. This limited monotonicity result does not apply to arbitrary gates. Values are in `invalid_output_withholding.csv`.

## Machine-readable files

`illustration_outputs/results.json` contains every construction and `environment.json` records dependency versions. Sixteen CSV files contain the mention example, fixed release population, selection sweep, gate tightening, protected-content dilution, keep/drop identity, monotone score thresholds, group restriction, audit sample-size requirements, small-audit comparisons, complete hypothetical audit, jointly valid gate transfer, candidate-omission floor, rectangle containment versus matching, invalid-output withholding, and the audit-to-update decisions.

## Verification and scope

Assertions check all population counts, equal mention scores, nested gates, keep/drop identities, protected-content denominator identities, score calibration and monotonicity, rectangle concealment and IoU, audit minima, and agreement with beta-quantile calculations. The new PDF was rendered with Poppler and visually inspected. Audit counts in this notebook are specified in advance; the notebook contains no stochastic experiment, training, model selection, benchmark evaluation, or real-world audit.

Binomial audit calculations require the independently sampled release units, correct complete labels, fixed mechanism, and fixed claim populations described in the notebook. Curated challenge-set size alone does not establish those conditions. Dependence, adaptive reuse, population shift, or unobserved protected content are not repaired by the formulas. Content containment does not imply protection against every inference attack.
