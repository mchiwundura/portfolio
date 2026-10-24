
---
name: project-writing
description: Writes and refines scientific, biomedical, data science, computational, machine learning, mathematical modelling, and software project documentation, portfolio case studies, technical reports, and project proposals.
---

# Purpose

Help me communicate the substance of scientific and technical projects through clear, rigorous, and engaging project write-ups.

A project write-up should explain more than what was built. It should communicate the problem being addressed, why it matters, the reasoning behind the approach, the implementation or methodology, the evidence produced, the limitations, and the project's wider significance.

My projects may combine pharmaceutical science, pharmacology, biology, statistics, mathematical modelling, machine learning, data analysis, and software engineering.

Write for an audience that may include scientific researchers, pharmacists, data scientists, software engineers, technical recruiters, and other professionals who may understand only part of the project's subject matter.

The goal is to make the work understandable and technically credible without diluting its scientific or computational substance.

# 1. Core principles

Every project write-up should aim to communicate:

- The problem and why it deserves attention.
- The scientific, technical, or practical question being investigated.
- The reasoning behind the chosen approach.
- The methods, tools, data, and implementation involved.
- The results or outputs that are actually available.
- What those results demonstrate, and what they do not demonstrate.
- The limitations, assumptions, and unresolved questions.
- The project's value, relevance, and possible next steps.

Do not treat these as mandatory headings. They are the underlying questions the writing should answer, in an order appropriate to the project.

Prefer substantive explanations over lists of technologies, unsupported claims of innovation, or promotional language.

A well-written project description should help the reader understand both the work itself and the thinking behind it.

# 2. Establish the project's purpose

Begin by identifying the problem, question, or opportunity that motivates the project.

Explain:
- What is currently difficult, unknown, inefficient, or insufficiently understood.
- Who or what is affected by the problem.
- Why existing approaches may be inadequate, where that inadequacy is supported by evidence.
- What the project aims to investigate, estimate, predict, explain, or enable.
- What a useful outcome would look like.

Distinguish the broader problem from the specific objective of the project.

A project may contribute to understanding a problem without solving it completely. Describe the contribution at the appropriate scale.

Avoid vague openings such as "In today's rapidly evolving technological landscape" or generic claims that a project will revolutionise an entire field.

Where the motivation is not established by the supplied material, research it when requested or flag the missing context. Do not invent a compelling problem statement merely to make the project sound important.

# 3. Explain the approach and reasoning

Describe not only what methods were used, but why they were appropriate for the question.

Where relevant, explain:
- The conceptual framework or underlying hypothesis.
- The choice of data, experimental system, model, algorithm, or software architecture.
- Important methodological alternatives and trade-offs.
- Assumptions required by the approach.
- How the approach connects to the underlying scientific or technical problem.
- What would make the approach unsuitable or unreliable.

Translate specialised concepts into accessible language without removing the details that make the work technically meaningful.

Avoid turning the methodology into a sequence of unexplained implementation steps. Help the reader understand the relationship between the problem, the chosen method, and the expected output.

Do not present a standard library, algorithm, or tool as a novel contribution merely because it was used in the project.

When the project's main contribution is integration, implementation, adaptation, analysis, or reproducibility rather than a new method, describe that contribution accurately.

# 4. Scientific and biomedical projects

For projects involving biological systems, pharmacology, clinical data, biomedical research, or experimental science, preserve scientific precision.

Where relevant, explain:
- The biological or clinical context.
- The underlying mechanism, pathway, hypothesis, or research question.
- The experimental design or analytical framework.
- The variables, endpoints, outcomes, or measurements of interest.
- The controls, comparators, and assumptions.
- The relationship between the methods and the biological interpretation.
- The uncertainty and limitations of the evidence.

Distinguish carefully between:
- In vitro findings and in vivo evidence.
- Preclinical evidence and clinical evidence.
- Association and causation.
- Mechanistic hypotheses and demonstrated mechanisms.
- Statistical significance and practical or biological significance.
- Predictive performance and clinical usefulness.
- Model-based estimates and experimentally observed measurements.

Do not imply that a computational prediction establishes biological activity, that a simulated result demonstrates an experimental effect, or that a preliminary finding establishes clinical efficacy.

Use appropriate scientific terminology and preserve units, concentrations, experimental conditions, statistical quantities, and endpoint definitions when supplied.

Do not invent experimental results, sample sizes, statistical significance, validation experiments, or mechanistic explanations.

When evidence supports multiple interpretations, explain the plausible alternatives rather than presenting the most favourable interpretation as established fact.

# 5. Data science and machine learning projects

For computational projects, make the analytical workflow understandable and reproducible.

Where relevant, cover:

- The prediction, classification, estimation, clustering, simulation, or inference task.
- The data sources, provenance, coverage, and intended population.
- The target variable and how it is defined.
- Data cleaning, preprocessing, missingness, feature construction, and transformations.
- The baseline or reference approach.
- Model selection and the rationale for the chosen methods.
- Training, validation, and test design.
- Evaluation metrics and why they are appropriate.
- Results relative to a meaningful baseline.
- Error analysis, uncertainty, limitations, and possible sources of bias.
- Reproducibility, computational requirements, and future improvements.

Include only the sections that are relevant and supported by the project.

Explain the purpose of important analytical decisions rather than simply listing packages, algorithms, or metrics.

Where applicable, consider data leakage, class imbalance, overfitting, confounding, distribution shift, external validity, and the difference between retrospective performance and real-world performance.

Do not describe a model as accurate, robust, generalisable, or clinically useful without evidence appropriate to the claim.

Distinguish a working proof of concept from a validated model and a validated model from a deployed, operational system.

If a model has not yet been evaluated, describe its evaluation plan rather than inventing performance figures.

When reporting metrics, provide enough context for readers to interpret them. Explain the baseline, evaluation data, relevant uncertainty, and limitations where available.

# 6. Mathematical modelling and simulation

For projects involving mathematical models, dynamic systems, simulations, or parameter estimation, explain the model's conceptual and mathematical structure.

Where relevant, describe:
- The system being represented and the question the model addresses.
- The principal variables, parameters, and state variables.
- The governing equations or modelling assumptions.
- Parameter sources, estimation methods, and initial conditions.
- Boundary conditions and relevant constraints.
- Numerical methods and implementation choices.
- Calibration, validation, sensitivity analysis, and uncertainty analysis.
- The interpretation and limitations of simulated outputs.

Introduce equations when they clarify the model or demonstrate meaningful technical depth. Define symbols and explain what the equations mean in the context of the system.

Do not include mathematics merely to make the write-up appear more technical.

Distinguish between mathematical correctness, numerical stability, calibration against observed data, and validation against independent evidence.

A simulation is conditional on its assumptions and parameterisation. Explain what can reasonably be inferred from its outputs and what requires further validation.

# 7. Software and implementation

When a project includes a software application, analytical pipeline, API, dashboard, or other technical system, explain the implementation in relation to the problem it serves.

Where relevant, describe:
- The system's purpose and intended users.
- The main components and their responsibilities.
- The data flow from input to output.
- Important architectural and technology choices.
- Integration between scientific methods and software components.
- Error handling, input validation, testing, and reliability.
- Reproducibility, deployment, and operational constraints.
- The current implementation status.

Mention programming languages, frameworks, libraries, databases, and infrastructure when they help explain the work or establish relevant technical competence.

Do not substitute a technology stack for an explanation of the system.

When useful, include architecture diagrams, workflow diagrams, mathematical formulations, selected code examples, visualisations, or screenshots. Each should clarify a specific part of the project.

Describe what has actually been implemented. Do not present planned functionality, mock interfaces, or proposed architecture as completed features.

# 8. Data integrity, provenance, and reproducibility

Treat data provenance and reproducibility as important parts of technical credibility.

Where relevant, document:
- The origin and version of datasets.
- Relevant access dates, licences, and usage restrictions.
- Data selection and exclusion criteria.
- Important preprocessing decisions.
- Experimental or computational settings.
- Software versions, dependencies, and execution requirements.
- Random seeds and sources of stochastic variation where applicable.
- The steps required to reproduce the principal analysis or output.

Respect privacy, confidentiality, and applicable data-use restrictions. Do not disclose sensitive information simply to make a project appear reproducible.

When external datasets or APIs are involved, distinguish between publicly available data and data that is genuinely suitable for the project's intended use.

If exact reproducibility is not possible, explain why and identify what information is missing.

Never fabricate data provenance, repository contents, benchmark results, software versions, or evidence of reproducibility.

# 9. Results and interpretation

Give results enough context to be meaningful.

When results are available:
- State the important findings clearly.
- Explain what they mean in relation to the original objective.
- Compare them with relevant baselines, controls, or previous evidence where appropriate.
- Identify unexpected findings and meaningful trade-offs.
- Explain uncertainty and limitations.
- Distinguish measured results from their interpretation.
- Avoid reporting every available metric when only a few answer the main question.

When visualisations are included, explain the question each figure addresses and what the reader should learn from it. Do not overstate what a chart, metric, or visual pattern establishes.

If results are preliminary, incomplete, or exploratory, label them accordingly.

If a project is still being developed, write honestly about its current stage. Explain what has been established, what remains to be implemented, and what would constitute meaningful progress.

Never invent numerical results, performance improvements, successful experiments, completed milestones, or comparisons that have not been established.

# 10. Limitations and critical reflection

Discuss limitations as part of the intellectual substance of the project, not as an afterthought.

Depending on the work, these may include:
- Limited data availability or representativeness.
- Simplifying assumptions.
- Small sample sizes or limited experimental scope.
- Uncertain parameter estimates.
- Weak or incomplete validation.
- Computational constraints.
- Model interpretability.
- Bias, confounding, or distribution shift.
- Deployment, infrastructure, or maintenance constraints.
- Regulatory, ethical, or practical considerations.

Explain why important limitations matter and how they affect the conclusions that can reasonably be drawn.

Where appropriate, distinguish limitations that are inherent to the chosen approach from limitations that could be addressed through additional work.

Propose realistic next steps that follow from the findings or unresolved questions.

Avoid both extremes: portraying every limitation as fatal or dismissing substantial weaknesses with a generic statement that more research is needed.

# 11. Communicating technical depth to different audiences

Adapt the level of explanation to the intended reader without changing the underlying facts.

For scientific audiences, prioritise methodological precision, relevant literature, assumptions, evidence, and interpretation.

For software and data science audiences, make the implementation, analytical workflow, architecture, evaluation, and reproducibility visible.

For interdisciplinary audiences, establish the problem and conceptual context before introducing specialised terminology. Explain unfamiliar concepts when they first become important.

For employers and technical recruiters, make the individual's contribution, technical judgement, problem-solving process, and demonstrated capabilities clear.

For a general audience, preserve the substance while reducing unnecessary jargon.

When a project serves multiple audiences, explain the central problem and contribution in accessible language, then provide technical depth in the relevant sections.

Do not assume that an audience unfamiliar with one discipline is incapable of understanding it.

# 12. Project status and ownership

Represent my role and the project's maturity accurately.

Distinguish between:
- An idea or proposed project.
- A research or design plan.
- An early proof of concept.
- A partially implemented project.
- A completed analysis or experimental study.
- A validated system or finding.
- A deployed or operational product.

These categories are not necessarily sequential, and different parts of a project may have different levels of maturity.

Use accurate language to describe my contribution. Distinguish my work from the contributions of collaborators, existing libraries, source datasets, and established methods.

Do not claim that I designed, implemented, validated, deployed, or independently completed work unless the supplied information supports that statement.

Do not invent personal motivation, project history, collaborators, user adoption, impact, or professional experience.

When the project is incomplete, present its current state constructively without disguising unfinished work.

# 13. Choosing the appropriate format

Adapt the write-up to the intended purpose rather than forcing every project into one structure.

**Portfolio case study**

Emphasise the problem, my contribution, technical and scientific reasoning, implementation, evidence, limitations, and lessons learned.

**Technical README**

Prioritise a concise project overview, scope, status, requirements, installation or execution instructions, usage, data and methods, reproducibility, and known limitations.

**Scientific or technical report**

Prioritise the research question, background, methodology, results, interpretation, limitations, and references.

**Project article**

Use a narrative that explains the problem, the reasoning behind the approach, the development process, the important findings, and the broader implications. Load the `blog-writing` skill when a substantial public-facing article is requested.

**Project proposal**

Explain the problem, rationale, objectives, proposed methods, data or resources required, evaluation strategy, feasibility, risks, and anticipated contribution. Clearly distinguish planned work from completed work.

**Project summary**

Provide a concise explanation of the problem, approach, current status, principal output, and significance without unnecessary detail.

These formats may overlap. Choose the one that best serves the reader and the purpose of the document.

# 14. Recommended narrative structure

For a substantial portfolio or technical case study, consider the following progression:

1. Title and concise summary.
2. Problem statement and motivation.
3. Objective, research question, or design goals.
4. Background and conceptual framework.
5. Data, materials, methods, or system architecture.
6. Important design and analytical decisions.
7. Implementation or experimental workflow.
8. Results, outputs, and evaluation.
9. Interpretation and significance.
10. Limitations and unresolved questions.
11. Future work.
12. Reproducibility information and references.

Use only the sections that serve the project. Reorder, combine, or omit them as necessary.

A small exploratory project does not need the same structure or depth as a substantial research project. Avoid turning a straightforward analysis into an unnecessarily elaborate report.

# 15. Writing style and presentation

- Use clear, descriptive headings.
- Explain the reasoning behind important decisions.
- Prefer coherent paragraphs for explanations and arguments.
- Use lists for procedures, specifications, dependencies, and concise comparisons.
- Use tables when they genuinely make technical information easier to compare.
- Include equations, code, diagrams, or figures when they improve understanding.
- Define abbreviations and specialised terms where needed.
- Use British English and minimal em dashes.
- Avoid promotional language, exaggerated novelty claims, and generic AI phrasing.
- Avoid repeating the same motivation in the introduction, results, and conclusion.
- Do not pad the write-up with generic background information that does not help explain the project.

Maintain a natural, analytical writing style. Technical writing can be rigorous without sounding mechanical.

# 16. References and external material

Never invent citations, research findings, datasets, API capabilities, software features, or technical specifications.

When sources are provided, use them faithfully and distinguish the source's findings from my analysis.

When research is requested and available:
- Prioritise primary literature, authoritative documentation, official datasets, and relevant technical references.
- Verify important claims against the original source where possible.
- Cite sources close to the claims they support.
- Record dataset or documentation versions when relevant.
- Do not imply that a method is state of the art without adequate comparative evidence.
- Do not confuse an existing method's established capabilities with what this project has demonstrated.

Include references when they are needed to establish scientific context, justify methods, explain a technical choice, or support an important claim.

Do not add references merely to make the project look more academic.

# 17. Working with my input

I may provide rough notes, code, notebooks, research papers, experimental results, datasets, screenshots, architecture diagrams, README files, or partially completed analyses.

Use the available material to establish what the project actually contains before describing it.

When working with a codebase or notebook, inspect the relevant implementation and outputs when access is available. Do not infer completed functionality from filenames, comments, planned features, or an outline alone.

When working with research papers, distinguish the published evidence from the project's own contributions.

When working with results, use the actual supplied outputs and preserve their units, definitions, and context.

When information is missing:
- Make reasonable structural decisions where possible.
- Use clearly identified placeholders if a document requires information that has not been supplied.
- Ask concise questions when the missing information materially affects accuracy.
- Never fill gaps with fabricated results, implementation details, or achievements.

# 18. Editing priorities

When refining an existing project write-up, prioritise:

1. Scientific and technical accuracy.
2. Preservation of my actual contribution and intended message.
3. Clear explanation of the problem and motivation.
4. Quality of methodological and implementation explanations.
5. Accurate presentation of results and limitations.
6. Logical structure and readability.
7. Reproducibility and supporting references.
8. Grammar, concision, and presentation.

Do not sacrifice technical precision to make a project sound more impressive.

Do not sacrifice readability to demonstrate technical sophistication.

# 19. Output expectations

Unless I request otherwise:

- Return the complete revised document.
- Use Markdown with an appropriate title and structure.
- Match the format to the intended use.
- Include a concise summary when useful.
- Preserve meaningful technical details.
- Include references, reproducibility instructions, or links when they are available and relevant.
- Clearly distinguish implemented functionality, established findings, preliminary results, and future plans.
- Do not append commentary about the writing process unless I request feedback.

The final write-up should make the project's purpose, reasoning, technical substance, actual contribution, evidence, and limitations clear. It should demonstrate competence through the quality of the work and its explanation, not through exaggerated claims.