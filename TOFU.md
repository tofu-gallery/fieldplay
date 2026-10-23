# Field Play on Tofu

Field Play is Andrei Kashcha's interactive vector-field playground. It draws particles moving through a field defined in GLSL, with a live code editor, random fields, color controls, pan/zoom, and shareable URLs. The simulation and editor run entirely in the visitor's browser using WebGL.

## Run a copy

| Setting | Value |
| --- | --- |
| Tofu framework preset | `vite` |
| Repository / build root | Repository root (`.`) |
| Package manager | npm, using the committed `package-lock.json` |
| Node.js | Node.js 22 recommended; verified with 22.22.3 and npm 10.9.8 |
| Install command | `npm ci` |
| Build command | `npm run build` |
| Output directory | `dist/` |
| Entry page | `dist/index.html`, served at `/` |
| Environment variables | None |
| Database | None |
| Account or API key | None |

For a local production preview, run `npm run preview -- --host 127.0.0.1 --port 5193 --strictPort` after building. Tofu serves the built files; the Vite preview server is only a local verification tool.

The browser must support WebGL. Saved fields live in the URL, not a server database. Bookmark or copy the current URL to preserve a field. The share dialog uses the current copy's address.

## Adaptations and reasons

- Removed Google Analytics from `index.html` so copies do not report visits to the upstream analytics property.
- Removed the already-disabled SoundCloud controls and their unused loader/audio-source modules. The old loader carried an upstream client identifier and depended on a separately loaded SoundCloud SDK. This copy needs neither that account nor its API.
- Pinned `glsl-parser` to the exact commit already recorded in the upstream lockfile, `3991baef1ae335388807764f8bd9207c6323c460`, and changed the lockfile's resolved Git URL from SSH to public HTTPS. A clean build therefore does not require GitHub SSH access, and a moving dependency branch cannot change the parser.
- Added this operating guide. Upstream branding, project name, existing icons, attribution, license, and the core rendering/editor behavior are preserved.

## Optional external features

Normal startup and the core playground require no external runtime service. The initial browser check observed no cross-origin resource requests.

Some optional auto-mode presets use public GitHub Gist images. User-supplied GLSL imports, images, or videos load the URL the user supplies and depend on that host's availability and CORS settings. The upstream documentation, author links, social sharing links, and GitHub-hosted Open Graph image remain unchanged.

The advanced console-only `window.startRecord()` workflow described in `ScreenRecording.md` expects a separately operated local FFmpeg server. Tofu does not run that optional server. This is not required to view, edit, randomize, or share fields.

## Verification

Verified locally on 2026-10-23, without a Tofu deployment:

```sh
npm --prefix /private/tmp/gallery-batch2/fieldplay ci --no-audit --no-fund
# PASS: installed 62 packages from the lockfile.

/Users/MacbookPro/.local/bin/tofu-build-lock npm --prefix /private/tmp/gallery-batch2/fieldplay run build
# PASS: Vite 5.4.21, 117 modules transformed, dist/index.html and assets emitted.
# tofu-build-lock only serializes builds on the verification machine.

npm --prefix /private/tmp/gallery-batch2/fieldplay run preview -- --host 127.0.0.1 --port 5193 --strictPort
# PASS: production preview started on port 5193.

curl --fail --silent --show-error --output /private/tmp/gallery-batch2/fieldplay/.git/tofu-verified-index.html --write-out 'HTTP %{http_code}\n' http://127.0.0.1:5193/
# PASS: HTTP 200.
```

A real browser check through gstack `/browse` and Aside verified WebGL rendering, the editor and controls, pause/resume, randomization, valid GLSL editing, invalid-code feedback without replacing the saved valid field, restoration from the URL, and sharing the copy's own URL. The checks recorded `CONSOLE_ERRORS=[]` and `EXTERNAL_RESOURCES=[]`. Built HTML, JavaScript, and source maps were also checked for the removed analytics and SoundCloud integration.

npm reported the upstream `inflight` and `glob` deprecation warnings; Vite reported its existing CommonJS Node API deprecation warning. They did not prevent installation, building, or the browser checks. This is build and functional verification, not a complete dependency-security audit or physical-device certification.

## Source and license

- Upstream: https://github.com/anvaka/fieldplay
- Upstream default branch: `main`
- Upstream base commit: `11d602df5338e7bd80b10c01334f60594c396539`
- Tofu fork: https://github.com/tofu-gallery/fieldplay
- Adaptation branch: `tofu`
- Verified release tag: `pin/2026-10-23`
- License: MIT; see the unchanged root `LICENSE`, copyright (c) 2017-2026 Andrei Kashcha. Source files with additional third-party notices retain them.

`main` mirrors upstream. All Tofu changes belong on `tofu`; the gallery should consume the exact commit referenced by the verified release tag.
