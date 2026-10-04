# Brand Guardian

A private marketing workspace for evidence-led brand compliance reviews.

## Included
- Dashboard, editable brand profiles, guidelines, rule framework, critical failures and configurable weights.
- PDF, DOCX, PPTX and pasted-text ingestion with page, slide or paragraph references.
- Persisted reports, category scores, findings, suggested copy changes and reviewer decisions.
- D1-backed records and R2-backed original files; private Sites access.

## Assessment scope
This MVP checks confirmed prohibited and required phrases using a deterministic review provider. It does not call an AI model or assess logos, colour, typography, imagery, layout, accessibility or subjective tone automatically. Draft and manual rules do not affect scores. Reports explicitly disclose those limits. Sample reports are illustrative and separate from saved reviews.

No readable text in a scanned or image-only document produces an error with a pasted-text fallback. DOCX uses paragraph references rather than inferred page numbers. Original files are never modified.

## Architecture
- lib/types.ts: Brand, BrandGuideline, BrandRule, Asset, Review, ReviewFinding and ScoreCategory.
- lib/ingestion.ts: browser document extraction, with lazy-loaded PDF parsing.
- lib/review-engine.ts: guideline organisation and provider interface; injectable ReviewProvider.evaluate.
- lib/scoring.ts: penalties, applicable-category weights and critical override.
- lib/persistence.ts: prepared D1 queries; db/schema.ts and drizzle/ hold schema migrations.
- app/api/files: original-file storage in R2.
- app/api/workspace: brands, reviews and decisions.
- app/dashboard.tsx, brand-editor.tsx and report.tsx: separate product surfaces.

Asset familyId and version are retained to support future version comparison. Each report keeps its findings, sources and score weights as an immutable assessment snapshot; review decisions are recorded separately within findings.

## Development
Dependencies use pnpm. Run `node scripts/run-framework.mjs dev --port 5187` for the local preview, `node scripts/run-framework.mjs build` for a Worker build and `node node_modules/typescript/bin/tsc --noEmit` for type checking. Follow README migration instructions in the Sites starter documentation when preparing another local database. Sites applies production migrations during publishing.

## Verification
Type check and production build pass. Browser checks cover brand creation, pasted guidelines, rule confirmation, critical override, weight normalisation, source references, accepted decisions, reload persistence and mobile rendering. Document-format checks cover PDF, DOCX and PPTX extraction and original-file retrieval.
