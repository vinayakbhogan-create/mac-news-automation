# MAC News Automation Pipeline — Architecture

## Purpose

This document describes the architecture visible in the private n8n workflow named **Motor Accident Leads**. The workflow is designed to identify motor-accident news from regional e-paper sources, extract structured information, and prepare qualifying records for downstream lead handling.

The production workflow is private. This document is a portfolio-safe description; it intentionally excludes credentials, private client configuration, and the raw workflow export.

## High-level flow

```text
Scheduled or manual trigger
          |
          v
Load client configuration
          |
          v
Route work by newspaper source
     /        |        \
    v         v         v
 Lokmat      Sakal     Pudhari
 extraction  extraction extraction
    \         |         /
     \        |        /
      v       v       v
  AI-based accident identification
  + structured data extraction
              |
              v
  Parse / validate / filter results
              |
              v
       Merge qualifying records
              |
              v
  Attach victim count / prepare summary
              |
              v
     Downstream lead creation
```

This is a conceptual overview, not a promise that every run follows exactly the same path. The workflow contains conditional branches, retry paths, optional notifications, and some deactivated nodes.

## 1. Trigger and configuration

The workflow includes a schedule trigger and a manual execution trigger. It also loads client configuration and routes source-specific records into the relevant newspaper branch. The configuration determines business context such as the target districts used during accident screening.

For the public repository, do not publish actual client names, IDs, emails, API keys, or private service configuration.

## 2. Lokmat pipeline

The Lokmat branch includes these processing stages:

1. Set publication-related dates.
2. Request issue information for the selected date.
3. Generate candidate article identifiers.
4. Fetch article metadata and parse image references.
5. Check whether an image is available.
6. Add source/edition metadata and prepare the image for analysis.
7. Apply item-count and image-size conditions before image analysis.
8. Use Gemini image analysis to identify motor-accident reports and return structured JSON.
9. Parse the AI response, retain metadata, and filter for records marked as accidents.
10. Pass qualifying records to the downstream merge/processing path.

The workflow includes retry and continue-on-error settings on selected HTTP and AI nodes. These settings help the run continue through some failures, but they do not guarantee that every source request or AI analysis succeeds.

## 3. Sakal pipeline

The Sakal branch fetches e-paper article/page data, parses edition and publication-date information, and extracts article text and image references.

The visible workflow includes a preliminary text-keyword filter using Marathi accident-related terms before AI classification. Candidate items are then sent to a Gemini text-model node with instructions to:

- identify motor-accident news;
- ignore advertisements, promotions, non-accident reports, and old accident references;
- enforce the configured geographic scope;
- return structured JSON with an accident flag and extracted fields.

The AI response is parsed, and records marked as accidents are filtered for downstream processing. The branch also contains image/article extraction and data-merge logic.

## 4. Pudhari pipeline

The Pudhari branch fetches article/page data and parses article metadata, image URLs, and publication-date information. It applies file-size and page-selection conditions before image processing.

Candidate images are analyzed using Gemini image-analysis nodes with structured extraction instructions. The response is parsed, metadata is combined with the AI result, and accident records are filtered for downstream processing.

## 5. AI-assisted extraction

The workflow uses Google Gemini nodes for image and text analysis. The prompts request strict JSON output and instruct the model to identify motor-accident reports within configured districts while excluding irrelevant content.

The visible extraction schema includes fields such as:

- accident classification flag;
- newspaper source and edition;
- publication or detected date;
- accident location;
- headline;
- cleaned Marathi article text.

Other parts of the workflow and output may add further fields. AI extraction is not infallible; output should be validated and monitored for false positives, missed reports, malformed responses, and inconsistent location/date values.

## 6. Parsing, filtering, and merging

Code nodes parse model responses and attempt to recover from malformed JSON in selected paths. IF/Filter nodes retain records meeting the accident conditions. Merge nodes combine branch outputs, and an empty-record cleanup step removes empty objects.

The workflow also includes victim-count-related processing, a summary stage, and downstream lead-creation nodes. Some email/message nodes are present but deactivated in the exported workflow; their presence should not be represented as proof that notifications are currently active.

## 7. Deduplication

The project’s documented business rule is to use **accident location + publication date** as the deduplication key.

This rule is intended to reduce duplicate records that may appear across sources or editions. It is a practical heuristic, not a guarantee of perfect identity matching: the same location may have multiple accidents on one date, and the same accident may be described with different location spellings. A production implementation should normalize locations and review collision cases.

Only describe this as a deduplication rule where it is actually applied in the relevant live workflow path; the public-facing documentation should not imply a stronger guarantee than the implementation provides.

## 8. Downstream output

The workflow contains lead-creation nodes for Clappia and later-stage nodes related to victim count, summary, and an additional downstream integration. The exact client/workplace identifiers and endpoints are intentionally omitted from this public document.

## Operational considerations

- Monitor HTTP and AI errors even where nodes are configured to continue after failure.
- Validate JSON shape and required fields before creating a lead.
- Respect the publishers’ terms and access restrictions.
- Keep API keys and credentials in n8n credentials or a secrets manager, never in public source files.
- Avoid publishing personal or sensitive victim information in screenshots or sample records.
- Keep the production workflow export private unless it has been carefully sanitized and reviewed.

## Public portfolio contents

Recommended public contents are this document, a sanitized workflow overview image, a redacted sample schema, and performance notes. Do **not** commit the raw production workflow JSON.
