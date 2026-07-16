# What Makes a Pull Request "AI Slop"? An Empirical Study of Maintainer Labels

This repository contains the data, code, and materials for an empirical study characterizing pull requests that open-source maintainers have explicitly labeled as "AI slop." The study analyzes 1,110 maintainer-labeled slop PRs matched against 1,110 control PRs, supplemented by 1,354 negative-control PRs, across 66 GitHub repositories.

**Author:** Sepehr Rasouli ([sepehrrasouli.r@gmail.com](mailto:sepehrrasouli.r@gmail.com))

## Abstract

Open source maintainers increasingly report being overwhelmed by AI-generated pull requests, prompting many repositories to adopt formal policies to manage the influx. Despite this, little is known about what characterizes these contributions or the accounts that submit them.

We set out to characterize AI-slop pull requests at both the PR and author levels. We collected 1,110 PRs that maintainers explicitly labeled as AI slop and matched them 1:1 with control PRs, closed, non-AI-labeled pull requests from the same repositories and time windows, across 66 repositories, supplemented by 1,354 additional PRs rejected for non-AI reasons (duplicate, invalid, or wontfix).

At the PR level, we did not find a reliable fingerprint. Effect sizes were modest, varied substantially across repositories, and collectively explained only 6% of the variance in which PRs received the slop label. The strongest PR-level signals, lexical diversity, body length, and comment count, were driven primarily by a single repository and did not generalize.

At the author level, however, clear and robust patterns emerged. Slop authors operated accounts that were significantly newer (median 1,430 vs. 2,351 days), had far fewer followers (median 2 vs. 6), achieved substantially lower merge rates (33% vs. 73%), and were much less likely to have any prior contribution history in the repository they targeted (11% vs. 26%). These author-level differences held across all sensitivity checks, including the removal of the largest repository from the sample.

Our findings suggest that while the PR-level characteristics of AI slop are repository-dependent, the type of contributor who submits AI slop is consistent: a newer, less established account with little connection to the project they are targeting. These results suggest that efforts to automatically detect AI-generated contributions should focus on **who is submitting** rather than **what the submission looks like**, and that the label "AI slop" itself carries different meanings across open source communities.

## Research Questions

- **RQ1:** What pull request-level characteristics are associated with maintainer-labeled AI-generated pull requests?
- **RQ2:** How do contributor histories and participation patterns differ between AI-labeled and non-AI-labeled pull requests?

## Key Findings

| Level | Finding |
|---|---|
| PR-level | No reliable, repository-general fingerprint; multivariate model explains only 6.4% of variance (Pseudo R² = 0.064) |
| PR-level | Strongest raw effects (lexical diversity, body length, comment count) driven primarily by one repository (`flathub/flathub`, 43% of pairs) and vanish or weaken when excluded |
| Author-level | Slop authors have newer accounts, fewer followers, lower merge ratios, and less prior history in the targeted repository |
| Author-level | These differences are robust to sensitivity checks, including exclusion of the largest repository |
| Labeling | Re-labeling using a blind codebook shows only fair agreement with maintainer labels overall (Cohen's Kappa = 0.25), varying from substantial to worse-than-chance across repositories |

## Data

The dataset was collected via the GitHub GraphQL and REST APIs between June 2025 and June 2026. It includes:

- **1,110 slop PRs** — labeled `AI Slop`, `AI spam`, `resolution: AI policy`, `ai-slop`, `ai-spam`, or `slop`
- **1,110 matched control PRs** — closed, non-merged, non-AI-labeled PRs from the same repository within a ±90–120 day window
- **1,354 negative control PRs** — closed for other reasons (`duplicate`, `invalid`, `wontfix`, `stale`, `not-planned`)
- **1,599 unique authors** (734 slop authors, 856 control authors)

across **66 repositories** with 1,000+ GitHub stars.

Matching was performed greedily on temporal proximity and changed-file-count similarity, with a strict exclusion list of known slop authors and bot accounts applied globally before sampling. Full matching, filtering, and quality-control procedures are described in Section 2 of the paper.

## Methodology Summary

- **Within-repository normalization:** All continuous PR-level features were converted to within-repository z-scores (median/IQR) to control for repository-level confounds before statistical testing.
- **PR-level tests:** Mann-Whitney U tests (rank-biserial effect sizes) for continuous features; chi-squared tests (odds ratios) for binary features; Bonferroni-corrected; Kruskal-Wallis/Dunn for the three-way (slop/control/negative-control) comparison.
- **Author-level tests:** Same approach applied to aggregated, first-appearance author-level features.
- **Multivariate model:** Logistic regression with repository fixed effects predicting the slop label from eight PR-level features.
- **Sensitivity checks:** Exclusion of `flathub/flathub` (43% of pairs), exclusion of the three largest repositories, and outlier-proportion analysis.
- **Label validation:** Blind re-labeling of 200 stratified-sampled PRs against a seven-item codebook (Appendix A of the paper), compared to original maintainer labels via Cohen's Kappa.

## Codebook

The seven-item AI-slop identification checklist used for blind re-labeling is included in `codebook/` (and reproduced in Appendix A of the paper). A PR was classified as AI slop if it met **two or more** of the seven criteria, covering formulaic section headers, template mismatch, explicit AI-generation disclosures, missing issue references, diff/description mismatches, unrelated file changes, and factually incorrect author responses.

## Limitations

- Label validation was performed by a single researcher.
- The first author is a `pip` triager and closed a subset of the `pip` slop PRs in this sample; the codebook was formalized from the same heuristics used to apply those labels, so the `pip` subset (<1% of the dataset) should not be treated as independent validation.
- `flathub/flathub` accounts for 43% of matched pairs and has distinctive labeling practices; results are reported with and without it.
- The multivariate model explains only 6.4% of variance, suggesting slop labeling depends heavily on unobserved factors (maintainer judgment, diff content, context).
- Analysis was restricted to repositories with 1,000+ stars, limiting generalizability to smaller or policy-less projects.
- Lexical/textual features were not systematically stripped of template boilerplate, which may introduce noise.

See Section 5.3 of the paper for full details.

## Citation

Citation metadata is provided in [`CITATION.cff`](CITATION.cff), which GitHub uses to generate a "Cite this repository" button automatically. You can also cite manually:

```bibtex
@misc{rasouli_ai_slop,
  author       = {Rasouli, Sepehr},
  title        = {What Makes a Pull Request "AI Slop"? An Empirical Study of Maintainer Labels},
  year         = {2026},
  howpublished = {\url{https://github.com/sepehr-rs/Characterizing-AI-slop}}
}
```
## License

This work is licensed under a Creative Commons Attribution 4.0 International License (CC-BY-4.0). You are free to share and adapt the material for any purpose, provided appropriate credit is given.

## Contact

Questions or issues can be directed to Sepehr Rasouli at [sepehrrasouli.r@gmail.com](mailto:sepehrrasouli.r@gmail.com), or opened as a GitHub issue in this repository.