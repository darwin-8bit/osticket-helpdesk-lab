# Sample Tickets — Worked End-to-End

Four common tickets to run through the [ITIL workflow](itil-incident-workflow.md). 
---

## #1 — "I can't log in to my PC" 
- **Type:** Incident · **Help topic:** Password Reset · **Impact:** Low (1 user) · **Urgency:** High
- **Priority:** P3 · **Tier:** T1
- **Diagnosis:** Account locked after X failed attempts.
- **Action:** Verified identity, unlocked account in Active Directory, reset password with "change at
  next logon".
- **Resolution:** User logged in; sent the "Password reset complete" canned response. 

## #2 — "My laptop won't turn on" 
- **Type:** Incident · **Help topic:** Hardware Issue · **Impact:** Low · **Urgency:** Medium
- **Priority:** P3 · **Tier:** T1 → T2
- **Diagnosis:** No power; ruled out charger/outlet remotely, then booked a hardware check.
- **Action:** Escalated to L2 for hardware swap; BIOS issue, provided a loan device to keep the user working.
- **Resolution:** Faulty battery replaced; ticket documented and closed. Logged as a known issue for model type

## #3 — "Email is down for the whole Sales team" 📧
- **Type:** Incident · **Help topic:** Software/Application · **Impact:** High (a team) · **Urgency:** High
- **Priority:** P1 · **Tier:** T1 → T2/T3 → Emergency Team
- **Diagnosis:** Multiple reports from one department → likely service-side
- **Action:** Raised priority, notified affected users, escalated immediately to L2/L3 and the M365
  admin; tracked Microsoft service health.
- **Resolution:** Mail flow restored; posted a knowledge-base note and a post-incident summary. **SLA
  response met at 12 min.**

## #4 — "Onboarding newest IT agent" 
- **Type:** **Service Request** · **Help topic:** New Hire (Onboarding) · **Priority:** P3
- **Action:** Followed the standard onboarding procedure — created the AD account (via the
  [onboarding script](../../ad-home-lab/scripts/New-BulkADUsers.ps1)), added to department groups,
  provisioned mailbox, assigned hardware, and permissions
- **Resolution:** Account ready and tested before the start date; checklist attached to the ticket.

---
