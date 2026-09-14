# Security Patrol Checkpoint Redirect

Permanent public checkpoint URL layer for the Security Patrol System.

Production checkpoint URL pattern:

`https://securitypatrolop.github.io/checkpoint/?point=<checkpoint-number>`

Example:

https://securitypatrolop.github.io/checkpoint/?point=1

Valid production checkpoint numbers are `1` through `22`.

---

## A Little Background

I’m not a professional software developer.

Back in university, I learned some C and C++. This was also the era when, during exams, we sometimes had to write code using an actual pen on actual paper — no compiler, no syntax highlighting, no Stack Overflow, just confidence and whatever you could remember.

So technically, I had some programming background.

Then many years passed.

With the help of AI, I somehow ended up building this.

What started as a simple idea to make security patrol records harder to fake gradually became a full patrol verification system with:

- QR checkpoint scanning
- strict checkpoint sequence
- GPS location verification
- patrol timing
- audit logs
- Supervisor monitoring
- signed completion records
- verification QR codes
- permanent checkpoint links

The goal was never to build a giant commercial security platform.

The goal was much simpler:

> Prove that a patrol actually reached the required checkpoint, in the correct order, at the correct location, and leave behind a record that can be independently verified.

AI has helped heavily with the coding, debugging, architecture, and occasionally explaining JavaScript to someone whose last serious programming memories involved C, C++, and handwritten exam code.

I still make the final decisions, test the system in the real environment, break things occasionally, fix them, and then try very hard not to break the parts that are already working.

This repository is one part of that system.

---

## About This Repository

This repository provides the permanent public URL layer used by the physical Security Patrol checkpoint QR signs.

The physical QR codes do **not** point directly to Google Apps Script.

Instead, they point to URLs like:

`https://securitypatrolop.github.io/checkpoint/?point=1`

through:

`https://securitypatrolop.github.io/checkpoint/?point=22`

The checkpoint number is passed through the redirect layer to the current Security Patrol web application.

---

## Why This Exists

Google Apps Script deployment URLs can change.

Physical QR signs are slightly more annoying to update.

So instead of encoding the current backend URL directly into every physical QR code, the QR signs point to this permanent GitHub Pages address.

If the backend URL changes, the redirect file can be updated while the physical QR signs remain untouched.

Or, stated more simply:

> I would rather edit one line of HTML than climb around replacing 22 QR signs.

That is the entire reason this layer exists.

---

## Production URL Format

The permanent checkpoint format is:

`https://securitypatrolop.github.io/checkpoint/?point=N`

Where `N` represents the checkpoint number.

Production currently uses checkpoint numbers:

`1` through `22`

Examples:

`https://securitypatrolop.github.io/checkpoint/?point=1`

`https://securitypatrolop.github.io/checkpoint/?point=22`

---

## Production File

The live redirect is located at:

`/checkpoint/index.html`

This is the production file used by the physical checkpoint QR codes.

Its main responsibility is simple:

1. Read the checkpoint number from the URL.
2. Confirm it is within the valid checkpoint range.
3. Build the current Security Patrol backend URL.
4. Pass the checkpoint number to the Guard system.
5. Redirect the browser.

The redirect is intentionally simple.

Its job is infrastructure, not patrol verification.

---

## Critical Rule

**DO NOT casually change the permanent checkpoint URL format.**

The physical QR signs already contain these URLs.

Changing:

`https://securitypatrolop.github.io/checkpoint/?point=N`

could require affected QR signs to be regenerated and physically replaced.

The backend destination may change.

The redirect implementation may change.

The physical checkpoint URL should remain stable.

This is one of the most important rules in the entire system.

---

## Current Checkpoints

Production currently uses 22 fixed checkpoint numbers.

The redirect accepts valid checkpoint numbers within the configured range.

Invalid or missing checkpoint numbers must not be treated as valid patrol checkpoints.

The checkpoint names and operational configuration are maintained by the main patrol system, not by this repository.

This repository only handles the permanent URL layer.

---

## What This Redirect Does Not Do

This repository does **not** verify patrol activity.

It does not independently determine:

- whether a patrol is active
- whether the guard scanned checkpoints in the correct order
- whether the guard is physically near the checkpoint
- whether GPS accuracy is acceptable
- whether a checkpoint is early, on time, or late
- whether a QR scan should be accepted
- whether a patrol is completed

Those decisions are handled by the authoritative backend.

In short:

> The QR identifies the checkpoint. The backend decides whether the visit counts.

---

## Typical Checkpoint Flow

A normal checkpoint visit works roughly like this:

1. The guard reaches a physical checkpoint.
2. The physical QR code contains a permanent GitHub Pages URL.
3. The checkpoint number is included in the URL.
4. GitHub Pages loads `/checkpoint/index.html`.
5. The redirect validates the checkpoint number.
6. The browser is redirected to the current Security Patrol web application.
7. The Guard system receives the checkpoint number.
8. The patrol backend checks sequence, GPS, timing and patrol state.
9. The checkpoint is accepted or rejected by the backend.

The redirect itself does not make the acceptance decision.

---

## Deployment

If the backend Web App URL changes:

1. Confirm the new production backend URL.
2. Open `/checkpoint/index.html`.
3. Update only the backend target URL.
4. Commit the change.
5. Wait for GitHub Pages to publish.
6. Test Checkpoint 1.
7. Test at least one other checkpoint.
8. Confirm the correct checkpoint reaches the Guard system.
9. Test using a real mobile phone.
10. Only consider the change complete after the production QR flow works.

Do not update the physical QR codes just because the backend URL changed.

That is exactly what this redirect layer is designed to avoid.

---

## Important

This repository must remain **PUBLIC**.

GitHub Pages serves the physical checkpoint URLs from this repository.

If the repository is changed to Private, the public checkpoint URLs may stop working and physical QR scans can return a 404 page.

Yes, this was learned through practical testing.

---

## Repository Structure

The important production structure is:

```text
securitypatrolop.github.io/
├── README.md
└── checkpoint/
    └── index.html
```

The checkpoint redirect must remain available at:

`/checkpoint/index.html`

---

## Security

This repository contains only public redirect infrastructure.

Do not store sensitive information here.

Do not commit:

- passwords
- signing secrets
- Supervisor credentials
- authentication tokens
- private patrol records
- private Google Sheet data
- private backend configuration
- API secrets

The actual patrol records, authentication and verification logic remain on the backend.

---

## Reliability Principles

This redirect should remain boring.

That is a compliment.

The important rules are:

- permanent QR URLs should remain stable
- valid checkpoint numbers should redirect correctly
- invalid checkpoint numbers should not be accepted
- backend URL changes should require only a small redirect update
- browser history should not be unnecessarily polluted by the redirect
- the redirect must remain publicly reachable
- changes should be tested from a real mobile device

This component should do one thing well:

> Keep the physical QR codes working.

---

## Why Not Put the Backend URL Directly in the QR?

Because the physical QR signs are permanent infrastructure.

Backend deployments are not.

Directly encoding a temporary or changeable application URL into physical signs would tightly couple the printed QR codes to the current hosting setup.

Using this redirect layer separates the two.

That gives the system flexibility to change backend deployments without touching the physical checkpoint signs.

---

## Related Components

The complete Security Patrol System also includes:

- Guard interface
- QR scanner
- Google Apps Script patrol backend
- GPS verification
- strict checkpoint sequence enforcement
- patrol timing
- Patrol Log
- Supervisor Portal
- signed patrol summaries
- public authenticity verification

This repository contains only the permanent checkpoint URL layer.

---

## Project Philosophy

The system is intentionally focused on patrol verification rather than becoming a large workforce-management platform.

The important questions are:

- Was the required checkpoint reached?
- Was it reached in the correct order?
- Was the guard actually at the required location?
- Was the checkpoint accepted by the backend?
- Can the completed patrol record be independently verified later?

This repository contributes one small but important part of that chain:

keeping the physical checkpoint identity stable.

---

## Final Note

This may be one of the simplest repositories in the whole project.

And that is exactly how I want it.

If everything is working correctly, nobody should notice this redirect exists.

They scan the QR, the patrol system opens, and life goes on.

The only time this repository becomes interesting is when someone accidentally makes it Private and all 22 checkpoints suddenly become very expensive-looking decorations.
