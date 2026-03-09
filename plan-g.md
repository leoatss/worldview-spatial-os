# Spy Satellite Simulator: Project Roadmap (plan-g)

## 1. Project Vision
To create an immersive, real-time "Spy Satellite Simulator" that allows users to view any point on Earth through the lens of active satellites, using photorealistic 3D data and tactical visual overlays.

## 2. Strategic Tech Stack
*   **Visualization:** Google Photorealistic 3D Tiles (via Map Tiles API).
*   **3D Engine:** CesiumJS (optimized for geospatial and orbital rendering).
*   **Orbital Engine:** `satellite.js` + CelesTrak TLE data.
*   **Visual Effects:** GLSL Post-processing shaders.
*   **Interface:** React + Tailwind CSS for a "Command & Control" HUD.

## 3. Core Architecture
### A. The Orbital Layer
Tracking 20,000+ objects in real-time. This involves fetching TLE (Two-Line Element) data and propagating orbits to get current Lat/Long/Alt.

### B. The Rendering Layer
Streaming gigabytes of 3D geometry from Google's servers. The camera must be synced to the satellite's "nadir" point (looking straight down).

### C. The Tactical Layer
Applying real-time filters (Night Vision, Thermal) and a "Heads-Up Display" (HUD) containing telemetry data.

## 4. Execution Milestones
1.  **Phase 1: Foundation** - Cesium + Google 3D Tiles integration.
2.  **Phase 2: Orbital Sync** - Real-time tracking of the ISS or Starlink.
3.  **Phase 3: The Lens** - Implementing GLSL shaders for tactical views.
4.  **Phase 4: C2 Interface** - Building the interactive sidebar and telemetry HUD.
