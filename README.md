# cloud-audit

The ZopDev free cloud cost audit campaign, packaged for deployment. One static
page served by nginx on **port 8000**.

Deploy this repository as a Git-sourced service. There is no build step, no
runtime dependency, and no application code — the Dockerfile copies four things
into an nginx image and exposes 8000.

```
index.html      the campaign
styles.css
audit.js        lead adapter
assets/         self-hosted fonts, ZopDev logos, provider marks
Dockerfile      nginx:1.27-alpine, listens on 8000
nginx.conf
```

## Run it locally

```bash
docker build -t cloud-audit . && docker run --rm -p 8000:8000 cloud-audit
```

## State of the page

This is a **pre-launch draft**, deliberately:

- It carries `<meta name="robots" content="noindex, nofollow">`.
- The form is in **preview mode**. `endpoint` and `privacyUrl` in `audit.js`
  are empty, so submitting sends nothing and stores nothing. To go live, set
  both to approved HTTPS addresses; the relay must return `{"success": true}`
  only after durable lead receipt, deduplicate by `requestId`, check origin,
  validate input and rate-limit server-side. No CRM credential belongs in
  client code.

Two claims still need confirming before this is shown publicly: the "one free
audit per organization every 30 days" limit stated in the first FAQ, and
whether Databricks and Snowflake spend is actually audited, since both marks
appear under a row labelled "FOR YOUR CLOUD".

Product truth, brand rules, the full launch checklist and the QA harness live in
the working repository, `Ravdesigns/cloud-audit-landing-page`. This repo is
deployment only, so nothing internal ships in the image.
