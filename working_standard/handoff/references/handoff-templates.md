# Handoff Templates

---

## Template 1: Design → Engineering Handoff

```
╔══════════════════════════════════════════════════════════════╗
║              DESIGN HANDOFF                                  ║
╚══════════════════════════════════════════════════════════════╝
Feature:        <feature name>
Designer:       <name>
Engineering lead: <name>
Handoff date:   <date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CURRENT STATE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Design files:   <link to Figma/design tool>
Status:         [Final | Final except: <what's still in flux>]
Screens:        <N> screens / states designed
What's final:   <list what engineering can build now>
What's not:     <list what is still being decided — DON'T build yet>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEY DECISIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <decision> — chosen: <X> — because: <reason>
□ <decision> — chosen: <X> — because: <reason>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OPEN ITEMS (do not build until resolved)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <item> — Owner: <designer/PM> — Expected by: <date>
□ <item> — Owner: <name> — Expected by: <date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KNOWN EDGE CASES AND SPECS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Empty states:   <link or description of empty state designs>
Error states:   <link or description>
Loading states: <link or description>
Responsive:     <breakpoints and behavior>
Accessibility:  <specific requirements>
Edge cases:     <list any unusual cases that were explicitly designed>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HOW TO CONTINUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Review the final screens in Figma — focus on [key screens] first
2. Start with [component/screen] — it has no dependencies
3. Don't start [X] until the open item about [Y] is resolved

Questions? Contact: <designer name + channel>
╚══════════════════════════════════════════════════════════════╝

RECEIVER ACCEPTANCE:
  [ ] I have reviewed the design files
  [ ] I understand what is final vs in flux
  [ ] I know who to contact with questions
  Signed: _________________ Date: _________
```

---

## Template 2: Engineering → QA Handoff

```
╔══════════════════════════════════════════════════════════════╗
║              DEV → QA HANDOFF                                ║
╚══════════════════════════════════════════════════════════════╝
Feature:        <feature name>
Developer:      <name>
QA lead:        <name>
Handoff date:   <date>
Branch/Build:   <branch name or build number>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CURRENT STATE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
What's built:   <what is complete and ready to test>
What's not:     <explicitly out of scope for this build>
Environment:    <where to test: staging URL, test credentials>
Test data:      <how to set up test scenarios — seeds, fixtures, scripts>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ACCEPTANCE CRITERIA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <criterion> — how to verify: <steps>
□ <criterion> — how to verify: <steps>
□ <criterion> — how to verify: <steps>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEY DECISIONS AND IMPLEMENTATION NOTES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <implementation detail QA needs to know> — because: <reason>
□ <edge case handled in code> — test by: <how>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KNOWN ISSUES (do not file as bugs)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <known issue> — status: <fixing in next PR / deferred / won't fix>
□ <known limitation> — by design: <reason>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AREAS TO FOCUS TESTING ON
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
High risk (test thoroughly):
  □ <area> — because: <why this is risky>

Lower risk (spot check):
  □ <area>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HOW TO CONTINUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. Set up test environment using: <link to setup instructions>
2. Start with: <the most critical acceptance criterion>
3. File bugs in: <tool + project>
4. Tag developer on blockers: <name + channel>
╚══════════════════════════════════════════════════════════════╝

RECEIVER ACCEPTANCE:
  [ ] Environment confirmed working
  [ ] Test data set up
  [ ] Acceptance criteria understood
  Signed: _________________ Date: _________
```

---

## Template 3: Team → Team / Person → Person Handoff

```
╔══════════════════════════════════════════════════════════════╗
║              OWNERSHIP HANDOFF                               ║
╚══════════════════════════════════════════════════════════════╝
What is being handed off: <project / feature / role / codebase>
From:           <name / team>
To:             <name / team>
Effective date: <date>
Transition period: <None | X days overlap ending on date>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CURRENT STATE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Where things stand:  <honest description of current state>
What is working:     <what is stable and running well>
What needs work:     <what has known issues or debt>
Phase:               <what lifecycle phase is this in>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEY DECISIONS (the "why" behind how things are)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <architectural/strategic decision> — chosen: <X> — because: <reason>
  (without this context, receiver may undo good decisions unknowingly)

□ <decision that looks wrong but isn't> — reason it's this way: <explanation>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
OPEN ITEMS AND IN-FLIGHT WORK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Item              Status          Urgency    New owner
──────────────   ─────────────   ────────   ─────────
<item>           In progress     High       <name>
<item>           Blocked by X    Medium     <name>
<item>           Not started     Low        <name>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KNOWN ISSUES AND RISKS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
□ <known technical issue> — impact: <what happens> — workaround: <if any>
□ <known risk> — likelihood: <H/M/L> — mitigation: <current approach>
□ <tech debt> — where: <location> — cost to carry: <estimate>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
KEY PEOPLE AND DEPENDENCIES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Stakeholders:   <who cares about this work and what they care about>
Dependencies:   <teams or systems this depends on — and key contacts>
Dependent on us: <who depends on this work — SLAs or commitments>
Key relationships: <people that have been important context holders>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ACCESS AND RESOURCES
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Repositories:   <links>
Documentation:  <links>
Dashboards:     <links to monitoring, analytics>
Credentials:    <not here — confirm transfer via secrets manager>
Runbooks:       <links to operational docs>
Slack channels: <relevant channels to join>

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
HOW TO CONTINUE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1. <first priority action>
2. <second priority action>
3. <third priority action>
Don't do: <anything the new owner should hold off on until they have more context>

Available for questions until: <date>
After that, contact: <escalation person if giver is unavailable>
╚══════════════════════════════════════════════════════════════╝

RECEIVER ACCEPTANCE:
  [ ] I have read and understood the full handoff document
  [ ] I have access to all listed resources
  [ ] I have no unresolved questions (or questions logged below)
  [ ] I accept ownership effective: <date>
  Signed: _________________ Date: _________

  Outstanding questions before accepting:
  □ <question> — needed from: <giver / other>
```

---

## Template 4: Escalation Handoff

For work that cannot be completed and is being escalated.

```
ESCALATION HANDOFF
══════════════════════════════════════════
From:           <name>
To:             <name / team>
Date:           <date>
Priority:       [Urgent | High | Normal]

WHAT I'M HANDING OFF:
  <brief description of the work/issue>

WHAT I'VE TRIED:
  □ <approach 1> — result: <what happened>
  □ <approach 2> — result: <what happened>

WHY I'M ESCALATING:
  <honest reason — missing knowledge / access / authority / time>

CURRENT STATE:
  <exactly where things stand right now>

WHAT THE RECEIVER NEEDS TO DO:
  <specific ask — not "please handle this" but "please do X">

RELEVANT CONTEXT:
  <links, logs, previous conversations>

URGENCY REASON:
  <why this needs to be resolved by when>

AVAILABLE FOR:
  <how long giver is available to answer questions about this>
══════════════════════════════════════════
```
