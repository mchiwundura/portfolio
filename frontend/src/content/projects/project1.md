---
title: 'Clinical Trial Outcome Prediction'
description: 'A machine learning project examining whether multimodal trial data can predict clinical trial outcomes, and how much published benchmark performance survives reproduction'
link: 'github.com/mchiwundura'
vector: '../../assets/clinical_trial_outcome.svg'
---

**Project status: literature review and design.** No model has been trained or evaluated. This page records what the published benchmarks establish, where their limits lie, and how the project will proceed.

## The problem

Clinical trials are the most expensive and uncertain stage of drug development. A reliable estimate of the probability that a trial will succeed could inform go/no-go decisions, portfolio prioritisation and trial design. The difficulty is that success depends on drug biology, disease biology, protocol design, operational execution and regulatory judgement. Historical trial data capture some of these factors well and others poorly.

The project asks a narrow question: how well can multimodal trial data predict trial success under a defensible evaluation, and which parts of published performance survive reproduction? It is not an attempt to infer biological efficacy from molecular structure alone.

"Prediction" is ambiguous. A model can estimate whether a trial will meet its primary endpoint, advance to the next phase, receive approval, or be terminated. These labels are related but not interchangeable, and their base rates differ. The first decision is therefore label definition, not model choice.

## Literature review

This is a focused review of the benchmark and modelling literature assembled for the project. It is not a systematic review.

### Task definition and labels

Most work formulates the task as binary classification. TOP defines success as meeting primary endpoints, while the HINT repository describes the same benchmark as predicting approval probability. CTP-LLM instead predicts phase transition, labelling trials by their progression through the regulatory process. The three labels answer different questions. Endpoint success is a narrower, more mechanistic outcome than approval, and phase transition incorporates regulatory and strategic decisions that are not captured by drug and disease features alone.

The distinction matters when comparing results. A model with 75% accuracy at predicting the transition from phase III to approval has not demonstrated 75% accuracy at predicting whether a phase II trial will meet its endpoint.

### The TOP benchmark

TOP contains 17,538 trials covering 13,880 small-molecule drugs and 5,335 diseases, with 57.0% successes and 43.0% failures. It provides drug structures, ICD-10 disease codes and hierarchy, unstructured eligibility criteria, and trial metadata. The phase I, II and III subsets contain 2,402, 7,790 and 5,741 trials respectively, and the benchmark uses a temporal split (Therapeutic Data Commons, 2026; Fu et al., 2022).

The benchmark is a substantial contribution: it made the task comparable across papers and released the data and code. Its limits are equally important. The 57% success rate is a property of the curated set, not a real-world prior. The dataset covers small-molecule drugs, so vaccines, biologics and cell therapies are outside its scope. The label is endpoint success, not the broader question of whether a programme is worth pursuing. The licence is non-commercial, which constrains how the data can be used.

### HINT

HINT encodes the drug as a molecular graph, the disease through its code and hierarchy, and the eligibility criteria as text. It adds knowledge embeddings from drug pharmacokinetic data and historical trial data, connects the representations through a hierarchical interaction graph, and predicts the trial outcome. The authors trained and validated on 1,160 phase I, 4,449 phase II and 3,436 phase III trials, reporting F1 scores of 0.665, 0.620 and 0.847 on test sets of 627, 1,653 and 1,140 trials. HINT outperformed the best baseline on most metrics (Fu et al., 2022).

The phase II result is the weakest, which is informative. Phase II trials vary widely in design, endpoint and population, and the available features do not capture operational problems such as slow recruitment, poor site performance or protocol amendments. The abstract reports F1, a threshold-dependent measure; calibration and subgroup performance are not summarised. For any decision-support use, those omissions matter more than a small gain in F1.

### SPOT

SPOT addresses two weaknesses in earlier models: the relations among similar trials and the skew towards common trial types. It identifies trial topics, clusters multi-source data into those topics, generates trial embeddings, orders them by topic and time, and uses meta-learning to adapt to each sequence. The authors report relative PR-AUC improvements of 21.5% on phase I, 8.9% on phase II and 5.5% on phase III over prior methods (Wang et al., 2023).

The design is well motivated. Trials are not independent; a drug's earlier trials, and the history of similar programmes, carry information that a flat feature vector loses. But the reported numbers are relative lifts on the same benchmark, not absolute performance, and they do not establish that the model generalises to new therapeutic areas or to trials outside TOP.

### LLMs and phase transitions

CTP-LLM fine-tunes GPT-3.5 on trial protocol texts and predicts phase transitions using a new PhaseTransition dataset. The authors describe it as the first LLM-based model for the task and report 67% accuracy across all phases, rising to 75% for the transition from phase III to approval (Reinisch et al., 2024).

The approach is interesting because it uses the protocol document directly, without hand-engineered features. It also raises evaluation questions. Accuracy is a weak metric when classes are imbalanced, and LLM evaluations can be contaminated by pretraining data. A temporal split helps, but it does not prove that the model has learned generalisable trial design signals rather than memorised known outcomes. The label is phase transition, not endpoint success, so the result should not be compared directly with HINT's F1 scores.

### Tooling and reproducible pipelines

PyTrial provides a common interface and examples for logistic regression, XGBoost, a multilayer perceptron, HINT and SPOT. It formalises the task as predicting an outcome before phase I, or using patient records from earlier phases to predict phase II or III outcomes (PyTrial, 2026). Its main value is discipline: a shared task definition, standard splits and simple baselines make it harder to mistake complexity for progress. The worked HINT example was marked incomplete when accessed, so the library is a starting point rather than a finished reproduction package.

Langouche's tutorial builds a feature matrix from ClinicalTrials.gov XML records, using tiny-biobert for indications and eligibility criteria, all-MiniLM-L6-v2 for sponsors, Morgan fingerprints for drugs, and XGBoost for prediction (Langouche, 2023). It is a clear demonstration of the engineering, but it is a tutorial rather than a peer-reviewed evaluation, and its comparison with HINT is the author's own. Its lesson is architectural: keep ingestion, embedding, modelling and evaluation separate and testable.

### Evidence synthesis

Qian et al. (2025) review AI methods for clinical trial outcome prediction, focusing on clinical text embedding, multimodal learning and prediction techniques, and discussing practical challenges. They note that shared benchmark data remain scarce, with TOP a notable exception. The review is a useful map, but it does not settle which architecture is best; preprocessing, splitting and label choices still vary between studies.

### What the evidence establishes

| Approach | Inputs | Label | Reported result | Main limitation |
|---|---|---|---|---|
| HINT | Drug graph, disease hierarchy, eligibility text, knowledge embeddings | Trial success | F1 0.665 / 0.620 / 0.847 | Phase II weak; calibration not reported; benchmark-specific |
| SPOT | Trial topics, temporal sequences, meta-learning | Trial success | Relative PR-AUC lift 21.5% / 8.9% / 5.5% | Relative gains; same benchmark |
| CTP-LLM | Protocol text | Phase transition | 67% accuracy; 75% for phase III to approval | Different label; accuracy only; contamination risk |
| PyTrial baselines | Tabular and multimodal features | Trial success | Reference baselines | Examples, not a validated benchmark |

The evidence supports a modest conclusion: multimodal models beat simple baselines on TOP, and phase III outcomes are more predictable than phase II outcomes. It does not establish clinical or commercial utility, causal understanding, or generalisation beyond small-molecule trials in a curated benchmark.

## Design implications

**Fix the label first.** The project will choose one outcome and justify it. Endpoint success is the most defensible benchmark label; approval and phase transition answer different questions and should be reported separately if used.

**Model phases separately.** The predictors and base rates differ by phase, and a pooled model can hide that. Phase-wise models also make comparison with HINT and SPOT possible.

**Use temporal splits and check for leakage.** Trial outcomes influence later trial design, and features derived from post-outcome information can leak. The split must respect the date at which each feature would have been available.

**Reproduce before extending.** A faithful baseline reproduction is more informative than a novel model trained on an unverified pipeline. The first target is to match the published baseline table within a stated tolerance, then reproduce HINT.

**Report calibration and uncertainty.** F1 alone is insufficient. The project will report PR-AUC, calibration curves, Brier score and bootstrap intervals, and will state the decision threshold used.

**Treat LLM results cautiously.** Protocol-text models are promising, but contamination and label differences make headline accuracy hard to interpret. Any LLM extension will use a strict temporal holdout and a clearly defined label.

## Project plan

The checkboxes record planned work. Only the literature review is complete; nothing below should be read as implemented functionality.

### Stage 1: Scope and label definition

- [x] Complete an initial review of the trial outcome prediction literature
- [ ] Choose the primary label and justify it against approval and phase transition
- [ ] Decide whether models are phase-specific or pooled
- [ ] Write a protocol with splits, metrics and success criteria
- [ ] Confirm compute, storage and licence constraints

### Stage 2: Data and provenance

- [ ] Obtain TOP under its non-commercial terms
- [ ] Verify trial counts, phase subsets and label definitions
- [ ] Audit drug and disease mapping, missingness and duplicates
- [ ] Reproduce the temporal split
- [ ] Document preprocessing and data provenance

### Stage 3: Baseline reproduction

- [ ] Reproduce logistic regression, XGBoost and MLP baselines with PyTrial
- [ ] Report PR-AUC, F1, calibration and confidence intervals
- [ ] Check for temporal leakage
- [ ] Establish a phase-wise baseline table

### Stage 4: Published model reproduction

- [ ] Reproduce HINT using the released code and weights
- [ ] Reproduce SPOT or a comparable sequential model
- [ ] Compare results with the published metrics
- [ ] Document deviations and unresolved discrepancies

### Stage 5: Evaluation and error analysis

- [ ] Analyse errors by phase, disease area and drug class
- [ ] Assess calibration and subgroup performance
- [ ] Run modality ablations
- [ ] Estimate uncertainty with bootstrap resampling
- [ ] Identify failure modes that a single metric hides

### Stage 6: Focused extension

- [ ] Choose one extension: protocol-text modelling, calibration, or class imbalance
- [ ] Implement it against the reproduced baselines
- [ ] Use a strict temporal holdout and check for contamination
- [ ] Report negative results as well as gains

### Stage 7: Reporting and reproducibility

- [ ] Publish code, configuration and environment files
- [ ] Write a model card with intended use and limitations
- [ ] Document data licence restrictions
- [ ] Release results in a form that permits comparison

## Limitations and risks

The main limitation is construct validity. Trial success is a composite of science, operations and regulatory judgement, and a model trained on historical outcomes may learn which programmes were already expected to succeed rather than what makes a trial succeed. The benchmark's class balance and therapeutic coverage compound this.

Temporal drift is the second risk. Clinical trial design, endpoints and regulatory standards change. A model trained on trials from one period may not transfer to another, and a random split would overstate performance.

The third is that operational variables are missing. Recruitment, site quality, protocol amendments and funding are not in TOP, yet they explain many failures. A model that ignores them is predicting a filtered version of the problem.

Finally, benchmark performance is not clinical or commercial utility. A model can rank trials well and still be useless to a decision-maker who needs calibrated probabilities, explanations and a clear decision rule.

## Open questions

- Which label is most useful to the intended user: endpoint success, approval or phase transition?
- How much of HINT's advantage survives a faithful reproduction with the same preprocessing?
- Does a protocol-text model add information beyond drug, disease and eligibility features?
- Can calibration be improved without sacrificing discrimination?
- What would a prospective evaluation look like, and is one feasible?

## References

- Fu, T., Huang, K., Xiao, C., Glass, L. M., & Sun, J. (2022). HINT: Hierarchical interaction network for clinical-trial-outcome predictions. *Patterns*, 3(4), 100445. https://doi.org/10.1016/j.patter.2022.100445
- Fu, T., Huang, K., Xiao, C., Glass, L. M., & Sun, J. (2022). *clinical-trial-outcome-prediction* [GitHub repository]. https://github.com/futianfan/clinical-trial-outcome-prediction
- Therapeutic Data Commons. (2026). *Clinical trial outcome prediction task overview*. https://tdcommons.ai/multi_pred_tasks/trialoutcome/ (Accessed 24 October 2026.)
- PyTrial. (2026). *Trial outcome prediction*. https://pytrial.readthedocs.io/en/main/tutorials/trial_outcome/trial_outcome.html (Accessed 24 October 2026.)
- Langouche, L. (2023, October 4). *Clinical trial outcome prediction: Part 1: Multi-modal health data embedding*. Towards Data Science. https://medium.com/data-science/clinical-trial-outcome-prediction-a4c6d279fd42
- Qian, L., Lu, X., Haris, P., Zhu, J., Li, S., & Yang, Y. (2025). Enhancing clinical trial outcome prediction with artificial intelligence: A systematic review. *Drug Discovery Today*, 30(4), 104332. https://doi.org/10.1016/j.drudis.2025.104332
- Wang, Z., Xiao, C., & Sun, J. (2023). SPOT: Sequential predictive modeling of clinical trial outcome with meta-learning. In *Proceedings of the 14th ACM International Conference on Bioinformatics, Computational Biology, and Health Informatics (BCB '23)*. https://doi.org/10.1145/3584371.3613001
- Reinisch, M., He, J., Liao, C., Siddiqui, S., & Xiao, B. (2024). CTP-LLM: Clinical trial phase transition prediction using large language models. In *2024 IEEE International Conference on Bioinformatics and Biomedicine (BIBM)* (pp. 3667-3672). https://doi.org/10.1109/BIBM62325.2024.10822746
