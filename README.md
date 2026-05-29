# Velociti Bikes — Marketing Site

Static marketing site for Velociti Bikes (Lagos, Nigeria).

- **`index.html`** — home: hero, about, bike showcase (4 cards), repair packages (3 tiers), Salesforce-ready Web-to-Lead enquiry form.
- **`repair.html`** — dedicated repair-ticket page with a Salesforce-ready Web-to-Case form (subject, service package, priority, bike model, issue description).

## Tech

- Plain HTML + Tailwind CSS (via CDN — zero build step)
- Inter font via Google Fonts
- Hero and product images hot-linked from Unsplash
- Vanilla JS for form validation + success state

## Local preview

Just open `index.html` in any modern browser. No install, no build.

## Deploy to GitHub Pages

1. Create a new GitHub repo (e.g. `velociti-bikes`)
2. From this folder:
   ```
   git init
   git add .
   git commit -m "Initial Velociti site"
   git branch -M main
   git remote add origin https://github.com/<your-user>/velociti-bikes.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Branch: `main` / `(root)` → Save**
4. Wait ~1 minute. Site lives at `https://<your-user>.github.io/velociti-bikes/`

## Go-live checklist

Before pointing real traffic at this site:

**Salesforce — Web-to-Lead (`index.html` enquiry form):** ✅ wired
- Endpoint: `https://webto.salesforce.com/servlet/servlet.WebToLead?encoding=UTF-8&orgId=00Dg500000AQs73`
- Hidden `oid` = `00Dg500000AQs73`
- Hidden `retURL` = `https://mchenryspagg.github.io/velociti-bike/index.html#enquire`
  - The form posts into a hidden `<iframe>` (`target="sf-lead-sink"`), so the main page does **not** navigate. The inline "Thanks…" success message is shown by JS immediately on valid submit — Salesforce's response (and the `retURL` redirect) loads invisibly inside the iframe.
  - This means the success UX works the same locally (file://) and on the live site.
- [ ] Test end-to-end: submit a real test lead from the live site, confirm it appears in Salesforce, confirm the inline success message shows.
- [ ] (Optional) Uncomment the `debug` / `debugEmail` hidden inputs in the form during testing — they will email you the raw submission and any Salesforce validation errors.

**Salesforce — Web-to-Case (`repair.html` ticket form):**
- [ ] Replace `<form action="#" ...>` with the real Salesforce Web-to-Case endpoint
- [ ] Add the hidden `<input type="hidden" name="orgid" value="YOUR_SALESFORCE_ORG_ID">`
- [ ] Confirm the `type` and `priority` picklist values exactly match the values in your Salesforce Case object
- [ ] Map `bike_model` to a custom field on Case (it's not a Salesforce standard field name)

**Content & polish:**
- [ ] Swap the Unsplash hero and bike images for owned/licensed photography
- [ ] Update the footer address ("123 Admiralty Way, Lekki Phase 1") to the real shop address
- [ ] Confirm repair package prices (₦35,000 / ₦120,000 / ₦250,000) match real pricing
- [ ] Add real analytics (Plausible / GA4)
- [ ] Add `<meta>` Open Graph and Twitter card tags for social sharing
- [ ] Confirm all required-field validations work in the browsers your audience uses

## Form field reference

### Web-to-Lead (`index.html` — enquiry form)
Field names match the Salesforce Lead object exactly:

| Label | Name | Type | Required |
|---|---|---|---|
| First Name | `first_name` | text (max 40) | yes |
| Last Name | `last_name` | text (max 80) | yes |
| Email | `email` | email | yes |
| Phone | `phone` | tel | yes |
| Company | `company` | text (defaults to `Self` if blank) | no |
| Lead Source | `lead_source` | hidden, value `Website` | yes |
| Description | `description` | textarea | no |

### Web-to-Case (`repair.html` — repair ticket form)
Field names match the Salesforce Case object (plus one custom field):

| Label | Name | Type | Required |
|---|---|---|---|
| Full Name | `name` | text (max 80) | yes |
| Email | `email` | email | yes |
| Phone | `phone` | tel | yes |
| Subject | `subject` | text (max 80) | yes |
| Service Package | `type` | picklist | yes |
| Priority | `priority` | picklist (Low/Medium/High) | no |
| Bike Make & Model | `bike_model` | text — **custom field**, map on go-live | no |
| Description | `description` | textarea | yes |
| Origin | `origin` | hidden, value `Web` | yes |
| Reason | `reason` | hidden, value `Service Request` | yes |
