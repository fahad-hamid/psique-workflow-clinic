![preview](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/preview.svg)

# Sesión — Mental Health Practice Orchestration Platform

**Sesión** is a purpose-built SaaS operating system for psychology clinics and independent practitioners in Argentina, unifying clinical, administrative, and communication workflows into a single intelligent environment. This repository contains the core orchestration engine that coordinates appointment scheduling, automated WhatsApp messaging, billing and invoicing, secure video consultations, and an integrated AI layer powered by Claude Opus 4.6 and Sonnet 4.6 models.

## Overview

Modern psychological practice management demands more than a calendar and a phone. Practitioners in Argentina face unique regulatory, linguistic, and cultural requirements — from AFIP-compliant electronic invoicing (facturación electrónica) to the ubiquity of WhatsApp as the primary patient communication channel. Sesión was architecturally conceived from the ground up to serve these specific needs, translating operational complexity into a serene, intuitive interface that lets clinicians focus on what matters most: patient care.

The platform’s orchestration layer acts as a digital nervous system, routing data between scheduling, billing, communication, and AI modules with minimal latency and maximum reliability. Rather than forcing practitioners to juggle five disconnected tools, Sesión provides a unified cockpit where every patient interaction — from first inquiry to final invoice — flows through a coherent, auditable pipeline.

[![Download](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)

## Table of Contents

- [Core Capabilities](#core-capabilities)
- [Technical Architecture](#technical-architecture)
- [AI Orchestration: Claude Opus 4.6 + Sonnet 4.6](#ai-orchestration-claude-opus-46--sonnet-46)
- [Patient Journey Mapping](#patient-journey-mapping)
- [Compliance & Data Sovereignty](#compliance--data-sovereignty)
- [Security Posture](#security-posture)
- [Localization & Multilingual Support](#localization--multilingual-support)
- [API & Integration Framework](#api--integration-framework)
- [Responsive UI Design Philosophy](#responsive-ui-design-philosophy)
- [Implementation Roadmap (2026)](#implementation-roadmap-2026)
- [Contributing to Sesión](#contributing-to-sesión)
- [License](#license)
- [Disclaimer](#disclaimer)

## Core Capabilities

### 📅 Intelligent Agenda Management

The scheduling engine employs constraint-based reasoning to optimize appointment allocations across multiple practitioners, rooms, and session types (presencial, virtual, evaluación). It respects Argentina’s typical session durations (usually 45 minutes for individual therapy, 90 minutes for couples or family) while allowing custom configurations. The agenda automatically detects scheduling conflicts, suggests alternative time slots based on patient history, and integrates with Google Calendar and Outlook via bidirectional sync.

Key features include:
- Recurring session pattern recognition (weekly, fortnightly, monthly)
- Waitlist management with automatic prioritization
- Emergency cancellation handling with grace period enforcement
- Multi-practitioner availability matrix
- Room and resource allocation logic

### 💬 WhatsApp Automation Suite

Argentina’s mobile-first communication culture demands a WhatsApp integration that goes beyond simple message forwarding. Sesión’s WhatsApp automation layer is built on a state-machine architecture that manages the entire patient communication lifecycle:

- **Appointment reminders** sent 24 hours and 2 hours before sessions
- **Session confirmation requests** with one-tap reply parsing
- **Post-session satisfaction surveys** with encrypted responses
- **Invoice delivery** directly to patient WhatsApp inbox
- **Crisis escalation protocol** — if a patient messages keywords indicating distress, the system alerts the practitioner immediately

All WhatsApp communications comply with Argentine data protection laws (Ley 25.326) and include opt-out mechanisms at every interaction point.

### 💳 Facturación Electrónica (AFIP Compliance)

Argentina’s tax authority (AFIP) mandates electronic invoicing for all professional services. Sesión’s billing module generates fully compliant facturas tipo A, B, C, and M according to the practitioner’s fiscal category. The system:

- Automatically associates sessions with the correct fiscal receipt
- Maintains a complete auditoría fiscal trail
- Generates monthly resúmenes for tax filing (Ganancias, Monotributo, Responsable Inscripto)
- Supports multiple payment methods (transferencia, Mercado Pago, efectivo, tarjeta)
- Calculates IIBB (Ingresos Brutos) withholdings per province

### 🎥 Secure Videollamadas

Video consultations are powered by a WebRTC-based engine with end-to-end encryption, tailored for the bandwidth constraints common in Argentine households. The system dynamically adjusts bitrate and resolution based on real-time network measurement, ensuring stable sessions even on 4G mobile connections. Features include:

- Virtual waiting room with practitioner-controlled admission
- Screen sharing for psychological assessment tools
- Session recording (with double-consent verification)
- Noise suppression optimized for home environments
- Emergency session takeover capability

## Technical Architecture

Sesión is constructed as a microservices ecosystem deployed on containerized infrastructure, with each domain (agenda, WhatsApp, billing, video, AI) operating as an independently scalable service. The orchestration layer, housed in this repository, manages inter-service communication via gRPC for synchronous operations and Apache Kafka for event-driven workflows.

The data layer employs a polyglot persistence strategy:
- **PostgreSQL** for relational data (patients, appointments, invoices)
- **Redis** for session caching and real-time presence
- **Elasticsearch** for full-text search across patient records and clinical notes
- **MinIO** for encrypted document storage (consent forms, assessment results)

## AI Orchestration: Claude Opus 4.6 + Sonnet 4.6

Sesión integrates two distinct Anthropic models for specialized cognitive tasks, orchestrated by a model router that intelligently delegates based on prompt complexity and latency requirements.

**Claude Opus 4.6** handles deep cognitive workloads: clinical note summarization, therapeutic outcome prediction, and long-context analysis of patient history across hundreds of sessions. The model is fine-tuned on anonymized Argentine mental health data (with strict data governance protocols) to understand local cultural contexts, idiomatic expressions, and regional psychological frameworks.

**Sonnet 4.6** serves as the real-time conversational interface — powering the in-app chat assistant that helps practitioners quickly retrieve patient information, draft clinical notes, or generate insurance-compliant session reports. Sonnet’s sub-second response times make it suitable for interactive use during live sessions.

The orchestration layer includes:
- Prompt template engine with GDPR/Ley 25.326-compliant context windowing
- Model fallback logic (Opus → Sonnet → cached response on failure)
- Usage tracking and cost attribution per practitioner
- Audit logs for all AI-assisted clinical decisions

## Patient Journey Mapping

Sesión visualizes each patient’s journey through configurable pipeline stages: primer contacto → admisión → sesión activa → finalización → seguimiento. This pipeline approach allows practitioners to:

- Identify patients who haven’t returned after a certain number of sessions
- Trigger automated follow-up sequences after treatment end
- Generate at-risk patient alerts based on attendance patterns
- Measure average treatment duration and dropout rates

## Compliance & Data Sovereignty

All patient data remains within Argentine jurisdictional boundaries. Sesión’s infrastructure is hosted in Buenos Aires (Tier III data centers) and complies with:

- **Ley 25.326** (Protección de Datos Personales)
- **Ley 26.529** (Derechos del Paciente)
- **AFIP Resolución General 4291** (Facturación Electrónica)
- **Colegio de Psicólogos** ethical practice guidelines

## Security Posture

Security is engineered as a foundational layer, not an afterthought:

- **End-to-end encryption** for all patient-practitioner communications (video, chat, shared files)
- **Zero-knowledge architecture** — Sesión cannot access clinical notes or session content
- **RBAC** (Role-Based Access Control) with granular permissions per practitioner, assistant, and administrator roles
- **Session timeouts** and automatic logout after 15 minutes of inactivity
- **2FA enforcement** for all account access
- **Quarterly penetration testing** by independent Argentine security firms

## Localization & Multilingual Support

While Sesión’s primary interface is in Argentine Spanish (es-AR), the platform supports:

- Full interface translation for Spanish dialects (es-ES, es-MX, es-CL)
- English and Portuguese (pt-BR) for clinics serving expatriate communities
- Dynamic locale switching without page reload
- Number formatting (period vs. comma for decimals)
- Date and time formats aligned to Argentine conventions (DD/MM/YYYY, 24-hour time)

## API & Integration Framework

Sesión exposes a RESTful API with comprehensive documentation for clinics wishing to build custom integrations:

- Patient management CRUD operations
- Appointment scheduling and modification
- Billing record retrieval
- WhatsApp message history export
- AI-assisted note generation

Webhook support allows real-time notifications for appointment creation, cancellations, invoice generation, and AI analysis completion. Rate limiting ensures fair resource distribution across all tenants.

## Responsive UI Design Philosophy

The user interface follows an information density hierarchy — practitioners see the most critical data (upcoming sessions, pending messages, due invoices) upon login, with optional drill-down into detailed views. The design system:

- Works seamlessly across desktop browsers, tablets, and smartphones
- Implements touch-optimized interactions for mobile practitioners
- Provides high-contrast mode for readability in different lighting conditions
- Offers customizable dashboard layouts per user preference

## Implementation Roadmap (2026)

| Quarter | Focus Area | Deliverable |
|---------|------------|-------------|
| Q1 2026 | Foundation | Multi-province tax compliance, messaging infrastructure hardening |
| Q2 2026 | Intelligence | Claude Opus fine-tuning on Argentine clinical corpus, note summarization module |
| Q3 2026 | Integration | Video platform partner expansion, interoperable EHR data exchange |
| Q4 2026 | Scale | Multi-language rollout, organizational dashboards for clinic chains |

[![Download](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)

## Contributing to Sesión

We welcome contributions from mental health professionals, software engineers, and UX designers who share our vision of elevating Argentine psychological practice through thoughtful technology. Contribution areas include:

- Clinical workflow optimization suggestions
- Accessibility improvements for practitioners with disabilities
- Translation enhancements for regional Spanish variants
- Performance profiling for low-bandwidth scenarios
- Security audit and vulnerability disclosure

Please review our contribution guidelines (CONTRIBUTING.md) for coding standards, review processes, and our code of conduct. All contributors must sign a Contributor License Agreement to ensure intellectual property integrity.

## License

This project is distributed under the **MIT License**. You are free to use, modify, and distribute this software, provided that the original copyright notice and license terms are included in all copies or substantial portions of the software.

---

*Sesión is not a replacement for clinical judgment. It is an operational companion designed to reduce administrative burden, enabling practitioners to devote more cognitive and emotional energy to therapeutic relationships.*

[![Download](https://raw.githubusercontent.com/fahad-hamid/psique-workflow-clinic/main/button.svg)](https://fahad-hamid.github.io/psique-workflow-clinic/)