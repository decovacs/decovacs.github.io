---
layout: post
title: "Health System Tables"
date: 2025-08-26 10:00:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
featured: true
---

<h5>Health System Tables</h5>
<p>In the OMOP Common Data Model (CDM), Health System tables provide the structural context in which clinical events occur. While clinical data tables (such as Person, Condition, Drug Exposure, or Measurement) describe the “what” of patient care—diagnoses, treatments, and outcomes—the Health System tables describe the “where” and “by whom”. These tables document healthcare providers, facilities, and locations, thereby grounding patient-level events within the broader organizational and geographic infrastructure of a country’s health system.</p>

<p>For the VigiVac project, this component is especially important. Brazil is a country of continental dimensions, with significant heterogeneity in healthcare infrastructure, regional resource allocation, and access to services. By correctly structuring Health System information, we ensure that subsequent analyses of vaccine effectiveness can account for contextual factors such as the place of care, the type of facility, and geographic disparities. This is particularly relevant when comparing outcomes across states, municipalities, and healthcare service types, or when conducting international federated analyses.</p>

<p>Below we describe the two main OMOP Health System tables built from VigiVac data and the rationale behind their construction.</p>

<h6>Location Table</h6>
<p>The Location table is designed to capture geographic information that situates patients, care sites, and health events within defined administrative boundaries. For the VigiVac project, this information was obtained from the Brazilian Institute of Geography and Statistics (Instituto Brasileiro de Geografia e Estatística, IBGE), which maintains the official territorial division of Brazil.</p>

<p>The attributes incorporated into the Location table include:
<ul>
	<li><b>Location ID:</b> a unique identifier generated within OMOP to represent each location.</li>
	<li><b>IBGE code:</b> the official numeric code uniquely assigned to every Brazilian municipality.</li>
	<li><b>City name:</b> standardized according to IBGE’s nomenclature.</li>
	<li><b>State identification:</b> corresponding to the federal unit (UF) of the municipality.</li>
	<li><b>Country name:</b> in this case, “Brazil,” ensuring international compatibility in federated research.</li>
</ul>

<p>This design allows researchers to link individuals and healthcare providers to precise geographic units, enabling spatial analyses of vaccine coverage, infection trends, and healthcare outcomes. For example, the Location table can be used to compare rural and urban municipalities, monitor regional differences in vaccination uptake, or assess whether certain areas experienced disproportionate burdens of severe COVID-19 cases.</p>

<h6>Care Site Table</h6>
<p>The Care Site table provides information about the healthcare facilities in which patient interactions (visits, hospitalizations, or vaccinations) take place. In Brazil, this data is sourced from the National Registry of Health Establishments (Cadastro Nacional de Estabelecimentos de Saúde, CNES), a foundational component of the country’s health information architecture.</p>

<p>The CNES plays multiple roles:</p>

<ul>
	<li>It supports the operationalization and integration of the Unified Health System (Sistema Único de Saúde, SUS).</li>
	<li>It ensures transparency by publishing information on healthcare infrastructure, installed capacity, and available services.</li>
	<li>It serves as the main reference for linking different national databases (e.g., surveillance, hospital management, vaccination).</li>
</ul>

<p>In OMOP, each care site is represented with the following attributes:</p>

<ul>
	<li><b>Care Site ID:</b> a unique identifier derived from the CNES code of the institution.</li>
	<li><b>Location ID (foreign key):</b> linking the care site to the municipality defined in the Location table, ensuring geographic consistency.</li>
	<li><b>Care site type:</b> mapped to OMOP standard concepts that classify the type of facility or service provided (e.g., hospital, outpatient clinic, vaccination center, pharmacy).</li>
	<li><b>Care site name:</b> the official name of the healthcare establishment, standardized according to CNES records.</li>
</ul>

<p>By including these attributes, the Care Site table enables analyses that go beyond patient-level outcomes to incorporate the institutional and structural context. For example, researchers can differentiate outcomes for patients treated in high-capacity urban hospitals versus small municipal clinics, or compare vaccination practices across different types of care sites.</p>
<p>This contextualization is not only relevant for internal Brazilian studies but also for cross-country comparisons. When harmonized under OMOP, the Brazilian Care Site table can be compared with equivalent structures in Pakistan or other countries, enabling federated analyses that respect local healthcare organization while ensuring international comparability.</p>
