# Bounded application to published LeakageBench aggregates

`Public_Protocol_Application.ipynb` contains an executed CPU-only analysis of actual published numerical inputs. It does not run a redaction model, generate predictions, or measure a deployment. Six cells executed successfully using only Python's standard library.

## Source and reproducibility

Source: Vishnu Prasad Vijaya Kumar, Santhosh Venkatesh, and Ivan P. Yamshchikov, *LeakageBench: Document-Level Leakage Risk for Redacting Personally Identifiable Information in Document Images*, arXiv:2609.02207v1, 2 September 2026. Retrieved 23 September 2026.

- Source URL: https://arxiv.org/html/2609.02207v1
- Input locator: Section 6, Table 3 (`S6.T3`), full-schema critical DocLeak and page validity.
- Full fetched HTML: 458,024 bytes; SHA256 `0664b482d98c482ddb81f92264a8a0514917ec2ee4f25da74ea85e5681eb2743`.
- Minimal factual input CSV SHA256: `19a6371e952b5d0b50d9305e57acba03055ab167eae644583273db1bac1f9076`.

The notebook parsed the source HTML and verified that all 14 input rows match Table 3. The delivered input contains only system labels and numerical facts with provenance. The full HTML and inspected histogram are cached separately under `qa/source_cache/` for verification and must not be included in the deliverable archive.

Run all notebook cells from the `analysis/` directory. Offline execution uses its embedded checksum-verified input, also provided in `public_protocol_inputs/`. A source-cache check runs when the cache exists. A remote verification switch is disabled by default. All generated results are under `public_protocol_outputs/`.

## Question and protocol

The proposed update withholds outputs failing the paper's reported page-validity predicate and leaves accepted predictions unchanged. This gate is fixed for the analysis, not chosen to obtain a reversal. All 14 published systems are included; five have validity below one.

Section 3.4 defines critical applicability through the fixed gold Direct/Linkage scope. Table 3 uses full-schema scoring, retaining unsupported types as misses. Table 4's model-specific supported-type denominators are excluded. Section 6 and Appendix B describe unusable outputs as empty predictions. The calculation assumes the reported page-validity predicate is the corresponding unusable-output predicate, rather than a different diagnostic flag or individual-box rejection.

For system $i$, the integer counts are a shared critical-applicable denominator $1\le D\le500$, critical failures $0\le F_i\le D$, all-page invalid outputs $0\le B_i\le500$, and invalid applicable outputs $A_i$. For published values $\widehat d_i$ and $\widehat v_i$, the explicit nearest-three-decimal constraints are

\[
|F_i/D-\widehat d_i|\le0.0005,
\qquad |(500-B_i)/500-\widehat v_i|\le0.0005.
\]

Both endpoints are included, admitting either half-unit tie convention. Every row must permit integer counts for the same $D$; intersecting all 14 critical-rate constraints gives $D\in\{493,\ldots,500\}$. Each completion must additionally satisfy

\[
\max\{0,B_i-(500-D)\}\le A_i\le\min\{B_i,F_i\},\qquad D>A_i.
\]

The final inequality requires a nonempty retained applicable subset. Its matching-failure proportion is $(F_i-A_i)/(D-A_i)$. Code variables are $V=500-B_i$ and $I=A_i$. The page-validity predicate means an unusable output scored as empty predictions, not an individual rejected box. Overall validity $V/500$ is distinct from applicable-page validity $(D-A_i)/D$.

## Main result

Assuming nearest three-decimal rounding, the 14 rows jointly permit D=493 through 500. Exact integer enumeration gives the following sharp bounds relative to these Table 3 constraints:

| System with validity below one | Overall validity | Retained critical pages | Critical DocLeak among valid outputs |
| --- | ---: | ---: | ---: |
| Presidio | 99.6% | 491–498 | 98.981670%–98.995984% |
| GLiNER2 | 99.8% | 492–499 | 99.390244%–99.398798% |
| OpenAI Privacy Filter | 80.6% | 396–403 | 98.737374%–98.759305% |
| Qwen3-VL-32B | 66.6% | 326–333 | 99.693252%–99.699700% |
| InternVL3-38B | 54.0% | 263–270 | 100% |

The full 14-row output is `validity_gate_bounds.csv`. The result supports a bounded conclusion after the proposed update: invalid-output withholding alone does not explain the remaining critical matching failures. It does not establish mask containment or task utility.

## Sensitivities and what is not identified

The source does not state its decimal formatting routine. `sensitivity_bounds.csv` repeats the calculation under truncation and with only each selected model's row. Truncation plus common applicability fixes D=500 and gives the upper endpoints above. Under nearest rounding and Qwen's own row alone, its retained critical failure remains 99.570815%–99.699700%. Privacy Filter's own-row bound expands to 0%–99.047619%, showing that its tight primary bound depends materially on the common-denominator information.

For the worked Privacy Filter row, the reported values are d=0.990 and v=0.806. The integer validity constraint gives B=97 and V=403. At the permitted common D=493, the failure constraint gives F=488. Feasible overlap is A=90 through 97: the lower endpoint is max(0,97-(500-493))=90, and the upper endpoint is min(97,488)=97. Both leave a positive retained denominator. A=97 gives (488-97)/(493-97)=391/396, while A=90 gives (488-90)/(493-90)=398/403. `fixed_denominator_nonidentifiability.csv` contains these exact completions. They share the published marginals and D but differ in the unreported overlap. They are mathematical witnesses, not reconstructed observations.

Additional information can resolve the ambiguity. If D=500 is separately established, then I=500-V and the exact retained count is recoverable. `known_D500_conditional_recount.csv` supplies that conditional calculation without treating D=500 as a reported fact. Full-text review did not identify an explicit critical-applicable count, and the inspected critical histogram combines zero and positive counts in its first bin. The identification claim is deliberately limited to Table 3's published marginals.

## Interpretation and delivery

These are deterministic finite-table bounds for typed IoU matching, not confidence intervals, deployment probabilities, final-artifact concealment rates, or privacy certificates. No independent-page assumption is used. The source corpus is a challenge set of 500 pages from 291 source documents. Compatible count completions must never be described as observed per-page logs. The absence of a repository in the inspected sources is not treated as proof of global unavailability.

Deliver the notebook, these notes, `public_protocol_inputs/`, and `public_protocol_outputs/`. Exclude the full primary paper and histogram caches. The existing hypothetical-illustration notebook remains separate.
