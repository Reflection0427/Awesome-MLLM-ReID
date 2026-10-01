# Paper tracker and review workflow

## What runs automatically

Every Monday, `Discover MLLM-ReID Papers` searches arXiv, OpenAlex, and Crossref with the queries in `config/paper-tracker.json`. It normalizes titles, arXiv IDs and DOIs, merges duplicate sources, scores relevance, removes known papers, and updates `data/candidates.json` on a dedicated pull-request branch.

The tracker never writes an unreviewed result into `data/papers.json`, `README.md`, or `assets/frameworks/`.

## Optional repository settings

- Secret `OPENALEX_API_KEY`: raises OpenAlex request limits when an API key is available.
- Variable `PAPER_TRACKER_EMAIL`: identifies polite API traffic to OpenAlex and Crossref.
- In `Settings → Actions → General → Workflow permissions`, select **Read and write permissions** and allow GitHub Actions to create pull requests.

## Review and accept a candidate

Read the full paper. Reject false positives, duplicate preprint/conference versions, and works without a material large-model plus ReID/retrieval contribution. Verify the title, method name, venue, year, paper link, code link, and project link.

Select exactly one author-created Figure from the official PDF and pin that PDF version. Priority: end-to-end framework → architecture → pipeline → method overview. For surveys and benchmarks, use taxonomy → benchmark pipeline → dataset construction when the earlier choices do not exist. Do not select result plots, tables, third-party illustrations, blog images, AI-generated images, or recreated diagrams.

Record the SHA-256 (`shasum -a 256 paper.pdf`), one-based PDF page, and crop rectangle in PDF points. Preserve the full legend, subfigure labels, and arrows. Then accept the candidate:

```bash
python scripts/accept_candidate.py "unique title text" \
  --section "Core MLLM / LVLM ReID" \
  --method "MethodName" \
  --year 2026 \
  --venue "CVPR 2026" \
  --source-pdf-url "https://arxiv.org/pdf/0000.00000v1" \
  --source-pdf-sha256 "64-lowercase-hex-characters" \
  --figure-label "Figure 2" \
  --figure-kind "framework" \
  --figure-page 3 \
  --figure-crop "32,78,560,410" \
  --code-url "https://github.com/example/project"
```

The acceptance tool moves the candidate, verifies the pinned PDF hash, renders its PNG, and regenerates README. Finish with:

```bash
python scripts/extract_figure.py --all --verify-only
python scripts/generate_readme.py --check
python -m unittest discover -s tests -v
```

## Figure rights

Every README image carries its Figure label, official PDF link, and rights notice. Add a license link only when an explicit reusable license is known. Do not copy the full caption. Only whitespace cropping and lossless PNG compression are permitted; do not redraw, recolor, or add content.

## Rejecting a candidate

Delete it from `data/candidates.json` and add its title to `ignored_titles` in `config/paper-tracker.json` so it does not return on the next scan.
