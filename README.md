# rehabireb-web (draft layout — nothing created yet)

Rehabireb's own deploy repo. Holds only what is Rehabireb's; all app code
stays in `EmiEliasDev/booking-app`, so a fix there reaches every clinic on
its next deploy.

```
rehabireb-web/
├── .github/workflows/deploy.yml   calls booking-app's build-clinic.yml
├── clinic.config.json             name, logo, colors, services, slots, copy overrides, contact
├── public/
│   ├── logo.png                   Rehabireb logo (copy of booking-app main:public/logo.png)
│   ├── favicon.svg                Rehabireb favicon (main:public/favicon.svg)
│   └── icons.svg                  (main:public/icons.svg, if still used)
└── README.md
```

## How a deploy works
1. Trigger: push to this repo, manual "Run workflow", or a booking-app
   `app-release` dispatch.
2. `deploy.yml` calls `build-clinic.yml` from booking-app with
   `clinic_id = vars.CLINIC_ID` and the two Supabase secrets.
3. That workflow checks out booking-app, (after gap #1) overlays this repo's
   `public/` and `clinic.config.json`, runs `npm run build`, and publishes
   `dist/` to this repo's Pages site.

## What Emiliano sets up
1. Create `EmiEliasDev/rehabireb-web` (private or public; Pages on a private repo needs a paid plan).
2. Settings → Pages → Source = **GitHub Actions**.
3. No custom domain: the site is `https://emieliasdev.github.io/rehabireb-web/`
   (`base_path: /rehabireb-web/` in deploy.yml). No CNAME file.
4. Actions secrets: `VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY` (anon only).
5. Actions variable: `CLINIC_ID` = the `clinic_id_for_VITE_CLINIC_ID` the live SQL prints.
6. booking-app must allow its reusable workflow to be called: if booking-app is
   private, Settings → Actions → General → Access → "Accessible from repositories
   owned by EmiEliasDev".
