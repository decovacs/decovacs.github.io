---
layout: post
title: "Vocabulary Tables"
date: 2025-08-26 08:02:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
---

<h5>Vocabulary Tables</h5>
<p>The Vocabulary tables in OMOP CDM are essential for ensuring semantic interoperability across heterogeneous health data sources. They provide standardized concept identifiers that allow clinical events—such as diagnoses, procedures, laboratory tests, and drug exposures—to be mapped consistently to internationally recognized terminologies. This harmonization is the cornerstone of OMOP’s ability to support comparative and reproducible research across institutions and countries.</p>

<p>For the VigiVac transformation, vocabulary data was obtained from Athena<sup>1</sup>, OHDSI’s official distribution platform for standardized vocabularies. The vocabulary bundle used was version V5.0.31 (updated May 2023), downloaded on October 16, 2023. The complete set of downloaded Athena files and the associated vocabulary list are available in the project’s reference folder<sup>2</sup>.</p>

<p>One important consideration concerns the Current Procedural Terminology (CPT-4) vocabulary. Because OHDSI does not hold redistribution rights for CPT-4 content, these terms are not directly included in Athena downloads. Instead, OHDSI provides instructions and scripts to update CPT-4 concepts using the Unified Medical Language System (UMLS) Application Programming Interface (API).</p>

<p>Following these guidelines, an institutional account was created on the UMLS website, and an API key was generated. The API was then used to retrieve the necessary CPT-4 descriptors and integrate them into the Concept table, ensuring that the OMOP vocabulary schema remained complete and up to date.</p>

<p>Through this process, the Vocabulary tables were fully populated with the latest standardized terminologies, including SNOMED CT, RxNorm, LOINC, CVX, ICD-10, and CPT-4, thereby establishing the semantic foundation required for accurate mapping of VigiVac data to OMOP.</p>

<blockquote class="blockquote single-quote">
  <p> <sup>1</sup><a href="https://athena.ohdsi.org/search-terms/start" target="_blank">https://athena.ohdsi.org/search-terms/start</a></p>
</blockquote>
<blockquote class="blockquote single-quote">
  <p> <sup>2</sup><a href="https://drive.google.com/drive/folders/1JpyieU-gwo88zSZClLCaNnX9GwooO6Le" target="_blank">https://drive.google.com/drive/folders/1JpyieU-gwo88zSZClLCaNnX9GwooO6Le</a></p>
</blockquote>