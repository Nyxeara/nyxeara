<div align="center">
  <h1>Nyxeara</h1>
  <p><strong>Dynamic Application Security Testing — Evidence, not guesses.</strong></p>
  <p>
    <a href="https://nyxeara.vip">Website</a> ·
    <a href="https://nyxeara.vip/docs">Documentation</a> ·
    <a href="https://nyxeara.vip/capabilities">Capabilities</a> ·
    <a href="https://nyxeara.vip/developers">API</a> ·
    <a href="https://nyxeara.vip/security">Security</a>
  </p>
</div>

## What Nyxeara is

Nyxeara is a **dynamic application security testing (DAST) platform** for live web applications and APIs. It discovers attack surface, runs controlled security tests, verifies findings with cryptographic evidence integrity, and supports investigation, automation, and CI/CD delivery.

The platform is built around a simple distinction: **a possible vulnerability is not the same thing as a verified finding.**

> **Discover → Test → Verify → Prove**

## Capabilities

### Dynamic Application Security Testing (DAST)

Four scan profiles — quick, standard, deep, full — with configurable crawl depth (1–5), page concurrency, same-domain enforcement, form/script/parameter extraction, technology fingerprinting (31 patterns), and 117 special-path probes. Authenticated scanning supports cookie-based, token-based (JWT/Bearer), HTTP Basic Auth, and session replay workflows.

### Vulnerability Detection — 22 Check Families

| Category | Families | Detection Methods |
|---|---|---|
| **Injection** | SQLi (6 DBMS), XSS, LFI, SSRF, RCE, CMDi, SSTI | Error-based, time-based, boolean blind, UNION, content indicators, response size diff, DOM sink matching |
| **Auth & Access** | JWT attacks, CSRF, OAuth, session attacks, password spraying, MFA bypass, token manipulation, brute force | Response status + body analysis per crafted path/header |
| **Web & API** | CORS (15 origin probes), open redirect (24 payloads), GraphQL introspection/batching/DoS, WebSocket, SOAP/SSE, XXE, prototype pollution, unsafe deserialization | Origin reflection, redirect chain analysis, schema introspection, SSTI pattern matching |
| **Cloud & Infra** | Cloud metadata (AWS/GCP/Azure), S3 enumeration, container escape, K8s discovery, IAM misconfig, cloud login portals | Target-specific payloads with response fingerprinting |
| **Discovery** | Admin panels, backup files, hidden files, API endpoints, debug paths, config exposures, VCS, log files | HTTP 200 + body analysis on 100+ common paths |
| **Headers & Config** | CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, cookie flags (Secure/HttpOnly/SameSite), CORS wildcard | Header presence + value validation |
| **DNS** | Zone transfer, subdomain enumeration, DNS records, DNSSEC, wildcards | Google DNS-over-HTTPS resolution |
| **Compliance** | PCI DSS, HIPAA, GDPR, SOC2, ISO 27001 | Security control presence validation |

**Payload count:** 1,500+ injection vectors across individual checks (330+) and family payloads (1,200+ from `payloads.json`).

### WAF Detection & Evasion

Dedicated WAF engine with fingerprinting, evasion payload generation (encoding, case mutation, parser-aware variants), circuit breaker monitoring, and automated WAF rule generation from scan findings. Accessible through a dedicated UI with 18 engine controls.

**Evidence:** `POST /api/waf-engine/fingerprint`, `POST /api/waf-engine/evasion`, `GET /api/waf-engine/findings`, `GET /api/waf-engine/circuit-breaker`, `POST /api/defense/waf-rules`, `POST /api/waf-engine/start`

### OAST — Out-of-Bound Application Security Testing

Configurable OAST callback listeners for detecting blind and deferred vulnerabilities (blind SSRF, XXE, SQLi, RCE). The collaborator system generates callback URLs, polls for interactions at 3-second intervals, and displays source IP, method, path, and protocol per captured callback.

**Evidence:** `POST /api/oast/start`, `GET /api/oast/callbacks`, `POST /api/oast/stop`, `GET /api/v2/collaborator/generate`, `GET /api/v2/collaborator/poll`

### External Tool Integrations — 22 Tools

Nmap, Gobuster, WPScan, Nikto, ffuf, sqlmap, httpx, WhatWeb, Dirb, Nuclei, Amass, Subfinder, Naabu, DNSRecon, WHOIS, Dig, Curl, OpenSSL, Host, Nslookup, Python3, Ping. Each implements the `ToolIntegration` contract: `validateConfig → buildCommand → parseOutput → getHealth`.

### Evidence & Verification

Every finding carries a complete **evidence chain**: the tool, the target, the captured response, and a **SHA-256 integrity digest** over the finding plus its evidence linkage. The verification lifecycle (6 states) is user-controlled — the scanner does not self-claim "verified." Confidence is attributed to its source (detector-recorded, investigator-recorded, or defaulted). Evidence passes through a redaction pipeline (15 sensitive headers, token patterns, PEM keys) before leaving the server.

**Stages:** `detected → needs-verification → verification-attempted → verified → false-positive → resolved`

### Investigation & AI Analysis

Full investigation platform with entity normalization (domain, host, ip, port, url), finding lifecycle management (5 states), deterministic correlation from shared values, conflict detection, duplicate detection, and evidence-grounded AI analysis with mandatory citations. Recommendations reference only real tools from the tool registry.

AI copilot (v2) adds hypotheses, evidence categories, 7 action types (investigate entity, run tool, compare evidence, etc.), and analysis persistence.

**Routes:** 27 investigation API routes covering full CRUD, events, timeline, graph, export, sharing, intelligence.

### Workflow Automation

Full workflow automation engine with **37+ declarative node types**, persistent in-process execution, wave-by-wave scheduling with bounded concurrency, human approval gates (`WAITING_APPROVAL` state), checkpoint-and-resume, failure strategies (stop/continue/retry/skip), secret injection at execution time, and safe expression evaluation.

**Lifecycle:** `draft → published → archived`

### CI/CD & SARIF

SARIF 2.1.0 export (full schema: tool driver, ruleId, level mapping, physicalLocation/uri), GitHub Code Scanning integration, severity-gated CI/CD exit codes, baseline comparison for regression detection, and signed webhooks.

**Evidence:** `POST /api/cicd/exit-code`, `POST /api/baseline/save`, `POST /api/baseline/diff`, `GET /scan/{id}/report.sarif`

### CLI

Full CLI with device auth flow, binary integrity checking (`x-cli-hash`), device fingerprint binding (403 on mismatch). Access to tools, investigations, findings, evidence, and workflows — all via REST.

**Routes:** 20 CLI API routes covering auth, devices, findings, investigations, evidence, executions, tools, workflows, health.

### Administration

26 admin route directories covering AI, artifacts, audit, billing, config, database, evidence, executions, investigations, jobs, logs, maintenance, notifications, overview, queues, roles, search, security, sessions, storage, system health, tools, users, workers, workflows, and workspaces.

### API Platform

- 26 route groups with structured response envelopes (`{ok, data, error, meta}`)
- API key management (`nx_` prefix, SHA-256 hash, scopes, expiration)
- Webhook engine (10 event types, delivery tracking, retry, HMAC-SHA256 signing)
- Report builder with PDF/HTML/JSON/CSV export and branding support
- Data export with automatic secret pattern redaction
- Configurable data retention policies
- Activity log / audit trail with user actions and timestamps
- Notification dispatch system

### God's Eye View

3D photorealistic geospatial globe with 8 live intelligence layers: flights (11,000+ ADS-B), ships (live AIS), satellites (838), CCTV (~800), radio (750+), fires (NASA FIRMS), submarine cables (712), and datacenters (4,351). Token-authenticated proxy with path rewriting.

## The engineering model

```text
Nyxeara
└── Vantage
    ├── discovery
    ├── crawling
    ├── dynamic security checks
    ├── authentication
    ├── verification
    ├── evidence
    └── analysis
```

**Nyxeara** is the product and platform. **Vantage** is the internal engine identity.

## Where it fits

- **DAST** — Test applications from the outside against running systems. Discover exposed functionality, exercise security checks, and retain the evidence behind meaningful results.
- **Offensive security** — Black-box workflow to examine the attack surface of web applications and APIs without source-code access.
- **Security research** — Observable behavior first, reproducible evidence second, conclusions after verification.
- **Engineering** — Security findings into developer workflows through API access, CLI, SARIF, CI/CD gates, signed webhooks, and investigation-oriented evidence.

## Evidence is the product boundary

**What exactly happened?** A useful finding should be inspectable. Nyxeara preserves the relevant request and response, records verification state, protects evidence integrity with SHA-256, and gives investigators enough context to understand the result rather than simply accepting a severity label.

## Responsible security

Nyxeara is intended for authorized security testing. Only scan systems you own or have explicit permission to test.

For vulnerability disclosures, see [`SECURITY.md`](SECURITY.md).

## Repository scope

This is the **flagship public Nyxeara repository**, separated from private infrastructure, deployment configuration, credentials, and customer data. Planned, experimental, private, or unverified capabilities are not presented as production features here.

## Explore

- **Product:** https://nyxeara.vip/
- **Capabilities:** https://nyxeara.vip/capabilities
- **Features:** https://nyxeara.vip/features
- **Documentation:** https://nyxeara.vip/docs
- **CLI:** https://nyxeara.vip/docs/cli
- **Security:** https://nyxeara.vip/security
- **API:** https://nyxeara.vip/developers
- **Research / blog:** https://nyxeara.vip/blog

## Status

Actively developed. The public surface represents the capabilities appropriate to document publicly; internal roadmaps and private implementation details are intentionally excluded.

<p align="center">
  <strong>NYXEARA</strong><br />
  <sub>Dynamic Application Security Testing · Offensive Security · Security Research</sub><br />
  <sub><em>Evidence, not guesses.</em></sub>
</p>