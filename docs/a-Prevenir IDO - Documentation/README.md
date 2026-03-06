---
sidebar_position: 1
---

# Index

> **Last Updated:** 2026-02-22
> **Total Documentation:** ~153KB, 5,064 lines across 8 files

---

## Overview

This documentation suite provides comprehensive architectural analysis of the A-Prevenir e-health kiosk system. It is designed to help senior programmers understand the system and make informed decisions about refactoring or rewriting.

### Quick Facts

| Attribute | Value |
|-----------|-------|
| **Project** | A-Prevenir IDO |
| **Organization** | CICESE |
| **Primary Language** | Java 8 |
| **UI Framework** | JavaFX 8 with FXML |
| **Database** | SQLite 3 |
| **Codebase Size** | ~17,088 lines of Java |
| **Architecture Style** | Modular Monolith with MVC |
| **Quality Score** | 1.9/5 ⚠️ |
| **Recommendation** | **Incremental Refactoring** |

---

## Documentation Files

| # | Document | Size | Lines | Description |
|---|----------|------|-------|-------------|
| 1 | [architecture.md](architecture.md) | 17KB | 448 | System overview, module inventory, architectural patterns, quality assessment |
| 2 | [module-interactions.md](module-interactions.md) | 16KB | 541 | 5 critical workflows with sequence diagrams, data flows, state machines |
| 3 | [design-decisions.md](design-decisions.md) | 21KB | 661 | 10 Architecture Decision Records (ADRs), trade-offs, refactoring roadmap |
| 4 | [api-overview.md](api-overview.md) | 24KB | 781 | Complete API reference for modules, controllers, services, and models |
| 5 | [hardware-integration.md](hardware-integration.md) | 20KB | 697 | 8 medical devices, communication protocols, HAL assessment |
| 6 | [testing-strategy.md](testing-strategy.md) | 26KB | 940 | Gap analysis, JUnit 5 setup, test templates, implementation roadmap |
| 7 | [getting-started.md](getting-started.md) | 16KB | 594 | Developer onboarding guide, environment setup, troubleshooting |
| 8 | [glossary.md](glossary.md) | 14KB | 402 | Domain terms, system terms, Spanish-English translations |

---

## Key Findings Summary

### Architecture Assessment

```
Quality Score: 1.9/5

Criterion                Score
─────────────────────────────────
Code Organization        3/5
Separation of Concerns   2/5
Testability              1/5  ⚠️
Documentation            1/5  ⚠️
Error Handling           2/5
Security                 2/5
Maintainability          2/5
```

### Critical Issues Identified

| Priority | Issue | Location | Severity |
|----------|-------|----------|----------|
| P1 | **UserData God Object** | `src/aPrevenir/Modelos/UserData.java` (1,099 lines) | 🔴 HIGH |
| P1 | **No Test Coverage** | `/test/` (empty directory) | 🔴 HIGH |
| P2 | **Static Utility Abuse** | `src/aPrevenir/Tools.java` | 🟡 MEDIUM |
| P2 | **Hardcoded Credentials** | Properties files | 🟡 MEDIUM |
| P3 | **No Dependency Injection** | Throughout codebase | 🟡 MEDIUM |

### Recommendation: Incremental Refactoring

**Why Refactor (not Rewrite):**
- System is functional and production-deployed
- Core business logic is sound
- Architecture problems are isolatable
- Rewrite risk exceeds refactoring risk

**Suggested Phases:**
1. Add Test Infrastructure (2-3 weeks)
2. Break Up God Objects (4-6 weeks)
3. Introduce Dependency Injection (3-4 weeks)
4. Security Hardening (1-2 weeks)
5. Modernization - Java 11+ (8-12 weeks, optional)

---

## Hardware Devices Documented

| Device | Spanish Name | Measurements |
|--------|--------------|--------------|
| Blood Glucose Meter | Glucómetro | Blood glucose (mg/dL) |
| Digital Scale | Báscula | Weight (kg) |
| Blood Pressure Monitor | Baumanómetro | Systolic/Diastolic (mmHg), Heart rate |
| Digital Thermometer | Termómetro | Temperature (°C) |
| Stadiometer | Estadímetro | Height (cm) |
| Digital Tape | Cinta Digital | Waist circumference (cm) |
| Lipid Analyzer | CardioCheck | TC, TG, HDL, LDL |
| ECG Device | ECG | Electrocardiogram |

---

## Reading Order

### For Architecture Review
1. [architecture.md](architecture.md) - Start here for system overview
2. [module-interactions.md](module-interactions.md) - Understand workflows
3. [design-decisions.md](design-decisions.md) - Review trade-offs and recommendations

### For Development
1. [getting-started.md](getting-started.md) - Environment setup
2. [api-overview.md](api-overview.md) - API reference
3. [glossary.md](glossary.md) - Term definitions

### For Hardware Work
1. [hardware-integration.md](hardware-integration.md) - Device details
2. [api-overview.md](api-overview.md) - Device interfaces

### For Quality Improvement
1. [testing-strategy.md](testing-strategy.md) - Test implementation plan
2. [design-decisions.md](design-decisions.md) - Refactoring priorities

---

## Diagram Compatibility

All documentation uses **Mermaid** diagrams, compatible with:
- GitHub Markdown rendering
- MkDocs with mermaid2 plugin
- GitLab
- Notion
- VS Code with Mermaid extension

---

## Document Conventions

- **File references**: Include path and line numbers (e.g., `src/aPrevenir/Tools.java:45-120`)
- **Cross-links**: Documents link to related sections
- **Severity ratings**: 🔴 HIGH, 🟡 MEDIUM, 🟢 LOW
- **Language**: Technical terms in English, Spanish terms preserved where meaningful
- **Timestamps**: Each document includes "Last Updated" date

---

## Contributing to Documentation

When updating documentation:
1. Update the "Last Updated" timestamp
2. Maintain cross-links between documents
3. Use Mermaid for diagrams
4. Include file path references for code
5. Update this README if adding new documents

---

*Documentation generated for A-Prevenir IDO architecture review - February 2026*
