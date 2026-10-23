# Motor Accident News Automation Pipeline

**An n8n-based news extraction and business automation system**

## Overview

This project demonstrates an automation pipeline designed for the Motor Accident Claims (MAC) domain. It processes accident-related news published across multiple regional e-papers and editions, converting unstructured news content into organized information that can support accident-claim identification and follow-up.

The system addresses a practical business challenge: manually monitoring multiple newspaper editions is time-consuming, repetitive, and difficult to scale.

## Business Problem

Accident-related reports are published across multiple regional newspapers and editions. Identifying relevant reports, organizing the information, and avoiding duplicate records requires considerable manual effort.

The objective was to automate this process through a repeatable workflow.

## Solution

I designed and implemented an n8n-based pipeline that automates news collection and processing, structures the extracted information, and applies deduplication logic.

### Key Capabilities

- Automated processing of accident-related news from multiple e-paper editions.
- Structured extraction and processing of news information.
- Deduplication using accident location and publication date.
- Automated workflow orchestration using self-hosted n8n.
- Workflow performance optimization to reduce execution time and AI token consumption.
- Support for downstream business processes related to motor accident claims.

## Technology Stack

- **Workflow orchestration:** n8n
- **Automation infrastructure:** Self-hosted environment
- **Data processing:** Workflow-based extraction, transformation and validation
- **AI-assisted processing:** Where applicable within the pipeline
- **Business domain:** Motor accident claims intelligence

## Performance Optimization

The initial execution took more than 55 minutes. Following workflow optimization, execution time was reduced to approximately 25 minutes in the reported run.

This represents a reduction of more than 50% in execution time.

The optimization objective was to reduce unnecessary processing and token consumption while retaining the required business outcome.

The accompanying technical documentation describes the optimization approach and the evidence needed to validate the results.

## Engineering Approach

My approach combines business analysis, solution architecture, workflow engineering, and iterative optimization.

Rather than treating this as a basic scraping task, I designed it as an end-to-end business process—from collecting unstructured information to preparing organized data for downstream use.

## My Contribution

I was responsible for designing and developing the automation workflow, defining the data-processing and deduplication approach, and optimizing execution performance.

This project demonstrates my practical experience in workflow automation, business process design, technical problem-solving, and building automation systems for real operational requirements.

## Project Evidence

The `assets/` directory will contain sanitized workflow screenshots, an architecture diagram, and execution evidence where available.

Production credentials, private endpoints, personal information, and confidential workflow exports are intentionally excluded from this public portfolio.

## About Me

**Vinayak Bhogan — AI Solutions Architect**

I bring 29+ years of experience in IT and product development, with a focus on AI automation, n8n workflows, AI-powered business systems, solution architecture, and SaaS platforms.

- LinkedIn: https://www.linkedin.com/in/vinayakbhogan

## Disclaimer

This repository documents the project and its engineering approach. It does not contain the complete production workflow or confidential implementation details.
