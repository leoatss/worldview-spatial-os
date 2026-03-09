# Spy Satellite Simulator: Technical Specification (plan-g-technical)

## 1. Infrastructure Requirements
*   **Google Maps API Key:** Must have "Map Tiles API" enabled.
*   **Cesium Ion Token:** (Optional but recommended for terrain/assets).
*   **Data Source:** `https://celestrak.org/NORAD/elements/gp.php?GROUP=active&FORMAT=tle`

## 2. Core Implementation Logic

### A. Initializing Cesium with Google 3D Tiles
```javascript
const viewer = new Cesium.Viewer('cesiumContainer', {
  terrainProvider: await Cesium.createWorldTerrainAsync(),
  animation: false,
  timeline: false,
});

try {
  const tileset = await Cesium.createGooglePhotorealistic3DTileset();
  viewer.scene.primitives.add(tileset);
} catch (error) {
  console.error("Failed to load Google 3D Tiles:", error);
}
```

### B. Satellite Propagation (satellite.js)
```javascript
import * as satellite from 'satellite.js';

function getSatellitePosition(tle1, tle2) {
  const satrec = satellite.twoline2satrec(tle1, tle2);
  const positionAndVelocity = satellite.propagate(satrec, new Date());
  const gmst = satellite.gstime(new Date());
  const positionGd = satellite.eciToGeodetic(positionAndVelocity.position, gmst);
  
  return {
    longitude: satellite.degreesLong(positionGd.longitude),
    latitude: satellite.degreesLat(positionGd.latitude),
    height: positionGd.height * 1000 // Convert to meters
  };
}
```

### C. Post-Processing Shaders (GLSL)
To create the "Spy" look, add a `PostProcessStage` to the Cesium scene:

**Night Vision Fragment Shader:**
```glsl
uniform sampler2D colorTexture;
varying vec2 v_textureCoordinates;
void main() {
    vec4 color = texture2D(colorTexture, v_textureCoordinates);
    float gray = dot(color.rgb, vec3(0.299, 0.587, 0.114));
    vec3 green = vec3(0.0, 1.0, 0.0) * gray * 1.5; // Amplify green
    gl_FragColor = vec4(green, 1.0);
}
```

## 3. Dependency List
```json
{
  "dependencies": {
    "cesium": "^1.115.0",
    "satellite.js": "^5.0.0",
    "react": "^18.2.0",
    "lucide-react": "^0.300.0"
  }
}
```

## 4. Key Implementation Challenges
*   **Camera Jitter:** Smoothing the transition between high-speed orbital frames.
*   **API Quotas:** Managing Tile API requests during high-speed "fly-bys."
*   **Coordinate Systems:** Mapping ECI (Inertial) to ECEF (Fixed) for precise targeting.
