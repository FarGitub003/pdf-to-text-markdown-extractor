# PDF to Text & Markdown Extractor

Give it PDF URLs, get clean text, Markdown and metadata for each one, ready for LLM and RAG pipelines. Reading order, columns, headings, lists, links and bookmarks are rebuilt. No OCR. You pay only for PDFs whose text was extracted.

**▶ Run it on Apify Store: https://apify.com/far003/pdf-to-text-markdown-extractor**

> This repository hosts documentation and examples; the Actor runs on the Apify platform.

## What it does

- Turns PDF URLs into clean text, Markdown and metadata, one dataset record per PDF, ready for an LLM, a vector store or a search index.
- Lines are rebuilt from the position of every word, columns are read one after the other, words hyphenated at the end of a line are joined, and repeated page headers, footers and page numbers are dropped.
- The Markdown keeps headings, lists, links and code blocks; the record also has page count, outline (bookmarks), links, dates, size and SHA-256.
- No OCR: a scanned PDF with no text layer is reported as `no_text`.
- The PDF is parsed as data: no JavaScript of the PDF is run. robots.txt is respected.

## Who it is for

- Teams that load PDFs (reports, filings, papers, manuals, specifications) into RAG pipelines.
- People who monitor a set of PDF documents and want to be told only about the ones that changed.
- Anyone who needs the same PDF facts in a stable shape.

## Example input

The prefilled input of the Actor. The record below is the one produced for the first PDF.

```json
{
    "urls": [
        "https://www.rfc-editor.org/rfc/rfc9309.pdf",
        "https://www.irs.gov/pub/irs-pdf/fw9.pdf"
    ]
}
```

## Example output

```json
{
  "url": "https://www.rfc-editor.org/rfc/rfc9309.pdf",
  "finalUrl": "https://www.rfc-editor.org/rfc/rfc9309.pdf",
  "status": "ok",
  "error": null,
  "charged": true,
  "httpStatus": 200,
  "fileName": "rfc9309.pdf",
  "sizeBytes": 177688,
  "sha256": "149bf3e7f8fa21eaab3cfacf425e624cc44aece2547f65b2f43b97d70511ef0d",
  "pageCount": 12,
  "pagesExtracted": 12,
  "pagesWithText": 12,
  "text": "…",
  "markdown": "…",
  "charCount": 22644,
  "wordCount": 3032,
  "language": null,
  "title": "RFC 9309: Robots Exclusion Protocol",
  "titleSource": "metadata",
  "subject": "This document specifies and extends the \"Robots Exclusion Protocol\" method originally defi…",
  "keywords": null,
  "producer": "WeasyPrint 56.1",
  "creationDate": "2022-09-10T00:39:45.000Z",
  "creationDateRaw": "D:20220909173945-07'00'",
  "pdfVersion": "1.7",
  "isTagged": false,
  "outline": [
    { "title": "RFC 9309", "level": 1, "page": 1 },
    { "title": "Robots Exclusion Protocol", "level": 1, "page": 1 },
    { "title": "Abstract", "level": 2, "page": 1 }
  ],
  "links": ["https://www.rfc-editor.org/rfc/rfc9309", "https://www.rfc-editor.org/info/rfc9309"],
  "hasForms": false,
  "likelyScanned": false,
  "truncated": false,
  "fullTextKey": null,
  "warnings": [],
  "change": null
}
```

## Use it from code

Set your token in the `APIFY_TOKEN` environment variable.

**Node.js** ([`apify-client`](https://docs.apify.com/api/client/js/))

```js
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('far003/pdf-to-text-markdown-extractor').call({
    "urls": [
        "https://www.rfc-editor.org/rfc/rfc9309.pdf",
        "https://www.irs.gov/pub/irs-pdf/fw9.pdf"
    ]
});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
console.log(items);
```

**Python** ([`apify-client`](https://docs.apify.com/api/client/python/))

```python
import os
from apify_client import ApifyClient

client = ApifyClient(os.environ["APIFY_TOKEN"])
run = client.actor("far003/pdf-to-text-markdown-extractor").call(run_input={
    "urls": ["https://www.rfc-editor.org/rfc/rfc9309.pdf", "https://www.irs.gov/pub/irs-pdf/fw9.pdf"]
})
items = client.dataset(run["defaultDatasetId"]).list_items().items
print(items)
```

## Pricing

Pay per result — see the current price on the Store page.

## Data source & licence

The input is the set of PDF URLs you provide. The Actor identifies itself honestly, reads robots.txt once per origin and for every redirect hop, refuses private or local network addresses, and removes credentials written inside a URL. Nothing is done to get around a refusal: walls and challenges are reported as `blocked`. You are responsible for having the right to process the documents you submit.

## Support

Open an issue in this repository or use the Issues tab on the Store page.

## Licence

The documentation and examples in this repository are released under the MIT licence (see `LICENSE`). The Actor itself is proprietary and runs on the Apify platform.
