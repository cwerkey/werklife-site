# werklife-site

Public marketing and privacy site for [werkLife](https://werklife.werkey.tech).

Plain HTML and CSS. No build step, no JavaScript, no external fonts, libraries or trackers. Hosted on GitHub Pages at the `werklife.werkey.tech` subdomain. The `.nojekyll` file tells Pages to serve the files as they are.

## Pages

- `index.html`: landing page (hero, Today, the four taps, Sermons, Read, the church year, privacy callout, testing call)
- `about.html`: why werkLife exists
- `privacy.html`: the privacy policy for the werkLife pre-release. GitHub Pages serves it at both `/privacy` and `/privacy.html`.

## Files

- `styles.css`: every style, with color tokens taken from the app's `Design/Palette.swift` and type from `Design/Typography.swift`
- `assets/`: the favicon, apple-touch icon, nav and About marks (light and dark), and a 512px icon for link previews, all resized from the app icon
- `assets/screens/`: simulator screenshots resized to 600px wide. They show sample data only.
- `CNAME`: the custom domain. Don't delete it.
- `.nojekyll`: turns off the Jekyll build

## Local preview

Open `index.html` in a browser, or run any static server:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deployment

### 1. GitHub Pages from `main`, `/ (root)`

In the repo on github.com:

1. **Settings → Pages**
2. **Source:** Deploy from a branch
3. **Branch:** `main`, folder `/ (root)`

With `.nojekyll` in place, Pages copies the files as they are, with no theme and no Jekyll build.

### 2. The CNAME file

`CNAME` contains `werklife.werkey.tech`, the custom domain the site is served at. Set the same domain under **Settings → Pages → Custom domain** if GitHub doesn't pick it up from the file.

### 3. Squarespace DNS

In Squarespace Domains → `werkey.tech` → DNS Settings → **Custom Records**, add one record:

| Host       | Type  | Data                 |
|------------|-------|----------------------|
| `werklife` | CNAME | `cwerkey.github.io.` |

That's the only record. The trailing dot is optional; Squarespace normalizes it.

- `cwerkey.github.io` is the account's `github.io` host, not a repo. Pages routes the subdomain to this repo through the `CNAME` file.
- Don't add an A record for `werklife`. The CNAME alone is enough.
- Propagation usually takes a few minutes, occasionally up to an hour.

### 4. Enforce HTTPS

Back in **Settings → Pages**, once DNS resolves:

1. Wait for the green "DNS check successful" next to the custom domain.
2. Once the certificate has been issued (it can take about 15 minutes after DNS verifies), check **Enforce HTTPS**.

### 5. Verify

- `https://werklife.werkey.tech/` serves the landing page.
- `https://werklife.werkey.tech/privacy` and `/privacy.html` serve the policy. Either is fine for App Store Connect → App Information → **Privacy Policy URL**.

## Updating content

Edit the relevant `.html` file, commit, push. Pages redeploys in about a minute.

## Notes

- Color tokens in `styles.css` mirror the app's charcoal-parchment palette, light and dark (`prefers-color-scheme`). If the app's palette changes, update both.
- The app's rules carry over: parchment only behind Scripture, the rust accent only for the main action, and current or selected states edged in the accent, never filled with ink.
- Type is Iowan Old Style (Palatino and Georgia as fallbacks), with Charter only for Scripture and small capitals for references. All of them ship on Apple devices, so nothing is downloaded.
- Scripture on the site is quoted only from the Berean Standard Bible (BSB), which is in the public domain, and the footer credits it. Don't quote the NIV, NLT or ESV here.
- The privacy policy's "Last updated" date is in `privacy.html`. Update it whenever the policy changes materially; past versions stay in this repo's history.
- To refresh a screenshot: `sips -Z 1304 shot.png --out /tmp/shot.png && sips -s format jpeg -s formatOptions 85 /tmp/shot.png --out assets/screens/name.jpg` (600 × 1304).
- To redo the icons from the app's 1024px icon: `sips -z 180 180 icon-light.png --out assets/apple-touch-icon.png` and `sips -z 32 32 icon-light.png --out assets/favicon-32.png`.
