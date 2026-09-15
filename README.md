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

### DAST & Scanning Engine

Four scan profiles — quick, standard, deep, full — with configurable crawl depth (1–5), page concurrency, same-domain enforcement, form/script/parameter extraction, technology fingerprinting (31 patterns), 117 special-path probes, and ZAP integration. Results persist to tool_executions, tool_results, saved_results, and Phase 5 findings.

### Vulnerability Detection — 22 Check Families

| Category | Families | Methods |
|---|---|---|
| **Injection** | SQLi (6 DBMS), XSS, LFI, SSRF, RCE, CMDi, SSTI | Error-based, time-based, boolean blind, UNION, content indicators, response size diff, DOM sink matching |
| **Auth & Access** | JWT attacks, CSRF, OAuth, session attacks, password spraying, MFA bypass, token manipulation | 8-auth-category family with response status + body analysis |
| **Web & API** | CORS (15 origin probes), open redirect (24 payloads), GraphQL introspection/batching/DoS, WebSocket, SOAP/SSE, XXE, prototype pollution, unsafe deserialization | Origin reflection, redirect chain analysis, schema introspection, SSTI pattern matching |
| **Cloud & Infra** | Cloud metadata (AWS/GCP/Azure), S3 enumeration, container escape, K8s discovery, IAM misconfig | Target-specific payloads with response fingerprinting |
| **Discovery** | Admin panels, backup files, hidden files, API endpoints, debug paths, config exposures, VCS, logs | 100+ common paths with HTTP 200 + body analysis |
| **Headers & Config** | CSP, HSTS, X-Frame-Options, X-Content-Type-Options, Referrer-Policy, Permissions-Policy, cookie flags (Secure/HttpOnly/SameSite), CORS wildcard | Header presence + value validation |
| **Compliance** | PCI DSS, HIPAA, GDPR, SOC2, ISO 27001 | Security control presence validation |

**Payload count:** 1,500+ injection vectors across 22 check files (9 individual + 13 families from payloads.json).

### Attack-Surface Discovery

BFS crawling (depth 1–5, configurable concurrency) extracting forms, scripts, parameters, and links. Technology fingerprinting covering 31 platforms and frameworks. 117 special paths probed. Surface enumerator reads DB entities and tool outputs to produce prioritized targets using scoring based on port count, endpoint density, API endpoints, and finding severity.

### WAF Detection & Evasion

Dedicated WAF engine with fingerprinting, evasion payload generation (encoding, case mutation, parser-aware variants), circuit breaker monitoring, and automated defense rule generation from scan findings. Accessible through a dedicated UI with 18 engine controls. Backend modules: waf_detect.py, waf_bypass.py, waf_generator.py, waf_fingerprint.py, waf_block_detect.py.

### OAST — Out-of-Band Application Security Testing

Configurable OAST callback listeners (default port 5555) for blind vulnerability detection. Start/stop/poll APIs plus a collaborator system with URL generation and 3-second polling. Captured interactions display source IP, method, path, and protocol. Supports blind SSRF, blind XXE, OOB RCE verification.

### Authenticated Security Testing

Cookie-based, token-based (JWT/Bearer), and HTTP Basic Auth strategies. Login form auto-detection from HTML. Auth config auto-detection from target URL. Extracted sessions replayed through the crawl and scan pipeline. Flask backend provides WebAuthn MFA (graceful degradation), Argon2id password hashing.

### Evidence & Verification

Every finding carries a complete evidence chain: tool, target, captured response, and SHA-256 integrity digest over the finding plus its evidence linkage (computeEvidenceDigest). 6-state verification lifecycle: detected → needs-verification → verification-attempted → verified → false-positive → resolved. Confidence basis attributed to source (detector-recorded, investigator-recorded, or defaulted). Redaction pipeline covers 15 sensitive headers, token patterns, PEM key blocks, and recursive JSON secrets (max depth 12).

### Investigations & AI Analysis

27 API routes covering entity normalization (domain, host, ip, port, url), finding lifecycle (5 states), deterministic correlation from shared values, conflict detection, duplicate detection, timeline reconstruction, and access control with sharing. Evidence-grounded AI analysis (571 lines) with mandatory citations, tool registry validation. AI copilot v2 adds hypotheses, evidence categories, 7 action types. STRiX agent integrates DeepSeek v4 Flash with 6 plugin tools.

### Workflow Automation

Full workflow engine with 37+ declarative node types, 1,162-line persistent in-process executor, wave-by-wave scheduling, human approval gates (WAITING_APPROVAL), checkpoint-and-resume, failure strategies (stop/continue/retry/skip), secret injection at execution time, safe expression evaluation, and typed data pipeline. Workflow lifecycle: draft, published, archived.

### CLI & API

20 CLI API routes covering device auth flow, token login/logout, session health, whoami, finding CRUD with transitions, investigation CRUD, evidence listing, tool registry listing/search/metadata, tool execution, and workflow CRUD. Binary integrity checking (SHA-256 via x-cli-hash) and device fingerprint binding (403 on mismatch). REST API with 26 route groups and structured response envelopes.

### CI/CD & SARIF

SARIF 2.1.0 export (full schema: tool driver, ruleId, level mapping, physicalLocation/uri). Server-side endpoints (GET /scan/{id}/report.sarif, POST /api/enterprise-scan/sarif) and client-side generation. Severity-gated CI/CD exit codes (POST /api/cicd/exit-code). Baseline comparison for regression detection (POST /api/baseline/save, POST /api/baseline/diff). GitHub Code Scanning integration via SARIF upload to Security tab.

### External Tool Integrations — 22 Tools

Nmap, Gobuster, WPScan, Nikto, ffuf, sqlmap, httpx, WhatWeb, Dirb, Nuclei, Amass, Subfinder, Naabu, DNSRecon, WHOIS, Dig, Curl, OpenSSL, Host, Nslookup, Python3, Ping. Each implements the ToolIntegration contract: validateConfig → buildCommand → parseOutput → getHealth.

### Reporting & Export

Report builder with templates, sections, customizable branding, and export in PDF, HTML, JSON, and CSV formats with automatic secret pattern redaction. DB-derived insights and export via Phase 5 insights engine.

### Webhooks & Notifications

Webhook engine with 10 event types, delivery tracking, automatic retry, and HMAC-SHA256 signing. Notification dispatch system for investigation events, finding transitions, and tool execution completions.

### Audit, Access Control & Retention

Role-based permissions with investigation-level sharing (owner-only by default; read/write/admin shares). Activity log and audit trail with user actions and timestamps. Data retention policies, investigation archiving. Merkle-chained audit log on Flask backend. Admin API covering 26 route directories.

### Deployment Infrastructure

Deployment versioning (v9.0.0, schema version tracking, compatibility checks), in-process job queue, job scheduler, artifact storage, worker agent management, autoscaling, resource governor, observability pipeline (counters, gauges, span-based traces, SQLite storage). Flask backend (7,615 lines, 36 Python modules) with SSE streaming scan results and real scan execution.

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

## Reproducibility and evidence

**What exactly happened?** A useful finding should be inspectable. Nyxeara preserves the relevant request and response, records verification state, protects evidence integrity with SHA-256, and gives investigators enough context to understand the result rather than simply accepting a severity label.

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