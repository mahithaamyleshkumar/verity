
# VERITY — API Contracts

Base URL: /api/v1

## Authentication
POST /auth/register
POST /auth/login
POST /auth/logout
GET  /auth/me

## Organisations
POST /organizations
GET  /organizations
GET  /organizations/{id}/members
POST /organizations/{id}/invitations

## Brands
POST /brands
GET  /brands
GET  /brands/{id}
PUT  /brands/{id}
PUT  /brands/{id}/guidelines
POST /brands/{id}/terms
POST /brands/{id}/policies
POST /brands/{id}/assets

## Reviews
POST /reviews
GET  /reviews
GET  /reviews/{id}
POST /reviews/{id}/upload
POST /reviews/{id}/analyse
GET  /reviews/{id}/status
GET  /reviews/{id}/findings
POST /reviews/{id}/approve
POST /reviews/{id}/reject

## Findings
GET  /findings/{id}
POST /findings/{id}/decision
POST /reviews/{id}/comments
POST /reviews/{id}/assign

## Evidence
GET  /claims/{id}
GET  /claims/{id}/evidence
POST /claims/{id}/verify
POST /claims/{id}/decision

## Governance
GET  /audit
GET  /analytics/overview
GET  /reviews/{id}/report
GET  /reviews/{id}/trust-graph

## Health
GET /health
GET /api/v1/health

## Standard Success Response

{
  "success": true,
  "data": {},
  "meta": {
    "request_id": "req_001"
  }
}

## Standard Error Response

{
  "success": false,
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "Resource not found."
  }
}
