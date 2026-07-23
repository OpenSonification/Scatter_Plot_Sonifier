# Scatter Plot Sonifier

An interactive scatter plot that turns data into sound. The horizontal axis
controls when a sound is played and the vertical axis controls its pitch, so
the shape, trend, and spread of the point cloud can be explored by listening as
well as looking.

**[Open the Scatter Plot Sonifier](https://opensonification.github.io/Scatter_Plot_Sonifier/)**

## What it does

The app generates points around an adjustable linear or quadratic model. It
offers two ways to hear the plot:

- **Points** plays the individual observations from left to right as a cloud of
  short tones.
- **Smooth** follows the fitted smooth curve with one continuous tone.

You can change the model while viewing the result, generate a new random
sample, or drag individual points to reshape the plot. Clicking empty space
previews the pitch at that location.

Everything is contained in [`index.html`](./index.html): the interface, styles,
plot rendering, statistical model, and Web Audio synthesis. There are no
external dependencies, trackers, or data uploads.

## Model controls

The generated mean curve is

```text
y = q(x - 0.5)² + mx + b + ε
```

where `m` is the line slope, `b` is the intercept, and `q` is the optional
quadratic coefficient. Noise is Gaussian and can be constant or
heteroskedastic:

```text
log Var(ε | x) = log(σ₀²) + h₁(x - 0.5) + h₂(x - 0.5)²
```

- **Baseline noise (`σ₀`)** sets the spread at the centre of the plot.
- **Linear log-variance (`h₁`)** tilts the spread from one side to the other.
- **Quadratic log-variance (`h₂`)** moves more spread towards the edges or the
  centre.
- **Smooth bandwidth** controls the boundary-corrected local-quadratic
  Gaussian smoother drawn through the observations.

If generated values extend outside the normalised `0–1` range, the complete
raw range is rescaled into the selected frequency range instead of being
clipped.

## Sound controls

You can adjust:

- minimum and maximum frequency;
- playback speed and point-tone length;
- point count, from 8 to 10,000;
- playback volume;
- sine, triangle, square, or sawtooth waveform; and
- logarithmic (musical) or linear (numeric) pitch spacing.

Browsers require a direct interaction before audio can start, so select
**Points**, **Smooth**, or use a playback shortcut to enable sound. For the
best playback experience, use a desktop browser and keep the tab active.

## Interaction and keyboard shortcuts

- Click empty plot space to preview its pitch.
- Click or drag a point to hear and reposition it.
- Press <kbd>Space</kbd> to play points, pause, or resume.
- Press <kbd>S</kbd> to play the smooth curve.
- Press <kbd>Esc</kbd> to stop playback.
- Press <kbd>R</kbd> to generate a new plot.

The interface follows the operating system's light or dark colour scheme,
supports pointer and keyboard input, exposes control labels and live status
updates to assistive technology, and respects reduced-motion preferences.

## Run locally

No installation or build is needed. Open `index.html` directly in a modern
desktop browser, or serve the repository with a small local web server:

```bash
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

## Deployment

GitHub Pages is deployed by
[`deploy-pages.yml`](./.github/workflows/deploy-pages.yml). Every push to
`main` packages `index.html` as a Pages artifact and publishes it. The workflow
can also be started manually from the repository's **Actions** tab.
