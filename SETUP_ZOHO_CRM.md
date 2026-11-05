# Zoho CRM Setup Guide — MediBook

Complete step-by-step guide to configure Zoho CRM for the MediBook appointment booking agent.

---

## 📋 Prerequisites

| Requirement | Details |
|-------------|---------|
| **Zoho CRM Edition** | Enterprise or Ultimate (required for Zia Agents) |
| **User Role** | CRM Administrator (to enable modules and deploy agents) |
| **Zia Agent Studio Access** | Available in Zoho CRM paid plans |
| **Email Account** | For IMAP/POP3 integration (autonomous email path) |

---

## Step 1: Enable the Services Module

The Services module stores your medical service catalog (General Consultation, Dental Checkup, Cardiac Consultation, etc.).

### Steps:
1. Log in to your Zoho CRM account with **Administrator privileges**
2. Go to **Setup > Customization > Modules and Fields**
3. Locate the **Services** module and click **Enable**
4. Add the Services module to relevant **teamspaces** (if your org uses teamspaces)

### Grant Profile Access:
1. Open the **Services** module
2. Click **More > Module Permission**
3. Select the profiles that need access (e.g., Administrator, Receptionist, Doctor)
4. Click **Save**

### Customize Services Layout:
1. Go to **Setup > Customization > Modules and Fields > Services > Standard Layout**
2. Add/remove fields as needed for your medical services (e.g., doctor specialization, room number, equipment required)
3. Click **Save**

---

## Step 2: Enable the Appointments Module

The Appointments module is added automatically when Services is enabled. It stores all patient appointments.

### Steps:
1. Go to **Setup > Customization > Modules and Fields**
2. Verify **Appointments** module is visible (auto-enabled with Services)
3. Open the **Appointments** module
4. Click **More > Module Permissions**
5. Select profiles that need appointment access
6. Click **Save**

### Appointment Layouts:
Three layouts exist — customize as needed:
- **Create Appointment** — for new bookings
- **Reschedule Appointment** — for moving existing appointments
- **Cancel Appointment** — for cancellations

Navigate to: **Setup > Customization > Modules and Fields > Appointments > Standard Layout**

---

## Step 3: Configure Medical Services

Add your healthcare services to the `Services__s` catalog.

### Sample Medical Services to Create:

| Service Name | Duration | Location Type | Members (Doctors) | Job Sheet Required |
|-------------|----------|---------------|-------------------|-------------------|
| General Consultation | 30 min | Client Address / Business Address | Dr. Sharma, Dr. Patel | No |
| Dental Checkup | 45 min | Business Address | Dr. Rao | No |
| Physiotherapy Session | 60 min | Business Address | Dr. Mehta | Yes |
| Cardiac Consultation | 45 min | Business Address | Dr. Singh | No |
| Blood Test Panel | 15 min | Business Address | Lab Technician | No |
| Individual Therapy | 50 min | Client Address | Dr. Kapoor | Yes |

### Steps:
1. Go to **Services module > + Service**
2. Fill in service info, availability, price, job sheet settings
3. Assign **Members** (doctors who can provide this service)
4. Set **Location** type (`"Business Address"` or `"Client Address"`)

---

## Step 4: Configure Appointment Preferences

Global controls for all appointments.

### Steps:
1. Go to **Setup > Customization > Modules and Fields > Appointments > Preferences**
2. Configure the following:

| Preference | Recommended Setting | Effect |
|------------|--------------------|--------|
| Ask users to mark appointment as completed | Enabled | Staff must manually mark complete |
| Mandate users to fill job sheet | Enabled (for services requiring it) | Job sheet mandatory before completion |
| Allow booking outside service availability | Disabled | Prevents bookings outside configured hours |
| Allow booking outside business hours | Disabled | Stays within business hours |
| Create a deal when appointment is completed | Optional | Auto-creates Closed Won deal |

### Deal Creation Config (if enabled):
- Deal defaults to **Closed Won**
- Amount maps from service price
- Closing date from appointment start date

---

## Step 5: Set Up Zia Agent Studio

MediBook is deployed as a Zia Agent.

### Configure Agent Portal:
1. Go to **Setup → Zia → Agents**
2. On the integration details page:
   - If you have an existing Zia Agents portal → click **Connect an Existing Zia Agents Portal**
   - If you don't have one → click **New Agent** to design and deploy

### Deploy the MediBook Agent:

#### Option A: Deploy Using Connection (Recommended for testing)
1. Open the MediBook agent in **Zia Agents**
2. Click **Deploy using Connection**
3. Map the tool parameters (services, appointments, contacts modules)
4. Select and configure the portal, then associate a connection
5. Click **Deploy**

#### Option B: Deploy as Digital Employee (Recommended for production)
1. Open the MediBook agent in **Zia Agents**
2. Click **Deploy as Digital Employee**
3. Map tool parameters
4. Configure portal and service deployment details
5. Click **Deploy**
6. Review permissions under **Consent** and confirm
7. Complete the four steps:
   - **Applications**: Link required portals (Zoho CRM)
   - **Permissions**: Choose role and profile for the Digital Employee
   - **Agent Details**: Enter name (email auto-fills with `@ziaagents.ai` domain)
   - **Summary**: Review configuration

---

## Step 6: Configure IMAP/POP3 for Email Path

Required for the autonomous email booking mode.

### Steps:
1. In Zoho CRM, go to **Setup > Channels > Email > Email Configuration**
2. In the **Email Integration tab**, choose your email service
3. Under **Choose the protocol**, select **IMAP** (recommended)
4. Select sync period (e.g., last 6 months, 1 year, or from a specific date)
5. Enter:
   - **Display name**: MediBook Assistant
   - **Email address**: appointments@yourclinic.com
   - **Password**: [your email password]
6. Click **Configure**
7. Once authorization is done, configure import filters and email sharing settings

> **Note:** Zoho CRM recommends IMAP for real-time email sync and Zia's email intelligence features (sentiment analysis, intent detection).

---

## Step 7: Create API/OAuth Credentials

If you need to connect external systems or the GitHub WorkDrive sync:

### Steps:
1. Go to **Zoho API Console** (https://api-console.zoho.com/)
2. Create a new **Server-based Client**
3. Note down:
   - **Client ID**
   - **Client Secret**
4. Generate a **Refresh Token** using the OAuth flow
5. Add these as **GitHub Secrets** in your repository:
   - `ZOHO_CLIENT_ID`
   - `ZOHO_CLIENT_SECRET`
   - `ZOHO_REFRESH_TOKEN`
   - `WORKDRIVE_ROOT_FOLDER_ID`

---

## Step 8: Verify Module API Names

MediBook uses exact API names. Verify these in your CRM:

| Module | API Name | Verify In |
|--------|----------|-----------|
| Services | `Services__s` | Setup > Customization > Modules and Fields > Services |
| Appointments | `Appointments__s` | Setup > Customization > Modules and Fields > Appointments |
| Contacts | `Contacts` | Setup > Customization > Modules and Fields > Contacts |
| Leads | `Leads` | Setup > Customization > Modules and Fields > Leads |

> ⚠️ **Critical:** Using `Services` or `Appointments` (without `__s`) will return `INVALID_MODULE` (400) errors.

---

## Step 9: Assign Doctors as Service Members

Each doctor must be a member of the services they can provide.

### Steps:
1. Open a **Service** record (e.g., General Consultation)
2. In the **Members** section, add the doctors who provide this service
3. Only members listed here can be assigned as `Owner` on appointments for this service
4. Repeat for all services

---

## Step 10: Test the Agent

### Test Scenarios:

| Test | Expected Outcome |
|------|-----------------|
| Email: "Book me a General Consultation tomorrow at 2 PM" | Appointment created, confirmation email sent |
| Chat: "Is Dr. Sharma free Friday morning for Dental Checkup?" | Available slots shown, booking confirmed |
| CRM Staff: "Book James for Cardiac Consultation Thursday 2 PM" | Appointment created with staff confirmation |
| Reschedule: "Move my appointment to Friday" | Appointment moved, reason captured |
| Cancellation: "Cancel my appointment" | Task created (email path), or cancelled (staff path) |

---

## 📁 GitHub Repository Integration (Optional)

The repository includes a GitHub Actions workflow that syncs documentation to Zoho WorkDrive.

### Required GitHub Secrets:
Go to your repository **Settings > Secrets and variables > Actions** and add:

| Secret | Value |
|--------|-------|
| `ZOHO_CLIENT_ID` | Your Zoho OAuth Client ID |
| `ZOHO_CLIENT_SECRET` | Your Zoho OAuth Client Secret |
| `ZOHO_REFRESH_TOKEN` | Your Zoho OAuth Refresh Token |
| `WORKDRIVE_ROOT_FOLDER_ID` | WorkDrive folder ID mapping to repo root |

### Workflow Trigger:
The workflow runs on every push to `main` branch for `.md` and `.yaml/.yml` file changes.

---

## 🛠️ Troubleshooting

| Issue | Cause | Fix |
|-------|-------|-----|
| `INVALID_MODULE` (400) | Wrong module API name | Use `Services__s` and `Appointments__s` (with `__s` suffix) |
| Agent can't read CRM records | Agent not deployed | Deploy agent using Connection or Digital Employee method |
| Emails not syncing | IMAP not configured | Set up IMAP/POP3 at Setup > Channels > Email |
| Doctor not assignable | Doctor not a service member | Add doctor to the Service's Members list |
| Past time rejected | Start time is in the past | Use future ISO 8601 timestamps |
| Zia Agent not available | Wrong CRM edition | Upgrade to Enterprise or Ultimate edition |

---

## 📞 Support

- **Zoho CRM Help:** https://help.zoho.com/portal/en/kb/crm
- **Zoho CRM API v8:** https://www.zoho.com/crm/developer/docs/api/v8/
- **Zia Agents Documentation:** Setup → Zia → Agents in your CRM

---

**Next Steps:** After completing this setup, push your code to `main` and the MediBook agent will be ready to handle patient appointments!