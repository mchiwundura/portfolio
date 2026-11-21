---
title: 'Efficient by Design: Rust and Healthcare Software in Africa'
description: 'Why a language built for memory safety and predictable efficiency is unusually well matched to the constraints of African healthcare software, and where the limits of that argument lie.'
pubDate: 'Mar 15 2026'
heroImage: '../../assets/blog-placeholder-4.jpg'
---

## Introduction

Healthcare software is not ordinary software. It holds data that people cannot choose to withhold, informs decisions that change what happens to a body, and is expected to keep working long after the team that built it has moved on. In a busy clinic, a crashed application is not an inconvenience measured in support tickets. It is an interrupted consultation, a lost record, and potentially a delayed treatment.

That is true everywhere. It is more consequential where the margin for error is thin. Across much of Africa, health systems operate with intermittent electricity, expensive or unreliable connectivity, ageing hardware, and a chronic shortage of the engineers who could maintain software once it has been deployed. The tools that do work have to work on what is actually present, not on the infrastructure a vendor assumed when writing the specification.

Programming language choice is usually treated as an internal engineering preference. In this context it is closer to an infrastructure decision. The language determines how much memory a system needs, how predictably it behaves under load, how easily it can be maintained by people who did not write it, and which classes of bug are caught before release rather than in a ward. Rust, a systems language built around memory safety without a garbage collector, has properties that line up unusually well with these constraints.

That is the argument of this article. It is not that Rust is transforming African healthcare software today. Evidence for that specific claim is thin, and the most concrete examples come from elsewhere. It is that the properties Rust offers are unusually relevant to the problems this context actually has, and that adopting them deliberately could remove one category of failure from systems that cannot afford many. The limits of that claim matter as much as the claim itself. Rust does not generate electricity, train engineers, fund maintenance, or make a poorly designed workflow useful. Where those are the binding constraints, a language change is a distraction.

The discussion proceeds in four parts. First, why the language matters in high-stakes, long-lived software. Second, where the efficiency argument is real rather than rhetorical. Third, the points at which it breaks down. Fourth, what adoption would require in practice.

## Part One: Why the Language Matters Here at All

#### 1.1 Bugs Are Not Equivalent Across Contexts

A wrong answer in a consumer application is usually annoying. A wrong answer in a clinical system can contribute to harm, and the difference is not one of degree alone. Clinical software operates inside a chain of decisions. A corrupted record, a dropped message, or a value silently replaced by a default can travel through that chain and influence care long after the original fault has been forgotten.

It is worth distinguishing between two kinds of failure here. Memory safety bugs are errors in how a program reads and writes memory: buffer overflows, use-after-free, double frees, and their relatives. They are a well-understood class with well-understood consequences, including crashes, data corruption, and arbitrary code execution. Clinical logic errors are different. A model trained on the wrong target, a dosage rule coded incorrectly, or a value mapped to the wrong patient can cause harm while every byte of memory is handled perfectly.

Rust addresses the first class directly and does nothing about the second. That is a narrower claim than is sometimes made on its behalf, but it is still a substantial one. The US Cybersecurity and Infrastructure Security Agency and its international partners report that roughly two-thirds of vulnerabilities in memory-unsafe languages relate to memory issues, and they list Rust among the languages that can prevent entire classes of these defects. Their 2025 guidance also notes, importantly, that adopting a memory-safe language is not a licence for complacency about other vulnerabilities (CISA and NSA, 2023; 2025).

The practical evidence for the scale of the effect is now considerable. Google began moving new Android development to memory-safe languages around 2019 and has published the results repeatedly. Memory safety vulnerabilities fell from 76 per cent of Android vulnerabilities in 2019 to 24 per cent in 2024 and below 20 per cent in 2025. In its 2025 update, Google estimated a memory safety vulnerability density of about 0.2 per million lines of Rust against a historical density closer to 1,000 per million lines in its C and C++ code. It also found that Rust changes were rolled back about four times less often than comparable C++ changes and spent about 25 per cent less time in code review (Vander Stoep and Rebert, 2024; Vander Stoep, 2025).

Two cautions are necessary. Google describes the relationship between language choice and vulnerability decline as a correlation, not a controlled experiment, and Android is not a hospital information system. What the data establishes is that eliminating a class of defect at the language level can change the reliability profile of a very large, long-lived codebase. That is the relevant principle, not the specific percentages.

#### 1.2 Software That Outlives Its Authors

Healthcare systems are unusual in how long they are expected to survive. A record system, a laboratory interface, or a device controller may remain in service for a decade or more, through several generations of hardware and the complete turnover of the team that built it. Longevity is therefore not a nice-to-have property. It is the condition under which the system has to be judged.

Most of what determines longevity is organisational: ownership, documentation, testing, funding, and the willingness to maintain unglamorous components. No language substitutes for those. But language choice affects how much slack a team has. Rust's compiler rejects entire categories of error before code runs, its type system makes many assumptions explicit, and its package manager makes dependencies and builds reproducible. In Google's data, the lower rollback rate and shorter review time for Rust are suggestive of a codebase that is easier to change without breaking. For a system that will be modified by strangers years from now, that matters.

There is a more specific reason this connects to low-resource settings. Systems in these environments are often maintained, if at all, by people working at distance from the original developers, on intermittent contracts, without the institutional memory that makes legacy code survivable. Anything that reduces the amount of tacit knowledge required to change code safely is worth having. Rust is not a substitute for documentation or local ownership, but it can reduce the penalty for imperfect ones.

## Part Two: Efficiency as a Constraint, Not a Slogan

#### 2.1 Where Performance Is Genuinely Binding

Healthcare software is described as data-intensive so often that the phrase has lost meaning. It is worth being concrete about where the data pressure actually falls. Interoperability middleware, clinical data repositories, medical image processing, and device firmware all involve work that scales badly with inefficient code.

Haste Health, which builds a clinical data repository in Rust, has published details of the load its systems handle. One health system described in its engineering notes processes around eight million HL7 version 2 messages a day, with roughly fifteen FHIR resources generated per message; another streams about eight thousand FHIR resources a second. Translating and indexing that volume is CPU-intensive work. The company's own benchmarks put a Rust implementation of FHIRPath evaluation at 11.7 microseconds against 51.5 microseconds for its TypeScript version, and transaction processing at 39 milliseconds against 110 milliseconds (Parkison, 2026). These are figures from one vendor's implementation, not independent benchmarks, but they illustrate the shape of the problem rather than establishing a universal speedup.

A similar pattern appears in medical imaging. Promaton rebuilt the annotation parser in its dental X-ray tool in Rust compiled to WebAssembly and reported about a fivefold improvement in loading time, reducing a fifty-second load of an 800MB scan to roughly ten seconds on the machine it tested (Evers, 2021). Again, this is a single account. It is included here because it shows the same mechanism at a different layer: a narrow, computationally expensive component moved to a language that handles it more efficiently, without rewriting the whole application.

The lesson in both cases is not that healthcare should be written in Rust. It is that there are layers where efficiency determines whether a system can do the job at all. Recognising those layers is more useful than applying the language indiscriminately.

#### 2.2 Life Without a Garbage Collector

Rust's most relevant property for low-resource environments is not raw speed. It is the combination of predictable memory use and the absence of a garbage collector. Resource management is deterministic: values are released when they leave scope, without a background process pausing the application to reclaim memory. Memory leaks are still possible, but the common patterns that produce them are constrained by the ownership model.

This matters for three kinds of system. Embedded devices and sensors operate with tiny memory budgets and real-time deadlines, where a garbage collection pause is not a performance optimisation problem but a missed measurement. Edge applications running on modest hardware benefit from smaller binaries and lower memory footprints. And offline-first tools, which must queue work locally and synchronise when a connection appears, benefit from predictable behaviour under constrained resources.

None of this changes the physical realities described in earlier discussions of health tech in Africa. A language cannot fix a power outage, a missing network, or a clinic with no computers. What it can do is lower the resource floor: make it possible to run a useful component on cheaper, older, or less connected equipment than would otherwise be required. That is a genuine advantage, but it is an advantage at the margin, and it should be described as one.

There is also a limit to how often it applies. A patient registration form, a dashboard, or a straightforward reporting service will usually be bottlenecked by network latency, database design, or user workflow long before it is bottlenecked by CPU. Rewriting such systems in Rust is effort spent on a constraint that is not binding.

## Part Three: Where the Argument Breaks Down

#### 3.1 Memory Safety Is Not Correctness

The most common overstatement in discussions of Rust is the implication that safe memory handling produces safe software. It does not. A program can be entirely memory-safe and still deliver the wrong dose, overwrite the wrong patient's record, misclassify an image, or silently drop a message when a queue fills. These are logic, specification, and data quality problems, and they are responsible for a large share of the failures that matter clinically.

This is why the argument for Rust has to be careful. The agencies promoting memory-safe languages also warn against assuming that the adoption of one removes the need for testing, validation, and secure design. In healthcare, the most consequential bugs are often not crashes. They are outputs that look plausible and are wrong. Rust reduces the chance that a programme will fail in one spectacular way while leaving the quieter, harder ways untouched.

The same caution applies to the broader claim about language choice. The history of health technology in low-resource settings is largely a history of contextual failure: pilots that assumed reliable power, platforms that demanded digital literacy the workforce did not yet have, systems dependent on donor funding that ended with the grant. A technically excellent language does not protect a project from any of those. It can make the resulting system more robust once the contextual decisions are right. It cannot make them right.

#### 3.2 The Skills Curve Is Itself a Constraint

Rust has a steep learning curve, and in a continent already facing a digital skills gap this is not a minor consideration. The borrow checker, lifetimes, and the need to reason explicitly about ownership require a different habit of thought from languages such as Python, JavaScript, or Java. Teams can learn it, but learning takes time, and time is scarce where delivery pressure is high and external support is expensive.

Engineers who have done this work describe both the benefit and the cost. The developer who rewrote the FreePulse medical device software in Rust argued that you do not need to be a Rust expert to gain from it, and reported better maintainability as a result. The same account notes that developer tooling was still maturing. More recently, Haste Health described encountering difficulties with lifetimes and with the absence of built-in reflection, requiring custom macros to support FHIR data modelling (Stevens, 2019; Parkison, 2026). These are manageable engineering problems for a well-resourced team. They are a meaningful burden for one that is not.

The risk in the African context is dependency. If the only people who can maintain a Rust-based clinical system are consultants in another country, the system has not become more sustainable by being written in a safer language. It has acquired a foreign dependency with a different name. This is precisely the pattern that makes health tech pilots collapse when funding ends.

There are reasons to think this can change. The Rust Africa association reports active communities in Nigeria, Cameroon, South Africa and Kenya, and describes training programmes, mentorship, university partnerships, and campus tours supported by the Rust Foundation and industry partners (Rust Africa, 2025). That is real institution-building. It is worth noting, however, that the association's stated industry focus is fintech, blockchain, AI and embedded systems, not healthcare specifically. The bridge from a general Rust community to a pool of engineers who can maintain clinical software has not yet been built.

#### 3.3 Standards and Toolchains Move Slowly

Healthcare software lives inside standards that predate most of the current language landscape. HL7 version 2 still carries an enormous share of clinical messaging. FHIR defines how modern systems exchange records. DICOM governs medical images. Safety-critical devices are developed against standards such as IEC 62304, and the toolchains used to build them are expected to be qualified and traceable.

Rust's position in this landscape is improving but narrow. Regulated toolchains are emerging, with Ferrocene providing documentation aligned to IEC 62304, and there are documented examples of safety-critical medical software built in Rust. One is OxyPrem's neonatal tissue oximetry system, where Rust and Ferrocene were used for sensor firmware on a system described as certifiable to IEC 62304 Safety Class B (Ferrous Systems, 2026). This is a genuine demonstration that the approach works in a regulated setting. It is also a specialist, recent, and well-resourced one.

The harder gap is hardware support. TTP, a medical device consultancy, notes that vendor support for microcontrollers has historically been provided in C, while much of the Rust support is community-developed and less complete. Its advice is to start with small components, evaluate them against existing implementations, and investigate hardware and toolchain constraints before committing (TTP, 2024). That is a sensible recommendation in any setting, and a necessary one where procurement and regulatory capacity are already stretched.

There is a parallel point about integration. The Orthanc PACS project has developed a Rust SDK for plugin development, and its maintainers note that Rust remains uncommon in medical imaging despite its fit with lightweight, low-end hardware (ChRIS Project, 2025). Work of this kind, contributed to established standards-based systems, is likely to matter more than new frameworks built from scratch.

## Part Four: What Adoption Would Actually Require

#### 4.1 Use Rust Where the Constraints Are Real

The case for Rust is strongest at specific layers. Interoperability middleware that translates between HL7 v2 and FHIR under heavy load. Parsers and validators that process large or untrusted inputs. Clinical data repositories where indexing and transaction processing dominate CPU time. Image processing components. Firmware and edge devices with tight memory and timing constraints. Long-lived services where the cost of change accumulates over years.

The case is weak elsewhere. Interfaces, simple CRUD services, and reporting tools should be written in whatever language the team can maintain well. A policy of adopting Rust everywhere would repeat the error of choosing a technology before identifying the problem. A policy of adopting it where its properties solve a named constraint is engineering.

#### 4.2 Build Skills Alongside Systems

If Rust is adopted, training has to be part of the project rather than an afterthought. That means budget for learning, local champions who can answer questions without an international call, and partnerships with universities and training institutions that produce engineers who can maintain the system after the initial team disperses. The existing Rust community initiatives in Africa are the natural starting point, but healthcare-specific capacity will require deliberate collaboration between that community and health institutions.

This is the sustainability argument in its simplest form. A system that cannot be maintained locally is not a finished system. It is a pilot waiting for its funding to end.

#### 4.3 Integrate With the Standards, Not Around Them

Healthcare data standards are imperfect and often frustrating, but they are the substrate on which systems interoperate. Rust adoption is most useful when it strengthens a team's ability to work with HL7 v2, FHIR and DICOM, and least useful when it produces a parallelism that nothing else can talk to. Where robust libraries exist, use them. Where they do not, contributing a well-tested implementation to an open-source project is a better use of effort than maintaining a private fork.

Haste Health's experience is instructive here: the team had to build custom derivation macros for FHIR reflection and serialisation because existing tooling did not cover the standard's edge cases. That is a real cost, and it is also an opportunity. Work of this kind, done once and shared, lowers the cost for everyone who follows.

#### 4.4 Treat Safety as a System Property

Memory safety belongs in a safety case, not in place of one. Clinical software still needs requirements tracing, verification, validation against realistic workflows, error handling that fails safely, access control, audit trails, and ongoing monitoring. Rust makes one part of that work easier and more reliable. It does not make the rest optional.

In practice this means adopting Rust as part of a wider engineering discipline: code review, automated testing, dependency management, and a defined maintenance budget. Where a system is regulated, it means engaging with the relevant standards and toolchain requirements early, because retrofitting qualification is expensive. The OxyPrem example is useful precisely because it shows these pieces being addressed together.

#### 4.5 Measure Outcomes Rather Than Adoption

The success of a language decision should be judged by what changes in the system: fewer defects reaching production, less unplanned downtime, lower resource use on target hardware, shorter review cycles, and a maintenance burden the local team can actually carry. Counting lines of Rust or announcing a migration is not evidence of anything. Google's willingness to publish both its engineering metrics and its caveats is a better model than the usual vendor claim that a rewrite made everything faster.

For African healthcare software, the relevant question is narrower still. Did the change allow a system to run reliably on the equipment available, be maintained by the people present, and survive the end of the funding cycle? If the answer is yes, the language choice was justified. If the answer is no, it was not, regardless of the benchmarks.

## Conclusion

There are two meanings of efficient by design in this discussion. The first is technical: software that uses memory and processing power carefully, behaves predictably, and fails less often. Rust is strong on this, and the evidence from large codebases and clinical data infrastructure suggests the advantage is real where those constraints bind. The second is institutional: software that can be maintained over years by the people who have it, without external rescue. Here Rust helps at the edges, by making code easier to reason about and change safely, but the decisive factors remain local skills, ownership, standards, and funding.

Confusing the two meanings is how technology decisions go wrong. A language can remove a class of memory bug and still leave a system that nobody can maintain, that solves the wrong problem, or that assumes infrastructure a clinic does not have. Those failures are not evidence against Rust. They are evidence that the hard parts of health technology were never primarily about the language.

The case for Rust in African healthcare software is therefore conditional. Where the technical properties meet a real constraint, and where the institutional conditions for maintenance exist or are being built, the adoption is worth making deliberately. Where they do not, it is a costly distraction from work that matters more. The value of the language is not that it is modern. It is that it narrows one category of failure in systems that cannot afford many, and does so without demanding more hardware than the setting can provide. That is a modest claim, and in this context it is enough to justify attention.

## References

- CISA and NSA. *The Case for Memory Safe Roadmaps: Why Both C-Suite Executives and Technical Experts Need to Take Memory Safe Coding Seriously*. 2023. [cisa.gov](https://www.cisa.gov/case-memory-safe-roadmaps)
- CISA and NSA. *Memory Safe Languages: Reducing Vulnerabilities in Modern Software Development*. 2025. [cisa.gov](https://www.cisa.gov/resources-tools/resources/memory-safe-languages-reducing-vulnerabilities-modern-software-development)
- Vander Stoep, J. and Rebert, A. *Eliminating Memory Safety Vulnerabilities at the Source*. Google Online Security Blog, 2024. [security.googleblog.com](https://security.googleblog.com/2024/09/eliminating-memory-safety-vulnerabilities-Android.html)
- Vander Stoep, J. *Rust in Android: move fast and fix things*. Google Security Blog, 2025. [blog.google](https://blog.google/security/rust-in-android-move-fast-fix-things)
- Parkison, R. *Writing a High-Performance Clinical Data Repository in Rust*. Haste Health, 2026. [haste.health](https://haste.health/blog/writing-rust)
- Evers, P. *How We Used Wasm and Rust to Speed Up Our Annotation Tool*. Promaton, 2021. [blog.promaton.com](https://blog.promaton.com/how-we-used-wasm-and-rust-to-speed-up-our-annotation-tool-4fd422607de7)
- Rust Africa. *Positioning Africa as a Powerhouse for Rust Adoption*. 2025. [rustafrica.org](https://rustafrica.org/positioning-africa-as-a-powerhouse-for-rust-adoption)
- Guite, T. *Memory safety in medical devices: why Rust is the language you can trust*. TTP, 2024. [ttp.com](https://www.ttp.com/insights/do-you-trust-your-software-why-you-should-seriously-consider-using-rust-in-your-next-diagnostic-and-medical-device)
- Ferrous Systems. *Case Study: OxyPrem, Delivering high accuracy, certifiable neonatal brain oxygen monitoring with Rust and Ferrocene*. 2026. [ferrocene.dev](https://ferrocene.dev/documents/OxyPrem_Case_Study_10_2026.pdf)
- Stevens, R. *Perspectives on Building a Medical Device with Rust*. 2019. [reecestevens.me](https://reecestevens.me/blog/perspectives-on-rust)
- ChRIS Project. *Advantages of Rust for Orthanc Plugin Development*. 2025. [chrisproject.org](https://chrisproject.org/blog/2025/07/21/advantages-of-rust-orthanc-plugins)
