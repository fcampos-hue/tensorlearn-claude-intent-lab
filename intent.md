# Intent: Results-ready notification

Sources: approved PRD N-17 (`inputs/01-approved-prd.md`, approved 2026-09-18) and the discovery notes of 2026-09-20 (`inputs/02-current-meeting.md`).

## Problem

- **P-1** The fictional clinic receives avoidable calls from patients checking whether their results are ready. PRD N-17: "The fictional clinic receives avoidable calls from patients checking whether results are ready."
- **P-2** Patients have no signal that a reviewed result is waiting for them, so they phone the clinic to find out. That takes staff time on status-only questions.

## Proposed outcome

- **I-1** Notify the patient when a result is ready for their review in the existing authenticated portal (PRD N-17, "Problem and outcome").
- **I-2** "Ready" means a doctor has reviewed **and** released the result. It does not mean that the laboratory upload finished (PRD N-17 constraints; product owner, discovery notes).
- **I-3** The notification only says that an update is available and directs the patient to sign in to the existing portal.

### How success will be observed

- **S-1** The number of results-status-only calls goes down. These calls are counted separately from clinical questions and compared against a measured baseline (PRD N-17; support lead, discovery notes).
- **S-2** Delivery failures are measured and handled without sending the patient duplicate messages (PRD N-17).
- **S-3** No numeric target is approved. "The baseline and numerical target will be agreed during discovery; none is approved yet" (PRD N-17).

## Affected users and systems

- **U-1** Patients, who receive the notification and view the result in the existing portal.
- **U-2** Doctors, whose review and release of a result is the trigger that makes a notification eligible.
- **U-3** Clinic support staff, who receive the status calls and will count status-only calls separately from clinical questions.
- **U-4** Systems: the existing authenticated portal and its existing result route, the result review/release workflow, the notification delivery path (channel still open, see OPEN-1), and the notification and operational logs.

## Out of scope

- **X-1** Portal redesign (PRD N-17, "Scope").
- **X-2** A new or rebuilt patient dashboard. It is a separate project (engineering lead, discovery notes).
- **X-3** Appointment scheduling (PRD N-17).
- **X-4** Clinical interpretation of results (PRD N-17).
- **X-5** Notifying third parties (PRD N-17). Notifying the referring doctor is not approved either: it stays open as OPEN-2, separate from this exclusion.

## Constraints

- **C-1** Review before notification: "A doctor must review and release the result before any notification is eligible" (PRD N-17). Lab upload completion alone never triggers a notification.
- **C-2** No clinical details: the message says only that an update is available. "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs" (PRD N-17). The privacy owner adds: "no sensitive content in messages or operational logs."
- **C-3** Access stays authenticated in the existing portal. There are no attached results and no public result link (PRD N-17).
- **C-4** Use the existing portal result route. The engineering lead confirms that "the existing portal path is adequate."
- **C-5** No duplicate notifications for the same released result version. Delivery failures are handled without duplicate patient messages (PRD N-17).
- **C-6** The only recipient is the patient. Nobody else may be notified until the owners of OPEN-2 decide.

## Open questions

- **OPEN-1** Which channel (SMS, email or portal push)? **Owners: product owner and privacy owner**, to decide after the consent/delivery review (PRD N-17; privacy owner, discovery notes: "Consent and the actual channel are still under review"). Not decided here.
- **OPEN-2** Should the referring doctor be notified? **Owners: clinical owner and privacy owner**, who must decide recipient authorization. "It is not approved by this PRD" (PRD N-17). Not decided here.
- **OPEN-3** What are the baseline and numeric target for status-only calls? These are to be agreed during discovery (PRD N-17). The baseline will be measured as the support lead asked. No target is set in this intent.

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
