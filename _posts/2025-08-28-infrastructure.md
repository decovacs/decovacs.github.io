---
layout: post
title: "Infrastructure"
date: 2025-08-26 08:04:00 +0300
tags: report
categories: [report]
author: Decovacs
post_image: "/assets/images/blog/blog-15.jpg"
author: "Decovacs"
---

<h5>Introduction</h5>
<p>The data processing pipeline for this project was deployed on a high-performance virtual machine (VM) equipped with 32 CPU cores and 189 GB of RAM, providing the computational capacity necessary to handle the scale and complexity of VigiVac data. To ensure reproducibility, portability, and simplified environment management, core components of the infrastructure were encapsulated using Docker containers.</p>
<p>Wherever feasible, we leveraged tools and packages developed and maintained by the OHDSI community, in line with best practices for implementing the OMOP Common Data Model (CDM).</p>
<p>For the Extract, Transform, and Load (ETL) processes, we adopted Python as the primary programming language, in combination with the Apache Spark framework, which supports efficient distributed processing of large-scale datasets. The project database was implemented in PostgreSQL version 15.2, selected for its open-source nature, high performance, scalability, and native compatibility with OMOP.</p>
<p>Because a number of OHDSI tools are developed in R, we also integrated R version 4.3.1 into the workflow. Specifically, the CommonDataModel R package<sup>1</sup> was used to generate the OMOP CDM schema within PostgreSQL.</p>
<p>The implementation followed the OMOP CDM version 5.4<sup>2</sup>, the latest fully supported release, ensuring compatibility with the full suite of OHDSI analytical tools. The PostgreSQL instance, running inside a Docker container within the VM, hosted the CDM schema created by the R package.</p>
<p>Finally, the transformation of the three heterogeneous data sources (SG, SRAG, and PNI) into the OMOP CDM tables was carried out through custom ETL scripts written in Python + Spark, with Jupyter Notebooks serving as the primary interface for documentation, iterative development, and transparency of the data transformation process.</p>

<blockquote class="blockquote single-quote">
  <p> <sup>1</sup><a href="https://github.com/OHDSI/CommonDataModel/" target="_blank">https://github.com/OHDSI/CommonDataModel/</a></p>
</blockquote>
 <blockquote class="blockquote single-quote">
  <p> <sup>1</sup><a href="https://ohdsi.github.io/CommonDataModel/cdm54.html" target="_blank"> https://ohdsi.github.io/CommonDataModel/cdm54.html</a></p>
</blockquote>