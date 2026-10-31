
# VERITY — Database Design

## 1. Database Overview

Database: PostgreSQL 18
ORM: SQLAlchemy
Migrations: Alembic
Primary Keys: UUID
Timestamps: UTC

## 2. Identity and Organisation

### users
- id
- name
- email (unique)
- password_hash
- status
- created_at
- updated_at

### organizations
- id
- name
- slug (unique)
- created_at

### memberships
- id
- organization_id (FK)
- user_id (FK)
- role
- status
- created_at

Unique constraint: organization_id + user_id

## 3. Brand Management

### brands
- id
- organization_id (FK)
- name
- industry
- description
- created_at

### brand_guidelines
- id
- brand_id (FK)
- target_audience
- voice_description
- formality
- created_at
- updated_at

### brand_voice_dimensions
- id
- guideline_id (FK)
- dimension
- score (0–100)

### brand_terms
- id
- brand_id (FK)
- term
- category
- replacement

### brand_policies
- id
- brand_id (FK)
- name
- condition
- severity
- enabled

### brand_claims
- id
- brand_id (FK)
- claim_text
- approved_context
- source_reference

## 4. Brand Assets

### brand_assets
- id
- brand_id (FK)
- name
- file_key
- asset_type
- created_at

### asset_versions
- id
- asset_id (FK)
- version_number
- checksum
- created_at

### asset_rules
- id
- brand_id (FK)
- asset_id (FK)
- placement
- usage_rules

## 5. Campaign Management

### reviews
- id
- organization_id (FK)
- brand_id (FK)
- created_by (FK)
- name
- status
- created_at
- updated_at

### uploaded_files
- id
- review_id (FK)
- filename
- storage_key
- mime_type
- checksum
- upload_status
- created_at

### extracted_content
- id
- file_id (FK)
- extracted_text
- extraction_method
- status
- created_at

### content_segments
- id
- extracted_content_id (FK)
- text
- page_number
- start_offset
- end_offset

## 6. AI Analysis

### analysis_runs
- id
- review_id (FK)
- engine
- version
- status
- started_at
- completed_at

### findings
- id
- review_id (FK)
- analysis_run_id (FK)
- type
- severity
- status
- title
- description
- source_engine
- confidence
- created_at

### finding_locations
- id
- finding_id (FK)
- file_id (FK)
- page_number
- start_offset
- end_offset

### suggested_corrections
- id
- finding_id (FK)
- original_text
- suggested_text
- explanation

## 7. Evidence Intelligence

### claims
- id
- review_id (FK)
- finding_id (FK)
- claim_text

### evidence_sources
- id
- url
- title
- publisher
- published_at

### evidence_records
- id
- claim_id (FK)
- source_id (FK)
- excerpt
- retrieved_at

### claim_assessments
- id
- claim_id (FK)
- state
- explanation
- assessed_at

### evidence_search_runs
- id
- claim_id (FK)
- query
- provider
- status
- executed_at

## 8. Human Review

### reviewer_assignments
- id
- review_id (FK)
- assigned_to (FK)
- assigned_by (FK)
- status

### reviewer_decisions
- id
- finding_id (FK)
- reviewer_id (FK)
- decision
- rationale
- created_at

### review_approvals
- id
- review_id (FK)
- approver_id (FK)
- decision
- rationale
- created_at

### comments
- id
- review_id (FK)
- user_id (FK)
- message
- created_at

## 9. Governance and Audit

### audit_events
- id
- organization_id (FK)
- actor_id (FK)
- action
- entity_type
- entity_id
- metadata
- created_at

### policy_versions
- id
- brand_id (FK)
- version
- policy_snapshot
- created_at

### notifications
- id
- organization_id (FK)
- user_id (FK)
- message
- read_at
- created_at

## 10. Database Rules

- Use UUID primary keys.
- Enforce foreign keys.
- Enforce unique email addresses.
- Enforce unique organisation slugs.
- Enforce unique organisation memberships.
- Preserve previous reviewer decisions.
- Keep audit events append-only.
- Store uploaded files outside the database.
- Store policy versions for reproducibility.
- Scope organisation-owned records to their organisation.
- Store timestamps in UTC.
