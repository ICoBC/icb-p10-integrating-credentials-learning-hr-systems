<!--
author:   ICoBC - International Council on Badges and Credentials
email:    info@icobc.net
version:  1.0.0
language: en
narrator: UK English Female
comment:  A hands-on lab in which learners design and demonstrate the data flow from learning completion to verifiable credential to talent or student record, including automation, identity matching and monitoring.
@style
:root { --icobc-navy:#02375B; --icobc-orange:#F78222; --icobc-cyan:#5DCAE7; --icobc-lime:#B1CF37; --icobc-mist:#F1F5F8; --icobc-line:#D8E2E9; --icobc-muted:#4F6576; }
body { font-family: "Open Sans", Arial, sans-serif; }
h1, h2, h3, h4 { color: var(--icobc-navy); }
a { color: var(--icobc-navy); }
table th { background: var(--icobc-mist); color: var(--icobc-navy); }
blockquote { border-left: 4px solid var(--icobc-orange); background: var(--icobc-mist); padding: 8px 16px; }
@end
-->

# Integrating Credentials with Learning and HR Systems

*Data flows from completion to credential to talent record.*

## About this course

This hands-on lab is for HR and learning technology owners, and for education IT and credential operations staff. You will work through realistic integration tasks in two settings: a corporate academy or HR department, and a university, college or training provider. By the end you will have an integration design and a working demonstration (or a documented configuration) that others can review.

The course belongs to the ICoBC Credentialing Skills Framework (ICSF), skill PLAT "Platforms and integration" (VRITE pillar: Trustworthiness), level L3 "Enable", stage A "Ability". At this level the skill is: selects platforms against requirements; integrates them with learning, HR and student systems; plans exit and data portability. At the Ability stage you practise the capability on realistic tasks, using the knowledge you have gained, in a controlled setting such as a case, simulation, sandbox or project.

**Prerequisite:** ICB-P07 Open Badges 3.0 and Verifiable Credentials: Hands-on Lab.

**Learning outcomes.** After this course you can:

1. Design the data flow between learning systems, credential platform and HR or student systems.
2. Configure automated issuing from completion data.
3. Import verified credentials into HR or learner records.
4. Plan identity matching, error handling and monitoring.

**Workload.** About 8 hours of notional learning. This file covers the self-paced part. Live or workshop elements appear as facilitator notes.

**Assessment.** An integration design document plus a working demonstration in a sandbox, or a documented configuration. A person assesses the final task with the rubric in "Assessment and badge". The knowledge check in that section supports the task. The file does not grade your design.

**Badge.** "Integrating Credentials with Learning and HR Systems (Ability, L3)", alignment code ICSF-PLAT-L3-A. Evidence: design document and demonstration record.

| Outcome | Module | How it is assessed |
|---|---|---|
| 1. Design the data flow between learning systems, credential platform and HR or student systems | Integration patterns | Design document (data flow section); knowledge check |
| 2. Configure automated issuing from completion data | Triggers and automation | Demonstration record or documented configuration; knowledge check |
| 3. Import verified credentials into HR or learner records | Bringing credentials into HR | Design document (import section); demonstration record; knowledge check |
| 4. Plan identity matching, error handling and monitoring | Identity matching | Design document (identity and error handling section); knowledge check |
| 4. Plan identity matching, error handling and monitoring | Monitoring | Design document (monitoring plan); knowledge check |

**How to work through the lab.** Keep one running project file. In each module you add a section to your integration design document. Use illustrative data only. Never use real personal data in a sandbox.

<details><summary>Facilitator notes</summary>

Suggested split of the 8 hours: about 5 hours self-paced (modules and quizzes), 1 hour for a live kick-off where participants choose their context (corporate or education), and about 2 hours of workshop time for the sandbox demonstration and peer review. Provide a sandbox or a shared test environment before the lab starts. If no sandbox is available, learners produce a documented configuration (screenshots or exported settings with annotations) instead. Remind participants not to paste real learner data into any shared tool.

</details>

## Integration patterns

### Why patterns come first

An integration moves data between systems that were built for different jobs. A learning management system (LMS) records enrolment and completion. A credential platform issues and stores credentials. An HR system holds the talent record. A student information system holds the learner record. Before you configure anything, decide which system owns which data and in which direction it moves.

Three questions shape every design:

- **Which system is the source of truth** for each data item (identity, completion, credential, skills)?
- **What starts the flow**: an event, a schedule, or a person?
- **What comes back**: do you need confirmation, a credential link, or a record update?

Current credential standards give you a common language for the credential itself. Open Badges 3.0 and CLR 2.0 are the current specifications [Established]. Platform support is still uneven, so check the conformance certification of each tool before you rely on it. Treat "supports Open Badges 3.0" as a claim to verify, not a fact to assume.

### The system landscape

<svg viewBox="0 0 800 450" width="100%" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>System landscape with data flows</title>
  <desc>Five boxes: learning system (cyan), credential platform (navy), HR or student system (orange), learner wallet (navy outline) and a monitoring log (lime). Arrows show completion data flowing from the learning system to the credential platform, credentials to the learner wallet, the verified credential from the wallet to the HR or student system, and events from all systems to the monitoring log.</desc>
  <rect x="0" y="0" width="800" height="450" fill="#FFFFFF"/>
  <rect x="30" y="60" width="190" height="90" rx="6" fill="#5DCAE7" stroke="#02375B" stroke-width="2"/>
  <text x="125" y="100" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Learning system</text>
  <text x="125" y="124" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Completion data</text>
  <rect x="305" y="60" width="190" height="90" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="100" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Credential platform</text>
  <text x="400" y="124" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Issues credentials</text>
  <rect x="580" y="60" width="190" height="90" rx="6" fill="#F78222" stroke="#02375B" stroke-width="2"/>
  <text x="675" y="100" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">HR or student system</text>
  <text x="675" y="124" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Talent or learner record</text>
  <rect x="305" y="230" width="190" height="80" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="266" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Learner wallet</text>
  <text x="400" y="290" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">Earner holds the credential</text>
  <rect x="30" y="350" width="740" height="60" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="386" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Monitoring log: events, errors, retries</text>
  <line x1="220" y1="105" x2="305" y2="105" stroke="#02375B" stroke-width="2"/>
  <polygon points="305,105 295,99 295,111" fill="#02375B"/>
  <text x="262" y="95" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">1 completion</text>
  <line x1="400" y1="150" x2="400" y2="230" stroke="#02375B" stroke-width="2"/>
  <polygon points="400,230 394,220 406,220" fill="#02375B"/>
  <text x="412" y="195" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">2 credential</text>
  <path d="M495 270 L675 270 L675 150" fill="none" stroke="#02375B" stroke-width="2"/>
  <polygon points="675,150 669,160 681,160" fill="#02375B"/>
  <text x="585" y="260" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">3 share and import</text>
  <line x1="125" y1="150" x2="125" y2="350" stroke="#D8E2E9" stroke-width="2"/>
  <line x1="675" y1="310" x2="675" y2="350" stroke="#D8E2E9" stroke-width="2"/>
</svg>

### Three common patterns

**Event-driven (push).** The learning system sends an event when a learner completes. The credential platform receives it and issues. This gives fast results and needs reliable event delivery.

**Scheduled batch (pull).** The credential platform reads a completion report at fixed times, for example each night. This is simple and easy to audit. It adds delay.

**Learner-mediated (share).** The earner receives the credential in a wallet and chooses to share it with HR or the student office. The receiving system verifies it. This respects learner control and suits cases where the employer or institution does not own the learning.

Many real designs combine them: event-driven or batch for issuing, learner-mediated for import.

### Data minimisation

Send only the fields the next system needs. A credential platform usually needs a stable learner identifier, an email address, the achievement identifier and the completion date. It rarely needs grades, health or line-manager details. Record in your design why each field is sent.

**Worked example: Two contexts, one pattern choice**

*Corporate academy (illustrative).* A learning and development lead at a corporate academy runs a compliance course in the LMS. Completion data is complete and clean. She chooses event-driven issuing so staff receive the credential within minutes. The credential is shared learner-mediated, and the HR system verifies it on import. Source of truth: LMS for completion, credential platform for the credential, HR system for the talent record.

*University (illustrative).* A credential operations officer at a university issues micro-credentials for short courses. Final marks are confirmed by an exam board after the course ends. He chooses a scheduled batch that runs only after the board signs off the results. Source of truth: student information system for results, credential platform for the credential. The wallet is the learner's own space.

In both cases the pattern follows from one fact: when the completion data becomes reliable.

**Practice activity: Map your landscape**

1. Choose your context (corporate academy or HR department; university, college or training provider).
2. List the systems involved. Use generic names such as "LMS", "credential platform", "HR system", "student information system".
3. For each of these data items, name the source of truth: learner identity, completion status, credential, skills record.
4. Choose one pattern (event-driven, batch, learner-mediated, or a combination) and write two sentences of justification.
5. Draw your own landscape on paper or in a diagram tool, with numbered arrows. Add it as section 1 of your integration design document.

<details><summary>Facilitator notes</summary>

Ask pairs to swap landscape drawings and find one data item without a named source of truth. Typical gaps: who owns the learner's email address, and who corrects a wrong completion date. Spend 15 minutes in plenary on the conformance question: how would your organisation check a vendor's claim of Open Badges 3.0 support?

</details>

### Check your understanding

**Question 1.** A university confirms course results only after an exam board meets. Which issuing pattern fits best?

[( )] Event-driven issuing at the moment the learner finishes the last activity
[(X)] Scheduled batch that runs after the exam board has confirmed results
[( )] Manual issuing by each tutor from a spreadsheet
[( )] No issuing until the learner asks for a credential
****
Results are only reliable after sign-off, so a batch run after confirmation avoids issuing credentials on unconfirmed data. Event-driven issuing would fire too early. Manual issuing adds error risk, and waiting for requests delays recognition.
****

**Question 2.** What is the main reason to name a "source of truth" for each data item?

[( )] It lets every system keep its own copy and edit it freely
[( )] It removes the need for monitoring
[(X)] It shows which system corrects an error and which copies must follow
[( )] It guarantees that all vendors support the same standard
****
A named source of truth tells you where a correction is made and which copies update afterwards. It does not remove the need for monitoring and does not guarantee vendor conformance.
****

**Question 3.** What should you do before relying on a tool's claim that it supports Open Badges 3.0?

[( )] Accept the claim, because the specification is current
[( )] Wait until all platforms support it equally
[(X)] Check the tool's conformance certification and test it in a sandbox
[( )] Replace Open Badges 3.0 with a proprietary format
****
Platform support is still uneven, so you verify conformance certification and test with sample data. Waiting or switching to a proprietary format creates delay or lock-in without solving the check.
****

**Question 4.** Which field is the best candidate to leave out of the data sent to a credential platform?

[( )] Stable learner identifier
[( )] Achievement identifier
[(X)] The learner's line manager's performance rating
[( )] Completion date
****
Data minimisation means sending only what the credential platform needs. A performance rating is not needed to issue a credential. The other three fields are the core of an issuing request.
****

## Triggers and automation

### From completion to issuing

Automated issuing turns a completion record into a credential without a person sending it by hand. Three parts make it work: a **trigger** that says when to issue, a **mapping** that says what to issue and to whom, and **rules** that stop wrong credentials going out.

### Triggers

A trigger is a condition in completion data. Typical triggers:

- A course status changes to "completed".
- A pass mark is reached on an assessment.
- A programme record shows that all required units are done.
- A staff member confirms that a practical assessment is passed.

Choose a trigger that matches the claim the credential makes. A badge for "completed the safety course" can fire on completion status. A badge that says "demonstrated safe handling" needs a trigger from an assessor decision, not from page views.

### Mapping

The mapping connects fields in the source to fields in the credential. In outline:

| Source field (illustrative) | Credential field | Note |
|---|---|---|
| Learner ID | Recipient identifier | Must match your identity rules |
| Course code | Achievement identifier | One course code maps to one achievement |
| Completion date | Issuance or achievement date | Agree the time zone and format |
| Course version | Achievement version | Keeps old and new versions apart |

Keep the mapping in a table that a colleague can read. A mapping that exists only inside a tool is hard to review.

### Rules and safeguards

Add rules before the credential is sent:

- **Eligibility:** only issue if all required conditions are true.
- **Duplicates:** do not issue the same achievement twice to the same person unless the rules allow renewal.
- **Review step:** for high-stakes credentials, hold the issue for a human check.
- **Revocation and correction:** plan what happens if the completion record is later corrected.

### Sequence of automated issuing

<svg viewBox="0 0 800 450" width="100%" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>Sequence of automated issuing</title>
  <desc>Four lanes: learner, learning system, credential platform, monitoring log. Steps: 1 learner completes the course; 2 learning system records completion and sends event; 3 credential platform checks rules and issues; 4 learner receives notification; 5 every step writes to the monitoring log.</desc>
  <rect x="0" y="0" width="800" height="450" fill="#FFFFFF"/>
  <rect x="20" y="30" width="170" height="40" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="105" y="56" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Learner</text>
  <rect x="215" y="30" width="170" height="40" rx="6" fill="#5DCAE7" stroke="#02375B" stroke-width="2"/>
  <text x="300" y="56" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Learning system</text>
  <rect x="410" y="30" width="170" height="40" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="495" y="56" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Credential platform</text>
  <rect x="605" y="30" width="170" height="40" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="690" y="56" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Monitoring log</text>
  <line x1="105" y1="70" x2="105" y2="420" stroke="#D8E2E9" stroke-width="2"/>
  <line x1="300" y1="70" x2="300" y2="420" stroke="#D8E2E9" stroke-width="2"/>
  <line x1="495" y1="70" x2="495" y2="420" stroke="#D8E2E9" stroke-width="2"/>
  <line x1="690" y1="70" x2="690" y2="420" stroke="#D8E2E9" stroke-width="2"/>
  <line x1="105" y1="110" x2="300" y2="110" stroke="#02375B" stroke-width="2"/>
  <polygon points="300,110 290,104 290,116" fill="#02375B"/>
  <text x="202" y="100" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">1 completes course</text>
  <line x1="300" y1="170" x2="495" y2="170" stroke="#02375B" stroke-width="2"/>
  <polygon points="495,170 485,164 485,176" fill="#02375B"/>
  <text x="397" y="160" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">2 completion event</text>
  <rect x="425" y="210" width="140" height="40" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="495" y="235" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">3 check rules</text>
  <line x1="495" y1="300" x2="105" y2="300" stroke="#02375B" stroke-width="2"/>
  <polygon points="105,300 115,294 115,306" fill="#02375B"/>
  <text x="300" y="290" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">4 credential offered</text>
  <line x1="495" y1="360" x2="690" y2="360" stroke="#02375B" stroke-width="2"/>
  <polygon points="690,360 680,354 680,366" fill="#02375B"/>
  <text x="592" y="350" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">5 write log entry</text>
</svg>

### Test before you switch on

Test with a small set of illustrative learners. Include normal cases and edge cases: a learner who completes twice, a learner with a missing email address, a course whose code changed, and a learner whose completion is later reversed. Record what happened in each case. This record becomes part of your demonstration.

**Worked example: Automated issuing for two contexts**

*Corporate academy (illustrative).* An HR operations analyst sets up issuing for a data protection awareness course. Trigger: LMS status "completed" and quiz score at or above the pass mark. Mapping: employee number to recipient identifier, course code to achievement identifier. Rule: no second credential within the validity period. Review step: none, because the course is low risk.

*Training provider (illustrative).* A credential operations officer at a training provider issues a certificate of attendance and a separate skills badge. The attendance certificate fires on "completed". The skills badge needs an assessor to mark "competent" in the learning system, and only that marking fires the second trigger. The two credentials use two different achievement identifiers, so the claims stay separate.

**Practice activity: Configure or document an issuing rule**

1. Pick one course or programme from your context.
2. Write the claim the credential makes in one sentence.
3. Choose the trigger that supports that claim. Explain why a weaker trigger would overstate the claim.
4. Complete a mapping table with at least four fields.
5. Write three rules (eligibility, duplicates, correction).
6. In your sandbox, configure the rule and run three test cases, or document the configuration step by step with annotated screenshots or exported settings.
7. Add the trigger, mapping, rules and test results as section 2 of your design document.

<details><summary>Facilitator notes</summary>

Run a 20-minute "break the rule" exercise: each pair gives a colleague a test case designed to trick the rules (for example a duplicate completion). The colleague predicts what the configuration will do before it runs. Discuss mismatches. If the sandbox cannot send events, use a batch import of a small file instead and note this in the demonstration record.

</details>

### Check your understanding

**Question 1.** A badge says "demonstrated safe handling of chemicals". Which trigger supports that claim best?

[( )] The learner opened all course pages
[( )] The learner enrolled in the course
[(X)] An assessor records a competent result for the practical task
[( )] The learner logged in on three days
****
The claim concerns demonstrated performance, so the trigger should come from an assessment decision. Page views, enrolment and logins show activity, not demonstrated handling.
****

**Question 2.** Why keep the field mapping in a readable table?

[( )] Tools cannot read tables, so it prevents automation
[( )] It replaces the need for testing
[(X)] A colleague can review and correct it without opening the tool
[( )] It is required by Open Badges 3.0
****
A readable mapping supports review and handover. It does not replace testing, and the specification does not require a table.
****

**Question 3.** A learner completes the same course twice. What does a sound duplicate rule do?

[( )] Issues two identical credentials every time
[( )] Deletes the first credential silently
[(X)] Applies the rule you chose in advance, for example no second credential unless renewal is allowed
[( )] Stops the integration for all learners
****
A duplicate rule applies a decision made in design, such as blocking a second credential or allowing a renewal. Silent deletion or stopping the whole integration are not controlled responses.
****

**Question 4.** What belongs in your test set for automated issuing?

[( )] Only learners with perfect data
[(X)] Normal cases and edge cases such as a missing email address or a reversed completion
[( )] Only real learner data from production
[( )] No tests, because the vendor has tested the product
****
Edge cases reveal rule gaps. Testing only perfect data hides problems, real personal data should not be used in a sandbox, and vendor testing does not cover your configuration.
****

## Bringing credentials into HR

### What "import" means

Importing a credential means that an HR or student system receives a credential, checks it, and stores the result in the person's record. The check matters more than the copy. A system that stores a PDF or an image without verification stores a claim, not a verified credential.

A verified import answers four questions:

1. **Who issued it?** Is the issuer known and, where relevant, on an accepted list?
2. **Is it authentic and unaltered?** Does the proof check out?
3. **Is it current?** Has it expired or been revoked?
4. **Does it belong to this person?** Does the recipient match the record? (Module 4 covers this in depth.)

If any answer is no or unknown, the system should not mark the credential as verified.

### Routes into the record

**Learner shares from a wallet.** The earner presents the credential to the HR or student system. The system verifies it and, with consent, stores the result.

**System retrieves by link.** The earner provides a link, and the system fetches and verifies the credential.

**Bulk import.** A set of credentials issued by your own organisation is loaded into the record. This suits internal credentials where the issuer and the record owner are the same organisation, but verification should still run.

### Learning and Employment Records

Learning and Employment Records (LER) bring credentials, skills and work history into one portable record. This area is **Emerging**. It is more advanced in the United States than in Europe. Treat LER capabilities in tools as claims to test, and design your integration so that it does not depend on any single LER feature.

### What to store

Store the minimum that supports your purpose. A practical set is:

- the credential identifier and the issuer identifier,
- the achievement name and identifier,
- the issue date and any expiry date,
- the verification result and the date of verification,
- a reference or link to the credential, not only a copy.

Keeping the verification date lets you show when the check happened. Plan how often to re-check credentials that can expire or be revoked.

### Data flow into the record

<svg viewBox="0 0 800 450" width="100%" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>Verified import into an HR or learner record</title>
  <desc>A flow from left to right: learner wallet presents a credential, a verification step checks issuer, proof, status and recipient, and then either stores the result in the HR or learner record or sends the credential to an exception queue.</desc>
  <rect x="0" y="0" width="800" height="450" fill="#FFFFFF"/>
  <rect x="30" y="170" width="170" height="90" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="115" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Learner wallet</text>
  <text x="115" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">Presents credential</text>
  <rect x="300" y="150" width="200" height="130" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="180" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Verification</text>
  <text x="400" y="206" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Issuer known?</text>
  <text x="400" y="228" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Proof valid?</text>
  <text x="400" y="250" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Not expired or revoked?</text>
  <text x="400" y="272" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Recipient matches?</text>
  <rect x="600" y="60" width="170" height="100" rx="6" fill="#F78222" stroke="#02375B" stroke-width="2"/>
  <text x="685" y="100" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">HR or learner record</text>
  <text x="685" y="124" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Stores verified result</text>
  <rect x="600" y="290" width="170" height="100" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="685" y="330" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Exception queue</text>
  <text x="685" y="354" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Human review</text>
  <line x1="200" y1="215" x2="300" y2="215" stroke="#02375B" stroke-width="2"/>
  <polygon points="300,215 290,209 290,221" fill="#02375B"/>
  <path d="M500 190 L550 190 L550 110 L600 110" fill="none" stroke="#02375B" stroke-width="2"/>
  <polygon points="600,110 590,104 590,116" fill="#02375B"/>
  <text x="552" y="150" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">pass</text>
  <path d="M500 240 L550 240 L550 340 L600 340" fill="none" stroke="#02375B" stroke-width="2"/>
  <polygon points="600,340 590,334 590,346" fill="#02375B"/>
  <text x="552" y="300" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">fail or unclear</text>
</svg>

### AI skills extraction: precautions

Some HR and learning systems use AI to extract skills from imported credentials, descriptions or evidence, and to write them into a skills profile. This can save effort. It also adds risk. Apply these precautions:

- **Keep the credential as the evidence.** The extracted skill is a suggestion derived from the credential. It is not a new verified claim.
- **Label machine-generated skills** so people see which entries came from the credential and which from a model.
- **Check against a defined skills taxonomy** and log which taxonomy version was used.
- **Allow human review and correction,** and let the person see and challenge skills attached to their record.
- **Avoid decisions on extracted skills alone.** Do not use them as the only basis for hiring, promotion or dismissal. Check applicable law and your policy. <!-- VERIFY: applicable rules on automated profiling and AI-assisted employment decisions in the jurisdictions of the learners; confirm with legal counsel before publishing -->
- **Test for errors and bias** with sample credentials and review the results regularly.
- **Do not claim competence from a credential.** A credential records an achievement under stated criteria. It does not prove that the person can do a job.

**Worked example: Import in two contexts**

*Corporate academy (illustrative).* A talent systems manager lets employees present credentials from external training providers. The HR system verifies the credential, matches the recipient to the employee record, and stores the verification result with the date. An AI skills tool proposes skills from the achievement description. The proposals appear in a "suggested" state and the employee and line manager confirm or reject each one.

*University (illustrative).* A registry officer accepts verified micro-credentials from partner providers as evidence for credit consideration. The student information system verifies the credential and places it in an "evidence" area of the learner record. A committee decides on credit. The system does not award credit automatically.

**Practice activity: Design the import**

1. Choose your context and the receiving system (HR system or student information system).
2. Write the four verification questions as rules for your system.
3. List the fields you will store, and for each one write why you need it.
4. Define what happens when verification fails or is unclear (exception queue, who reviews, within what time).
5. Add a short paragraph on AI skills extraction: will you use it, what precautions apply, who reviews the output.
6. In your sandbox, import two test credentials (one valid, one with an unknown issuer or expired status) and record the outcome, or document the planned configuration. Add this as section 3 of your design document.

<details><summary>Facilitator notes</summary>

Discuss the difference between "stored" and "verified". Ask participants for one example of a record in their organisation that holds a claim nobody has checked. For the AI discussion, prepare two sample extracted skill lists, one plausible and one with a clear error, and let groups decide how their process would catch the error. Keep legal advice out of the room: remind participants to consult their own counsel.

</details>

### Check your understanding

**Question 1.** Which action turns an imported credential into a verified entry in a talent record?

[( )] Saving an image of the badge in the record
[( )] Copying the achievement name by hand
[(X)] Checking issuer, proof, status and recipient, then recording the result and date
[( )] Asking the manager to confirm that it looks right
****
Verification means checking the issuer, the proof, the current status and the recipient. An image or hand-copied text stores a claim without checking it.
****

**Question 2.** An AI tool proposes skills from an imported credential. What is the right treatment?

[( )] Add them as verified skills immediately
[(X)] Label them as machine-suggested and allow human review and correction
[( )] Use them as the sole basis for a promotion decision
[( )] Delete the credential and keep only the skills
****
Extracted skills are suggestions derived from the credential. They should be labelled, reviewable and correctable. Using them alone for decisions or discarding the evidence adds risk.
****

**Question 3.** Why store the verification date with the credential?

[( )] It replaces the credential's own issue date
[( )] It makes the record look more complete
[(X)] It shows when the check happened and helps you decide when to re-check
[( )] It proves the holder is competent in a job
****
The verification date documents the check and supports re-checking when credentials can expire or be revoked. It does not replace the issue date, and no credential proves job competence.
****

**Question 4.** How should you treat LER features in a tool, given the status of LER?

[( )] As settled and universal across Europe and the United States
[( )] As irrelevant to integration design
[(X)] As emerging: test them and avoid depending on a single LER feature
[( )] As a legal requirement
****
LER is emerging and more advanced in the United States than in Europe. Test claims and keep your design independent of any single feature.
****

## Identity matching

### The problem

A credential names a recipient. An HR or student record names a person. Matching decides whether they are the same person. A wrong match attaches a credential to the wrong record, which harms the person who lost it and the person who received it. A missed match leaves a valid credential unlinked.

### Identifiers

Choose identifiers in a stable order of preference:

1. **A stable internal identifier** that both systems hold, such as an employee or student number, passed at issuing time.
2. **A verified email address** that the earner has confirmed.
3. **A decentralised or wallet-based identifier** where your platforms support it.
4. **Name and date of birth** only as a last resort, as these collide and change.

Email addresses change when people leave, marry or switch roles. Names collide and vary in spelling. Do not rely on a name alone.

### Match rules

Write match rules as a decision list:

- **Exact match** on a stable identifier: link automatically.
- **Single match on a verified email:** link, and log the rule used.
- **Multiple candidates or partial match:** send to human review.
- **No candidate:** hold the credential and contact the earner.

Never link automatically on a weak match. The cost of a manual review is small compared with the cost of a wrong link.

### Error handling

Plan for these cases and write the response for each:

| Case | Response (example) |
|---|---|
| Email changed after issuing | Match on stable identifier; update the contact data |
| Two people share a name | Human review, using a second identifier |
| Credential issued to a personal email, record holds a work email | Ask the earner to confirm ownership, then link |
| Leaver whose record is archived | Follow retention policy; do not re-create the record only to link |
| Wrong credential linked | Unlink, record the correction, tell the affected people |

Define who owns the exception queue, how quickly cases are handled, and how the earner is told.

### Privacy

Identity matching handles personal data. Use the least data needed, restrict who can see the exception queue, and set a retention period for match logs. <!-- VERIFY: applicable data protection law and retention rules for the organisation's jurisdiction; confirm with the data protection officer -->

### Matching decision flow

<svg viewBox="0 0 800 450" width="100%" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>Identity matching decision flow</title>
  <desc>A top-down decision flow. Start with a credential. If a stable identifier matches exactly, link automatically. If not, check for a single verified email match and link with a log entry. If multiple or partial matches exist, send to human review. If no candidate exists, hold the credential and contact the earner.</desc>
  <rect x="0" y="0" width="800" height="450" fill="#FFFFFF"/>
  <rect x="300" y="20" width="200" height="50" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="51" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Credential arrives</text>
  <rect x="270" y="110" width="260" height="50" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="141" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Stable identifier matches exactly?</text>
  <rect x="270" y="210" width="260" height="50" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="241" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Single verified email match?</text>
  <rect x="270" y="310" width="260" height="50" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="341" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Several or partial candidates?</text>
  <rect x="600" y="110" width="180" height="50" rx="6" fill="#F78222" stroke="#02375B" stroke-width="2"/>
  <text x="690" y="141" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Link automatically</text>
  <rect x="600" y="210" width="180" height="50" rx="6" fill="#F78222" stroke="#02375B" stroke-width="2"/>
  <text x="690" y="241" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Link and log the rule</text>
  <rect x="600" y="310" width="180" height="50" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="690" y="341" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">Human review</text>
  <rect x="270" y="395" width="260" height="40" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="400" y="420" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">No candidate: hold, contact earner</text>
  <line x1="400" y1="70" x2="400" y2="110" stroke="#02375B" stroke-width="2"/>
  <line x1="400" y1="160" x2="400" y2="210" stroke="#02375B" stroke-width="2"/>
  <text x="410" y="190" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">no</text>
  <line x1="400" y1="260" x2="400" y2="310" stroke="#02375B" stroke-width="2"/>
  <text x="410" y="290" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">no</text>
  <line x1="400" y1="360" x2="400" y2="395" stroke="#02375B" stroke-width="2"/>
  <text x="410" y="382" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">no</text>
  <line x1="530" y1="135" x2="600" y2="135" stroke="#02375B" stroke-width="2"/>
  <text x="565" y="127" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">yes</text>
  <line x1="530" y1="235" x2="600" y2="235" stroke="#02375B" stroke-width="2"/>
  <text x="565" y="227" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">yes</text>
  <line x1="530" y1="335" x2="600" y2="335" stroke="#02375B" stroke-width="2"/>
  <text x="565" y="327" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">yes</text>
</svg>

**Worked example: Matching in two contexts**

*Corporate academy (illustrative).* A people analytics lead notices that 12 of 400 illustrative completions in a test run carry personal email addresses, while the HR system holds work addresses. The rule "match on employee number passed at issuing" links all 12 without relying on email. The team also adds a rule: an email change in the HR system updates the credential platform contact the next night.

*University (illustrative).* A student records officer handles a batch of credentials for a short course open to the public. Public participants have no student number. The rule: students link on student number; public participants link on a verified email and are stored as external learners. Two participants share a name and one email domain, so the rule "multiple candidates go to review" applies and a person checks a second identifier.

**Practice activity: Write the match rules and error plan**

1. Choose your context and list the identifiers each system holds.
2. Write a decision list of match rules, from strongest to weakest, with the action for each.
3. Complete an error handling table with at least five cases, including one case specific to your context.
4. Name the owner of the exception queue and set a target handling time (label it illustrative).
5. In your sandbox, create five test records that cover an exact match, an email-only match, a name collision, no candidate and a changed email. Run them and record the result, or document the planned configuration.
6. Add this as section 4 of your design document.

<details><summary>Facilitator notes</summary>

Hand out a short list of ten fictitious people with near-identical names and mixed identifiers. Ask groups to apply their rules by hand and report how many need human review. Discuss whether the review load is acceptable and how to reduce it, for example by capturing a stable identifier at enrolment.

</details>

### Check your understanding

**Question 1.** Which identifier is the best basis for automatic matching?

[( )] The person's full name
[( )] The person's date of birth
[(X)] A stable internal identifier held by both systems and passed at issuing time
[( )] The first part of the email address
****
A stable identifier shared by both systems is the most reliable. Names collide and change, dates of birth collide, and parts of email addresses are not unique.
****

**Question 2.** Two employees share the same name and department. A credential arrives with only that name. What should the system do?

[( )] Link to the employee who joined first
[( )] Link to both employees
[(X)] Send the credential to human review and request a second identifier
[( )] Reject all credentials with that name permanently
****
Several candidates call for human review with an additional identifier. Guessing or linking to both creates wrong links.
****

**Question 3.** A learner's email address changes after issuing. Which design handles this best?

[( )] Match only on email address
[(X)] Match on a stable identifier and update the contact data when email changes
[( )] Re-issue the credential to the new address and delete the old one
[( )] Ask the learner to create a new account each time
****
A stable identifier survives an email change. Matching only on email breaks, and re-issuing creates duplicates and confusion.
****

**Question 4.** Why log the rule used for each automatic match?

[( )] To make the log longer
[( )] To avoid human review altogether
[(X)] To let you trace and correct a wrong link and to review how well the rules work
[( )] Because logs replace verification
****
A logged rule makes each link traceable and supports improvement. Logs do not replace verification or human review.
****

## Monitoring

### Why monitor

An integration that worked on the day of testing can fail later: a field changes, a course code is retired, a certificate expires, a connection drops. Without monitoring, failures appear only when a learner complains. Monitoring gives you early signs and evidence for audit.

### What to monitor

Group your checks in four areas:

- **Flow health:** Did events or batches arrive? How many records were processed, issued, skipped or failed?
- **Data quality:** How many records lacked an identifier, email or course mapping?
- **Matching:** How many credentials were linked automatically, sent to review, or left unmatched?
- **Credential status:** How many credentials are close to expiry, expired or revoked, and did the receiving systems update?

### Simple measures

Define a small set of measures, each with an owner and a threshold. Examples (thresholds are illustrative):

| Measure | Question it answers | Illustrative threshold |
|---|---|---|
| Completions without a credential after 24 hours | Is issuing keeping up? | Alert if above 2 per cent |
| Records failing identity matching | Is data quality dropping? | Alert if above 5 per cent in a week |
| Exception queue age | Is review timely? | Alert if any case is older than 5 working days |
| Re-verification failures | Are expired or revoked credentials still trusted? | Alert on any failure |

### Error handling in practice

Plan these behaviours:

- **Retry:** transient errors, such as a timeout, are retried a set number of times with a pause.
- **Dead-letter or exception queue:** records that keep failing go to a queue for a person.
- **Alerting:** the right person is told, not everyone.
- **Idempotence:** running the same batch twice does not issue duplicates.
- **Runbook:** a short document says what to check and who to call.

### Reviews and change control

Review the measures on a schedule, for example monthly. Record changes to triggers, mappings and rules, with date and reason. Before a course code changes or an upgrade happens, run your test set again. Link monitoring to your exit and portability plan: keep an export of issued credentials and mappings so you can move to another platform without losing the history.

### Monitoring view

<svg viewBox="0 0 800 450" width="100%" role="img" xmlns="http://www.w3.org/2000/svg">
  <title>Monitoring loop</title>
  <desc>A loop with five steps: collect events from all systems, measure against thresholds, alert the owner, fix and retry or review, then improve rules and update the runbook. An arrow returns from improve to collect.</desc>
  <rect x="0" y="0" width="800" height="450" fill="#FFFFFF"/>
  <rect x="30" y="170" width="130" height="90" rx="6" fill="#B1CF37" stroke="#02375B" stroke-width="2"/>
  <text x="95" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Collect</text>
  <text x="95" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">events and logs</text>
  <rect x="190" y="170" width="130" height="90" rx="6" fill="#FFFFFF" stroke="#02375B" stroke-width="2"/>
  <text x="255" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Measure</text>
  <text x="255" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">against thresholds</text>
  <rect x="350" y="170" width="130" height="90" rx="6" fill="#F78222" stroke="#02375B" stroke-width="2"/>
  <text x="415" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Alert</text>
  <text x="415" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">the owner</text>
  <rect x="510" y="170" width="130" height="90" rx="6" fill="#5DCAE7" stroke="#02375B" stroke-width="2"/>
  <text x="575" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Fix</text>
  <text x="575" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">retry or review</text>
  <rect x="670" y="170" width="110" height="90" rx="6" fill="#F1F5F8" stroke="#02375B" stroke-width="2"/>
  <text x="725" y="210" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="16" fill="#02375B">Improve</text>
  <text x="725" y="234" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#02375B">rules, runbook</text>
  <line x1="160" y1="215" x2="190" y2="215" stroke="#02375B" stroke-width="2"/>
  <line x1="320" y1="215" x2="350" y2="215" stroke="#02375B" stroke-width="2"/>
  <line x1="480" y1="215" x2="510" y2="215" stroke="#02375B" stroke-width="2"/>
  <line x1="640" y1="215" x2="670" y2="215" stroke="#02375B" stroke-width="2"/>
  <path d="M725 260 L725 350 L95 350 L95 260" fill="none" stroke="#02375B" stroke-width="2"/>
  <polygon points="95,260 89,270 101,270" fill="#02375B"/>
  <text x="410" y="340" text-anchor="middle" font-family="Open Sans, Arial, sans-serif" font-size="14" fill="#4F6576">repeat on a schedule</text>
</svg>

**Worked example: Monitoring in two contexts**

*Corporate academy (illustrative).* A learning technology owner sets a daily check: completions in the LMS compared with credentials issued. On one Monday the check shows 40 completions and 31 credentials. The gap traces to a course code that was renamed. The owner updates the mapping, re-runs the batch (idempotent, so no duplicates), and adds "course code change" to the change checklist.

*Training provider (illustrative).* A credential operations officer reviews a weekly report of credentials near expiry and of verification failures at the employer side. Three employers report that re-verification fails after a key change at the provider. The officer follows the runbook, restores the key reference, and records the incident with date and fix.

**Practice activity: Build the monitoring plan**

1. Choose four measures from the four areas (flow, data quality, matching, status).
2. For each, write the question it answers, the owner, an illustrative threshold and the action when exceeded.
3. Describe retry, exception queue, alerting and idempotence for your integration.
4. Write a half-page runbook for one failure scenario of your choice.
5. Describe your exit and portability plan in five lines: what you export, in which format, how often, and who checks the export.
6. In your sandbox, trigger one failure on purpose (for example an unknown course code) and record how it appears in your logs, or document the planned configuration. Add this as section 5 of your design document.

<details><summary>Facilitator notes</summary>

Run a 20-minute incident drill. Give each group a short incident description (for example "40 completions, 31 credentials") and ask them to use their own runbook to find the cause. Collect what was missing from the runbooks and add it to the shared checklist. Remind groups that thresholds in this course are illustrative and must be set from their own volumes and risk.

</details>

### Check your understanding

**Question 1.** What does idempotence mean for a batch issuing job?

[( )] The job runs faster each time
[(X)] Running the same batch again does not create duplicate credentials
[( )] The job deletes old credentials before each run
[( )] The job runs only once per year
****
Idempotence means a repeated run gives the same result as one run, so a re-run after a fix creates no duplicates.
****

**Question 2.** Which measure shows that issuing is falling behind?

[( )] Number of courses in the catalogue
[(X)] Completions without a credential after an agreed time
[( )] Number of logins to the credential platform
[( )] Length of the course descriptions
****
Completions without a credential after the agreed time compare input with output of the issuing flow. The other measures do not show issuing delay.
****

**Question 3.** What is the purpose of an exception queue?

[( )] To hide failed records from the logs
[( )] To delete records that fail twice
[(X)] To hold records that keep failing so a person can review and resolve them
[( )] To speed up the retry process
****
An exception queue holds persistent failures for human review. It does not hide or delete them.
****

**Question 4.** Why include an export of issued credentials and mappings in your monitoring routine?

[( )] To increase storage costs
[( )] To avoid verifying credentials
[(X)] To support exit and data portability if you change platform
[( )] Because the specification requires daily exports
****
Regular exports keep your history portable and make a platform change possible without data loss. The specification does not require daily exports.
****

## Assessment and badge

### The assessed task

You submit two pieces of evidence, assessed by a person using the rubric below. The file does not grade your work.

1. **Integration design document** (about 6 to 10 pages). It contains these sections: (1) system landscape and data flow, (2) triggers, mapping and rules for automated issuing, (3) import into the HR or learner record, including verification and AI skills extraction precautions, (4) identity matching rules and error handling, (5) monitoring plan and exit and portability plan.
2. **Demonstration record.** A working demonstration in a sandbox (a short screen recording or a step-by-step log with timestamps), or a documented configuration with annotated screenshots or exported settings. It shows at least: one automated issue from completion data, one verified import, one identity match case and one handled error.

Use illustrative or synthetic data only. Do not submit real personal data.

### Rubric

Each criterion is rated on four levels. The assessor records a level and a short comment.

| Criterion | Not yet | Developing | Meets the standard | Strong |
|---|---|---|---|---|
| Data flow design (Outcome 1) | Landscape missing or systems unclear | Landscape shown, sources of truth or direction partly missing | Landscape with named sources of truth, directions and a justified pattern | As "meets", plus data minimisation reasons for each field and a stated alternative pattern |
| Automated issuing (Outcome 2) | No trigger or mapping | Trigger or mapping present but does not support the claim | Trigger matches the claim, mapping table and rules for eligibility, duplicates and correction, tested | As "meets", plus edge-case tests and a review step where risk is high |
| Import into records (Outcome 3) | Credential stored without checks | Some checks described, outcome or exceptions unclear | Issuer, proof, status and recipient checked; stored fields justified; exceptions handled; AI precautions stated | As "meets", plus re-verification schedule and a clear human review route for extracted skills |
| Identity matching and errors (Outcome 4) | No match rules | Rules rely on weak identifiers or lack error cases | Ordered match rules, at least five error cases with responses and an owner | As "meets", plus privacy measures and a plan to reduce manual review |
| Monitoring (Outcome 4) | No monitoring | Measures listed without owners or thresholds | Four measures with owners, thresholds and actions; retry, exception queue, idempotence and runbook described | As "meets", plus exit and portability plan tested with an export |
| Demonstration (Outcomes 2 and 3) | No evidence of operation | Partial evidence, steps not reproducible | Evidence shows issue, import, match and error handling and others can follow it | As "meets", plus documented test results and lessons learned |
| Clarity and trustworthiness | Unclear, unsupported claims | Mostly clear, some unsupported claims | Clear, consistent, claims supported by the design, no claim that a credential proves job competence | As "meets", plus open questions and risks listed with owners |

### Pass rule

To earn the badge, the submission must reach "Meets the standard" or "Strong" on every criterion. A rating of "Developing" or "Not yet" on any criterion means the assessor returns the submission with comments, and you may resubmit. The assessor is a person who is not the author of the submission.

### Evidence submitted for the badge

- The integration design document.
- The demonstration record (recording, log or documented configuration).
- The completed rubric with the assessor's name, date and comments.

The badge is "Integrating Credentials with Learning and HR Systems (Ability, L3)", alignment code ICSF-PLAT-L3-A.

<details><summary>Facilitator notes</summary>

Brief assessors before the lab and agree one calibration submission. Check that every submission uses synthetic data. Offer a 30-minute feedback conversation after assessment. For learners without a sandbox, accept a documented configuration, and ask for at least annotated screenshots or exported settings that show each step.

</details>

### Final knowledge check

The ten questions below support the task and cover all four outcomes. Answer them before you submit.

**Question 1.** A university issues credentials only after an exam board confirms results. Which design choice supports a trustworthy flow?

[( )] Event-driven issuing at the last quiz submission
[(X)] Batch issuing after the confirmed results are released, with the student system as source of truth
[( )] Issuing by hand from tutors' notes
[( )] Letting learners request credentials at any time
****
Results are reliable only after sign-off, so issuing after confirmation avoids credentials on unconfirmed data. This supports Outcome 1, designing the data flow.
****

**Question 2.** Which statement describes a good data flow design?

[( )] Every system stores every data item to be safe
[(X)] Each data item has a named source of truth and only needed fields are sent
[( )] The direction of data is decided when problems appear
[( )] The credential platform owns all learner data
****
A good design names the source of truth, direction and purpose of each field, and applies data minimisation. This supports Outcome 1.
****

**Question 3.** A badge claims "demonstrated customer complaint handling". Which trigger best supports the claim?

[( )] The learner finished watching the videos
[( )] The learner enrolled in the programme
[(X)] An assessor records a competent result from observed or evidenced practice
[( )] The learner downloaded the course handout
****
The trigger must match the claim. An assessor decision supports a claim about demonstrated performance. This supports Outcome 2, configuring automated issuing.
****

**Question 4.** Which rule prevents the same achievement being issued twice by accident?

[( )] A rule that deletes the older credential
[(X)] A duplicate check on learner identifier and achievement identifier, with idempotent runs
[( )] A rule that stops issuing for the whole course
[( )] A rule that emails the learner twice
****
A duplicate check combined with idempotent processing prevents accidental double issuing. This supports Outcome 2.
****

**Question 5.** What should an HR system check before marking an imported credential as verified?

[( )] That the image looks professional
[( )] That the manager approves
[(X)] Issuer, proof, current status and recipient match
[( )] That the credential was uploaded as a PDF
****
Verification covers issuer, proof, status and recipient. Appearance, approval and file format do not verify a credential. This supports Outcome 3.
****

**Question 6.** An AI tool extracts skills from an imported credential. Which precaution is essential?

[( )] Treat the skills as verified because the credential is verified
[( )] Use the skills as the only basis for a hiring decision
[(X)] Label the skills as machine-suggested and allow people to review and correct them
[( )] Remove the credential after extraction
****
The credential is the evidence; extracted skills are suggestions. Labelling and review keep the record trustworthy. This supports Outcome 3.
****

**Question 7.** Which identifier is the best basis for automatic identity matching?

[( )] Full name
[( )] Date of birth
[(X)] A stable identifier held by both systems and passed at issuing time
[( )] A nickname from the learner profile
****
A shared stable identifier is the most reliable. Names and dates of birth collide or change. This supports Outcome 4, identity matching.
****

**Question 8.** Two possible records match a credential. What does a sound design do?

[( )] Link to the first record found
[( )] Link to both records
[(X)] Send to human review and ask for a second identifier
[( )] Delete the credential
****
Multiple candidates call for human review. Guessing or linking to both creates wrong links. This supports Outcome 4.
****

**Question 9.** Which monitoring measure shows that data quality is falling?

[( )] The number of courses offered
[( )] The number of staff in the HR team
[(X)] The percentage of records failing identity matching or missing a mapping
[( )] The number of pages in the runbook
****
Rising failures in matching or mapping show falling data quality, and a threshold with an owner turns this into an alert. This supports Outcome 4, monitoring.
****

**Question 10.** Why keep an export of issued credentials and mappings as part of operations?

[( )] To make the integration slower
[( )] To avoid monitoring
[(X)] To support exit and data portability if you change platform
[( )] Because every specification requires daily exports
****
Regular exports keep your history portable and make a platform change possible without data loss. This links monitoring to the exit plan in Outcome 4.
****

## Sources and further reading

Status as of November 2026; check before publishing.

- 1EdTech Consortium: Open Badges Specification, version 3.0, and Comprehensive Learner Record (CLR) Standard, version 2.0. Official specification texts and conformance information are published by 1EdTech. <!-- VERIFY: exact current titles, version numbers and conformance programme details on the 1EdTech website -->
- World Wide Web Consortium (W3C): Verifiable Credentials Data Model, version 2.0, and related Decentralized Identifiers (DIDs) specification. <!-- VERIFY: current status (Recommendation) and version of each W3C specification -->
- ICoBC: Credentialing Skills Framework (ICSF), skill PLAT "Platforms and integration" and the VRITE pillars. Obtain the current version from ICoBC (info@icobc.net).
- ICoBC course ICB-P07: Open Badges 3.0 and Verifiable Credentials: Hands-on Lab (prerequisite).
- Learning and Employment Records (LER): the topic is emerging and more advanced in the United States than in Europe. Consult official publications of the bodies in your jurisdiction. <!-- VERIFY: add specific official LER sources after checking they exist and are current -->
- Your national or regional data protection authority for guidance on personal data in credentials, identity matching and AI-assisted profiling. <!-- VERIFY: name the relevant authority and guidance for each jurisdiction the course will be used in -->
