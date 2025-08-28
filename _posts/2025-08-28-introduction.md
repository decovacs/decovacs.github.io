---
layout: post_2
title: "Introduction"
date: 2025-08-26 08:00:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
---

<h5>Introduction</h5>
<p>This report presents the procedures undertaken to transform data from the VigiVac Project into the OMOP Common Data Model (CDM), a standardized framework developed by the Observational Medical Outcomes Partnership (OMOP). The primary objective of this transformation is to harmonize Brazilian health surveillance data with international standards, thereby enabling federated comparative analyses with partner datasets, including those from Pakistan.</p>
<p>The OMOP CDM is a relational data model designed to structure observational healthcare data in a consistent format, allowing integration of heterogeneous sources for large-scale analytics. Its development and ongoing refinement are supported by the Observational Health Data Sciences and Informatics (OHDSI) community, a global, open-science collaborative network. OHDSI not only maintains the OMOP model but also develops analytical libraries, provides guidelines, and advances methods to ensure semantic and structural interoperability of health data. Together, OMOP and OHDSI form the foundation for evidence-based, data-driven decision-making in healthcare systems worldwide.</p>
<p>It is important to note that this report represents a partial and iterative milestone in the VigiVac–OMOP integration process. Adapting complex, hierarchical datasets into the OMOP structure requires a series of ETL (Extract, Transform, Load) operations that are continuously refined. New findings may necessitate revisiting earlier decisions, and thus, the results presented here should be considered provisional.</p>
<p>The VigiVac Project itself is a digital surveillance initiative aimed at evaluating the effectiveness of COVID-19 vaccines across Brazil. It focuses on differences in protection across demographic groups, geographic regions, and clinical contexts. The initiative consolidates three major national data sources available through DataSUS:</p>
<ul>
  <li>Severe Acute Respiratory Syndrome (SRAG) – hospitalizations and severe outcomes.</li>
  <li>e-SUS Notifica (SG) – notifications of flu-like syndrome cases.</li>
  <li>National Immunization Program (PNI) – COVID-19 vaccination records.</li>
</ul>
<p>By integrating these complementary databases, VigiVac enables robust estimation of vaccine effectiveness in preventing both mild and severe infections, as well as COVID-19–related mortality. The analyses described in this report are based on a data snapshot extracted in August 2022.</p>
