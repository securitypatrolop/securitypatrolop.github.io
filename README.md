# Security Patrol Checkpoint Redirect

Permanent public checkpoint URL layer for the Security Patrol System.

## A Little Background

I’m not a professional software developer.

Back in university, I learned some C and C++. This was also the era when, during exams, we sometimes had to write code using an actual pen on actual paper — no compiler, no syntax highlighting, no Stack Overflow, just confidence and whatever you could remember.

So technically, I had some programming background.

Then many years passed.

With the help of AI, I somehow ended up building this.

What started as a simple idea to make security patrol records harder to fake gradually became a patrol verification system involving QR checkpoints, GPS verification, strict checkpoint sequencing, audit logs, signed patrol records and independent verification.

This repository handles one very specific part of that system:

> Making sure the physical checkpoint QR codes keep working even if the backend URL changes.

---

## About This Repository

The physical checkpoint QR signs do **not** point directly to Google Apps Script.

Instead, they point to permanent GitHub Pages URLs such as:

`https://securitypatrolop.github.io/checkpoint/?point=1`

through:

`https://securitypatrolop.github.io/checkpoint/?point=22`

This repository then redirects each valid checkpoint to the current Security Patrol backend.

---

## Why This Exists

Google Apps Script deployment URLs can change.

Physical QR signs are slightly more annoying to update.

So instead of printing the backend URL directly into every QR code, the physical signs point to this permanent redirect layer.

If the backend URL ever changes, only the redirect configuration needs to be updated.

The QR signs themselves stay exactly the same.

Or, stated another way:

> I would rather edit one line of HTML than climb around replacing 22 QR signs.

---

## Production File

The production redirect is:

`/checkpoint/index.html`

This file contains the current Google Apps Script Web App URL used by the patrol system.

If the Apps Script deployment URL changes, update only the redirect target.

Do not redesign this file unnecessarily.

Its job is intentionally boring.

Boring infrastructure is good infrastructure.

---

## Permanent Checkpoint URL Format

The physical checkpoint URLs use this format:

`https://securitypatrolop.github.io/checkpoint/?point=N`

Where `N` is the checkpoint number.

Production currently uses:

`1` through `22`

Example:

`https://securitypatrolop.github.io/checkpoint/?point=1`

---

## Critical Rule

**DO NOT change the permanent checkpoint URL format casually.**

These URLs are already encoded into physical QR signs.

Changing the URL structure could require every affected QR sign to be regenerated and physically replaced.

The redirect target may change.

The physical checkpoint URL should not.

---

## What the Redirect Does

For a valid checkpoint number, the redirect:

1. Reads the `point` parameter.
2. Confirms that it is within the valid checkpoint range.
3. Builds the current backend URL.
4. Passes the checkpoint number to the Guard system.
5. Redirects the browser without leaving an unnecessary redirect page in browser history.

Invalid checkpoint numbers are not treated as valid patrol checkpoints.

---

## Security Model

This repository does **not** verify a patrol.

It only routes the checkpoint URL to the patrol system.

The actual patrol verification is performed by the backend, including:

- checkpoint sequence
- active patrol state
- GPS validation
- checkpoint timing
- duplicate scans
- incorrect checkpoint scans
- final patrol completion

So:

> The QR identifies the checkpoint. The backend decides whether the visit counts.

---

## Deployment

When changing the redirect:

1. Confirm the new backend URL is correct.
2. Update `/checkpoint/index.html`.
3. Commit the change.
4. Wait for GitHub Pages to publish.
5. Test Checkpoint 1.
6. Test at least one other checkpoint.
7. Confirm the correct checkpoint reaches the Guard interface.
8. Test from a real phone before considering the change complete.

---

## Important

This repository must remain **PUBLIC**.

GitHub Pages serves the physical checkpoint URLs from this repository.

If the repository is made Private, the checkpoint URLs may stop working and physical QR scans can return a 404 page.

Yes, this was also learned through field testing.

---

## Do Not Store Here

Do not commit:

- passwords
- signing secrets
- private spreadsheet IDs
- Supervisor credentials
- private patrol data
- authentication tokens
- backend secrets

This repository should contain only public redirect infrastructure.

---

## Related Components

The complete Security Patrol System also includes:

- Guard interface
- QR scanner
- Google Apps Script backend
- GPS verification
- Supervisor Portal
- Patrol Log
- signed patrol summaries
- public authenticity verification

This repository is only the permanent checkpoint URL layer.
