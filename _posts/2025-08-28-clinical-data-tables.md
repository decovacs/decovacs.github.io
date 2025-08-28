---
layout: post
title: "Clinical Data Tables"
date: 2025-08-26 08:15:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
---

<h5>Clinical Data Tables</h5>
<p>In the OMOP Common Data Model (CDM), Clinical Data Tables serve as the core structures for capturing information about study participants and their interactions with the healthcare system. These tables include key elements such as medications administered, medical procedures performed, diagnoses and symptoms recorded, laboratory results, and details of hospital or outpatient visits. Together, they provide a comprehensive representation of a patient’s clinical history.</p>

<p>The data in OMOP is organized according to a relational entity model, in which each type of information is stored in a separate table dedicated to a specific clinical domain. This design ensures modularity, reduces redundancy, and facilitates interoperability across diverse healthcare databases. Relationships among the tables are established through unique identifiers, allowing researchers to connect events at the level of the individual patient and the healthcare encounters they experience.</p>

<p>In the context of the VigiVac project, the focus lies on a selected subset of these tables—those most relevant to mapping COVID-19 vaccination and surveillance data into the OMOP framework. These include domains such as conditions (to capture COVID-19 symptoms and comorbidities), drug exposures (to represent vaccine administration), visits (to describe healthcare encounters), and measurements (to include laboratory test results). While OMOP encompasses a much broader set of tables covering additional domains, only this subset is directly applicable to the present stage of analysis (Figure 1).</p>

<p>By concentrating on these core tables, the study ensures that the most critical aspects of the VigiVac data are faithfully represented within OMOP, while also establishing a foundation for future expansion into additional domains as research needs evolve.</p>

 
Figure 1: Flowchart showing which data sources are included in each OMOP table.


<h6>Person Table</h6>
<p>The Person table constitutes one of the foundational components of the OMOP Common Data Model (CDM), functioning as the central identity management structure for all individuals represented in the database. Each record in this table corresponds to a unique patient and consolidates core demographic attributes that are essential for subsequent linkage to clinical events and healthcare encounters recorded in other OMOP tables.</p>

<p>For the VigiVac project, the Person table integrates information from three main data sources: SRAG (Severe Acute Respiratory Syndrome surveillance), SG (Flu Syndrome notifications via e-SUS Notifica), and the National Vaccination Program (PNI). A given individual may appear in one, two, or all three of these sources. To harmonize and avoid duplication, a deterministic linkage process was implemented to match individuals across datasets and consolidate their demographic information into a single Person record (Figure 1).</p>

<p>Because demographic attributes can sometimes be missing or inconsistent across databases, the project established a hierarchical priority rule for data selection:</p>

<ol>
	<li><b>SRAG</b> records are prioritized, as they generally provide the most complete and reliable demographic fields.</li>
	<li>If no SRAG entry exists, information from the <b>PNI vaccination database</b> is used.</li>
	<li>Finally, if the person is found only in <b>SG</b>, this source is used to populate their record.</li>
</ol>

<p>This strategy reflects the empirical knowledge developed by the VigiVac team regarding the quality and completeness of each source. Nevertheless, even with deterministic linkage, some fields may remain empty or inconsistent, requiring careful curation.</p>

<p>Within OMOP CDM 5.4, certain demographic attributes must be standardized using controlled vocabularies. In particular:</p>
<ul>
	<li><b>Sex </b>is mapped to the OMOP attribute gender_concept_id, using standardized codes (e.g., male → 8507; female → 8532).</li>
	<li><b>Race/ethnicity </b>is mapped to race_concept_id, also using OMOP standard concepts (see Table 1).</li>
</ul>

<p>Through this mapping, demographic characteristics are represented in a consistent and interoperable manner, ensuring that analyses using the VigiVac data can be compared with other OMOP-compliant datasets worldwide.</p>


Person mapping
Variable	Category	OMOP Attribute	target_concept_id	target_vocabulary_id
Gender	Male (Masculino)	gender_concept_id	8507	Gender
Gender	Female(Feminino)	gender_concept_id	8532	Gender
Ethnicity	White (Branca)	race_concept_id	8527	Race
Ethnicity	Asian (Amarela)	race_concept_id	8515	Race
Ethnicity	Black (Preta ou Parda)	race_concept_id	38003598	Race
Ethnicity	American Indian (Indígena)	race_concept_id	38003572	Race
Table 1: Concept mapping codes respective to the Person’s table


<h6>Observation Period Table</h6>
<p>The Observation Period table is a key structural component of the OMOP Common Data Model (CDM). It defines the intervals of time during which an individual is considered to be under systematic observation within the available data sources. In other words, it establishes the temporal boundaries within which two important assumptions hold:</p>

<ol>
	<li><b>Clinical events</b> experienced by the individual are expected to be captured and documented in the relevant event tables (e.g., conditions, drug exposures, measurements).</li>
	<li><b>The absence of a record</b> during that period implies that the event did not occur, at least within the observable scope of the data source.</li>
</ol>

<p>Each person in the database can have one or more observation periods. These periods must not overlap, but they may be consecutive if, for example, an individual leaves and later re-enters the system. It is important to note, however, that events may exist outside of the defined observation periods (e.g., prior hospitalizations or conditions recorded in other registries). The absence of documentation in such cases does not guarantee that no event occurred, only that it was not observed in the available data.</p>

<p>From an analytical perspective, this table is crucial for epidemiological calculations. Incidence and prevalence rates should only be estimated within active observation periods, since time outside these intervals cannot be assumed to represent systematic follow-up. OMOP also warns that very short observation periods (e.g., one day) can introduce bias into rate calculations. To address this, it is generally recommended to impose a minimum observation time when defining research cohorts, ensuring that only individuals with sufficient follow-up are included in an analysis.</p>

<p>For the VigiVac ETL process, a pragmatic strategy was adopted to create one observation period per individual. The start date was defined as the first recorded event across any of the three data sources (SRAG, SG, or PNI vaccination data). The end date was set as the most recent database update available at the time of extraction. For individuals with a documented death in SG or SRAG, the date of death was used to close the observation period, thereby ensuring consistency between mortality outcomes and the observation timeline.</p>

<p>Finally, the OMOP CDM requires each record in this table to include a period_type_concept_id, which specifies the origin of the observation period. In this study, the periods were mapped using the OMOP standard concept for Electronic Health Records (EHR) [32817], as shown in Table 2. This mapping ensures transparency in how observation windows were defined and allows harmonization with other OMOP-based studies.</p>

Observation period mapping
Attribute	target_concept_id	target_vocabulary_id
period_type_concept_id	32817(EHR)	Type Concept
Table 2: Detail of observation period mapping


<h6>Visit Occurrence Table</h6>
<p>The Visit Occurrence table captures patient interactions with the healthcare system, serving as the structural backbone for linking clinical events in OMOP CDM. Each entry corresponds to a healthcare encounter, defined primarily by its start and end dates, which delineate the temporal scope of the visit and establish the table’s architecture.</p>

<p>A notable distinction arises across the three VigiVac databases regarding the nature of visits (Table 3). In e-SUS Notifica (SG) and the Vaccination (PNI) system, encounters are expected to occur within a single day—representing, respectively, the notification of a flu-like syndrome case or the administration of a vaccine dose. Conversely, records from SRAG often reflect hospitalizations, where visits may extend over multiple days. This difference directly affects the <b>visit_concept_id</b> assigned to each record, requiring careful mapping to OMOP visit types (outpatient, pharmacy/dispensing, or inpatient).</p>

<p>In addition to temporal and typological attributes, the table integrates key identifiers. The <b>person_id</b> is obtained through deterministic linkage with the Person table, ensuring that visits are consistently associated with individual patients across all three data sources. The <b>care_site_id</b>, in turn, is derived by joining with the Care Site table created earlier, anchoring each visit to the corresponding healthcare facility.</p>

<p>Every record in the Visit Occurrence table is assigned a unique, sequential <b>visit_occurrence_id</b>, which not only distinguishes visits but also enables relational integrity across the database. This identifier is essential for establishing links with dependent tables such as Condition Occurrence, Drug Exposure, and Measurement, thereby ensuring that clinical events are correctly contextualized within specific encounters.</p>


Visit occurrence mapping
Database	OMOP Attribute	target_concept_id	target_vocabulary_id
SG	visit_concept_id	9202(outpatient)	Visit
SRAG	visit_concept_id	9201(inpatient)	Visit
Vaccines	visit_concept_id	38004338(pharmacy)	NUCC
	visit_type_concept_id	32817(EHR)	Type Concept
Table 3: Details of the type of visit and type of record for Visit Occurrence

<h6>Condition Occurrence Table</h6>
<p>The Condition Occurrence table records clinical events that reflect the presence of a disease, illness, or symptom in a patient’s medical history. These events can be documented either through a diagnosis provided by a healthcare professional or through symptoms and signs reported by the patient. In the OMOP CDM framework, this table plays a central role in standardizing heterogeneous clinical information into structured, comparable data.</p>

<p>Both the SRAG and SG databases provide information on signs and symptoms, though with distinct formats. In SRAG, these data are captured partly as one-hot encoded attributes and partly within an unstructured free-text field. The coded attributes can be directly extracted and mapped to OMOP, while the free-text entries require pre-processing to ensure consistency and structure. In SG, signs and symptoms are stored exclusively in a single free-text field, where terms are listed and separated by commas. This format likewise requires pre-processing before integration into OMOP.</p>

<p>To process free-text data, the metaphone algorithm was applied, which reduces variability caused by phonetic differences in spelling. This phoneme-based approach enabled consolidation of multiple lexical variants into unified terms. In addition, n-gram text analysis was employed to capture compound terms, thereby improving recognition of multi-word expressions. Together, these methods addressed common challenges such as synonyms, abbreviations, spelling errors, and linguistic variation in Brazilian Portuguese. Following this normalization, free-text fields were converted into structured, one-hot encoded attributes.</p>

<p>The structured signs and symptoms obtained from both SRAG and SG were then mapped to standardized OMOP concepts using the tool Usagi (see Tables 4 and 5). This ensured semantic interoperability and compatibility with OHDSI vocabularies such as SNOMED.</p>

<p>Each Condition Occurrence is linked to the corresponding person_id from the Person table, preserving consistency across the dataset. A unique condition_occurrence_id is generated for every record through sequential numbering. Since the data originate from routine health information systems, the attribute condition_type_concept_id is defined as <i>EHR</i>. The condition_start_date is determined according to the available information, typically the notification date or, when present, the date of symptom onset.</p>



Condition Occurrence mapping

Attribute	Category	OMOP attribute	target_concept_id	target_vocabulary_id
Symptom	Fever	condition_concept_id 	437663	SNOMED
Symptom	Cough	condition_concept_id 	254761	SNOMED
Symptom	Sore throat	condition_concept_id 	259153	SNOMED
Symptom	Dyspnea	condition_concept_id 	312437	SNOMED
Symptom	Respiratory discomfort	condition_concept_id 	4089221	SNOMED
Symptom	oximetry<95% 	condition_concept_id 	40493512	SNOMED
Symptom	Diarrhea	condition_concept_id 	196523	SNOMED
Symptom	Vomiting	condition_concept_id 	441408	SNOMED
Symptom	Abdominal pain	condition_concept_id 	200219	SNOMED
Symptom	Fatigue	condition_concept_id 	4223659	SNOMED
Symptom	Loss of smell \ anosmia	condition_concept_id 	4185711	SNOMED
Symptom	Loss of taste \ ageusia	condition_concept_id 	4289517	SNOMED
Symptom	Nausea	condition_concept_id 	31967	SNOMED
Symptom	Headache	condition_concept_id 	378253	SNOMED
Symptom	Mialgia	condition_concept_id 	442752	SNOMED
Symptom	Runny nose	condition_concept_id 	4276172	SNOMED
Symptom	Asthenia	condition_concept_id 	79908	SNOMED
Symptom	Forgetfulness	condition_concept_id 	4206332	SNOMED
Symptom	Neurological changes	condition_concept_id 	4318660	SNOMED
Table 4: Description of each symptom and its respective standard vocabulary code



Mapeamento Condition Ocurrence

Variable	Category	OMOP attribute	target_concept_id	target_vocabulary_id
Comorbidities	Chronic cardiovascular disease
	condition_concept_id 	4028244	SNOMED
Comorbidities	Chronic hematological disease	condition_concept_id 	443723	SNOMED
Comorbidities	Down's syndrome	condition_concept_id 	439125	SNOMED
Comorbidities	Chronic liver disease	condition_concept_id 	4212540	SNOMED
Comorbidities	Asthma	condition_concept_id 	317009	SNOMED
Comorbidities	DM	condition_concept_id 	201820	SNOMED
Comorbidities	Chronic neurological disease	condition_concept_id 	4134145	SNOMED
Comorbidities	Other chronic lung disease	condition_concept_id 	4186898	SNOMED
Comorbidities	Immunodeficiency or immunodepression	condition_concept_id 	4153516	SNOMED
Comorbidities	Chronic kidney disease	condition_concept_id 	46271022	SNOMED
Comorbidities	Obesity	condition_concept_id 	4215968	SNOMED
Comorbidities	Former smoker	condition_concept_id 	903651	OMOP Extension
Comorbidities	Smoker	condition_concept_id 	903657	OMOP Extension
Comorbidities	Postpartum woman	condition_concept_id 	440465	SNOMED
Comorbidities	Hypertension	condition_concept_id 	316866	SNOMED
Comorbidities	Parkinson	condition_concept_id 	4140090	SNOMED
Comorbidities	Schizophrenia	condition_concept_id 	435783	SNOMED
Comorbidities	Depression	condition_concept_id 	440383	SNOMED
Comorbidities	Epilepsy	condition_concept_id 	380378	SNOMED
Comorbidities	Rheumatoid arthritis	condition_concept_id 	80809	SNOMED
Comorbidities	Alcoholic	condition_concept_id 	4218106	SNOMED
Comorbidities	Lymphoma	condition_concept_id 	432571	SNOMED
Comorbidities	Alzheimer's	condition_concept_id 	378419	SNOMED
Comorbidities	Breast cancer	condition_concept_id 	4112853	SNOMED
Comorbidities	Cardiopath	condition_concept_id 	4134586	SNOMED
Comorbidities	Hypothyroidism	condition_concept_id 	140673	SNOMED
Table 5: Description of each comorbidity and its standard vocabulary code



<h6>Drug Exposure Table</h6>
<p>The Drug Exposure table captures information on instances where a patient is exposed to a pharmacological product, defined as any substance ingested, injected, or otherwise introduced into the body. In OMOP CDM, this domain encompasses a wide range of therapeutic products, including prescription medications, over-the-counter drugs, vaccines, and large-molecule biological therapies. Each record in this table represents a discrete exposure event, contextualized with details about the product, dosage, route of administration, and date.</p>

<p>In the case of the VigiVac project, drug exposures correspond primarily to COVID-19 vaccination records. Four vaccines were administered in Brazil during the study period, each associated with a specific manufacturer:</p>
<ul>
	<li>AstraZeneca (ChAdOx1 nCoV-19)</li>
	<li>CoronaVac (Sinovac/Butantan)</li>
	<li>Pfizer-BioNTech (BNT162b2, BioNTech/Fosun Pharma/Pfizer)</li>
	<li>Janssen (Ad26.COV2.S, Janssen-Cilag)</li>
</ul>

<p>To ensure semantic alignment with OMOP standards, vaccines recorded in Brazilian information systems were mapped to international vocabularies. This process involved cross-referencing national identifiers with RxNorm, CVX, and other controlled terminologies distributed by OHDSI. The mapping process considered not only the vaccine names but also their dosage forms, concentrations, target populations, manufacturers, and compositions, thereby ensuring precise and unambiguous representation.</p>

<p>The mapping was carried out using a combination of National Drug Codes (NDC), where available, and the official labels of vaccines applied in Brazil, as documented in national vaccination records. By reconciling these data sources, we established accurate equivalences between Brazilian coding practices and OMOP standard concepts (see Table 6).</p>

<p>This careful alignment is essential for enabling federated analyses across countries: by representing vaccine exposures with shared vocabularies, it becomes possible to compare effectiveness, safety, and equity of vaccination programs in a harmonized manner.</p>



Drug Exposure Mapping

Variable	Category	variável omop	target_concept_id	target_vocabulary_id
Vaccine	AstraZeneca	drug_concept_id	724905	CVX
Vaccine	Coronavac-Sinovac/Butantan	drug_concept_id	36941610	RxNorm Extension
Vaccine	BioNTech/Fosun Pharma/Pfizer	drug_concept_id	37003436	RxNorm
Vaccine	Ad26.COV2.S - Janssen-Cilag
	drug_concept_id	739906	RxNorm
Table 6: Description of each vaccine and its correspondent standard code



<h6>Measurement Table</h6>
<p>The Measurement table in OMOP CDM is designed to store structured records that capture quantitative or categorical values obtained through standardized clinical assessments. These measurements typically arise from laboratory tests, diagnostic procedures, physiological recordings (e.g., vital signs), or pathological examinations. Unlike general observations, measurements are distinguished by the fact that they derive from a systematic test or procedure that yields a reproducible result. In OMOP, both the order of the test and the resulting value—when available—are represented in this table. If a test is documented without a result, it is assumed that the procedure was performed but that the outcome was not recorded.</p>

<p>For the VigiVac data, the Measurement table is populated primarily with records of COVID-19 diagnostic tests. Both the e-SUS Notifica (SG) and SRAG databases contain information about laboratory testing, making them the key sources for this domain. Specifically, two types of tests were systematically extracted:</p>
<ul>
	<li><b>RT-PCR tests</b> for SARS-CoV-2 RNA detection.</li>
	<li><b>Antigen tests</b> for SARS-CoV-2 protein detection.</li>
</ul>

<p>Prior to integration into the OMOP framework, each test and its possible outcomes had to be mapped to standardized concepts in the OMOP vocabulary system (see Table 7). For example, RT-PCR tests were aligned with LOINC concepts that represent nucleic acid detection, while test results were mapped to standardized answer concepts such as <i>Positive</i> or <i>Negative</i>. This semantic harmonization ensures that test results are directly comparable across institutions and countries, supporting downstream analyses such as vaccine effectiveness studies.</p>

<p>Through this mapping and structuring process, the Measurement table provides a reliable representation of diagnostic evidence, reinforcing the validity of subsequent analyses that depend on laboratory-confirmed outcomes of COVID-19 infection.</p>



Measurement Mapping

Attribute	Category	OMOP attribute	target_concept_id	target_vocabulary_id
Covid test	SARS-CoV+SARS-CoV-2 (COVID-19) Ag [Presence] in Respiratory system specimen by Rapid immunoassay	measurement_concept_id	757685	LOINC
Covid test	SARS-CoV-2 (COVID-19) RNA in Respiratory specimen by NAA with probe detection	measurement_concept_id	3964944	LOINC
Result	Positive	value_as_concept_id	45878584	LOINC
Result	Negative	value_as_concept_id	45878583	LOINC
–	–	measurement_type_concept_id	32817(EHR)	Type Concept

Table 7: Description of the measurement mapping



<p>Each record in the Measurement table must be explicitly linked to the corresponding healthcare encounter through the visit_occurrence_id, retrieved from the Visit Occurrence table. This linkage ensures that laboratory tests and their results are properly contextualized within the broader clinical trajectory of the patient. In addition, the date of measurement must be recorded. When available, this date is taken directly from the SRAG or SG databases; however, in cases of missing information, the measurement date can be inferred based on related clinical events (e.g., notification date or hospitalization period).</p>

<p>To maintain relational integrity and ensure unique identification of each event, every measurement is assigned a system-generated measurement_id, created through sequential numbering. This unique identifier is essential not only for distinguishing individual test results but also for enabling downstream integration with other OMOP domains and analytical pipelines.</p>
