# USC Transplant Surgery Fellowship — Portal

The member portal for the USC Transplant Surgery Fellowship's sites: **https://portal.usctransplant.life**

https://usctransplant.life and www forward here. Squarespace sets that forwarding under Domains → usctransplant.life → Website.

This repository holds only the pages. GitHub Pages serves them. They contain no personal data.
The server lives in the private `timeoff-backend` repository, mainly in `registry.gs`.
Its README covers administration, accounts and handover for all three sites.

## What people can do

**Every member**

- Looks people up: names, emails, phone numbers, fellowship years or job titles, and roles.
- Opens the sites they can use.
- Edits their own details, including fellowship years. Admins control email, roles and who they are.
- Chooses which automatic emails reach each of their addresses, under **Emails you get**.
  The schedule emails link straight there (`/#emails`).
- Makes a Google account's email their primary email, under **How you sign in**.
  That card appears after a code sign-in, or after a Google sign-in with a non-primary email.

**Admins**

- Add, edit and remove people.
- Set **who each person is**: transplant fellow, HPB fellow, attending, or other.
- Set each person's role per site: none, View, Edit, Admin, or Edit + Admin.
  Only fellows can have Edit. Edit lets them request time off and puts them on the call schedule.
- Set fellows' **start and graduating years**. A fellow counts from August of the start year through July 31 of the graduating year.
- Approve or decline **sign-ups**.
- **Invite** people who have never signed in.
- Change anyone's emails.
- Share the calendars with every fellow ("Check calendar sharing").
- Read the recent changes.

**Super admins** also create and change super admins, and see **Site activity**.
Site activity lists each person's visits, how they signed in, and what they did.

**New people** sign up at `/signup.html`. They sign in with Google or an emailed code, then describe themselves.
An admin approves them. The page also explains how to create a Google account for a non-Gmail address.

**Invited people** land on `/welcome.html?invite=…` from their invitation email.
The page explains the sites. They sign in with the invited email or another one.
Another email becomes their primary email, and the invited one becomes their alternate.

## Shared sign-in

A cookie for `usctransplant.life` holds the sign-in. `assets/api.js` reads and writes it on all three sites.

- Signing in on any site signs in on the other two.
- Signing out on any site signs out of all three. Pages already open notice at their next request.
- The cookie expires with the token: an hour for Google, days for an emailed code.
- The server never reads the cookie. Each request sends the token, and the server checks it.
- A cookie works in Safari too. Safari keeps storage in hidden frames apart per subdomain, so a frame can't share a sign-in.

## How it fits together

```
Browser ──► this repository (GitHub Pages)
   │  "Sign in with Google" or an emailed code → a sign-in token (kept in a usctransplant.life cookie)
   ▼
Apps Script web app (private repository: timeoff-backend, registry.gs)
   checks the token, then reads and saves the member list in Script Properties
```

The time-off site and the call schedule ask the same member list who may do what.

## Files

| File | Purpose |
|---|---|
| `index.html` | The portal |
| `signup.html` | Sign-up page, with help for creating a Google account |
| `welcome.html` | Invitation landing page. Without an invitation, it opens the portal |
| `assets/portal.css` | Styles for this site only |
| `assets/api.js`, `assets/ui.js`, `assets/styles.css`, `assets/fonts/` | Shared with `timeoff` and `schedule`. Keep them identical |
| `.github/workflows/shared-files.yml` | Fails when the shared files differ from the other two sites |
| `CNAME` | Custom domain for GitHub Pages |

## Making changes

- **Pages**: edit here and commit to `main`. GitHub Pages publishes within a minute or two.
- **Shared files**: make the same change in `timeoff` and `schedule`. The **Shared files** check catches differences.
- **New sign-in client**: update `CLIENT_ID` in `assets/api.js`. The `timeoff-backend` README lists every other place.
- **New or moved site**: update `RG_APPS` in `registry.gs`. A site outside `usctransplant.life` also needs `COOKIE_DOMAIN` changed in `assets/api.js`.
- **Roles, apps and the rules for changing people**: edit `registry.gs` in `timeoff-backend`.

## Hosting

- **GitHub Pages**: Settings → Pages → Deploy from branch `main`, folder `/`. Custom domain `portal.usctransplant.life`. Enforce HTTPS on.
- **DNS** (Squarespace, `usctransplant.life`): `CNAME portal → usc-transplant-surgery-fellowship.github.io`.
- **Sign-in**: the Google sign-in client lists `https://portal.usctransplant.life` as an authorized JavaScript origin.
