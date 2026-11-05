# How to Create the MediBook Agent — Zia Agent Studio Guide

Complete step-by-step guide to create and deploy the MediBook AI agent in Zoho CRM's Zia Agent Studio.

---

## 📋 Prerequisites

| Requirement | Details |
|-------------|---------|
| Zoho CRM Edition | Enterprise or Ultimate (required for Zia Agents) |
| Zia Agent Studio | Available in Zoho CRM paid plans |
| Zoho CRM Connection | Agent must be connected to your CRM org |
| Tools/Functions | 42 CRM tools (services, appointments, contacts, emails, tags) |

---

## Step 1: Create a New Agent in Zia Agent Studio

1. Go to **Zia Agent Studio** (https://ziaagents.zoho.com)
2. Sign in with your Zoho account
3. Click **"New Agent"**
4. Enter:
   - **Agent Name**: `MediBook`
   - **Agent Description**: `Medical appointment booking assistant for Zoho CRM`
   - **Agent Type**: Standard Agent
5. Click **Create**

---

## Step 2: Add Agent Instructions (Instructor File)

Copy the entire content from `agents/medical_appointment_assistant_agent.md` into the agent's instructions field.

### Key Sections to Include:

#### Agent Identity
```
You are MediBook, a medical appointment booking assistant for Zoho CRM.
You help patients book, reschedule, and cancel medical appointments.
You work in 3 modes: Email (autonomous), Website Chat, and CRM Internal (staff).
```

#### Session Continuity (CRITICAL)
- Re-read the entire conversation history before every response
- Never re-ask for information already given in any prior message
- Track your own promises — follow through with actual tool calls
- Carry all collected details forward across every turn
- Never restart the conversation flow from Step 1

#### Module API Names (MANDATORY)
| Module | CORRECT API name | WRONG (causes error) |
|--------|-----------------|---------------------|
| Services | `Services__s` | `Services`, `Service` |
| Appointments | `Appointments__s` | `Appointments`, `Appointment` |
| Contacts | `Contacts` | (this one has no suffix) |
| Leads | `Leads` | (this one has no suffix) |

#### Core Rules
- **Never invent or guess** a patient, doctor, service, availability, date, or address
- **Never book on details** the customer did not state
- **Services must always be fetched live** from CRM — never from memory or documents
- **One outcome per email**: book OR reschedule OR ask OR Task — never more than one
- **Maximum 2 ask emails** per booking request; after that, escalate to a Task
- **Cancellations always escalate** to a Task (never auto-executed via email)
- **Human-handoff requests** escalate to a Task for the record owner
- **Tag everything**: `MediBook` tag on all appointments and tasks

---

## Step 3: Upload Knowledge Base Files

Upload these 7 files as knowledge base (from the `knowledge-base/` folder):

| # | File | Purpose | When Used |
|---|------|---------|-----------|
| 1 | `kb_email_booking_path.md` | Email-triggered booking flow | When triggered by inbound email |
| 2 | `kb_website_chatbot_agent.md` | Website chatbot behavior | When operating as website chat widget |
| 3 | `kb_crm_staff_agent.md` | CRM internal staff path | When invoked by CRM staff |
| 4 | `kb_customer_faqs_and_edge_cases.md` | Patient FAQs & edge cases | All modes (behavioral reference) |
| 5 | `kb_industry_scenarios.md` | Healthcare industry playbooks | All modes (industry context) |
| 6 | `zoho_crm_booking_appointments_kb.md` | Appointments module admin ref | All modes (CRM operations) |
| 7 | `zoho_crm_managing_services_kb.md` | Services module admin ref | All modes (CRM operations) |

### How to Upload:
1. In Zia Agent Studio → **Knowledge Base** section
2. Click **"Add Knowledge"**
3. Upload each `.md` file individually
4. Set visibility: **"Agent accessible"**
5. Click **Save** after each upload

---

## Step 4: Configure Tools (42 CRM Tools)

MediBook requires 42 Zoho CRM tools. Refer to `specs/medibook_spec_v2.yaml` for complete tool definitions.

### Customer/Contact Tools
| Tool | Purpose |
|------|---------|
| `crm_searchRecords` | Search contacts by name/email/phone |
| `crm_searchContactByEmail` | Find contact by email |
| `crm_searchContactByPhone` | Find contact by phone |
| `crm_getRecords` | Get records from any module |
| `crm_createRecord` | Create new contact/lead |

### Service & Appointment Tools
| Tool | Purpose |
|------|---------|
| `crm_getRecords` (Services) | Fetch live service catalog |
| `crm_getRecords` (Appointments) | Fetch existing appointments |
| `crm_createRecord` (Appointments) | Create new appointment |
| `crm_updateRecord` (Appointments) | Reschedule or cancel appointment |

### Email Tools
| Tool | Purpose |
|------|---------|
| `getemailsofarecord` | List emails on a contact's thread |
| `crm_getSpecificCrmEmailContent` | Read single email body |
| `crm_sendMail` | Send confirmation/ask emails |

### Availability Tools
| Tool | Purpose |
|------|---------|
| `getUsersFreeOrBusyDetails` | Check doctor availability (Zoho Calendar) |

### Tag & Task Tools
| Tool | Purpose |
|------|---------|
| `crm_getTags` | Check if tag exists |
| `crm_createTag` | Create tag if absent |
| `crm_associateTags` | Apply tag to record |
| `crm_createRecord` (Tasks) | Create escalation task |

### How to Add Tools:
1. In Zia Agent Studio → **Tools** section
2. Click **"Add Tool"** for each
3. Map parameters as per `specs/medibook_spec_v2.yaml`
4. Set authentication: **Zoho CRM Connection**

---

## Step 5: Configure Agent Settings

### Model Settings
| Setting | Value |
|---------|-------|
| Model | GPT-4 or equivalent |
| Temperature | 0.3 (low for accuracy) |
| Max Tokens | 4096 |

### Agent Behavior
| Setting | Value |
|---------|-------|
| Session Continuity | Enabled (remember conversation history) |
| Tool Calling | Enabled |
| Max Tool Calls | 20 per turn |

### Persona
| Setting | Value |
|---------|-------|
| Tone | Warm, professional, efficient |
| Language | English (with Hindi support if needed) |
| Signature | Always sign emails as "MediBook" |

---

## Step 6: Deploy to Zoho CRM

### Option A: Deploy Using Connection (Recommended for testing)
1. Open agent in Zia Agents → Click **"Deploy using Connection"**
2. Map the tool parameters (services, appointments, contacts modules)
3. Select and configure the portal, then associate a connection
4. Click **Deploy**
5. Redirected to CRM → Set up activation conditions

### Option B: Deploy as Digital Employee (Recommended for production)
1. Open agent in Zia Agents → Click **"Deploy as Digital Employee"**
2. Map tool parameters
3. Configure portal and service deployment details
4. Click **Deploy**
5. Review permissions under **Consent** and confirm
6. Complete the four steps:
   - **Applications**: Link required portals (Zoho CRM)
   - **Permissions**: Choose role and profile for the Digital Employee
   - **Agent Details**: Enter name (email auto-fills with `@ziaagents.ai` domain)
   - **Summary**: Review configuration

---

## Step 7: Configure Activation Conditions

### Email Mode (Autonomous)
- **Trigger**: Incoming customer email
- **Condition**: Email received on integrated mailbox
- **Action**: Agent reads email, books/reschedules/asks/escalates

### Chatbot Mode
- **Trigger**: Website chat widget message
- **Condition**: Visitor starts chat
- **Action**: Agent collects name + email, then assists

### CRM Staff Mode
- **Trigger**: Staff invocation inside CRM
- **Condition**: Staff opens agent in CRM
- **Action**: Agent assists with booking, reschedule, cancel

---

## Step 8: Test the Agent

### Test Scenarios:

| Test | Input | Expected Output |
|------|-------|-----------------|
| Email booking | "Book me a General Consultation tomorrow at 2 PM" | Appointment created, confirmation email sent |
| Chat booking | "Is Dr. Sharma free Friday morning for Dental Checkup?" | Available slots shown, booking confirmed |
| CRM Staff | "Book James for Cardiac Consultation Thursday 2 PM" | Appointment created with staff confirmation |
| Reschedule | "Move my appointment to Friday" | Appointment moved, reason captured |
| Cancellation | "Cancel my appointment" | Task created (email) or cancelled (staff) |
| Missing info | "Book an appointment" | Ask email sent requesting missing details |
| Unknown sender | Email from non-CRM contact | Silent no-op (reason in run summary) |

---

## 📁 Files Needed for Agent Creation

| File | Use In |
|------|--------|
| `agents/medical_appointment_assistant_agent.md` | Agent instructions (instructor file) |
| `knowledge-base/*.md` (7 files) | Knowledge base upload |
| `specs/medibook_spec_v2.yaml` | Tool definitions & parameters |
| `tools/user_unavailability.yaml` | Unavailability API tool spec |

---

## 🔗 Quick Reference: Where to Find Things

| What You Need | Where to Find |
|---------------|---------------|
| Agent Instructions | `agents/medical_appointment_assistant_agent.md` |
| Email Flow Logic | `knowledge-base/kb_email_booking_path.md` |
| Chatbot Behavior | `knowledge-base/kb_website_chatbot_agent.md` |
| Staff Agent Rules | `knowledge-base/kb_crm_staff_agent.md` |
| Patient FAQs | `knowledge-base/kb_customer_faqs_and_edge_cases.md` |
| Industry Scenarios | `knowledge-base/kb_industry_scenarios.md` |
| CRM Admin Ref | `knowledge-base/zoho_crm_booking_appointments_kb.md` |
| Service Config | `knowledge-base/zoho_crm_managing_services_kb.md` |
| Tool Definitions | `specs/medibook_spec_v2.yaml` |

---

## ⚠️ Common Mistakes to Avoid

1. **Using wrong module names**: Always use `Services__s` and `Appointments__s` (with `__s` suffix)
2. **Not checking availability first**: Always call `getUsersFreeOrBusyDetails` before booking
3. **Sending too many emails**: Max 2 ask emails per request, then escalate to Task
4. **Inventing data**: Never guess patient, doctor, service, or address details
5. **Not tagging records**: Every appointment and task must have the `MediBook` tag
6. **Skipping idempotency check**: Always check if appointment already exists before creating
7. **Resending untouched fields**: Never resend `Location`, `Service_Name` in reschedule updates
8. **Wrong user_id in email fetch**: Always get `user_id` from `getemailsofarecord` list response

---

## 📞 Support

- **Zoho CRM Help:** https://help.zoho.com/portal/en/kb/crm
- **Zoho CRM API v8:** https://www.zoho.com/crm/developer/docs/api/v8/
- **Zia Agents Documentation:** Setup → Zia → Agents in your CRM

---

**Repository:** https://github.com/stymrj/MediBook  
**Setup Guide:** [SETUP_ZOHO_CRM.md](SETUP_ZOHO_CRM.md)  
**Use Case Spec:** [docs/MediBook_use_case_submission.md](docs/MediBook_use_case_submission.md)
5. Click **Save** after each tool