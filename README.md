# Star Pile and Fingertip Stirring Interaction

This is a static interactive web page built with HTML5 Canvas and MediaPipe Hands. The page uses the camera to detect hand landmarks: when the thumb and index finger pinch together, a star is created. Each star falls into the water, creates a splash, sinks, collides with other stars, and gradually forms a pile. Moving your fingertip near the stars stirs them, creating the feeling of gently moving stars around in water.

The project does not require a build tool. The main implementation lives in `index.html`, so it can be opened as a static page or deployed directly with GitHub Pages.

## Live Deployment

This repository already includes a GitHub Pages deployment workflow:

- Workflow file: `.github/workflows/static.yml`
- Trigger: every push to the `main` branch, or a manual run from GitHub Actions
- Deployment content: the full static repository

If GitHub Pages is enabled in the repository settings, the page is usually available at:

```text
https://yanzhenhao01.github.io/-/
```

## Features

- Hand tracking: detects one hand with MediaPipe Hands.
- Pinch-to-create interaction: creates a star when the thumb tip and index fingertip are close enough.
- Animated water surface: draws a soft moving wave with Canvas.
- Lightweight physics: stars fall, rotate, splash, slow down in water, collide, and pile up.
- Fingertip stirring: nearby stars are pushed by fingertip movement.
- Color picker: the settings button in the top-left corner switches the star color.
- Mouse fallback: when camera interaction is unavailable, clicking creates stars and mouse movement stirs nearby stars.

## How to Use

1. Open the page and allow camera access.
2. Wait for the AI hand-tracking environment to load.
3. Place your hand in front of the camera.
4. Pinch your thumb and index finger together to create a star.
5. Move your index finger to stir stars in the water.
6. Use the settings button in the top-left corner to change the star color.

Mouse controls are also supported:

- Click the canvas to create a star.
- Move the mouse to push nearby stars.

## Local Development

Camera access usually requires a secure context, so it is better to serve the page through a local HTTP server instead of opening the HTML file directly.

Using Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## Project Structure

```text
.
├── .github/
│   └── workflows/
│       └── static.yml      # GitHub Pages deployment workflow
├── index.html              # Page markup, styles, interaction logic, and Canvas animation
└── README.md               # Project documentation
```

## Technical Overview

The page is mainly composed of:

- `MediaPipe Hands`: loaded from a CDN to detect hand landmarks.
- `Camera Utils`: provided by MediaPipe to connect the camera and feed frames into the hand-tracking model.
- `Canvas 2D`: used to draw stars, the water surface, splash particles, and the animation loop.
- A simplified particle and physics system: velocity, gravity, damping, collision checks, and state changes simulate the stars moving through water.

Important runtime state includes:

- `stars`: stores each star's position, velocity, size, color, rotation, and motion state.
- `splashes`: stores splash particles created when a star hits the water.
- `lastFingerPos`: tracks the current fingertip position.
- `fingerVel`: tracks fingertip velocity, which is used to push stars.
- `currentStarConfig`: stores the currently selected star color.

Each star follows this general lifecycle:

1. A star is created by a pinch gesture or a mouse click.
2. The star starts in the `falling` state with randomized velocity and rotation.
3. When it reaches the water surface, it switches to the `sinking` state and creates splash particles.
4. While in the water, it is affected by damping, subtle drift, fingertip pushes, and collisions with other stars.
5. After reaching the bottom, it enters the `settled` state and contributes to the growing pile.

## Browser Requirements

- Latest Chrome, Edge, or Safari is recommended.
- Camera interaction requires browser camera permission.
- Online access should use HTTPS; local development on `localhost` usually works for camera permissions.
- The page loads MediaPipe scripts from the jsDelivr CDN, so an internet connection is required.

## Possible Improvements

- Add mobile touch interactions.
- Add controls for clearing the canvas or pausing the animation.
- Split CSS and JavaScript into separate files for easier maintenance.
- Add more star shapes, color themes, and sound effects.
- Optimize collision performance when many stars are on screen.
