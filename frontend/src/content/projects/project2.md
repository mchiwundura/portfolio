---
title: 'Zimbabwe ADR Surveillance System'
description: 'A pharmacovigilance project exploring whether routine reporting data and machine learning can support earlier detection and triage of adverse drug reaction signals in Zimbabwe'
link: 'github.com/mchiwundura'
vector: '../../assets/zimbabwe_adr.svg'
---

**Project status: literature review and design.** No model has been trained on Zimbabwean data, and no performance claims are made. This page sets out the evidence base and the planned work.

## The problem

Zimbabwe's Medicines Control Authority (MCAZ) is the national pharmacovigilance centre. Reports of suspected adverse drug reactions are submitted by health professionals, assessed, and entered into the national case management system before being forwarded to VigiBase, the World Health Organization's global database. An electronic pharmacovigilance system now allows online reporting, with offline submissions synchronised later (MCAZ, 2024).

The system functions, but it is slow and narrow. A review of 4,071 individual case safety reports submitted from Zimbabwe to VigiBase between 1993 and 2017 found that 71.5% came from targeted spontaneous reporting, 16.7% from vaccine surveillance and only 11.9% from passive spontaneous reporting. The median timeliness of reporting was 548 days (IQR 266-1131), and the median completeness score was 0.90. Antiretrovirals were implicated in 62.9% of reports, antibiotics in 27.9%, vaccines in 16.7% and anti-tubercular medicines in 13.3%. The authors concluded that the system reports on a limited therapeutic spectrum and probably underestimates the national ADR burden (Masuka and Khoza, 2020).

The city-level evidence points the same way. In Harare, only 41% of antiretroviral ADRs documented in clinical records in 2016 were reported to MCAZ; the case detection rate was 4 per 100,000, and reporting forms were available in about a quarter of surveyed facilities. Most health workers cited a lack of feedback as a reason for not reporting (Mugauri et al., 2018). In Kadoma, one form was completed for 20 reactions experienced in the district, and 79% of health workers did not perceive the system as necessary (Muringazuva et al., 2017). An earlier study at Parirenyatwa Hospital found that 47.2% of respondents did not know how to report an ADR, 47.1% were unaware of a formal reporting scheme, and only 20.1% had ever reported one (Khoza et al., 2004).

Under-reporting is not unique to Zimbabwe. Hazell and Shakir's systematic review estimated a median under-reporting rate of 94% across 37 studies from 12 countries, with similarly high rates for serious reactions (Hazell and Shakir, 2006).

The working objective is to test whether historical report data can rank incoming reports by the likelihood that they represent a serious, unexpected, or otherwise actionable reaction, so that staff can prioritise review. The system would support existing causality assessment, not replace it. It is not a patient-level risk model, and it does not claim to establish causality from observational reports.

"Prediction" can mean three different things here. It can mean detecting a statistical signal in a database, predicting which patients are at risk before exposure, or triaging reports for review. These are different tasks with different data and different evaluation. The project addresses the third.

## Literature review

This is a focused review of the Zimbabwean pharmacovigilance literature and the methodological literature on signal detection and machine learning for adverse drug reactions. It is not a systematic review.

### The Zimbabwean reporting pathway

Masuka and Khoza (2020) provide the most detailed national analysis in the sources reviewed here. Their analysis shows a system that has become functional but remains dependent on targeted reporting rather than routine passive reporting. The long median timeliness suggests that many reports reach the database well after the clinical event, which limits how quickly a signal can be detected. The therapeutic skew towards antiretrovirals, antibiotics, vaccines and anti-tubercular medicines is consistent with the areas where reporting has been actively encouraged.

The earlier studies explain why. Khoza et al. (2004) found low awareness of the reporting scheme and poor feedback from the national centre. Mugauri et al. (2018) and Muringazuva et al. (2017) found the same pattern more than a decade later: health workers were willing to report, but unclear about the process, unconvinced that reporting mattered, and rarely given feedback. The Harare evaluation concluded that the system was simple and acceptable but unstable, unrepresentative and insensitive. The Kadoma evaluation found it simple and stable but neither useful nor acceptable.

Active surveillance has been tried. Nyambayo et al. (2023) report a randomised trial of SMS-based active surveillance for adverse events following immunisation (Zm-STARSS), and a later descriptive study covers the national AEFI system and COVID-19 vaccine safety updates (Nyambayo et al., 2026). These are important precedents. They test whether digital and active approaches are feasible in Zimbabwe, but they concern vaccine safety and active follow-up rather than general ADR triage.

The practical implication is that a model will be trained on data shaped by reporting behaviour. Missing reports are not missing at random: they depend on the drug, the facility, the health worker and the attention the event received. Any evaluation that ignores this will overstate performance.

### Signal detection methods

The standard first-line approach is disproportionality analysis. Proportional reporting ratios and reporting odds ratios compare the proportion of reports containing a given drug-event pair with the proportion expected if the drug and event were independent. They are transparent, computationally simple and interpretable, which is why they remain widely used (Montastruc et al., 2011).

Their limits are equally well known. Small cell counts produce unstable estimates. Reporting patterns change over time and differ between countries. Confounding by indication can create associations that are not causal. A statistical signal is a hypothesis for review, not a conclusion. Bayesian methods address some of the instability, but they do not solve the underlying reporting bias. Human assessment remains essential.

### Machine learning and natural language processing

The machine learning literature is broad but uneven. Kim et al. (2022) reviewed 72 studies of statistical and machine learning methods for ADR detection. Fifty-one used statistical methods and 21 used machine learning. Electronic medical record data were analysed almost exclusively with regression, while FAERS data were analysed with disproportionality methods. DrugBank was the most common database for machine learning. The authors found no settled methodological standard; the choice of method depended heavily on the data source.

Pilipiec et al. (2022) reviewed 16 studies applying natural language processing to user-generated text for pharmacovigilance. Fourteen reported positive findings for ADR identification, but all included studies were assessed as having only medium reliability and validity. The evidence supports NLP as a complementary source, not a replacement for structured reporting.

Hu et al. (2024) reviewed 10 studies that used machine learning to predict adverse drug events from electronic health records. Random forest was the most common method. The mean area under the summary ROC curve was 0.76, with a very wide confidence interval (0.26-0.95), and the pooled estimate was 0.72 (95% CI 0.68-0.75). The wide interval reflects heterogeneity in populations, outcomes and model design. Common predictors included length of stay, number of prescribed drugs and admission type, which are markers of frailty and exposure rather than specific mechanisms.

Chalabianloo et al. (2026) provide a more recent and larger review: 59 studies and 191 model implementations for adverse drug event prediction in outpatient settings. Logistic regression, random forest and XGBoost were the most common algorithms. Of the studies that reported AUC, 85% exceeded 0.70 on internal validation. However, only 33.9% addressed class imbalance and only 18.6% conducted external validation. The authors concluded that the models are promising but not yet rigorous enough for clinical use.

Warner et al. (2025) reviewed artificial intelligence in pharmacovigilance signal management. Machine learning models generally outperformed traditional frequentist or Bayesian disproportionality measures on the reported metrics, but methodological transparency was mixed, and only some studies used public positive and negative control datasets. Research effort was concentrated on signal detection rather than validation, prioritisation or evaluation.

Taken together, the evidence supports three conclusions. First, machine learning can add value, particularly for text-heavy tasks such as case narrative processing. Second, the field lacks external validation and standard reporting. Third, most models are built for detection or patient-level prediction, not for the operational problem of deciding which incoming report to review first.

### Detection, prediction and triage

| Task | Question | Typical methods | Output |
|---|---|---|---|
| Signal detection | Is there a statistical association in the database? | PRR, ROR, Bayesian methods | A signal for review |
| Patient-level prediction | Which patients are at risk of an ADR? | EHR and claims-based machine learning | A risk score |
| Report triage (this project) | Which incoming report should be reviewed first? | NLP and structured machine learning | A ranked review queue |

These tasks are often conflated. A model that predicts patient risk from electronic health records is not a signal detection system, and a disproportionality statistic is not a triage model. The distinction determines the data, the label and the evaluation.

### What the evidence means for Zimbabwe

Zimbabwe's report volume is small. A model with millions of parameters would be difficult to justify, and a complex architecture would be hard to validate. The realistic approach is a regularised, interpretable model trained on a modest feature set, with disproportionality as the baseline and human review as the final decision.

Temporal validation is essential because reporting practice changes. A facility holdout would test whether the model generalises beyond the hospitals that contribute most reports. Evaluation should focus on recall at a realistic review budget, calibration and alert burden, not on accuracy at an arbitrary threshold.

The local evidence also suggests that the strongest contribution may be workflow rather than modelling. A ranked queue that saves reviewers time, with explanations and similar historical cases, could be useful even if its discrimination is modest. A model that ignores the reporting pathway will fail even if its statistics are good.

## Design implications

**Define the triage label before modelling.** The most defensible outcome is a review priority assigned by pharmacovigilance staff, or a proxy such as seriousness combined with novelty against the existing product label. Causality is a separate assessment and should not be collapsed into a prediction target.

**Use disproportionality as a baseline.** A model that cannot beat, or usefully complement, proportional reporting ratios and reporting odds ratios is not worth deploying. The baseline should be implemented with confidence intervals and the same temporal splits as the model.

**Split by time and facility.** Reports are not exchangeable across years, and facilities differ in reporting practice. Temporal validation is the minimum; a facility holdout tests external validity.

**Evaluate at the operating point.** The relevant question is how many actionable reports appear in the top N reviewed. Precision-recall AUC, recall at a fixed review budget, calibration and alert burden are more informative than ROC-AUC alone.

**Require explanations.** A reviewer needs to see why a report was ranked highly: the drug, the event term, the patient factors and similar historical reports. Without that, the queue will not be trusted.

**Treat governance as part of the design.** Data access, ethics approval, retention, access control and terminology licences need to be settled before modelling. MedDRA and WHO Drug are licensed terminologies, and VigiBase extracts require permission.

## Project plan

The checkboxes record planned work. Only the literature review is complete; nothing below should be read as implemented functionality.

### Stage 1: Scope, governance and literature

- [x] Review the Zimbabwean pharmacovigilance and machine learning literature
- [ ] Confirm the triage decision and the staff who will act on it
- [ ] Agree data access with MCAZ for VigiBase or e-PV extracts
- [ ] Document ethics, data protection, retention and access controls
- [ ] Write a protocol with the label definition, baselines and evaluation plan

### Stage 2: Data audit and provenance

- [ ] Characterise available fields, completeness and timeliness
- [ ] Quantify duplicate reports, revisions and late entries
- [ ] Map drug names to WHO Drug or ATC and events to MedDRA, subject to licence terms
- [ ] Describe therapeutic skew and reporting sources
- [ ] Produce a data dictionary and a reproducible extraction script

### Stage 3: Baselines and reference pipeline

- [ ] Implement disproportionality analysis with confidence intervals and thresholds
- [ ] Build a tabular baseline using logistic regression and gradient boosting
- [ ] Establish temporal and facility-based train, validation and test splits
- [ ] Fix the evaluation protocol and metrics before model development
- [ ] Document negative results and failed preprocessing attempts

### Stage 4: Representation and modelling

- [ ] Build text features for case narratives and free-text event descriptions
- [ ] Add structured features: age, sex, dose, concomitant medicines and comorbidities
- [ ] Represent drugs through ATC codes and events through the MedDRA hierarchy
- [ ] Handle missingness explicitly rather than imputing silently
- [ ] Compare against the Stage 3 baselines with ablation studies

### Stage 5: Evaluation and error analysis

- [ ] Report discrimination, calibration and recall at realistic review budgets
- [ ] Analyse performance by age, sex, HIV status, facility level and report source
- [ ] Inspect false negatives and the reports the model would have deprioritised
- [ ] Estimate alert burden per reviewer per week
- [ ] Assess whether performance holds under temporal and facility holdouts

### Stage 6: Prototype

- [ ] Build a review queue with ranked reports and explanations
- [ ] Show similar historical reports for each candidate
- [ ] Log reviewer decisions for future evaluation
- [ ] Implement access control and an audit trail
- [ ] Test with a small group of pharmacovigilance staff

### Stage 7: Prospective validation and reporting

- [ ] Run in silent mode alongside existing review
- [ ] Compare model rankings with reviewer assessments
- [ ] Report limitations, failure modes and any negative findings
- [ ] Publish the protocol and code where permissions allow
- [ ] Decide whether the approach merits wider deployment

## Limitations and risks

The most serious limitation is the label. If "actionable" is defined by retrospective reviewer judgement, the model learns to reproduce past priorities, including their biases. If it is defined by seriousness alone, it may mostly learn which drugs are already known to be hazardous. Neither is useless, but neither is the same as detecting new signals.

Reporting bias is the second. Spontaneous reports are generated by awareness, media attention and regulatory action, not only by underlying risk. A model trained on them can amplify existing attention patterns. Temporal validation helps but does not remove the problem.

Data volume is the third. Zimbabwe's annual report counts are small relative to the feature space that text and multimodal models can absorb. Regularisation, simpler models and careful feature selection are likely to matter more than architecture.

External validity is the fourth. A model trained on reports from a few large facilities may not generalise to district hospitals or to medicines that are rarely reported. Facility holdouts and subgroup analysis are necessary, not optional.

Finally, the project's value depends on workflow. A ranked list that adds work without reducing review time will be abandoned. The prototype stage therefore needs to measure reviewer time and agreement, not just model metrics.

## Open questions

- Which outcome can be labelled reliably from existing records, and at what cost?
- Can facility-level data be linked without creating re-identification risk?
- Does a model add value over disproportionality analysis at the report volumes Zimbabwe actually receives?
- How should vaccine and AEFI reports be handled, given that they are a substantial share of the data?
- What evidence would convince MCAZ and facility staff that the system is safe to use alongside existing review?

## References

- Masuka, J. T., & Khoza, S. (2020). An analysis of the trends, characteristics, scope, and performance of the Zimbabwean pharmacovigilance reporting scheme. *Pharmacology Research & Perspectives*, 8(5), e00657. https://doi.org/10.1002/prp2.657
- Mugauri, H., Tshimanga, M., Mugurungi, O., Juru, T., Gombe, N., & Shambira, G. (2018). Antiretroviral adverse drug reactions pharmacovigilance in Harare City, Zimbabwe, 2017. *PLOS ONE*, 13(12), e0200459. https://doi.org/10.1371/journal.pone.0200459
- Muringazuva, C., Chirundu, D., Mungati, M., Shambira, G., Gombe, N., Bangure, D., Juru, T., & Tshimanga, M. (2017). Evaluation of the adverse drug reaction surveillance system Kadoma City, Zimbabwe 2015. *Pan African Medical Journal*, 27, 55. https://doi.org/10.11604/pamj.2017.27.55.11090
- Khoza, S., Madungwe, I., Nyambayo, P., Mthethwa, J., & Chikuni, O. (2004). Adverse drug reactions reporting at a referral hospital in Zimbabwe. *Central African Journal of Medicine*, 50(11-12), 104-107.
- Nyambayo, P. P. M., Gold, M. S., Mehta, U. C., Clarke, S., Manyevere, R., Chirinda, L., Zifamba, E. N., & Nyamandi, T. (2023). Efficacy and feasibility of SMS m-Health for the detection of adverse events following immunisation (AEFIs) in resource-limited setting: The Zimbabwe stimulated telephone assisted rapid safety surveillance (Zm-STARSS) randomised control trial. *Vaccine*, 41(45), 6700-6709. https://doi.org/10.1016/j.vaccine.2023.09.037
- Nyambayo, P. P. M., Manyevere, R., Chirinda, L., Marekera, S. F., Nyamandi, T., Chaitezvi, R. P., Rukwata, R. T., Mehta, U., & Gold, M. S. (2026). Zimbabwe adverse events following immunisation surveillance system: A descriptive study with COVID-19 vaccine safety updates. *Southern African Journal of Infectious Diseases*, 41(1), 785. https://doi.org/10.4102/sajid.v41i1.785
- Medicines Control Authority of Zimbabwe. (2024). *External electronic pharmacovigilance (e-PV) user manual* (PVM-04 Rev 1). https://portal.mcaz.co.zw/wp-content/uploads/2025/07/PVM-04-Rev-1_October-2024-External-e-PV-User-Manual.pdf
- Hazell, L., & Shakir, S. A. W. (2006). Under-reporting of adverse drug reactions: A systematic review. *Drug Safety*, 29(5), 385-396. https://doi.org/10.2165/00002018-200629050-00003
- Montastruc, J. L., Sommet, A., Bagheri, H., & Lapeyre-Mestre, M. (2011). Benefits and strengths of the disproportionality analysis for identification of adverse drug reactions in a pharmacovigilance database. *British Journal of Clinical Pharmacology*, 72(6), 905-908. https://doi.org/10.1111/j.1365-2125.2011.04037.x
- Kim, H. R., Sung, M., Park, J. A., Jeong, K., Kim, H. H., Lee, S., & Park, Y. R. (2022). Analyzing adverse drug reaction using statistical and machine learning methods: A systematic review. *Medicine*, 101(25), e29387. https://doi.org/10.1097/MD.0000000000029387
- Pilipiec, P., Liwicki, M., & Bota, A. (2022). Using machine learning for pharmacovigilance: A systematic review. *Pharmaceutics*, 14(2), 266. https://doi.org/10.3390/pharmaceutics14020266
- Hu, Q., Li, J., Li, X., Zou, D., Xu, T., & He, Z. (2024). Machine learning to predict adverse drug events based on electronic health records: A systematic review and meta-analysis. *Journal of International Medical Research*, 52(12), 3000605241302304. https://doi.org/10.1177/03000605241302304
- Chalabianloo, N., Ahmadi, F., Omrani, M. A., Abdullah, S. S., Rostamzadeh, N., Jafari, A., Izzedin, L., Sedig, K., & Muanda, F. T. (2026). Machine learning methods for predicting adverse drug events: A systematic review. *British Journal of Clinical Pharmacology*, 92(2), 422-444. https://doi.org/10.1002/bcp.70377
- Warner, J., Prada Jardim, A., & Albera, C. (2025). Artificial intelligence: Applications in pharmacovigilance signal management. *Pharmaceutical Medicine*, 39(3), 183-198. https://doi.org/10.1007/s40290-025-00561-2
