# Runbook — apply this config to Yext

Account ID: **4744809** (flyinghighindustries). No Yext CLI needed — Yext pulls this repo from GitHub directly. Entity content is created via inline `curl` (mirrors Sendoplex IT_DEPLOYMENT_GUIDE Step 1).

---

## Step 1 — Repo on GitHub

Already pushed to <https://github.com/flyinghighindustries/alexander-maasik-config>. If you need to push updates:

```bash
cd ~/alexander-maasik-config
git push
```

---

## Step 2 — Connect the repo to Yext

1. Yext → **Account Settings → Resources** (Estonian: *Konfiguratsiooniressursside lisamine GitHubist*).
2. Click **Connect GitHub repo**.
3. Pick `flyinghighindustries/alexander-maasik-config`, branch `main`.
4. **Save**.

---

## Step 3 — Repull and apply

Same screen:

1. Click **Repull** — Yext fetches the latest commit.
2. Click **Apply** — Yext creates the 15 custom fields + attaches them to `location`.

Expected after Apply:

- **Knowledge Graph → Configuration → Custom Fields** → 15 fields visible (`c_tagline`, `c_heroSubheadline`, …, `c_ctaBody`).
- **Knowledge Graph → Configuration → Field Eligibility → location.default** → those 15 fields listed.

If you get a `$schema` error on any file, paste the error here and we'll iterate.

---

## Step 4 — Enable Estonian language

Yext → **Account Settings → Languages** → add **Estonian (et)** as alternate. English stays primary.

---

## Step 5 — Set the API key for the next two steps

```bash
export YEXT_API_KEY="paste_your_api_key_here"
```

(Don't paste the key into chat. Rotate it after first use.)

---

## Step 6 — Create the Alexander Maasik entity

Copy-paste the whole block into your terminal. The response will include `"meta": { "id": "1234567890..." }` — **save that numeric ID**, it becomes `YEXT_PUBLIC_LOCATION_ENTITY_ID` later.

```bash
curl -X POST \
  "https://api.yextapis.com/v2/accounts/me/entities?api_key=${YEXT_API_KEY}&v=20231201" \
  -H "Content-Type: application/json" \
  -d @- <<'JSON'
{
  "meta": { "entityType": "location" },
  "name": "Alexander Maasik",
  "address": {
    "line1": "Tallinn",
    "city": "Tallinn",
    "region": "Harju maakond",
    "postalCode": "10111",
    "countryCode": "EE"
  },
  "mainPhone": "+3725000000",
  "emails": ["alexander.maasik@nobeldigital.ee"],
  "linkedInUrl": "https://www.linkedin.com/in/amaasik",
  "facebookPageUrl": "https://www.facebook.com/alexander.maasik",
  "twitterHandle": "amaasik",
  "c_tagline": "Marketing strategy for startups that need measurable growth.",
  "c_heroSubheadline": "Eight years building content, SEO, and PPC programmes that tie back to revenue — not vanity metrics.",
  "c_aboutBody": "I've spent the last eight years leading marketing for B2B SaaS and product companies — most of that time in startups where every euro had to defend itself. I work across content marketing, SEO, PPC, and go-to-market strategy, and I train leaders and teams to keep marketing accountable to the business. I write, I measure, and I push back when a tactic can't be tied to a number. If you want activity, hire an agency. If you want results you can defend in a board meeting, let's talk.",
  "c_serviceTitles": [
    "Marketing strategy & GTM",
    "Content marketing & SEO",
    "Leadership & team training"
  ],
  "c_serviceBodies": [
    "A defensible plan for the next two to four quarters — positioning, channels, OKRs, and the dashboard that proves it's working.",
    "Editorial programmes built around buying intent. Topic strategy, brief writing, on-page execution, and the analytics to retire what doesn't earn its place.",
    "Workshops and 1:1 coaching for marketing managers and founders. How to set OKRs, run a weekly cadence, and report up without spin."
  ],
  "c_serviceEmailSubjects": [
    "Strategy & GTM enquiry",
    "Content & SEO enquiry",
    "Training & coaching enquiry"
  ],
  "c_approachTitles": [
    "Tie every activity to a number",
    "Ship weekly, decide monthly",
    "Write before you spend"
  ],
  "c_approachBodies": [
    "If we can't draw a line from a campaign to revenue, pipeline, or activation, we stop running it.",
    "Small experiments, honest readouts, and quarterly bets — not annual plans that calcify by March.",
    "Clear positioning is cheaper than paid acquisition. We fix the message first."
  ],
  "c_caseClients": [
    "B2B SaaS · Workflow tooling",
    "Series A · Compliance",
    "Estonian scale-up · D2C"
  ],
  "c_caseResults": [
    "Organic pipeline 3.4× in 14 months",
    "CAC down 38%, payback under 6 months",
    "First content hire ramped in 8 weeks"
  ],
  "c_caseBodies": [
    "Rebuilt the editorial calendar around buying-intent keywords. Retired 60% of legacy content; the surviving pages now drive most of inbound.",
    "Repositioned around a single category. Cut three paid channels, doubled down on one, and instrumented the funnel end-to-end.",
    "Hiring brief, interview rubric, and 90-day plan. Coached the new lead for the first quarter."
  ],
  "c_ctaHeadline": "Have a marketing problem worth solving?",
  "c_ctaBody": "One email is enough. Tell me what you're trying to move and where you're stuck. I read everything I get and respond within two working days."
}
JSON
```

---

## Step 7 — Populate the Estonian profile

```bash
export ENTITY_ID="paste_id_from_step_6"

curl -X PUT \
  "https://api.yextapis.com/v2/accounts/me/entityprofiles/${ENTITY_ID}/et?api_key=${YEXT_API_KEY}&v=20231201" \
  -H "Content-Type: application/json" \
  -d @- <<'JSON'
{
  "c_tagline": "Turundusstrateegia idufirmadele, kes vajavad mõõdetavat kasvu.",
  "c_heroSubheadline": "Kaheksa aastat sisuturunduse, SEO ja PPC programme, mis seovad iga eurot tuluga — mitte tühjade näitajatega.",
  "c_aboutBody": "Viimased kaheksa aastat olen juhtinud turundust B2B SaaS- ja tootefirmades — peamiselt idufirmades, kus iga euro pidi end ise õigustama. Töötan sisuturunduse, SEO, PPC ja turule sisenemise strateegia kallal ning koolitan juhte ja meeskondi, et hoida turundus äri ees vastutavana. Kirjutan, mõõdan ja vaidlen vastu, kui taktikat ei saa siduda numbriga. Kui soovid tegevust, palka agentuur. Kui soovid tulemusi, mida saab nõukogus kaitsta, räägime.",
  "c_serviceTitles": [
    "Turundusstrateegia & GTM",
    "Sisuturundus & SEO",
    "Juhtimine & meeskonna koolitus"
  ],
  "c_serviceBodies": [
    "Selgelt põhjendatud plaan järgmiseks kahele kuni neljale kvartalile — positsioneerimine, kanalid, OKR-id ja tulemustahvel, mis tõestab, et see töötab.",
    "Sisuprogrammid, mis on ehitatud ostukavatsuse ümber. Teemastrateegia, briifide kirjutamine, lehesisene teostus ja analüütika, mis loobub sellest, mis tulemust ei too.",
    "Töötoad ja personaalne juhendamine turundusjuhtidele ja asutajatele. Kuidas seada OKR-e, hoida nädalast rütmi ja raporteerida ülespoole ilma ilustusteta."
  ],
  "c_serviceEmailSubjects": [
    "Strateegia & GTM päring",
    "Sisu & SEO päring",
    "Koolitus & juhendamine päring"
  ],
  "c_approachTitles": [
    "Seo iga tegevus numbriga",
    "Tee nädalas, otsusta kuus",
    "Kirjuta enne kui kulutad"
  ],
  "c_approachBodies": [
    "Kui me ei suuda kampaaniat siduda tulu, müügitorustiku ega aktiveerimisega, lõpetame selle.",
    "Väikesed eksperimendid, ausad ülevaated ja kvartaalsed panused — mitte aastaplaanid, mis märtsiks kivinevad.",
    "Selge positsioneering on odavam kui tasuline klientide soetamine. Parandame esmalt sõnumi."
  ],
  "c_caseClients": [
    "B2B SaaS · Töövoo tarkvara",
    "A-seeria · Vastavus",
    "Eesti scale-up · D2C"
  ],
  "c_caseResults": [
    "Orgaaniline torustik 3,4× 14 kuuga",
    "CAC -38%, tasuvusaeg alla 6 kuu",
    "Esimene sisutöötaja täiel võimsusel 8 nädalaga"
  ],
  "c_caseBodies": [
    "Ehitasin sisukalendri ümber ostukavatsuse märksõnade järgi. Pensionile 60% vanast sisust; allesjäänud lehed toovad nüüd suurema osa sissetulevatest kontaktidest.",
    "Positsioneerisin ümber ühe kategooria järgi. Kärpisin kolm tasulist kanalit, panustasin ühele ning instrumenteerisin müügilehtri otsast lõpuni.",
    "Värbamise briif, intervjuumaatriks ja 90-päevane plaan. Juhendasin uut juhti esimese kvartali jooksul."
  ],
  "c_ctaHeadline": "Kas sul on turundusprobleem, mida tasub lahendada?",
  "c_ctaBody": "Üks kiri on piisav. Kirjelda, mida proovid liigutada ja kus oled kinni. Loen kõike, mis tuleb, ja vastan kahe tööpäeva jooksul."
}
JSON
```

---

## Step 8 — Upload the two photos

In Yext UI:

1. **Knowledge Graph → Entities** → open Alexander Maasik.
2. Scroll to `c_heroPortrait` → upload the standing portrait JPG.
3. Scroll to `c_aboutPhoto` → upload the seated square JPG.

Both fields are `STATIC` localization, so the same image serves both languages.

---

## Step 9 — Verify

1. **Knowledge Graph → Entities → Alexander Maasik** → English profile fully populated.
2. Switch profile dropdown to Estonian → all `c_*` text fields populated; built-in fields fall back to English.
3. **Knowledge Graph → Configuration → Custom Fields** → all 15 `c_*` fields visible.

Once verified, deploy the [`alexander-maasik-site`](https://github.com/flyinghighindustries/alexander-maasik-site) repo via **Yext → Pages → Sites → New Site → Connect Git Repo**. Set env vars on the site:

- `YEXT_PUBLIC_LOCATION_ENTITY_ID` = the numeric id from Step 6
- `YEXT_PUBLIC_LOCATION_LOCALE_CODE` = `en,et`

---

## Rotate the API key

The key shared in chat earlier is compromised — rotate it as soon as the entity is created:

1. Yext → **Account Settings → Developer → API Credentials**.
2. Regenerate the Management API key.
3. Update local env / CI secrets.

---

## Troubleshooting

| Symptom                                                          | Fix                                                                                                            |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `Invalid $schema field`                                          | Schema URI not recognized by this Yext account version. Paste the rejecting line here and we'll adjust.        |
| `Resource file is missing $schema`                               | A non-resource JSON file is in the repo. Move/delete it. (The runbook keeps entity JSON inline for this reason.) |
| Apply errors "field already exists"                              | Name collision — rename existing field in Yext UI or remove the duplicate from this repo.                      |
| `Account ID does not match credentials`                          | Use the `me` macro in URLs, not the literal account ID.                                                        |
| POST returns `"unknown field"`                                   | Schema not yet applied — finish Steps 2–3 first.                                                                |
| `entityprofiles/{id}/et` returns 404                             | Estonian not enabled (Step 4).                                                                                 |
