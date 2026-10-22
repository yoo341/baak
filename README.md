# Baak

Fisher safety tracker with **Safe AI** risk checks and a **GMet sea dashboard**.
A static site: `index.html` plus `data/gmet.json`. No build step.

## Put it online with GitHub Pages

1. Create a repository and upload everything in this folder (keep the `.github` folder).
2. Go to **Settings → Pages**, choose **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. After a minute the site is live at `https://<your-username>.github.io/<repo>/`.

## Install on a phone (Chrome)

Baak is an installable web app. Open the GitHub Pages address in Chrome and tap **Install app** at the top of the page, or the **Install** bar at the bottom. If Chrome has not offered it yet, open the Chrome menu and tap **Install app** or **Add to Home screen**. It then opens full screen from the home screen and shows the last saved sea data when offline. The site must be served over https, which GitHub Pages does.

After you change files, raise the number in `CACHE = 'baak-v1'` in `sw.js` so phones pick up the update.

## Live GMet data

GitHub Pages cannot run code on its own, so a scheduled GitHub Action reads GMet's bulletin and saves it to `data/gmet.json`. The page loads that file when it opens.

GMet publishes a coastal and maritime forecast PDF about 10:00 and 16:00 each day. Files are named like `COASTLINE_AND_MARITIME_IBF_FORECAST_E06072025.pdf` (A = morning, E = evening, then day, month, year). The folder number in the link changes every time, so the script needs a page that links to the latest PDFs.

Set it up once:

1. In the repository go to **Settings → Secrets and variables → Actions → Variables**.
2. Add a variable named `GMET_LISTING_URL` with the address of the meteo.gov.gh page that lists the marine bulletins.
3. Open **Actions → Update GMet bulletin → Run workflow** to test it. You can also paste one bulletin PDF address into the `pdf_url` box.

If the action fails, the page keeps showing the example values and you can still type the bulletin figures into the form on the page.

Check the parser on your machine:

```
python3 scripts/update_gmet.py --self-test
```

The parser was tested against the text of one bulletin (`tests/sample_bulletin.txt`). Run it on a few real PDFs first, because GMet may change the layout. It needs `pdftotext` (poppler-utils).

## What is still a prototype

- Trips and names on the page are invented examples held in the browser. Nothing is saved or shared between users.
- Real tracking needs a backend: SMS and USSD through a telecom provider, a database, and phone calls for the escalation steps. Keypad SMS and USSD cannot run on GitHub Pages.
- Safe AI is a rules model. Test it against real trip and incident records before relying on it.
- Voice prompts in local languages must be recorded by native speakers and tested with fishing communities.
- The short code `*000#` is a placeholder.
