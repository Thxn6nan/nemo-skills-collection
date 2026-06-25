# Release Checklist & Communication Templates

---

## Pre-Release Checklist by Output Type

### Code / Feature Release
```
QUALITY
□ work-audit: no Critical or High defects
□ All tests passing in staging/pre-prod environment
□ Performance acceptable at expected load
□ Error handling verified (what happens when things go wrong)

COMMUNICATION
□ Release notes written (what changed, what's new, what broke)
□ Known issues documented
□ Support team briefed

ROLLBACK
□ Rollback procedure written and tested
□ Previous version still accessible if needed
□ Rollback owner identified
```

### Document / Report Release
```
QUALITY
□ work-audit passed (no Critical/High defects)
□ project-qa: requirement coverage confirmed
□ anti-hallucination: all data and numbers verified
□ Reviewed by someone other than the author
□ Final version clearly labeled (not a draft)

COMMUNICATION
□ Cover note explains purpose and required action
□ Distribution list confirmed (right people, no one missing, no one extra)
□ Format accessible to all recipients (PDF vs editable, etc.)

ROLLBACK
□ If correction needed: process for issuing corrected version defined
□ Original version archived
```

### Process / Workflow Change Release
```
QUALITY
□ Change tested with a small group before wide release
□ Old process documented (needed for rollback)
□ Edge cases and exceptions defined

COMMUNICATION
□ Affected people notified with enough lead time
□ Training or guidance provided if needed
□ FAQ prepared for common questions

ROLLBACK
□ Revert-to-old-process steps documented
□ Timeline for rollback decision defined
```

---

## Communication Templates

### Internal Team Update
```
Subject: [Released] <what was released>

<what was shipped in 1-2 sentences>

What's changed:
- <change 1>
- <change 2>

What you need to do: <specific action, or "nothing — just awareness">

Questions? <contact>
```

### Client Delivery
```
Subject: <Project name> — <Deliverable name> Delivered

Hi <name>,

Please find attached/linked <deliverable> for <project>.

<1 paragraph: what this covers and why it matters>

Next steps:
- What we need from you: <action + deadline if any>
- What happens next: <next milestone>

Please don't hesitate to reach out with any questions.

<name>
```

### Release Notes (User-Facing)
```
## <Version or Date> — <One-line summary>

### What's new
<Feature or change, explained in user terms>

### What's fixed
<Bug or issue resolved>

### Known issues
<Any known problems and workarounds>

### How to get it
<Instructions if action needed>
```

### Rollback Notification
```
Subject: [Action] <what was released> — Temporary rollback

Hi <recipients>,

We've temporarily rolled back <what was released> released on <date>.

Reason: <brief, honest explanation>

Current status: You are back on <previous version/process>

What this means for you: <specific impact>

Expected resolution: <timeline or next steps>

We'll update you by <date> with our plan.

<name>
```
