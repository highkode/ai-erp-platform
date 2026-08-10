# High Kode AI ERP Platform — System Architecture

**Version:** 1.3  
**Status:** P0 Baseline  
**Repository:** `https://github.com/highkode/ai-erp-platform`

---

## 1. Purpose

This document defines the technical architecture of High Kode's AI-first
business operating platform.

The primary objective is not to build a generic SaaS platform first.

The objective is:

> **High Kode operates its own business through an AI-first operating system.**

High Kode is the first and most important customer of the system.

The architecture is designed so that proven internal capabilities can later
become reusable products.

---

# 2. Core Architecture Principles

The following principles are considered architectural invariants.

1. **ERPNext is the System of Record.**
2. **`high_kode_ai` is the AI Execution Layer.**
3. **`high_kode_integrations` is the deterministic Integration Layer.**
4. **AI autonomy belongs to Actions, not Agents.**
5. **Business Operations Data is separated from End-Customer Content.**
6. **High Kode is Customer Zero.**
7. **DingDongBot and AIWeb remain independent products.**
8. **Product Architecture is separate from Business Operating Architecture.**
9. **P0 focuses on internal operational proof.**
10. **Multi-tenancy is not a P0 feature.**
11. **Flutter AI interfaces are P0.5/P1, not P0.**
12. **`high_kode_ai` is independent from HUF.**
13. **Clean-room development applies to HUF-informed concepts.**
14. **Every autonomous capability must be explicitly registered as an Action.**
15. **All material actions must be auditable.**

---

# 3. High-Level Architecture

```text
                         HIGH KODE
                    AI-First Business
                           │
                           ▼
                    ┌───────────────┐
                    │    ERPNext    │
                    │ System of     │
                    │ Record        │
                    └───────┬───────┘
                            │
              ┌─────────────┴─────────────┐
              │                           │
              ▼                           ▼
   high_kode_integrations          high_kode_ai
   Integration Layer              AI Execution Layer
              │                           │
              │                           ├── Agents
              │                           ├── Planning
              │                           ├── Tools
              │                           ├── Actions
              │                           └── Runs
              │
              ▼
       External Products
       ├── DingDongBot
       └── AIWeb