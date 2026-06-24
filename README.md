# gravity sandbox

A 2D Newtonian n-body toy. Fling bodies, watch orbits form, collide, and slingshot.
Real physics, one file, no build step.

## Run it

Open `index.html` in a browser. That is the whole app. It also works when served
from any static host.

To serve it locally:

```
python3 -m http.server
```

Then open http://localhost:8000.

## Controls

- Drag on empty space to launch a body. The arrow shows direction and speed.
- Pick a mass with dust / planet / star and scale it with the size slider.
- Tool `brush` holds and drags to emit a stream of small bodies. `eraser` deletes bodies under the cursor.
- Hold Space and drag to pan. Scroll to zoom toward the cursor.
- Toggle velocity vectors, trails, grid, and merge sound.
- Load a preset: sun + 3 planets, binary star, figure-8, or a chaotic cluster.
- Save a system by name to browser storage, then load or delete it later.
- Copy link writes the current system into the URL so you can share it.
- Rewind bar scrubs back through the last ~600 frames. Release while scrubbed to resume from that point.
- Pause, then press the arrow keys to step one frame at a time.

## Physics

Pairwise gravity `F = G*m1*m2 / (r^2 + s^2)` with a softening term `s`, summed to
acceleration. Velocity Verlet with a fixed step, and extra substeps when the speed
slider runs above 1x. Bodies merge after a full step with conserved momentum and
summed mass. Velocity is clamped, and any body that goes non-finite is dropped so
the simulation never freezes.

## Host on Cloudflare Pages

1. Push this repo to GitHub.
2. In the Cloudflare dashboard, open Workers and Pages, then create a Pages project and connect the GitHub repo.
3. Leave the build command blank.
4. Set the build output directory to the repo root: `/`.
5. Deploy. Every push to the connected branch publishes a new version.

## External code

Two libraries load from a CDN: Tone.js for the merge sound and LZ-string for
share-link compression. Everything else is inline in `index.html`. If the CDN is
blocked, the simulation still runs; sound and share links are the only parts that
need them.
