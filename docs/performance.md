# MAC News Automation Pipeline — Performance Notes

## Summary

The MAC news automation workflow processes multiple regional newspaper sources and uses conditional processing plus AI-assisted classification/extraction to identify relevant motor-accident reports.

The available execution-history screenshot shows successful runs with the following durations:

| Execution date shown | Status | Duration |
|---|---|---:|
| 16 Oct 2026 | Succeeded | 21m 57.463s |
| 15 Oct 2026 | Succeeded | 21m 35.946s |
| 14 Oct 2026 | Succeeded | 19m 44.437s |
| 13 Oct 2026 | Succeeded | 22m 57.669s |
| 12 Oct 2026 | Succeeded | 28m 24.214s |
| 11 Oct 2026 | Succeeded | 20m 39.501s |
| 10 Oct 2026 | Succeeded | 21m 53.531s |

These are the durations visible in the supplied screenshot. They demonstrate that the workflow has completed successfully in roughly 20–22 minutes in several runs, with the shown sample ranging from about 19m 44s to 28m 24s.

## Optimization history — evidence limitation

The developer reports that an earlier version took more than 55 minutes and that subsequent optimization reduced execution time and AI token consumption. However, screenshots or execution records showing the original baseline are not available.

For that reason, the **greater-than-55-minute baseline is a user-reported historical figure, not independently verified by the surviving evidence**. Do not present a precise before/after percentage as independently measured unless the original execution records, logs, or another reliable baseline become available.

Suggested portfolio wording:

> I optimized the workflow to reduce unnecessary processing and AI token consumption. The available execution history shows successful runs of approximately 19m 44s to 28m 24s, with several runs around 20–22 minutes. The earlier execution baseline was reported to exceed 55 minutes, but the original execution evidence is no longer available.

## Optimization techniques visible in the workflow

The workflow configuration supports describing these as implementation techniques, without claiming that any one technique accounts for a measured share of the speed-up:

- **Early filtering:** the Sakal path checks for selected Marathi accident-related keywords before invoking AI analysis.
- **Conditional AI processing:** IF and Filter nodes determine which candidate items proceed to deeper classification or extraction.
- **Image-size checks:** the workflow includes conditions that filter or route images based on file size.
- **Item/page limits:** some branches include item-count or page-selection conditions.
- **Targeted extraction prompts:** Gemini prompts instruct the model to focus on relevant accident reports, avoid irrelevant content, and return structured JSON.
- **Response parsing and filtering:** Code, IF, and Filter nodes parse AI results and retain qualifying accident records.
- **Retry/error handling:** selected request and AI nodes have retry and/or continue-on-error behavior.

These are observations from the workflow configuration. To attribute the improvement quantitatively, compare controlled runs with the same sources, date range, candidate volume, and output requirements.

## Recommended future benchmark

For a defensible benchmark, record at least three runs for each workflow version under comparable conditions:

1. Workflow version or commit/export checksum.
2. Run start time and total duration.
3. Number of newspaper editions/pages and candidate articles.
4. Number of images downloaded and analyzed.
5. AI model and total input/output token usage, where available.
6. Number of qualifying accident records.
7. Number of failed/retried requests and AI calls.
8. Any changes to filtering, limits, or source availability.

Compare median duration and token usage, not just a single fastest run. Also check that the optimized version retains comparable detection quality and does not silently miss accident reports.

## Public portfolio and privacy

The public repository should contain documentation and sanitized evidence only. Do not upload the raw production workflow export, credentials, API keys, private endpoints, client IDs, or identifiable victim records.

The uploaded workflow export appeared to contain a Clappia API key. Treat that key as exposed and rotate it before further sharing. Remove it from any public-facing artifacts and check other workflow nodes for credentials or private configuration before publishing screenshots.
