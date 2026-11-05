
# MediBook — Hospital & Clinic Appointment Booking Assistant

> An AI-powered Zia Agent for Zoho CRM that automates patient appointment booking, rescheduling, and cancellation across email, website chat, and internal staff channels — no double-booking, no dropped requests, no manual data entry.

## 📋 Overview

**MediBook** is a Zia Agent built on **Zoho CRM** that helps multi-specialty clinics, dental centers, diagnostic labs, physiotherapy clinics, and hospitals manage patient appointments intelligently. It runs in **three deployment modes** from a single shared booking domain:

| Mode | Channel | Confirmation Style | Best For |
|------|---------|-------------------|----------|
| **Email (Autonomous)** | Inbound patient emails | Auto-confirms; no human per step | High-volume inbound booking requests |
| **Website Chatbot** | Website chat widget | Confirms before every write | New patient acquisition & self-serve booking |
| **CRM Internal (Staff)** | Inside Zoho CRM | Staff confirms each write |

---

## ✨ Key Features

### 📅 Smart Appointment Management
- **Book** new patient appointments across all three channels
- **Reschedule** existing appointments with availability checks
- **Cancel** appointments (email path escalates cancellations to a Task for safety)

### 🏥 Multi-Service Support
Works with any service in your Zoho CRM `Services__s` catalog — general consultations, dental checkups, physiotherapy, cardiac consults, lab tests, therapy sessions, and more.

### 👨‍⚕️ Two-Layer Availability Checking
1. **Service-level**: Checks the doctor is a member of the chosen service
2. **Calendar-level**: Uses `getUsersFreeOrBusyDetails` (Zoho Calendar) to verify the doctor is actually free at the requested time

### 📧 Three Communication Channels
- **Email**: Autonomous reading, booking, and confirmation replies
- **Chatbot**: Conversational patient-facing assistant on your website
- **CRM Staff**: Internal assistant for reception and coordination teams

### 🛡️ Anti-Fabrication & Data Integrity
- Never invents or guesses patient, doctor, service, availability, date, or address details
- All data fetched live from Zoho CRM — never from memory or prior conversations
- CRM is the single source of truth

## 🏥 Supported Medical Services

MediBook works with any service in your Zoho CRM `Services__s` catalog. Common examples:
- General Consultation (physician, family medicine)
- Dental Checkup and Cleaning
- Physiotherapy Session
- Cardiac Consultation
- Diagnostic Lab Tests (blood work, X-ray, MRI)
- Pediatric Consultation
- Gynecology Consultation
- Dermatology Consultation
- Individual Therapy Session (CBT, talk therapy)
- Couples Counseling Session
- Family Therapy Session
- Psychiatric Consultation
- Psychological Assessment

## 🏗️ Project Structure

```
MediBook/
├── README.md                          # This file
├── .gitignore                         # Git ignore rules
├── .github/
│   └── workflows/
│       └── workdrive-sync.yml         # GitHub Actions: syncs docs to Zoho WorkDrive
├── agents/
│   └── medical_appointment_assistant_agent.md   # Agent persona & behavioral rules
├── docs/
│   └── MediBook_use_case_submission.md          # Full use case specification
├── knowledge-base/
│   ├── kb_crm_staff_agent.md          # CRM internal staff path KB
│   ├── kb_customer_faqs_and_edge_cases.md       # Patient FAQs & edge case handling
│   ├── kb_email_booking_path.md     # Email-triggered booking flow KB
│   ├── kb_website_chatbot_agent.md  # Website chatbot behavior KB
│   ├── kb_industry_scenarios.md     # Healthcare industry playbook (5 industries)
│   ├── zoho_crm_booking_appointments_kb.md      # Appointments module admin reference
│   └── zoho_crm_managing_services_kb.md         # Services module admin reference
├── scripts/
│   └── sync_to_workdrive.py           # Python script to sync repo changes to Zoho WorkDrive
├── specs/
│   └── medibook_spec_v2.yaml          # Agent specification (function calling format)
└── tools/
    └── user_unavailability.yaml       # Zoho CRM User Unavailability API spec
```

## 🔑 Core Business Rules

### Booking Rules
1. **CRM is the single source of truth** — all patient, doctor, and appointment data comes from live Zoho CRM queries
2. **Services are fetched live** — never from knowledge documents, memory, or prior conversations
3. **Doctors must be eligible and available** — provider must be a member of the chosen service AND free at the requested time
4. **Times must be in the future** — past appointment times are rejected
5. **No request is silently dropped** — every failure ends with a tagged Task for human follow-up
6. **Never invent or guess** — never guess a patient, doctor, service, availability, date, or address

### Email Path Rules
- One outcome per email: book OR reschedule OR ask OR Task
- Maximum 2 ask emails per booking request; then escalate to Task
- Cancellations always escalate to a Task (never auto-executed)
- Human-handoff requests escalate to a Task

### Tagging
- Every appointment created: tagged MediBook
- Every Task created: tagged MediBook
- Email confirmations signed: MediBook

## 🚀 Getting Started

> **New to Zoho CRM?** Follow the step-by-step [Zoho CRM Setup Guide](SETUP_ZOHO_CRM.md) to configure your CRM from scratch before deploying MediBook.

### Prerequisites
- **Zoho CRM** Enterprise or Ultimate edition (required for Zia Agents)
- **Services module** enabled in Zoho CRM
- **Appointments module** enabled in Zoho CRM
- **Org mailbox** integrated with CRM via IMAP/POP3 (for email path)
- **Zia Agent Studio** access

### CRM Module API Names

| Module | API Name |
|--------|----------|
| Services | Services__s |
| Appointments | Appointments__s |
| Contacts | Contacts |
| Leads | Leads |

### Mandatory Appointment Fields

| Field | API Key | Notes |
|-------|---------|-------|
| Appointment Name | Appointment_Name | Label, e.g. "Sudev - General Consultation" |
| Appointment For — module | Appointment_For.module.api_name | e.g. Contacts |
| Appointment For — name | Appointment_For.name | Patient's display name |
| Appointment For — id | Appointment_For.id | Patient's CRM record ID |
| Service Name — name | Service_Name.name | Exact service name from Services module |
| Service Name — id | Service_Name.id | Service record ID |
| Start Time | Appointment_Start_Time | ISO 8601 format; must be future |
| Owner | Owner | Doctor's CRM user ID |
| Location | Location | "Business Address" or "Client Address" |
## 📁 Key Files Explained

| File | Purpose |
|------|---------|
| gents/medical_appointment_assistant_agent.md | Agent identity, tone, email trigger path, chat mode, CRM internal mode, booking steps, guardrails |
| docs/MediBook_use_case_submission.md | Full use case submission: problem, solution, walkthrough, invocation, observability |
| knowledge-base/kb_email_booking_path.md | Email-triggered booking flow: prerequisites, APIs, worked examples, error handling |
| knowledge-base/kb_website_chatbot_agent.md | Chatbot persona, language rules, conversation flows, email templates |
| knowledge-base/kb_crm_staff_agent.md | Staff assistant KB: domain model, booking flow, reschedule/cancel, notifications, API reference |
| knowledge-base/kb_customer_faqs_and_edge_cases.md | Patient FAQs, edge cases, tone reference for emotional states |
| knowledge-base/kb_industry_scenarios.md | 5 healthcare industry playbooks with example conversations |
| knowledge-base/zoho_crm_booking_appointments_kb.md | Appointments module admin & operations reference |
| knowledge-base/zoho_crm_managing_services_kb.md | Services module admin & configuration reference |
| specs/medibook_spec_v2.yaml | Agent specification for function calling / tool definitions |
| scripts/sync_to_workdrive.py | Syncs Markdown/YAML changes from Git to Zoho WorkDrive |
| 	ools/user_unavailability.yaml | OpenAPI spec for Zoho CRM User Unavailability API |

## 📊 Observability & Monitoring

Every agent run produces a **run summary** recording:
- Outcome and reason
- Silent no-ops (unknown sender, non-booking email)
- Every appointment and Task carries the MediBook tag
- Appointments also carry an Additional_Information stamp

**Recommended reports/dashboards:**
- Appointments per service
- Reschedules/cancellations by month
- Appointments per doctor
- Creation trends
- Filtering by agent tag to measure agent-created volume and escalation rate

## 🛠️ Error Handling

All unrecoverable errors escalate — never silently fail:

| Mode | Escalation Action |
|------|-------------------|
| **Email** | Create Task on contact + send acknowledgement email to patient |
| **Chat** | Tell patient "I wasn't able to complete that — let me connect you with someone who can help" + create Task |
| **CRM Staff** | Inform staff user of failure; appointment action always stands even if notification fails |

## 🔐 Data Privacy

- **Never expose** Zoho CRM internal identifiers to website visitors
- **Never reveal** another patient's data
- **Never mention** Zoho, CRM, modules, records, or leads to chat visitors
- Website visitors see only: "our service catalog," "our booking system," "your assigned specialist"

## 🔄 Sync to Zoho WorkDrive

The repository includes a GitHub Actions workflow (.github/workflows/workdrive-sync.yml) that automatically syncs .md and .yaml/.yml file changes to Zoho WorkDrive on every push to main.

**Required GitHub Secrets:**
- ZOHO_CLIENT_ID
- ZOHO_CLIENT_SECRET
- ZOHO_REFRESH_TOKEN
- WORKDRIVE_ROOT_FOLDER_ID

## 🤝 Contributing

1. Create a feature branch from main
2. Make your changes
3. Submit a pull request for review
4. After merge, changes auto-sync to Zoho WorkDrive

## 📄 License

Internal project — for use with Zoho CRM Zia Agent Studio deployments.

---

**Built with ❤️ for healthcare providers using Zoho CRM.**
