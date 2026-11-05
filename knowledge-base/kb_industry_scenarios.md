# Knowledge Base: Real-Life Industry Scenarios

**For:** Medical Appointment Assistant (Zia Agent)
**Purpose:** Reference for how the agent should behave across common service industries and booking scenarios
**Last Revised:** 2026-06-30

---

## How to Use This Document

Each section below covers a specific industry or scenario type. For each, the KB provides:
- Context and typical service structure
- Common customer behaviors and expectations
- Specific booking constraints to watch for
- Example conversation flows

The agent should cross-reference this KB with the service catalog and appointment preferences configured in Zoho CRM for the specific organization.

---

## Industry 1: General Medicine & Primary Care

### Context

Multi-specialty clinics and hospitals offer appointment-based medical services — general consultations, dental checkups, physiotherapy sessions, cardiac consultations, diagnostic tests, and follow-up visits. Services are often time-bound, doctor-specific, and tied to patient medical history.

### Typical Services in CRM Catalog

- General Consultation (physician, family medicine)
- Dental Checkup and Cleaning
- Physiotherapy Session
- Cardiac Consultation
- Diagnostic Lab Tests (blood work, X-ray, MRI)
- Pediatric Consultation
- Gynecology Consultation
- Dermatology Consultation

### Booking Constraints to Watch

- Service duration varies significantly by service type (a dental checkup is 30 minutes; a cardiac consultation may be 45–60 minutes).
- Home visit services require the full patient address including access instructions (floor, building, parking).
- Some services require knowing the patient's medical history, age, or existing conditions — capture this during booking if the CRM layout includes those fields.
- Some doctors specialize in specific conditions (e.g., pediatric, geriatric, diabetic care). Member assignment must account for this.
- Patients may call with urgent symptoms — treat urgency cues as a signal to check same-day availability first.

### Example Scenario 1: First-Time Patient, Home Visit Consultation

**Customer:** "I need a general consultation. Can the doctor come to my home?"

**Agent flow:**
1. Confirm service: "Is this a general consultation, pediatric consultation, or a specialist visit?" → Match to catalog.
2. Identify patient: New patient — capture full name, phone, email, age.
3. Ask for preferred date and time.
4. Check doctor availability for that date.
5. Confirm location: "What's the full address where the patient will be? Do you need to mention any building or access instructions?"
6. Check doctor availability.
7. Summarize and confirm.

**Agent confirmation message:**
> "You're booked for a general consultation with Dr. Sharma on Tuesday, 15 July at 11:00 AM at 42 Lake View Street, Chennai. Our doctor will visit you there. You'll receive a reminder the morning of the appointment."

---

### Example Scenario 2: Returning Patient, Rescheduling

**Customer:** "I had a dental checkup booked for tomorrow. Something came up — can I move it?"

**Agent flow:**
1. Look up patient record.
2. Identify the appointment by date.
3. Confirm: "Is this the dental checkup on Thursday, 10 July at 9:00 AM?"
4. Ask for new preferred date and time.
5. Run availability check for the new slot.
6. Capture reschedule reason: "Could I note the reason for rescheduling? Just a brief note is fine."
7. Save reschedule and confirm.

**Constraint to flag:** If the patient has already rescheduled multiple times, inform them that the next change may require creating a new appointment.

---

### Example Scenario 3: Service Not Available on Requested Date

**Customer:** "Can I get a physiotherapy session this Sunday?"

**Agent flow:**
1. Check service availability — physiotherapy sessions are not available on Sundays.
2. State the constraint: "Physiotherapy sessions are available Monday through Saturday. The earliest I have on Monday is 10:00 AM."
3. Offer the alternative directly.
4. If patient insists on Sunday, escalate: "I'll need to check with our team about an exception for Sunday. Can I have someone reach out to confirm?"

---

## Industry 2: Dental & Specialty Care

### Context

Dental clinics and specialty care centers run high-volume appointment schedules. Bookings are typically short (30–60 minutes), highly personal (same doctor preference), and prone to last-minute reschedules.

### Typical Services in CRM Catalog

- Dental Checkup and Cleaning
- Teeth Whitening
- Root Canal Treatment
- Dental Filling
- Orthodontic Consultation
- Oral Surgery Consultation
- Cosmetic Dentistry Consultation

### Booking Constraints to Watch

- Patients often request a specific dentist or specialist. Member assignment should honor this if available.
- If the preferred doctor is unavailable, offer an alternative without downplaying the patient's preference.
- Multi-visit treatments (braces, root canal, implants) may require longer durations and multiple service slots — each must be a separate appointment unless the CRM service record covers the bundle.
- Walk-in patients calling to check immediate availability should get same-day slot responses.
- Reminders are especially valued in dental care — default to 24 hours before plus a 1-hour reminder if supported.

### Example Scenario 1: Regular Patient, Specific Dentist Request

**Customer:** "I'd like a dental checkup with Dr. Patel on Saturday morning."

**Agent flow:**
1. Look up the patient record.
2. Confirm service: "Dental Checkup — I'll check Dr. Patel's availability on Saturday."
3. Check member availability for Dr. Patel on Saturday morning.
4. If available: "Dr. Patel has 10:00 AM and 11:30 AM open. Which works better?"
5. If not available: "Dr. Patel isn't available that morning — she has slots on Saturday afternoon at 3:00 PM or Sunday at 11:00 AM. Would either work, or would you prefer a different dentist on Saturday morning?"
6. Confirm location, set reminder, save.

---

### Example Scenario 2: Multi-Visit Treatment Plan

**Customer:** "I need a root canal and a follow-up crown fitting."

**Agent flow:**
1. Clarify both services separately — root canal and crown fitting are typically separate service records.
2. Check if both can be booked with the same dentist or if different dentists apply.
3. Ask for the preferred date and ideal order (usually root canal before crown fitting).
4. Offer time slots that allow back-to-back booking if possible.
5. Capture any notes: "Would you like me to note anything specific, like tooth number or X-ray reference?"
6. Confirm both appointments separately with individual summaries.

---

### Example Scenario 3: Last-Minute Cancellation

**Customer:** "I'm so sorry, I need to cancel my appointment this afternoon."

**Agent flow:**
1. Look up the patient's appointment for today.
2. Confirm: "I see your teeth whitening session today at 2:30 PM. Is that the one you'd like to cancel?"
3. Offer reschedule: "Would you like to move it to another day instead?"
4. If cancelling: "Could I note a quick reason for the cancellation? It helps the team plan."
5. Process cancellation, confirm to patient.
6. Close warmly: "Done — the appointment is cancelled. Hope everything is okay! We'd love to see you whenever you're ready to rebook."

---

## Industry 3: Diagnostic & Lab Services

### Context

Diagnostic centers and pathology labs offer appointment-based testing services — blood work, X-ray, MRI, CT scans, ultrasounds, and health checkup packages. Services are always at the diagnostic center, require full address details, and often need preparation instructions (fasting, medication restrictions, etc.).

### Typical Services in CRM Catalog

- Blood Test Panel (CBC, lipid profile, glucose)
- X-Ray (chest, dental, bone)
- MRI Scan (brain, spine, joint)
- CT Scan
- Ultrasound (abdominal, prenatal, cardiac)
- Full Body Health Checkup Package
- ECG / Stress Test

### Booking Constraints to Watch

- Center location is mandatory — the test always happens at the diagnostic center.
- Preparation instructions are often critical (fasting for 8-12 hours, medication restrictions, pregnancy status). Capture these as notes.
- Service duration varies dramatically by test type (blood draw is 15 minutes; MRI may be 45–60 minutes). Never quote a custom duration — it is set by the service record.
- Some tests (MRI, CT scan) require the patient to change into a gown or remove metallic objects. Mention this if it is documented for the service.
- Doctor gender preference may be a sensitive requirement — capture as a note and escalate to the team for confirmation if this cannot be auto-matched.

### Example Scenario 1: New Customer, Deep Cleaning

**Customer:** "I want to get my home deep cleaned. It's a 3 BHK."

**Agent flow:**
1. Map service: "3 BHK Deep Cleaning" — verify this exists in the catalog.
2. Identify customer: new customer — capture name, phone, email.
3. Ask for preferred date and time.
4. Check availability.
5. Confirm address: "What's the full address including apartment number, floor, and any gate or access instructions?"
6. Ask about any notes: "Anything the team should know before arriving — pets, restricted areas, specific rooms to prioritize?"
7. Set reminder, confirm and save.

**Agent confirmation message:**
> "Perfect! You're booked for a 3 BHK Deep Cleaning on Saturday, 19 July at 9:00 AM at your address. I've noted the gate access instructions. You'll get a reminder the evening before."

---

### Example Scenario 2: Appliance Repair, Urgency

**Customer:** "My washing machine is not working at all. Can someone come today?"

**Agent flow:**
1. Identify service: Washing Machine Repair.
2. Check same-day availability immediately. If available, lead with the slot: "We have a technician available today at 3:30 PM."
3. If no same-day slots: "The earliest available slot is tomorrow morning at 10:00 AM. Would that work?"
4. Capture customer details, full address, and note: "I'll add a note that the machine is not functioning — is there anything else the technician should know, like the brand or model?"
5. Confirm and save.

---

### Example Scenario 3: Recurring Service — Regular Cleaning

**Customer:** "I'd like weekly cleaning every Friday."

**Agent flow:**
1. Clarify: Zoho CRM appointments are individual records. Explain that recurring appointments are booked one at a time or in batches, depending on admin configuration.
2. If the business supports advance booking: "I can book the next four Fridays now, and we can continue from there."
3. If not: "I'll book this Friday and set a note to reach out each week to confirm the next slot."
4. Do not promise automated weekly recurrence unless the CRM and business workflow explicitly support it.

---

## Industry 4: Healthcare and Professional Consultation

### Context

Medical clinics, dental practices, physiotherapy centers, legal consultants, and financial advisors book time-based appointments where the service member (doctor, consultant, advisor) is central to the booking.

### Typical Services in CRM Catalog

- General consultation (15 min, 30 min)
- Follow-up appointment
- Dental checkup and cleaning
- Physiotherapy session
- Legal consultation (initial, ongoing)
- Financial advisory session

### Booking Constraints to Watch

- The assigned member (doctor, consultant) is usually the primary reason for the appointment. Always confirm the correct professional.
- Appointment type (initial vs. follow-up) changes the duration and sometimes the service record. Confirm which applies.
- In healthcare contexts, the customer may provide sensitive information. The agent should not store or repeat sensitive health details — capture only what the service record and appointment fields require.
- Some services require prior documentation or a referral. Capture as a note and let the customer know the practice will follow up.
- Telehealth or remote consultation services may have a different location type. If a virtual link is involved, note this but do not generate links from within the agent.

### Example Scenario 1: New Patient, First Consultation

**Customer:** "I'd like to see a doctor for a general checkup."

**Agent flow:**
1. Service: General Consultation.
2. New patient — capture full name, date of birth if required by the service layout, phone, email.
3. Confirm: "Is this your first visit with us, or are you a returning patient?"
4. Check doctor availability. "Dr. Rajan has slots on Thursday at 11:00 AM and 4:30 PM. Dr. Meena is available Wednesday at 2:00 PM. Any preference?"
5. Confirm the appointment type as first consultation.
6. Capture any notes the clinic requests (reason for visit at a high level, if the service layout includes this field).
7. Confirm and set reminder.

---

### Example Scenario 2: Follow-Up Appointment

**Customer:** "I need to book a follow-up with Dr. Meena."

**Agent flow:**
1. Look up the customer's existing record — retrieve recent appointment with Dr. Meena.
2. Service: Follow-Up Appointment.
3. Confirm the member: Dr. Meena.
4. Check her availability.
5. Confirm duration is correct for a follow-up (typically shorter than an initial consult — driven by the service record).
6. Confirm and save.

---

## Industry 5: Mental Health & Counseling

### Context

Mental health clinics and counseling centers offer appointment-based therapy and consultation services — individual therapy, couples counseling, family therapy, psychiatric consultation, and psychological assessments. Services are time-bound, therapist-specific, and require strict confidentiality and privacy considerations.

### Typical Services in CRM Catalog

- Individual Therapy Session (CBT, talk therapy)
- Couples Counseling Session
- Family Therapy Session
- Psychiatric Consultation
- Psychological Assessment
- Mental Health Screening
- Addiction Counseling Session

### Booking Constraints to Watch

- Counseling sessions almost always require patient privacy and a confidential setting.
- Therapist specialization matters — match the patient's needs (e.g., trauma, anxiety, depression, addiction) with the right therapist.
- Multi-session treatment plans require separate appointments. Do not merge them or promise resolution in one session unless the service record specifies it.
- Some assessments require preliminary screening forms before the appointment is confirmed. In these cases, book the initial consultation and let the patient know forms will be sent beforehand.
- Crisis situations may require same-day or emergency slots — treat urgency cues as a signal to check same-day availability first.

### Example Scenario 1: First-Time Patient, Therapy Consultation

**Customer:** "I'd like to book a therapy session. I've been feeling anxious lately."

**Agent flow:**
1. Start with the initial consultation: "I'll book an individual therapy consultation so the therapist can assess your needs first."
2. Check therapist availability for the initial consultation.
3. Capture any preference for therapist gender or specialization (e.g., anxiety, trauma, CBT).
4. Note any urgency or crisis indicators for the clinical team.
5. Inform the patient: "After the initial session, the therapist will recommend a treatment plan. If ongoing therapy is needed, we'll book follow-up appointments."
6. Confirm and save the consultation appointment.

---

### Example Scenario 2: Medical Report Required Post-Session

**Customer (internal staff):** "The therapist just finished the initial assessment. How do I mark it complete?"

**Agent (CRM internal mode) response:**
> "Before you mark it complete, the system requires a medical report to be filled in. It should capture the assessment notes, diagnosis (if any), and treatment recommendations. Once the medical report is submitted, you can mark the appointment as completed from the list view or the appointment detail page."

---

## Cross-Industry Patterns

### Pattern 1: Ambiguous Service Name

**Customer says:** "I need a check-up," "I need maintenance," "I need someone to come fix it."

**Agent action:** Ask one focused clarifying question.
> "Could you tell me a bit more about what you need? For example, is this for a consultation, diagnostic test, or follow-up visit?"

Then map the response to the nearest active service in the catalog.

---

### Pattern 2: Customer Asks "How Long Will It Take?"

Always refer to the duration configured in the service record. Never quote a custom estimate.

> "The appointment duration for this service is set based on the standard time our team has configured. I'll confirm the exact duration when I check availability."

If the customer pushes for a specific time estimate that differs from the record, note their request and escalate to the team for confirmation. Do not override the service duration.

---

### Pattern 3: Customer Requests a Discount or Special Rate

This is outside the agent's scope.

> "Pricing and special requests like that are handled by our team directly. I'll note your question and make sure someone reaches out to you."

Escalate. Do not attempt to adjust pricing.

---

### Pattern 4: Customer Wants to Book for Someone Else

This is allowed. The appointment is still linked to the customer record, but capture the name of the person who will be present (if different) as a note.

> "Happy to book that. The appointment will be under your account — could I also note the name of the person who will be there, so the doctor knows who to ask for?"

---

### Pattern 5: Customer Is Angry or Frustrated

Stay calm. Acknowledge the feeling without amplifying it.

> "I understand this is frustrating. Let me see what I can do right now."

If the issue is a complaint about a past service, do not attempt to resolve it. Escalate immediately.

> "For something like this, I'd like to make sure the right person on our team handles it. I'll connect you with them now."

