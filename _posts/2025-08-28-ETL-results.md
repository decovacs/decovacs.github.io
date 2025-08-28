---
layout: post
title: "ETL Results"
date: 2025-08-26 08:11:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
---

<h5>ETL Results</h5>

<p>Table 8 shows the number of records in each OMOP table after executing the ETL pipeline on the source data. The Person table covers ~80% of Brazil’s population, demonstrating near-national representation. On average, each person is associated with ~3 healthcare visits, reflecting multi-source data integration. Measurements cover ~25% of individuals, indicating the scale of diagnostic testing data.</p>



OMOP Table	Number of Records
Person	187.634.532
Care Site	482.479
Drug Exposure	446.134.022
Location	5.570
Measurement	48.297.708
Observation period	188.537.327
Condition occurrence	36.895.473
Visit occurrence	531.604.051
Table 8: size of each OMOP CDM table after ETL 



<h6>Data Quality Considerations</h6>
<p>Challenges identified:</p>
<ul>
	<li>Incomplete or inconsistent demographic variables (sex, race, date of birth).</li>
	<li>Underreporting of mild COVID-19 cases in SG.</li>
	<li>Potential regional disparities in data completeness.</li>
	<li>Need for validation of deterministic linkages across databases.</li>
</ul>

<p>Future steps include systematic data quality assessment using OHDSI’s Data Quality Dashboard and domain-specific plausibility checks.</p>
