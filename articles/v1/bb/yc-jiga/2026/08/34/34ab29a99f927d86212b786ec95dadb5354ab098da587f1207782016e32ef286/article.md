---
schema_version: "1.0.0"
document_id: "34ab29a99f927d86212b786ec95dadb5354ab098da587f1207782016e32ef286"
company_key: "yc-jiga"
company: "Jiga"
source_id: "yc-jiga-news-import-07676ebb04d8"
canonical_url: "https://jiga.io/articles/cpk-vs-ppk/"
published_at: "2026-08-18T15:01:11+00:00"
first_seen_at: "2026-08-20T01:58:45.018415+00:00"
fetched_at: "2026-08-20T01:58:46.131739+00:00"
content_hash: "sha256:fa146a58502b21357e28a25f8856b32acdb81055d7bd952dabb43ecf0487c168"
---

# Cpk vs Ppk: What the numbers really tell you about a supplier’s process

A supplier submits a capability report showing Cpk = 1.78. On paper, the process appears[highly capable](https://ilssi.org/process-capability-analysis-cp-cpk-pp-and-ppk-demystified/) . But should you approve production based on that figure alone? This is a key decision that is triggered by a[first article inspection (FAI)](https://jiga.io/articles/first-article-inspection) .


Process capability indices are among the most widely referenced statistical tools in manufacturing, yet they are also among the most misinterpreted. Higher values are generally better, but a real understanding of how the numbers actually measure is required, to clarify manufacturing risk.


The key is recognising that capability indices are not


*quality* scores. They are statistical descriptions of process behaviour. Used correctly, they help answer practical engineering questions:


- Is this process capable of meeting specification?


- Will it remain capable throughout production?


- Is the reported performance representative of normal manufacturing?


- Can this supplier consistently produce conforming parts?


Understanding the distinction between Cp, Cpk, Pp and Ppk is the key to supplier qualification,[Production Part Approval Process (PPAP)](https://jiga.io/articles/first-article-inspection) , Statistical Process Control (SPC) and ongoing quality assurance. Equally, knowing when


*not* to rely on a capability study can prevent costly production problems.


This guide explains the four capability indices, shows how Cpk differs from Ppk, and demonstrates how engineers use capability studies to evaluate manufacturing processes rather than simply compare numbers. The focus is practical application, helping you interpret capability reports with greater confidence and make better-informed supplier decisions.


If you’re evaluating multiple manufacturers, capability studies are most useful when considered alongside[design-for-manufacturing](https://jiga.io/articles/dfma-techniques) reviews, production planning and supplier quality systems. Together, they provide a far more complete picture of production readiness than any single statistic alone.


### Cpk vs Ppk in one sentence


Both Cpk and Ppk measure how well a manufacturing process fits within engineering tolerances, accounting for how closely the process is centred between the specification limits.


The difference lies in how process variation is calculated.


- Cpk estimates capability using short-term, within-subgroup variation, showing how the process performs when operating under stable conditions.


- Ppk uses overall process variation, measuring how the process has actually performed throughout a production run.


In its most basic terms, Cpk measures what the process is


*capable of* . Ppk measures what the process has


*actually achieved* .


This distinction is more than statistical. It separates a process that performs well during a tightly controlled capability study from one that continues performing well after thousands of parts have been produced.


The functions of the capability analysis tools are best expressed by analogy. Race performance is informed both by the car/driver momentary capability (Cpk), but it also integrates the endurance, variability of conditions and more, in delivering an overall outcome (Ppk).


### Why capability matters


Poor process capability typically results in:


- scrap


- rework


- supplier disputes


- delayed PPAP approval


- warranty risk


- higher inspection cost


Capability studies exist to reduce these risks before production begins. They’re a central plank in


[quality systems](https://jiga.io/aerospace-and-defense) of all flavors


This stainless steel pneumatic valve stem is a typical CNC machined component whose production control and part quality will be positively influenced by the use of Cpk and Ppk statistical analysis.


Imagine two suppliers machining the same precision valve stem.


Supplier Cpk Ppk


A 1.67 1.64


B 1.82 1.18


At first glance, Supplier B appears superior because it reports the higher Cpk.


Supplier A’s Cpk and Ppk are almost identical, indicating that the process remains consistent under normal production conditions. Supplier B demonstrates excellent short-term capability, but the much lower Ppk suggests that additional variation is occurring during routine manufacture. Tool wear, material changes, operator differences or process adjustments may all be contributing to reduced long-term performance.


For supplier qualification, this relationship between Cpk and Ppk is often more informative than either value on its own.


## Understanding Cp, Cpk, Pp and Ppk


The four capability indices are closely related, but they answer different engineering questions. Understanding how they fit together makes it much easier to interpret a capability report.


Index Measures Accounts for process centering? Typical use


Cp Potential process capability No Initial capability assessment


Cpk Short-term process capability ✓ Stable manufacturing process


Pp Overall process performance No Long-term production performance


Ppk Overall process performance with centring ✓ Production capability assessment


Cp and Cpk describe


*capability* , while Pp and Ppk describe


*performance* .


### Cp: How much variation does the process have?


Cp compares the natural spread of the process with the available engineering tolerance.


USL = upper specification limit


LSL = lower specification limit


6σ = 6 standard deviations of the measurand


It assumes the process is perfectly centred between the specification limits.


For that reason, Cp answers only one question:


*If this process were centred correctly, would it be capable of meeting tolerance?*


A high Cp indicates that the process has relatively little


[in-tolerance](https://jiga.io/articles/cnc-precision-machining) variation compared with the specification width. However, it says nothing about where the process is actually running.


Consider a CNC lathe producing a shaft diameter of


**20.00 ±0.05 mm** .


If every shaft measures between


**20.01 mm** and


**20.03 mm** , the process variation is very small, resulting in a high Cp.


If the process drifts towards


**20.05 mm** , however, defective parts may soon be produced even though Cp remains unchanged.


This is why Cp is rarely used on its own for production decisions.


### Cpk: Is the process centred as well as capable?


Cpk builds on Cp by considering both variation and centring.


Instead of looking only at process spread, it measures the distance from the process mean to the nearest specification limit.


Where:


USL = upper specification limit (of tolerance spread)


LSL = lower specification limit


*x̄* = mean of the measured dimension set


The


**minimum** function simply means that Cpk always uses the side of the process closest to failure.


Suppose the same shaft process gradually drifts upward.


Although the variation remains unchanged, the process mean moves closer to the upper specification limit.


Cp hardly changes.


Cpk decreases because the available safety margin has become smaller.


This makes Cpk the preferred capability index for monitoring stable manufacturing processes. It reflects not only how tightly the process is controlled, but also whether it is correctly centred within the tolerance band.


Annotated capability distribution: a normal curve plotted against LSL and USL, showing the process mean offset from center and the resulting reduction in Cpk.


### Pp and Ppk: Measuring real production performance


Pp and Ppk use the same principles as Cp and Cpk, but they calculate variation from the entire production dataset rather than from short-term subgroups.


That means they include the effects of normal manufacturing:


- tool wear,


- material lot variation,


- machine warm-up,


- operator changes,


- environmental conditions,


- routine adjustments,


- and production over multiple shifts.


Pp measures the overall spread of the process without considering centering, while Ppk includes both variation and process location.


Because they represent actual production rather than ideal operating conditions, Pp and Ppk are often used during production validation and supplier qualification.


The relationship between Cpk and Ppk is particularly valuable. When the two values remain close, the process is generally behaving consistently over time. When Ppk is significantly lower than Cpk, it indicates that additional variation is entering the process during routine production.


That difference reveals much about, for example,


[aerospace CNC manufacturing](https://jiga.io/articles/aerospace-cnc-machining) , it is where capability studies become truly useful.


### The one difference that matters


The mathematical difference between Cpk and Ppk is much smaller than the engineering difference.


Both indices compare process variation against the engineering tolerance while accounting for process centring. The only distinction is how the standard deviation is calculated.


- Cpk uses within-subgroup standard deviation, representing the process under stable operating conditions.


- Ppk uses overall standard deviation, representing the variation observed throughout the production run.


That single change shifts the question being answered.


**Cpk asks:**


*How capable is this manufacturing process when it is operating normally and under statistical control?*


**Ppk asks:**


*How has this manufacturing process actually performed over time?”*


### Short-term capability versus long-term performance


Every manufacturing process contains two sources of variation.


The first is inherent process variation. A perfectly maintained machine producing identical parts under identical conditions will exhibit small dimensional variances. These arise from machine resolution, material microstructure and measurement uncertainty.


The second source is production variation. As manufacturing continues, additional influences begin to affect the process:


- gradual tool wear,


- material batch variation,


- machine warm-up,


- operator changes,


- maintenance activities,


- fixture wear,


- environmental temperature,


- and process adjustments.


Cpk largely reflects the first category.


Ppk captures both.


Consequently, Ppk will often be slightly lower than Cpk as it reflects gritty reality


### A practical example


Consider the valve stem, with the critical diameter of 20.00 ±0.05 mm.


During process validation, the supplier measures fifty consecutive parts produced immediately after optimisation.


The results show a Cpk ofc1.74


The process appears highly capable.


Several weeks later, another study is performed using production data collected across multiple shifts.


During this period:


- the mould has accumulated production hours,


- resin has been supplied from several batches,


- different operators have run the machine,


- preventive maintenance has been completed,


- and small process adjustments have been made to compensate for changing ambient conditions.


The updated study reports Cpk 1.71, Ppk 1.34


The underlying process is still capable. Cpk has barely changed.


However, the additional variation introduced during normal production has reduced Ppk.


Nothing in the report suggests the supplier is incapable. Instead, it indicates that routine manufacturing introduces more variation than was evident during the original capability study.


This illustrates the range of possible outcomes in terms of centering and spread of the critical valve face dimension, the OD of the bi-directional conical sealing faces. This is one of several critical dimensions that would be used in an effective Cpk/Ppk analysis.


### Interpreting the relationship


Looking at Cpk or Ppk in isolation tells only part of the story.


Comparing them often reveals far more.


Relationship Typical Interpretation


Cpk ≈ Ppk Stable, well-controlled process


Cpk slightly higher than Ppk Normal long-term production variation


Cpk much higher than Ppk Additional variation entering production; investigate the cause


A significant difference does not


*automatically* indicate poor manufacturing. It indicates that the process deserves further investigation.


Possible causes include:


- tool life exceeding planned limits,


- inconsistent machine setup,


- excessive process adjustments,


- differences between mould cavities,


- inconsistent raw material properties,


- inadequate preventive maintenance,


- or environmental influences affecting the process.


Some causes may be entirely benign once understood. Others may require corrective action before production approval.


### Capability values should support engineering judgement


One of the most common mistakes during supplier evaluation is treating capability indices as a ranking system.


For example:


Supplier Cpk Ppk


Supplier A 1.55 1.52


Supplier B 1.83 1.19


It is tempting to conclude that Supplier B is the stronger manufacturer because the reported Cpk is higher.


In reality, Supplier A may represent the lower production risk.


Its capability remains consistent during normal manufacturing, suggesting that the process is well controlled and repeatable. Supplier B clearly has the potential to produce excellent parts, but something occurring during production is reducing long-term performance, showing reduced process maintenance.


This is why experienced Supplier Quality Engineers review capability studies alongside process controls, measurement systems and production history, rather than relying on a single statistical value.


### Why this matters for supplier qualification


Capability studies are intended to reduce uncertainty.


A supplier whose Cpk and Ppk remain closely aligned is demonstrating that short-term process capability translates into consistent production performance.


Where the gap is significant, the study raises useful questions:


- Is the process drifting over time?


- Is tool wear being managed effectively?


- Are all production shifts performing consistently?


- Does material variation require tighter control?


- Is Statistical Process Control identifying trends early enough?


Answering those questions provides far greater confidence than simply accepting or rejecting a supplier based on one capability number.


The capability study provides understanding of how a manufacturing process behaves, rather than merely calculating statistics.


## What is considered a good Cpk or Ppk?


Capability targets should always be agreed during APQP rather than assumed after production begins.


Sooner or later, every capability discussion leads to the same question:


***What Cpk should we be looking for?***


There is no universal answer.


Capability indices are not pass-or-fail scores. They are statistical measures of manufacturing risk, and the appropriate target depends on the application, the criticality of the feature and the customer’s quality requirements.


A cosmetic feature on a consumer product does not require the same level of process capability as a sealing surface in a hydraulic valve or a datum feature on an aerospace component.


Capability targets should always be defined within the context of the product being fit for purpose.


### Common capability benchmarks


Although customer requirements always take precedence, the following values are widely used throughout manufacturing.


Cpk or Ppk Typical Interpretation


Less than 1.00 Process is unlikely to consistently meet specification. Improvement is normally required before production approval.


1.00 to 1.33 Marginal capability. Suitable only for some non-critical characteristics or where agreed with the customer.


1.33 or higher Generally regarded as capable for many production applications.


1.67 or higher Frequently specified for critical characteristics or higher-risk applications.


2.00 or higher Indicates an exceptionally capable and well-controlled process.


These values are industry conventions rather than universal standards. Automotive, aerospace, medical and semiconductor manufacturers frequently apply different acceptance criteria based on product risk and contractual requirements.


### Why 1.33 became the benchmark


A capability index of 1.33 represents a process whose natural variation fits comfortably within the specification limits, while leaving a reasonable operating margin.


Assuming the process is:


- statistically stable,


- approximately normally distributed,


- correctly centred,


- and measured using a capable inspection system,


- a Cpk of around 1.33 provides confidence that non-conforming parts should occur only infrequently.


If the process is drifting, if measurement error is significant or if production conditions differ from the capability study, the calculated Cpk may overstate the true manufacturing capability.


### Bigger numbers do not automatically mean lower risk


Capability reports should never be compared without understanding how the data were collected.


Consider these two studies.


Supplier Cpk Study Summary


A 2.05 Thirty parts measured immediately after machine setup


B 1.56 Three hundred consecutive production parts measured over three shifts


**Supplier A** reports the higher capability.


**Supplier B** provides the stronger evidence.


The second study captures the variation that naturally occurs during production and is therefore


*far* more representative of the process outcomes that customers will experience.


## Capability depends on process stability


Capability calculations assume that the manufacturing process is statistically stable.


Without stability, capability indices become unreliable predictors of future production.


Imagine a milling cutter that gradually wears during production. Early parts measure close to nominal.


As the cutter wears, dimensions steadily drift towards the upper specification limit.


The calculated Cpk may still appear acceptable because it averages the entire dataset, but the process itself is changing continuously.


Future production is therefore unlikely to behave like the measured sample.


This is why Statistical Process Control (SPC) should always precede capability analysis.


Control charts answer the question:


***Is the process predictable?***


Capability indices answer the next question:


***Can this predictable process consistently meet specification?***


Both are required before capability results can be interpreted with confidence.


## A capability study is only as good as its measurement system


Even an excellent manufacturing process can appear incapable, if the inspection system introduces measurement variation. A capability study cannot be better than the measurement system used to produce it.


Before accepting any capability study, engineers should confirm that:


- the measurement equipment is appropriate for the tolerance,


- calibration is current,


- Measurement System Analysis (MSA) has been completed where required,


- Gage R&R results are acceptable,


- and inspection procedures are consistent between operators.


If measurement error represents a significant proportion of the observed variation, the calculated Cp, Cpk, Pp and Ppk values become progressively less meaningful.


In practice, validating the measurement system is often just as important as validating the manufacturing process itself.


## Capability should inform decisions, not replace them


Capability studies provide objective evidence, but they should never replace engineering judgement.


A supplier with a Cpk of 1.45 supported by robust process controls, excellent documentation and consistent production history may represent less manufacturing risk than another reporting Cpk 1.90 from a narrowly controlled demonstration study.


The role of capability analysis is not to produce a single acceptance number.


Its value lies in helping engineers understand how reliably a manufacturing process will perform once production begins.


## Capability studies in APQP, PPAP and supplier qualifications


Process capability does not exist in isolation. It forms part of a broader quality framework that demonstrates whether a manufacturing process is ready for production and can continue meeting specification throughout the life of the programme.


A capability study may show that a process is statistically capable, but it does not confirm that the manufacturing system as a whole is ready for production. Engineers still need evidence that the process has been planned correctly, validated under representative conditions and can be controlled over time.


This is where Advanced Product Quality Planning (APQP), Production Part Approval Process (PPAP) and Statistical Process Control (SPC) work together.


### APQP: Designing capability into the process


Capability should be considered long before the first production part is manufactured.


During APQP, engineers identify the product characteristics that are critical to function, assess manufacturing risks and develop a process capable of consistently meeting specification.


Typical APQP activities include:


- Reviewing engineering drawings and GD&T


- Identifying Critical-to-Quality (CTQ) characteristics


- Completing Design and Process FMEAs


- Selecting manufacturing processes


- Planning inspection methods


- Defining Statistical Process Control requirements


- Establishing capability targets for key characteristics


The objective is simple: design a manufacturing process that is capable by default, rather than relying on inspection to remove defective parts.


This is also where Design for Manufacturing (DFM) becomes valuable. Tight tolerances, inaccessible features and unnecessarily complex geometries reduce achievable process capability and increase manufacturing cost. Resolving those issues before tooling is commissioned is usually far more effective than trying to improve capability afterwards.


### PPAP: Demonstrating production readiness


While APQP plans the process, PPAP demonstrates that it works.


A typical PPAP submission includes evidence that the supplier understands both the product and the manufacturing process. Depending on the required submission level, documentation may include:


- Ballooned engineering drawings


- Dimensional inspection results


- Material certifications


- Process Flow Diagram


- Process FMEA


- Control Plan


- Measurement System Analysis (MSA)


- Gage R&R studies


- Initial capability studies


- Appearance Approval Report, where applicable


- Part Submission Warrant (PSW)


Capability indices are but one element of this package.


A supplier reporting an excellent Cpk but providing incomplete inspection data or an unvalidated measurement system has not demonstrated full production readiness.


A slightly lower capability value supported by robust documentation, representative sampling and mature process controls should provide greater confidence that production will remain stable.


### SPC: Maintaining capability after approval


Production approval is not the end of capability analysis.


Once manufacturing begins, the priority shifts from demonstrating capability, to maintaining it.


Statistical Process Control provides the mechanism for doing that.


Control charts monitor process behaviour in real time, allowing engineers to identify trends before parts fall outside specification. Rather than relying solely on end-of-line inspection, SPC detects gradual changes while corrective action is still relatively inexpensive.


Typical examples include:


- progressive tool wear during machining,


- mould temperature drift during injection moulding,


- fixture wear in assembly operations,


- or increasing dimensional variation caused by changing material properties.


Capability studies provide periodic confirmation that the process remains capable. SPC provides continuous assurance that it is remaining under control.


The two tools complement one another. Neither should be used in isolation.


This shows a specialist valve seat cutter that cuts a seal face in one Z axis motion. The precision of the seat relies entirely on the condition of the two cutter inserts, and the illustrated damage and edge wear will reduce the cut accuracy, and skew the tolerances to be increasingly off center to the required outcome.


### Capability is one piece of supplier qualification


When assessing a potential supplier, capability studies should be evaluated alongside other indicators of manufacturing maturity.


These include:


- Process stability demonstrated through SPC


- Validated measurement systems


- Production capacity


- Preventive maintenance programmes


- Operator training


- Corrective-action procedures


- Traceability systems


- Previous quality performance


- Responsiveness to engineering changes


Taken together, these factors provide a much more reliable indication of supplier capability than any single statistical index.


This broader perspective is particularly important when sourcing custom manufactured components. Two suppliers may report similar capability values yet differ substantially in process control, documentation quality and long-term production consistency.


For procurement teams managing multiple suppliers, evaluating capability within the context of the supplier’s overall quality system leads to more reliable sourcing decisions and fewer production issues after launch.


**CTA** **:** When comparing manufacturing partners, capability studies should be reviewed alongside DFM feedback, production controls and quality documentation. Evaluating the complete manufacturing process provides a more reliable basis for supplier selection than capability indices alone.


## Supplier approval checklist


Capability studies provide valuable evidence, but they cannot be viewed in isolation. Before approving a supplier for production, confirm that the reported capability reflects a stable, representative manufacturing process and is supported by an effective quality system.


Use the following checklist as a practical guide during supplier qualification.


### 1. Does the study represent normal production?


A capability study should reflect the conditions under which production parts will actually be manufactured.


Confirm that:


- Production tooling was used.


- Normal production settings were maintained.


- Production operators ran the process.


- Representative material batches were included.


- The study covered a sufficient production run.


- All relevant machines or mould cavities were represented.


Capability measured immediately after machine optimisation may not reflect day-to-day production performance.


### 2. Is the process statistically stable?


Capability indices only have meaning if the underlying process is predictable.


Review supporting SPC data and confirm:


- Control charts show the process is in statistical control.


- Any special causes of variation have been investigated.


- Corrective actions have been implemented where necessary.


If the process is unstable, capability values alone should not be used for production approval.


### 3. Is the measurement system capable?


Reliable capability analysis depends on reliable measurement.


Verify that:


- Appropriate measuring equipment was used.


- Calibration is current.


- Measurement System Analysis (MSA) has been completed where required.


- Gage R&R results demonstrate acceptable repeatability and reproducibility.


- Measurement resolution is appropriate for the specified tolerance.


Poor measurement systems can significantly distort capability calculations.


### 4. Does the report contain enough supporting evidence?


A useful capability report should be transparent and traceable.


Look for:


- Drawing revision


- Characteristic measured


- Sample size and subgroup size


- Specification limits


- Control charts


- Histogram


- Raw measurement data


- Machine or mould identification


- Material batch information


- Date of production


The more complete the report, the easier it is to assess whether the results genuinely represent production capability.


### 5. Can the supplier maintain capability?


Finally, determine how capability will be monitored after production begins.


Ask questions such as:


- How frequently are capability studies reviewed?


- What capability targets are monitored?


- What triggers corrective action?


- How is tool wear managed?


- How are process changes controlled?


- How are customers notified if capability deteriorates?


These discussions often reveal more about a supplier’s manufacturing maturity than the reported capability values themselves.


## Conclusion


Understanding the difference between Cp, Cpk, Pp and Ppk is much more than interpreting statistics. It is about understanding how a manufacturing process behaves, and how much confidence you can place in the parts it produces.


Cp and Cpk describe the potential capability of a stable process. Pp and Ppk describe how that process performs under normal production conditions. Used together, they help distinguish processes that perform well during validation from those that continue performing well throughout production.


Capability studies are therefore best viewed as one component of supplier qualification. When combined with Design for Manufacturing, APQP, PPAP, SPC and robust measurement systems, they provide objective evidence that a supplier can consistently manufacture parts to specification.


For engineers, the most useful question is rarely,


*What is the Cpk?*


Instead, it is,


*What do these results tell us about the process?*


That shift in perspective turns capability analysis from a statistical exercise into a practical engineering tool for reducing manufacturing risk and making better sourcing decisions. Capability is evidence in a wide ranging analysis, not a score to be alone used as a decider.


**Selecting a manufacturing partner involves more than comparing capability indices. Jiga acts as the supplier of record for custom manufactured components, coordinating DFM analysis, supplier qualification and production oversight so that promising capability studies translate into reliable production performance.**


## Frequently Asked Questions


What is the difference between Cpk and Ppk?


Cpk measures the short-term capability of a statistically stable process using within-subgroup variation. Ppk measures long-term process performance using overall production variation. Comparing the two helps identify whether additional variation is entering the process during normal manufacturing.


What is considered a good Cpk?


For many manufacturing applications, a Cpk of 1.33 or greater is considered capable, while 1.67 or higher is often specified for critical characteristics. Actual acceptance criteria should always be based on customer requirements and the risk associated with the feature being measured.


Can Ppk be higher than Cpk?


Yes, although it is uncommon. Depending on the subgroup definition and calculation method, Ppk can occasionally exceed Cpk. Unexpected results should prompt a review of the study methodology rather than an assumption that the calculations are incorrect.


Does PPAP require Cpk or Ppk?


Many PPAP submissions include capability studies for designated characteristics, but the exact requirements are customer-specific. Initial submissions commonly report Ppk because it reflects observed production performance, while Cpk is often used for ongoing process monitoring once production has stabilised.


Can a capability study guarantee production quality?


No. Capability studies estimate process performance based on measured data, but they cannot guarantee future results. They should always be interpreted alongside Statistical Process Control, Measurement System Analysis, production documentation and engineering judgement.
