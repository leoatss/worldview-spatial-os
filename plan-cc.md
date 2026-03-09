# WorldView Spy Satellite Simulator — Build Plan

## Inspiration

Based on the article ["I Built a Spy Satellite Simulator in a Browser"](https://www.spatialintelligence.ai/p/i-built-a-spy-satellite-simulator) from Spatial Intelligence. The author built a browser-based geospatial intelligence simulator called **WorldView** that turns any browser into a real-time surveillance dashboard — running 8 AI coding agents in parallel over a weekend.

---

## Core Tech Stack

| Layer | Technology | Notes |
|-------|-----------|-------|
| 3D Globe | **CesiumJS** (open-source, Apache 2.0) | Planet-scale rendering, GIS features |
| 3D City Models | **Google Photorealistic 3D Tiles** | Same tech as Google Earth; free API key needed |
| Shader Rendering | **WebGPU** | Custom fragment shaders for visual filters |
| Orbital Prediction | **satellite.js** | Computes satellite positions from TLE data |
| Frontend | Browser-based (likely vanilla JS or React) | No native app needed |

---

## Real-Time Data Feeds

| Feed | Source | What It Provides |
|------|--------|-----------------|
| Live Aircraft | [OpenSky Network](https://opensky-network.org/) | 7,000+ live aircraft positions (free, rate-limited) |
| Military Flights | [ADS-B Exchange](https://www.adsbexchange.com/) | Crowdsourced military/gov flight tracking |
| Satellite Orbits | [CelesTrak TLE](https://celestrak.org/) | 180+ satellites in actual orbital paths |
| Vehicle Flow | [OpenStreetMap](https://www.openstreetmap.org/) | Road network + simulated traffic visualization |
| CCTV Cameras | Public camera aggregators | Geolocated feeds projected onto 3D building models |

---

## Rendering / Shader Modes

- **Night Vision (NVG)** — green phosphor effect
- **FLIR Thermal Imaging** — heat signature color mapping
- **CRT Scan Lines** — retro monitor aesthetic
- **Anime Cel-Shading** — Studio Ghibli style
- **Military Targeting Reticles** — HUD overlay with crosshairs, distance, bearing

---

## Key Features

1. Real-time orbital tracking (clickable satellites with metadata)
2. Live air traffic visualization (callsigns, altitude, heading)
3. Geographically-anchored CCTV feed integration
4. Globe-view navigation down to city-level street detail
5. "God mode" detection overlays highlighting vehicles and aircraft
6. Multiple visual filter modes (NVG, FLIR, CRT, etc.)

---

## Reference Projects on GitHub

| Repo | Description | Relevance |
|------|-------------|-----------|
| [shawn14/worldview](https://github.com/shawn14/worldview) | CesiumJS + Google 3D Tiles, live aircraft/satellite tracking, military-style filters | Closest match — good fork candidate |
| [kevtoe/worldview](https://github.com/kevtoe/worldview) | Real-time tactical intel platform — CesiumJS globe with flights, satellites, earthquakes, traffic, CCTV | Full-featured reference |
| [dsuarezv/satellite-tracker](https://github.com/dsuarezv/satellite-tracker) | Three.js + React + satellite.js — 3D satellite tracker with CelesTrak data | Satellite subsystem reference |
| [nasa-gibs/worldview](https://github.com/nasa-gibs/worldview) | NASA official — browse 1,000+ full-res satellite imagery layers | Imagery layer reference |
| [CesiumGS/cesium](https://github.com/CesiumGS/cesium) | The open-source 3D globe engine powering most of these | Core dependency |
| [NASA JPL 3d-tiles-renderer](https://community.cesium.com/t/3d-tiles-renderer-a-3d-tiles-implementation-for-three-js-from-nasa-jpl/35187) | Three.js 3D Tiles implementation from NASA JPL | Alternative to CesiumJS |
| [dgreenheck/webgpu-claude-skill](https://github.com/dgreenheck/webgpu-claude-skill) | Claude skill for WebGPU + Three.js development | Useful for shader work |

---

## Build Approach

### Phase 1: Globe + Navigation
- Set up CesiumJS with Google Photorealistic 3D Tiles
- Implement globe-to-street-level navigation
- Basic camera controls (orbit, zoom, fly-to)

### Phase 2: Satellite Tracking
- Fetch TLE data from CelesTrak
- Use satellite.js to compute real-time positions
- Render satellite icons on the globe with click-to-inspect

### Phase 3: Live Flight Tracking
- Integrate OpenSky Network API
- Display aircraft positions with callsigns, altitude, heading
- Auto-refresh on interval

### Phase 4: Shader Effects
- Implement WebGPU (or WebGL fallback) post-processing pipeline
- Build NVG, FLIR, CRT shader modes
- Add toggle UI for switching modes

### Phase 5: CCTV Integration
- Source public camera feeds with known lat/lng
- Project video feeds onto 3D building surfaces
- Clickable camera markers on the globe

### Phase 6: HUD & Overlays
- Military-style targeting reticles
- Distance/bearing indicators
- Detection overlay ("God mode") for vehicles and aircraft

---

## API Keys & Accounts Needed

- Google Cloud (Maps Platform) — for Photorealistic 3D Tiles
- OpenSky Network — free account for higher rate limits
- ADS-B Exchange — API access (free tier available)
- CelesTrak — no key needed (public TLE data)

---

## Coverage & Articles

- [Original Article — Spatial Intelligence](https://www.spatialintelligence.ai/p/i-built-a-spy-satellite-simulator)
- [Bit Rebels — Ex-Google Maps PM built it in 3 days](https://bitrebels.com/technology/ex-google-maps-pm-coded-a-real-time-worldview-spy-inspired-dashboard-in-3-days/)
- [UBOS — A New Frontier in Geospatial Intelligence](https://ubos.tech/news/spy-satellite-simulator-a-new-frontier-in-geospatial-intelligence/)
- [Privacy Guides Discussion](https://discuss.privacyguides.net/t/worldview-a-browser-based-spy-satellite-simulator-built-with-ai/35794)
