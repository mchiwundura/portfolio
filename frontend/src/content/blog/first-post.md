---
title: 'What Makes Clinical AI Harder Than Other AI Problems'
description: 'Why clinical machine learning is harder than other AI problems: contested labels, biased records, weak evaluation, and the institutional realities of care.'
pubDate: 'Feb 08 2026'
heroImage: '../../assets/blog-placeholder-3.jpg'
---

## Introduction

One of the more consequential results in modern machine learning was also one of the least clinically relevant. In 2012, a convolutional neural network called AlexNet reduced the top-5 error rate on the ImageNet classification benchmark from 26.2 per cent to 15.3 per cent. The margin mattered less than the method. It suggested that a general-purpose learner, given enough labelled examples, could outperform hand-engineered features across a wide range of perceptual tasks.

Medicine noticed. If a network could learn the difference between a cat and a dog from a million labelled photographs, the reasoning went, it could learn the difference between a pneumothorax and a normal chest radiograph from a few hundred thousand labelled images. Radiology became the test case, and for a while the results looked extraordinary. Within a few years, deep learning systems were reported to match or exceed clinicians on tasks ranging from diabetic retinopathy grading to skin lesion classification. Review articles described a convergence of human and artificial intelligence that would change how medicine was practised, and the framing was persuasive because the early results were genuinely impressive.

Those results were often real in the narrow sense that the model performed well on the dataset it was given. What proved harder was everything else. The number of AI-enabled medical devices authorised by regulators has grown quickly, and the largest share remains in radiology, but the number that have been shown in prospective studies to improve what happens to a patient is much smaller. The gap between those two numbers is the subject of this article.

It is tempting to explain the gap in terms of maturity: the field is young, the evidence will accumulate, and the tools will improve. That explanation is partly true and mostly unhelpful, because it implies that the missing ingredient is time. The more accurate explanation is that medicine is an unusually poor substrate for the assumptions that supervised learning depends on.

In most AI problems, the world supplies a label. A photograph is a cat or it is not; a word is the next word or it is not; a transaction is fraudulent or it is not. In clinical AI, the label is an argument. It is a judgement made by a clinician, recorded in a system designed for billing, applied to a patient whose outcome depends on what the institution did next. The model learns from that argument, and then it is asked to act on patients who have not yet been argued about.

The argument that follows is organised around four sources of difficulty: the target, the data, the evaluation, and the institution. Each is harder in medicine than in the domains where machine learning matured. Each is systematically underweighted in the way clinical AI is discussed, and each explains part of the gap between an impressive result and a useful tool.

None of this means clinical AI is futile. It means the difficulty is located somewhere other than where most of the engineering effort goes.

## Part One: The Target Is Contested

#### 1.1 What Is the Model Actually Predicting?

Consider sepsis. In 2016, the Sepsis-3 consensus defined it as life-threatening organ dysfunction caused by a dysregulated host response to infection, and operationalised organ dysfunction as an acute increase in the Sequential Organ Failure Assessment score of two points or more. The definition was a genuine achievement: it replaced a vague clinical impression with a reproducible operational criterion. It is also, for machine learning purposes, an argument rather than a measurement. No laboratory test returns a verdict of sepsis.

Consequently, sepsis prediction models are trained against proxies. Some use diagnostic codes. Some use the combination of blood cultures and antibiotics. Some use a composite of surveillance criteria and administrative codes. Each proxy defines a slightly different disease. A model trained to predict the ordering of blood cultures has learned something about clinician suspicion, which is not the same thing as infection. A model trained to predict diagnostic codes has learned something about documentation and reimbursement. Neither has learned the biology of a dysregulated host response, and neither is necessarily interchangeable with the other.

This is not pedantry about definitions. The choice of target determines what the model is for. A tool that predicts a documentation event is an audit tool. A tool that predicts physiological deterioration is a triage tool. A tool that predicts the administration of antibiotics is a process measure. All three can be useful. They cannot be substituted for one another, and a paper that reports one while implying another is not reporting a clinical result.

Readmission is another instructive case, because it looks like a clean outcome and is not. A readmission is partly a clinical event and partly an administrative one. It depends on housing, social support, transport, discharge timing, and whether the hospital is financially penalised for it. A model trained to predict readmission is therefore learning a mixture of pathophysiology and social circumstance, with a strong dose of local discharge policy. If the model is then used to target additional support at high-risk patients, it may work. If it is used as a measure of hospital quality, it is measuring something else entirely.

#### 1.2 The Label Is a Decision, Not an Observation

Even when the target is well defined, the label often records a decision rather than a fact about the patient.

Take the prediction of unplanned intensive care transfer. Whether a patient is transferred depends on the patient, but also on whether a bed exists, whether the ward is appropriately staffed, whether the consultant on call is aggressive about escalation, and whether the family can afford the consequences. The model learns the threshold at which clinicians in that hospital, at that time, decided to transfer. That threshold is a policy. It is a reasonable policy, refined over years by people who understand the constraints. But it is not a property of the patient, and a model that reproduces it will look accurate precisely because the policy is consistent.

The same problem appears in less obvious places. Laboratory tests are not ordered at random. A lactate measurement happens because someone was worried about perfusion. A blood gas happens because someone was worried about oxygenation. The absence of a test is not evidence of the absence of disease; it is often evidence that no one was worried, or that the test was unavailable, or that the patient was stable enough to wait until morning. Agniel and colleagues demonstrated how the processes that generate electronic health record data, including who gets tested and when, can create or distort associations between variables and outcomes. The data are a record of care. They are not a record of physiology.

There is a further complication that is easy to miss. A deployed model changes the world it is predicting. Suppose a model flags a patient as likely to deteriorate, and the clinical team, alerted, intervenes. The deterioration does not occur. The next iteration of the training data records a patient who did not deteriorate. The model is now being trained on a world it has already altered, and the association it learned has been partly destroyed by its own success.

#### 1.3 Ground Truth Disagrees With Itself

The phrase ground truth comes from the physical sciences, where it means a measurement against which instruments can be calibrated. In medicine it usually means a committee.

Expert labels are opinions with error bars. Radiologists disagree with each other on a meaningful fraction of studies, and the same radiologist may disagree with an earlier reading of the same study. Pathology, often treated as the arbiter, has its own observer variability. Where a consensus panel is used, the label is the panel's opinion, which is more reliable than any single reader's and still not the truth. A model trained to match one expert may be learning that expert's idiosyncrasies. A model trained to match a panel may be learning the panel's compromises.

There is also a selection problem. Consider a model trained to detect a condition that can only be confirmed by biopsy. The label exists only for patients who had a biopsy, and patients who had a biopsy are not a random sample of patients with the condition. The model learns from a selected population and is then applied to an unselected one. This is verification bias, and it is common wherever the reference standard is invasive, expensive, or clinically discretionary.

For many clinical questions there is no biopsy, no definitive test, and no adjudication that settles the matter. There is only a decision that a group of trained people was willing to defend.

This is the first reason clinical AI is harder than other AI problems. In benchmark tasks, the target is given. In medicine, the target must be constructed, defended, and then checked against the decision the model is meant to support. Most of that work happens before any model is trained, and it is rarely reported.

## Part Two: The Data Are a Record of Care, Not of Biology

#### 2.1 The Electronic Record Is Administrative Exhaust

The electronic health record is not a research database that happens to be used for care. It is an administrative and legal instrument that happens to contain clinical information.

Its structure reflects billing requirements, legal defensibility, operational coordination, and regulatory reporting. Diagnoses are coded according to classification systems designed for reimbursement and statistics, not for phenotyping. When a new code is introduced, the recorded incidence of the corresponding condition changes without any change in the underlying disease. When a payment rule changes, documentation patterns change with it.

Documentation intensity also varies by clinician, by ward, and by time of day. Two patients with identical physiology can generate very different volumes of data depending on where they are cared for and who is caring for them. A model trained on the richer record may learn the habits of the people who wrote it.

Timestamps, which sequence models depend on, are less reliable than they appear. Charting happens after the fact. A value entered at 14:00 may have been measured at 13:40 or recalled from earlier. Medication administration is recorded when it is scanned, and the scan can precede or follow the act. None of this is a scandal; it is what happens when busy people use a system under time pressure. But a model that treats the timestamp as the moment of measurement is reasoning about a fiction.

Most consequentially, the frequency of observation is itself a signal. Vital signs are recorded more often for sicker patients. That is clinically sensible and statistically treacherous: the frequency of measurement is a proxy for severity, and a model can learn the proxy instead of the physiology. It will perform well, because the proxy is genuinely predictive. It will also fail in any setting where monitoring practice differs.

#### 2.2 Shortcuts Are Everywhere, and They Travel Badly

Zech and colleagues trained pneumonia detection models on chest radiographs from several institutions and found that performance fell sharply when the models were applied to data from a hospital they had not seen. The models had learned institution-specific features: equipment, acquisition protocols, and possibly markers embedded in the image. They had learned to recognise the hospital, and the hospital was correlated with the diagnosis.

This is shortcut learning, and clinical data are unusually rich in shortcuts because the process that generates the data is itself informative. The scanner, the ward, the ordering physician, the referral pathway, and the time of year all leave traces that correlate with the label. A model will use them if they are easier to learn than the underlying signal, which they usually are.

The consequence is that clinical models are often less portable than their reported performance suggests. Finlayson and colleagues describe dataset shift as a routine feature of clinical deployment rather than an edge case. Populations change, referral patterns change, coding rules change, imaging equipment is replaced, treatment guidelines are revised, and occasionally a new disease arrives. A model trained on last year's data is, in a precise sense, a model of last year's hospital.

The COVID-19 pandemic provided an involuntary natural experiment. Models trained on pre-pandemic chest imaging encountered a different disease mix, different acquisition protocols, and different patient populations within months. Some degraded quietly. Others were never monitored closely enough to notice.

#### 2.3 Missing Data Is a Signal, and Often a Misleading One

Missing data in medicine is rarely missing at random. It is missing because a test was not indicated, because the equipment was broken, because the patient declined, because the result was normal and therefore not repeated, or because the patient never reached care at all.

Each of these mechanisms carries information, and each distorts a model differently. A model trained in a well-equipped hospital learns to expect a full panel of laboratory results. Deployed in a clinic without a laboratory, it receives imputed values, or zeros, and returns a confident answer built on absent evidence. This is the most common way in which a model that performed well in development fails in practice: not with an error message, but with a plausible output.

There is a deeper version of the problem. The patients who are missing from a health system's data are often those who never reached it, and they are frequently the people the system most needs to serve. A model trained on the patients who did reach care has learned about a selected population, and the selection is not random with respect to need.

#### 2.4 Representation Is Not a Diversity Exercise

In 2019, Obermeyer and colleagues examined a widely used commercial algorithm that identified patients for extra care management. The algorithm predicted health care costs and used them as a proxy for clinical need. Because less money had historically been spent on Black patients at equivalent levels of illness, the proxy systematically understated their need. At a given risk score, Black patients were substantially sicker than White patients. Correcting the target would have raised the share of Black patients identified for additional care from 17.7 per cent to 46.5 per cent.

The lesson is not that the developers were malicious. It is that a proxy can encode a history of unequal access while appearing clinically neutral, and that the resulting bias can be invisible in aggregate accuracy metrics. The model was accurate at predicting cost. It was wrong about need.

Representation problems also operate at the level of institutions and countries. Much of the published clinical machine learning literature is trained and tested on a small number of publicly available intensive care databases, the best known of which comes from a single academic medical centre in Boston. A model developed there has learned the practice patterns, coding conventions, and patient mix of that institution. It may generalise. Nobody knows until someone checks.

Internationally, the problem is larger. Populations differ in disease prevalence, comorbidity, pharmacogenomics, treatment protocols, and the depth of their records. In settings where electronic records are used mainly for registration, the clinical detail required to train a model may not exist at all. Where it does exist, it is often held by institutions that have neither the capacity nor the mandate to curate it for research. In genomics, Martin and colleagues showed that polygenic risk scores derived largely from European-ancestry cohorts perform less well in other populations, a problem that follows directly from who was studied.

An unvalidated model is not neutral. Deploying one is an experiment conducted on patients, usually without their knowledge and without the protections that a formal trial would require.

#### 2.5 Privacy and the Social Licence

Clinical AI depends on data that people did not donate for research. They generated it while receiving care, often at a moment of vulnerability, and often without reading the notice on the clipboard.

In 2017, the UK Information Commissioner's Office concluded that the Royal Free London NHS Foundation Trust had not complied with data protection law when it supplied patient records to DeepMind for a kidney injury alerting application. The project had clinical support and a plausible public benefit. It failed at the level of legal basis and public legitimacy. The details matter less than the precedent: technical good intentions are not a substitute for consent, transparency, and a lawful basis for processing.

De-identification is also weaker than it is often assumed to be. Removing names and hospital numbers is not anonymisation. Dates, postcodes, rare diagnoses, and unusual combinations of events can re-identify individuals, particularly in small populations. The smaller the dataset, the easier the re-identification, which is an uncomfortable property for research on rare diseases.

Health data are also distributed unevenly in power. The populations least represented in training data are often the least able to negotiate the terms on which their data are used. A governance model that treats them as a data source rather than a constituency will produce both ethical and practical problems, because the same populations are the ones whose trust the system needs in order to be deployed at all.

## Part Three: Accuracy Is Not Evidence

#### 3.1 Why a Good Model Can Be a Useless Test

Suppose a model screens for a condition with a prevalence of 1 per cent in the population being screened. It has 95 per cent sensitivity and 95 per cent specificity, numbers that a paper would describe as excellent. Apply it to 10,000 people.

| | Condition present | Condition absent | Total |
| --- | --- | --- | --- |
| Positive | 95 | 495 | 590 |
| Negative | 5 | 9,405 | 9,410 |
| Total | 100 | 9,900 | 10,000 |

The positive predictive value is 95 divided by 590, or about 16 per cent. Four out of five positive flags are false. If the confirmatory test is invasive, expensive, or unavailable, the model's clinical value is questionable regardless of its area under the curve. The same model used in a specialist clinic where prevalence is 30 per cent would have a positive predictive value of about 89 per cent. Same model, same accuracy, radically different usefulness.

This is not a trick of arithmetic. It is the reason screening programmes are judged on positive predictive value and on the number of people who must be investigated to find one case, not on sensitivity and specificity alone. It is also why a model can be genuinely impressive as a discriminator and still be a poor screening tool. The relevant question is always: in this population, at this threshold, what happens to the people who are flagged?

Discrimination and calibration are also different properties, and the distinction matters. A model can rank patients correctly and still assign probabilities that are systematically wrong. If the output is used as a probability, for example to decide whether to admit or to discharge, miscalibration is a clinical error even when the ranking is perfect. Decision curve analysis, developed by Vickers and Elkin, addresses a related question: whether using the model to guide a decision produces more benefit than treating everyone, or treating no one, at a given threshold of risk. It is a more demanding test than an ROC curve, and it is used far less often.

#### 3.2 From Retrospective to Prospective

The clinical AI literature has a validation problem that is not primarily about statistical technique. It is about what counts as evidence.

Nagendran and colleagues reviewed studies comparing deep learning with clinicians and found that most were retrospective, conducted at single sites, and reported claims that ran ahead of their designs. Prospective studies were scarce; randomised trials were scarcer. A separate systematic review of deep learning for medical imaging found that few studies were at low risk of bias and that most comparisons with clinicians were not made on the same test data, which makes the comparison difficult to interpret.

The most instructive case is not a study but a deployment. The Epic Sepsis Model is a proprietary sepsis prediction tool that has been implemented at hundreds of hospitals in the United States. It was externally validated in 2021 by Wong and colleagues, using 38,455 hospitalisations at a single academic health system. The model's area under the curve was 0.63, substantially below the performance reported by the vendor. It failed to identify 1,709 of 2,552 patients who developed sepsis, about 67 per cent, while generating alerts for 18 per cent of all hospitalisations. In a health system where clinicians were already identifying sepsis reasonably well, the model added little and consumed a great deal of attention.

The failure was not that the model was badly built. It was that its performance had never been independently established in the population where it was being used, and that widespread adoption had preceded external validation. That sequence is common, and it is backwards.

The COVID-19 imaging literature shows the same pattern at its most extreme. Roberts and colleagues screened 2,212 studies describing machine learning models for diagnosis or prognosis from chest radiographs and CT scans, included 415 after initial screening, and concluded that none of the models they assessed were of potential clinical use, because of methodological flaws or underlying biases. This is an unusually stark finding. The direction, however, is consistent with the wider literature.

Reporting guidelines such as TRIPOD+AI and risk-of-bias tools such as PROBAST have improved how studies are described. They cannot manufacture evidence that was never collected.

#### 3.3 The Human in the Loop Is Not a Safety Net

The phrase human in the loop does a great deal of work in AI governance. It implies a person who reviews the model's output, recognises when it is wrong, and intervenes. In practice, the loop is often a person clicking approve on a screen they do not have time to interrogate.

The evidence on automation bias in medicine is old and consistent: people accept computer-generated advice more readily than they should, particularly under time pressure, and particularly when the advice is plausible. Gaube and colleagues showed physicians chest radiographs along with diagnostic advice, some of it deliberately wrong. Accuracy fell when the advice was wrong, whether or not it was labelled as coming from an AI system. Less experienced physicians did not discount the AI advice. The label changed how the advice was rated; it did not reliably change how it was used.

There is a second-order effect that is harder to measure. Repeated reliance on a pre-reader can erode the skill that is supposed to provide the safeguard. A radiologist who spends the day reviewing a queue of AI-prioritised studies is not practising the same skill as a radiologist who works through the list unaided. This is a plausible concern rather than a well-quantified one, but it is not speculative enough to ignore.

Alert fatigue compounds both problems. An alert that fires on 18 per cent of admissions is not an alert; it is background noise. Clinicians learn to dismiss it, and the dismissal is rational. The cost is that the true positive and the false positive are dismissed together, and no one can tell them apart without doing the work the alert was supposed to save.

Human oversight is a meaningful safeguard only when the human has the time, the information, and the authority to disagree. In many deployments, at least one of those is missing.

#### 3.4 The Counterfactual Is Expensive

The only way to know whether a tool improves outcomes is to compare what happened with what would have happened without it. That comparison requires a counterfactual, and in medicine the credible ones are expensive.

Randomised trials of AI tools are difficult for familiar reasons. The intervention is often a piece of information rather than a drug, so blinding is awkward. The standard of care changes during the trial. Clinicians may decline to enrol patients in a trial that withholds a tool they believe in. And the outcome that matters, such as mortality or serious complications, is rare enough that the trial must be large, which makes it slow and costly.

The result is a reliance on surrogate endpoints: discrimination, sensitivity at a fixed operating point, agreement with expert readers. Each measures something real. None measures benefit. A model that agrees with experts 90 per cent of the time has demonstrated that it can reproduce expert judgement, not that expert judgement plus the model is better than expert judgement alone.

Practical alternatives exist. Silent-mode deployment, in which the model runs and its predictions are recorded but not shown, allows a prospective comparison with usual care. Stepped-wedge designs, in which the tool is introduced sequentially across sites, can approximate randomisation without withholding it from anyone permanently. These designs are imperfect. They are also far better than the alternative, which is to deploy and hope.

## Part Four: The Clinic Is a System, Not a Test Set

#### 4.1 Workflow Is the Product

One of the most instructive published accounts of a clinical AI deployment failing for non-technical reasons comes from Beede and colleagues, who evaluated a deep learning system for diabetic retinopathy screening in clinics in Thailand. The model performed well in the laboratory. In the clinics, the constraints were different. Image quality varied, the camera and the connection were not always available, and the referral pathway could not absorb the volume of patients the model identified. Nurses developed workarounds to keep the service running. The model's discrimination was not the binding constraint.

This is the most common failure mode in clinical AI, and it is rarely visible in a paper. Clinical work is already saturated: physicians spend nearly two additional hours on electronic record and desk work for every hour of direct clinical face time. A model that adds thirty seconds to a consultation, requires a separate login, or produces an alert at a moment when nothing can be done will fail regardless of its area under the curve. A model that identifies patients who cannot be treated in the setting where it runs has produced a list, not a benefit.

The interface matters as much as the model. A probability without a recommended action transfers the interpretive burden to the clinician. A recommendation without an explanation transfers the liability. An explanation consisting of a heat map transfers neither, and may create false confidence. Ghassemi and colleagues argue that post-hoc explanations of the kind commonly attached to clinical models can be unstable, can misrepresent the model's reasoning, and are not a substitute for validation. Explainability has become a way of satisfying a requirement without answering the question the requirement was meant to ask.

#### 4.2 Incentives Decide What Gets Deployed

Hospitals are not neutral adopters. They are organisations with budgets, obligations, and competing priorities, and the economics of a tool determine whether it survives contact with them.

Consider a model that predicts avoidable admissions. In a fee-for-service system, preventing an admission reduces occupancy and revenue. The same model is valuable under capitation, where the institution bears the cost of care. The model is identical; the incentive is not. A vendor that ignores this will build a good product and fail to sell it, or sell it to an institution that has no reason to use it well.

Procurement adds another layer. Cycles are long, specifications are written before anyone knows what the model can do, and contracts rarely account for maintenance. Pilots are funded by grants and innovation budgets; operations are funded by nobody. The result is a familiar pattern: a successful pilot that cannot be sustained, not because it failed, but because it worked and no one had budgeted for it to keep working.

#### 4.3 Regulation Was Designed for Something Else

Medical device regulation was built around products that do not change after approval. A model that is retrained on new data is, in a meaningful sense, a different product, and the regulatory system has had to invent ways of accommodating that.

The FDA's approach to AI-enabled software as a medical device has moved towards documenting intended use, managing change through predetermined change control plans, and applying good machine learning practice principles developed with international partners. The EU AI Act, which entered into force in 2024, classifies AI systems that are medical devices requiring third-party conformity assessment as high risk, adding obligations around data governance, documentation, human oversight, and post-market monitoring. The World Health Organization has published guidance on the ethics and governance of AI for health, and on regulatory considerations for its implementation.

These frameworks are converging on similar requirements: a defined intended use, representative data, prospective evidence, human oversight, and ongoing monitoring. The gap is less in the principles than in the capacity to enforce them. A regulator that cannot inspect a factory is unlikely to audit a model's training data, and many national regulators have neither the staff nor the statutory tools to do so.

Liability remains unsettled. If a model recommends an action and a clinician accepts it, who is responsible? The law in most jurisdictions places the duty on the clinician, which is defensible and produces a predictable response: clinicians ignore the model, or document that they did. Product liability for software that makes clinical recommendations is still developing in most legal systems, and the uncertainty is itself a barrier to adoption.

#### 4.4 Models Decay

Machine learning systems accumulate a particular kind of technical debt. Sculley and colleagues described it in 2015: data dependencies, pipeline glue, configuration, and the entanglement of the model with the rest of the system. In medicine, add clinical guidelines that change, coding systems that are revised, equipment that is replaced, and staff who leave.

Post-market surveillance for clinical AI is thin. There is no equivalent of a phase IV trial for a model. Performance monitoring requires outcome data, which arrives late and incompletely. Retraining requires revalidation, which costs money and may require regulatory notification. Many deployed models are monitored for usage rather than accuracy, because usage is easy to measure and accuracy is not.

A model without a maintenance budget has a scheduled failure date. It may be a quiet failure, in which the model continues to produce outputs that no one checks, or a loud one, in which a shift in the population produces a visible error. Both are avoidable, and neither is avoided by good intentions at launch.

#### 4.5 Trust Is Infrastructure

Patients are asked to accept that a model informed their care, often without being told. Clinicians are asked to accept a tool whose reasoning they cannot inspect. Institutions are asked to accept liability for a system they did not build and cannot fully audit. Each of these is a trust relationship, and trust is not a communications problem.

Trust is asymmetric. It accumulates slowly, through consistent performance and honest reporting, and it is lost quickly. The Royal Free case is instructive not because the technology was bad but because the process by which it was introduced damaged a relationship that the technology depended on.

Public scepticism about AI in medicine is often treated as a deficit to be corrected with education. It is better understood as a reasonable response to a system that has not yet earned confidence, and to a series of claims that have not always survived contact with evidence. The way to earn confidence is not to explain the technology more persuasively. It is to validate it more rigorously and to be candid about what it cannot do.

## Part Five: What Follows for People Building This

The four problems above are not a reason to stop. They are a specification. If the difficulty is in the target, the data, the evaluation, and the institution, then that is where the work has to happen.

#### 5.1 Start From the Decision

A clinical model is a means to a decision, and the decision should be specified before the model is built. What action will change? Who takes it? When? What is the current process, and how well does it work? What is the cost of a false positive, in money, time, and harm? What is the cost of a false negative?

These questions sound like product management, and they are. They are also the difference between a model that is used and a model that is admired. If the honest answer is that the model will be one more input among many, with no defined action attached, the burden of proof is higher, not lower. A model that changes nothing has no clinical value, however good its metrics.

#### 5.2 Write the Evaluation Before the Model

Specify the target definition, the population, the comparator, the metric, the decision threshold, and the analysis plan before any data is touched. Register the plan if possible. Use TRIPOD+AI for reporting and PROBAST for risk of bias, not as paperwork but as a checklist of the ways the study could mislead.

Plan external validation at a site you do not control, with a population you did not select. A model validated only where it was developed has not been validated; it has been fitted. If external validation is impossible, say so, and describe the population in which the model should not be used.

Clinical evidence is not the only evidence that matters. Health technology assessment bodies increasingly ask for economic evidence, because a tool that improves outcomes at unsustainable cost will not be adopted. NICE's Evidence Standards Framework for digital health technologies is one example of how those expectations are being formalised.

#### 5.3 Treat Deployment as a Study

Silent-mode evaluation should precede live use wherever the clinical stakes justify it. Once live, define what will be monitored, what threshold constitutes a problem, and what will trigger a rollback. Monitor outcomes, not just usage. A model that is used constantly and changes nothing is a failure that looks like success.

Include the people who will use the tool in the design of the workflow around it, not just the interface. The most common reason a good model fails is that it was inserted into a process that could not accommodate it.

#### 5.4 Budget for Maintenance

Most of the engineering work in a clinical AI system is data engineering, and most of the cost over its lifetime is maintenance. Plan for retraining, revalidation, and the regulatory consequences of change. Assign ownership. A model with no owner will drift until someone notices, and the someone who notices is usually a clinician dealing with a wrong output.

In low-resource settings, this consideration is decisive. A system that requires a specialist team in another country to keep it running is not a sustainable system, however good its area under the curve. The same principle applies to connectivity, power, and spare parts: design for the environment that exists, not the one that would make the model easier to deploy.

#### 5.5 Build the Record First

In many health systems, the binding constraint is not modelling capacity. It is the absence of reliable, structured, longitudinal records. A model cannot be trained on data that was never captured, and it cannot be monitored with outcomes that are never recorded.

Investing in the record is slower and less visible than investing in the model. It is also more likely to matter. A well-curated registry with a simple risk score will often outperform a sophisticated model trained on fragmented data, and it will be easier to maintain, easier to audit, and easier to explain to the people whose care it informs.

#### 5.6 Be Explicit About Uncertainty

Report calibrated probabilities rather than bare classifications. State the population in which the model was validated, the outcomes it was validated against, and the conditions under which it should not be used. Model facts labels, proposed by Sendak and colleagues, are a practical template: intended use, validation data, performance, limitations, and the appropriate response to a positive or negative output.

Overclaiming is not only an ethical problem. It is the mechanism by which the field loses credibility, and credibility is the resource that clinical AI most needs.

## Conclusion

Clinical AI is harder than other AI problems for reasons that are mostly not about AI. The target is contested, because medicine rarely supplies a label that is not also a judgement. The data are a record of care, because the record was built for administration and the care was shaped by the record. The evaluation is expensive, because the counterfactual requires a trial and the trial is slow. The institution is decisive, because a model that does not fit a workflow, an incentive, or a regulatory framework will not be used, and an unused model has no effect.

This is why the field's most impressive results and its least impressive deployments can coexist without contradiction. A model that predicts well in a notebook and poorly in a ward has not solved a clinical problem. It has solved a data problem that happened to resemble one.

The useful question is therefore not whether a model can learn from medicine. It is what would have to change, in the record, in the workflow, in the evidence, and in the institution, for its prediction to change what happens to a patient. Most of the work is in that second question. Most of it is not machine learning.

## References

- Wong A, Otles E, Donnelly JP, et al. External validation of a widely implemented proprietary sepsis prediction model in hospitalized patients. *JAMA Internal Medicine*. 2021. [doi:10.1001/jamainternmed.2021.2626](https://doi.org/10.1001/jamainternmed.2021.2626)
- Obermeyer Z, Powers B, Vogeli C, Mullainathan S. Dissecting racial bias in an algorithm used to manage the health of populations. *Science*. 2019. [doi:10.1126/science.aax2342](https://doi.org/10.1126/science.aax2342)
- Zech JR, Badgeley MA, Liu M, Costa AB, Titano JJ, Oermann EK. Variable generalization performance of a deep learning model to detect pneumonia in chest radiographs. *PLOS Medicine*. 2018. [doi:10.1371/journal.pmed.1002683](https://doi.org/10.1371/journal.pmed.1002683)
- Finlayson SG, Subbaswamy A, Singh K, et al. The clinician and dataset shift in artificial intelligence. *New England Journal of Medicine*. 2021. [doi:10.1056/NEJMc2104626](https://doi.org/10.1056/NEJMc2104626)
- Agniel D, Kohane IS, Weber GM. Biases in electronic health record data due to processes within the healthcare system. *BMJ*. 2018. [doi:10.1136/bmj.k1479](https://doi.org/10.1136/bmj.k1479)
- Nagendran M, Chen Y, Lovejoy CA, et al. Artificial intelligence versus clinicians: systematic review of design, reporting standards, and claims of deep learning studies. *BMJ*. 2020. [doi:10.1136/bmj.m689](https://doi.org/10.1136/bmj.m689)
- Liu X, Faes L, Kale AU, et al. A comparison of deep learning performance against health-care professionals in detecting diseases from medical imaging: a systematic review and meta-analysis. *The Lancet Digital Health*. 2019. [doi:10.1016/S2589-7500(19)30123-2](https://doi.org/10.1016/S2589-7500(19)30123-2)
- Roberts M, Driggs D, Thorpe M, et al. Common pitfalls and recommendations for using machine learning to detect and prognosticate for COVID-19 using chest radiographs and CT scans. *Nature Machine Intelligence*. 2021.
- Gaube S, Suresh H, Raue M, et al. Do as AI say: susceptibility in deployment of clinical decision-aids. *npj Digital Medicine*. 2021. [doi:10.1038/s41746-021-00385-9](https://doi.org/10.1038/s41746-021-00385-9)
- Ghassemi M, Oakden-Rayner L, Beam AL. The false hope of current approaches to explainable artificial intelligence in health care. *The Lancet Digital Health*. 2021. [doi:10.1016/S2589-7500(21)00208-9](https://doi.org/10.1016/S2589-7500(21)00208-9)
- Beede E, Baylor E, Hersch F, et al. A human-centered evaluation of a deep learning system deployed in clinics for the detection of diabetic retinopathy. *Proceedings of the 2020 CHI Conference on Human Factors in Computing Systems*. 2020.
- Vickers AJ, Elkin EB. Decision curve analysis: a novel method for evaluating prediction models. *Medical Decision Making*. 2006. [doi:10.1177/0272989X06295361](https://doi.org/10.1177/0272989X06295361)
- Collins GS, Moons KGM, Dhiman P, et al. TRIPOD+AI statement: updated guidance for reporting clinical prediction models that use regression or machine learning methods. *BMJ*. 2024. [doi:10.1136/bmj-2023-078378](https://doi.org/10.1136/bmj-2023-078378)
- Wolff RF, Moons KGM, Riley RD, et al. PROBAST: a tool to assess the risk of bias and applicability of prediction model studies. *Annals of Internal Medicine*. 2019.
- Sendak MP, Gao M, Brajer N, Balu S. Presenting machine learning model information to clinical end users with model facts labels. *npj Digital Medicine*. 2020. [doi:10.1038/s41746-020-0253-3](https://doi.org/10.1038/s41746-020-0253-3)
- Topol EJ. High-performance medicine: the convergence of human and artificial intelligence. *Nature Medicine*. 2019. [doi:10.1038/s41591-018-0300-7](https://doi.org/10.1038/s41591-018-0300-7)
- Martin AR, Kanai M, Kamatani Y, Okada Y, Neale BM, Daly MJ. Clinical use of current polygenic risk scores may exacerbate health disparities. *Nature Genetics*. 2019. [doi:10.1038/s41588-019-0379-x](https://doi.org/10.1038/s41588-019-0379-x)
- Sculley D, Holt G, Golovin D, et al. Hidden technical debt in machine learning systems. *Advances in Neural Information Processing Systems*. 2015.
- Singer M, Deutschman CS, Seymour CW, et al. The Third International Consensus Definitions for Sepsis and Septic Shock (Sepsis-3). *JAMA*. 2016.
- Sinsky C, Colligan L, Li L, et al. Allocation of physician time in ambulatory practice: a time and motion study in 4 specialties. *Annals of Internal Medicine*. 2016.
- World Health Organization. *Ethics and governance of artificial intelligence for health: WHO guidance*. 2021. [who.int](https://www.who.int/publications/i/item/9789240029200)
- US Food and Drug Administration. [Artificial intelligence-enabled medical devices](https://www.fda.gov/medical-devices/software-medical-device-samd/artificial-intelligence-enabled-medical-devices).
- European Union. *Artificial Intelligence Act* (Regulation (EU) 2024/1689). 2024.
- National Institute for Health and Care Excellence. *Evidence Standards Framework for Digital Health Technologies*. 2022.
