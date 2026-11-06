# Fellowship Portal

The member list for the USC Transplant Surgery Fellowship's sites: **https://portal.usctransplant.life**

- **Everyone on the member list** sees it: names, emails, phone numbers, fellowship years or job titles, and what each
  person can do on each site — with links to the sites they can use. Each person can edit their own information
  (not their primary email, roles or ticks), choose which automatic emails they get at each of their addresses
  (**Emails you get**; the schedule emails link to it as `/#emails`), and make a Google account's email their primary
  email (**How you sign in**).
- **Admins** add, edit and remove people and set their role on each site (time off, call schedule, portal): none,
  viewer, fellow and/or admin. They mark **attendings** (offered by last name when adding a living donor case) and
  **HPB fellows** (who may share a service with another fellow), set fellows' **start and graduating years** (fellows can set their own too; a fellow
  counts from August of their start year through July 31 of their graduating year, so incoming fellows are added in
  the spring and graduates drop out on Aug 1 by themselves), approve or decline **sign-ups**, share the calendars
  with every fellow, change anyone's emails, **invite** people who haven't signed in yet, and see the recent changes.
  Only a super admin can make or change super admins, and only super admins see **Site activity**: each person's
  visits (when, how long, how they signed in) and what they did.
- **New people** sign up at `/signup.html`: they sign in (with Google, or a code emailed to any address) and say who
  they are; an admin approves them. The page also explains how to make a Google account with a non-Gmail address.
- **Invited people** land on `/welcome.html?invite=…` (the link in their invitation email): it says what the sites are
  for, and they sign in with the invited email or another one (which then becomes their primary email).
- **https://usctransplant.life** (and www) forwards here: Squarespace → Domains → usctransplant.life → Website →
  Domain forwarding.

It also holds `session.html`, which keeps the sign-in for all three sites: sign in on any of them and you are
signed in on the others; Sign out on any of them signs out of all, including pages already open.

## How it fits together

```
Browser ──► this repository (GitHub Pages: the pages people see)
              │  "Sign in with Google" → a Google ID token (kept by session.html for all three sites)
              ▼
            Google Apps Script web app (private repository: timeoff-backend, registry.gs)
              checks the token, then reads and saves the member list (Script Properties)
```

The same server runs the time-off site (https://timeoff.usctransplant.life) and the call schedule
(https://schedule.usctransplant.life), and both ask the member list who may do what. This repository holds only the
pages — no personal data.

| File | What it is |
|---|---|
| `index.html` | The portal |
| `signup.html` | Signing up (and how to make a Google account with any email address) |
| `welcome.html` | Where invitation emails link: accepting an invitation |
| `session.html` | The shared sign-in (opened invisibly by every page of the three sites) |
| `assets/portal.css` | Styles used only by this site |
| `assets/styles.css`, `assets/ui.js`, `assets/api.js`, `assets/fonts/` | **Shared with the time-off and schedule sites** — keep identical in all three repositories (a GitHub check compares them) |
| `CNAME` | The custom domain for GitHub Pages |

## Making changes

- Page changes: edit here and commit to `main`; GitHub Pages publishes within a minute or two.
- Shared files: make the same change in the `timeoff` and `schedule` repositories. The **Shared files** check on the
  Actions tab fails when the copies differ.
- If the Google sign-in client changes, update `CLIENT_ID` in `session.html` too (and see the time-off README).
- If a site's address changes or a site is added, update `SITES` in `session.html` (the sites allowed to use the sign-in)
  and `RG_APPS` in `registry.gs`.
- Roles, apps and the rules for changing people are in the private `timeoff-backend` repository (`registry.gs`).

## Hosting

GitHub Pages: Settings → Pages → Deploy from branch `main` / root, custom domain `portal.usctransplant.life`, Enforce HTTPS.
DNS (Squarespace, domain usctransplant.life): CNAME `portal` → `usc-transplant-surgery-fellowship.github.io`.
Sign-in: the Google Cloud OAuth client lists `https://portal.usctransplant.life` as an authorized JavaScript origin.
