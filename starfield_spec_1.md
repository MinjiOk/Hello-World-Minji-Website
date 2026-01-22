# Starfield Particle System - Specification Document

## Project Overview
An interactive web-based starfield particle system where white particle dots radiate outward from the center of the screen against a black background. Users can control various properties of the particle system through interactive sliders.

## Visual Design

### Layout
- **Background**: Pure black (#000000)
- **Particles**: White dots (#FFFFFF) - color adjustable via slider
- **Canvas**: Full-screen, responsive to window size
- **Controls Panel**: Fixed position in bottom-left corner with vertical sliders
- **Spaceship Overlay**: Full-screen overlay image of spaceship interior

### Spaceship Interior Overlay
- **Visual**: Image overlay depicting the interior view of a spaceship cockpit/window
- **Coverage**: Full-screen overlay layer
- **Transparency**: Semi-transparent or with transparent center/window area to view starfield
- **Style Options**:
  - **Option A**: Cockpit frame around edges with transparent center window
  - **Option B**: Partial opacity overlay of full cockpit interior
  - **Option C**: Windowed sections showing starfield through ship windows
- **Z-Index Layering**: 
  - Bottom layer: Black background
  - Middle layer: Canvas with particles and trails
  - Top layer: Spaceship interior overlay
  - Topmost layer: Controls panel (must remain visible and interactive)
- **Design Notes**: Overlay should enhance immersion without obscuring too much of the starfield effect
- **Asset**: Will require spaceship interior PNG/SVG with transparency

### Particle Behavior
- **Origin**: All particles spawn from the center of the screen
- **Movement**: Particles move outward in random directions (360° radially)
- **Trail Effect**: Particles leave trails behind them (length controllable via slider)
- **Lifecycle**: Particles respawn at center when they exit the screen boundaries

### Center Spawn Indicator
- **Visual**: Black filled circle at the exact center of the screen
- **Radius**: Directly proportional to the spawn radius slider value
  - When spawn radius = 0: center dot has minimal/zero radius (invisible or 1px)
  - When spawn radius = 100: center dot has 100px radius
- **Purpose**: Visual indicator showing the area from which particles can spawn
- **Rendering**: Drawn on top of particles to remain visible

## Interactive Controls

The website should feature four sliders that allow real-time adjustment of particle properties:

### 1. Trail Length Slider
- **Purpose**: Controls how long the particle trails appear
- **Range**: 0 (no trail, just dots) to 100 (long trails)
- **Effect**: Adjusts the opacity fade/trail rendering of particle paths
- **Default**: 50 (medium trail length)

### 2. Speed Slider
- **Purpose**: Controls how fast particles move outward from center
- **Range**: 0 (stationary) to 50 (very fast)
- **Effect**: Multiplies the velocity of all particles
- **Default**: 25 (medium speed)

### 3. Particle Count Slider
- **Purpose**: Controls the total number of active particles
- **Range**: 10 (sparse) to 1000 (dense)
- **Effect**: Adds or removes particles from the system dynamically
- **Default**: 200 particles

### 4. Spawn Radius Slider
- **Purpose**: Controls the radius from the center point where particles initially spawn
- **Range**: 0 (exact center point) to 100 pixels (circular area)
- **Effect**: Creates variation in spawn position - higher values make particles appear from a larger circular area around the center; also controls the size of the center indicator dot
- **Default**: 0 (all spawn from exact center)

### 5. Star Color Slider
- **Purpose**: Controls the color of the star particles
- **Range**: 0 to 360 (HSL hue value in degrees)
- **Effect**: Changes particle color across the full spectrum:
  - 0° = Red
  - 60° = Yellow
  - 120° = Green
  - 180° = Cyan
  - 240° = Blue
  - 300° = Magenta
  - 360° = Red (full circle)
- **Implementation**: Uses HSL color with fixed saturation (100%) and lightness (50%)
- **Default**: 0 (white stars) - Note: Can use special value for white, or 0 for red as starting color

## Technical Requirements

### Core Technologies
- **HTML5 Canvas** for rendering particles and trails
- **Vanilla JavaScript** for particle system logic and animation
- **CSS3** for styling controls and layout
- **RequestAnimationFrame** for smooth 60fps animation

### Particle System Logic

#### Particle Properties
Each particle should have:
- Position (x, y coordinates)
- Velocity (direction and speed)
- Trail history (array of previous positions)

#### Animation Loop
1. Clear canvas (with transparency for trail effect)
2. Update particle positions based on velocity
3. Draw particle trails
4. Draw particle dots
5. Check boundaries and respawn particles that exit screen
6. Repeat using requestAnimationFrame

#### Trail Implementation
Trails can be achieved by:
- **Option A**: Drawing with partial canvas clearing (leaving ghosting effect)
- **Option B**: Storing position history for each particle and drawing fading lines
- Trail length slider adjusts either opacity of ghosting or number of history points rendered

### Performance Considerations
- Optimize for 60fps with up to 1000 particles
- Use efficient canvas drawing methods
- Consider throttling slider updates to avoid performance issues
- Implement particle pooling to avoid constant object creation/destruction

## User Interface

### Controls Panel Design
```
┌──────────────┐
│  Starfield   │
│   Controls   │
├──────────────┤
│              │
│ Trail Length │
│      ║       │
│      ║       │
│      ●       │
│      ║       │
│      ║       │
│     50       │
│              │
│    Speed     │
│      ║       │
│      ●       │
│      ║       │
│     25       │
│              │
│   Particle   │
│    Count     │
│      ║       │
│      ●       │
│      ║       │
│     200      │
│              │
│    Spawn     │
│    Radius    │
│      ●       │
│      ║       │
│      0       │
│              │
│  Star Color  │
│      ║       │
│      ●       │
│      ║       │
│      0       │
│              │
└──────────────┘
```

**Position**: Fixed in bottom-left corner of screen
**Layout**: Vertical orientation with sliders stacked top to bottom
**Dimensions**: Approximately 150-200px wide, height adjusts to fit all controls

### Control Panel Features
- **Position**: Fixed bottom-left corner (e.g., 20px from bottom, 20px from left)
- **Orientation**: Vertical layout with sliders arranged top to bottom
- **Slider Style**: Vertical sliders with values displayed below each
- Semi-transparent dark background for readability
- Real-time value display below each slider
- Smooth, responsive slider controls
- Labels above each slider for clarity
- Optional minimize/maximize button for controls panel
- Styling should not distract from the starfield effect
- **Z-index**: Must be above spaceship overlay to remain interactive and visible

## Responsive Design
- Canvas should resize to match window dimensions
- Particles should respawn at new center when window resizes
- Controls should remain accessible on mobile devices
- Touch-friendly slider controls for mobile
- Consider vertical layout for controls on mobile devices

## Browser Compatibility
- Modern browsers with Canvas support (Chrome, Firefox, Safari, Edge)
- Fallback message for browsers without Canvas support
- Graceful degradation for older browsers

## Performance Targets
- Maintain 60fps with default settings (200 particles)
- Maintain 30fps+ with maximum particles (1000)
- Smooth slider interaction with no lag
- Efficient memory usage (no memory leaks from particle creation)

## Optional Enhancements (Future Considerations)
- Background color picker (to change from black)
- Particle size slider
- Multiple spawn points option
- Gravity/physics effects toggle
- Save/load preset configurations
- Export as video or animated GIF
- Pause/play button
- Reset to defaults button
- Fullscreen mode toggle
- Different spaceship interior overlays to choose from
- Particle glow/bloom effect toggle

## File Structure
```
starfield-website/
├── index.html          # Main HTML structure
├── css/
│   └── styles.css      # Styling for controls and layout
├── js/
│   ├── particle.js     # Particle class definition
│   ├── starfield.js    # Main particle system logic
│   └── controls.js     # Slider control handlers
├── assets/
│   └── spaceship-interior.png  # Spaceship cockpit overlay image
└── README.md           # Project documentation
```

## Implementation Notes

### Canvas Setup
- Use full window dimensions: `canvas.width = window.innerWidth`
- Handle window resize events to update canvas size
- Set up 2D rendering context

### Particle Direction Calculation
```javascript
// Random angle in radians (0 to 2π)
const angle = Math.random() * Math.PI * 2;

// Convert to velocity components
const velocity = {
    x: Math.cos(angle) * speed,
    y: Math.sin(angle) * speed
};
```

### Trail Effect Implementation
For optimal visual effect, consider using:
- `globalAlpha` to fade trails
- Previous frame persistence (partial canvas clearing)
- Motion blur effect through opacity layering

### Spawn Radius Implementation
```javascript
// Random position within spawn radius circle
const spawnAngle = Math.random() * Math.PI * 2;
const spawnDistance = Math.random() * spawnRadius;

const spawnX = centerX + Math.cos(spawnAngle) * spawnDistance;
const spawnY = centerY + Math.sin(spawnAngle) * spawnDistance;
```

### Center Indicator Dot Implementation
```javascript
// Draw black circle at center with radius matching spawn radius
ctx.fillStyle = '#000000';
ctx.beginPath();
ctx.arc(centerX, centerY, spawnRadius, 0, Math.PI * 2);
ctx.fill();
```

### Star Color Implementation
```javascript
// Convert slider value (0-360) to HSL color
const hue = colorSliderValue;
const particleColor = `hsl(${hue}, 100%, 50%)`;

// For white stars, use special case when slider is at 0
// or implement a separate toggle for white vs colored
```

## Success Criteria
- ✅ Particles radiate smoothly from center of screen
- ✅ All five sliders work in real-time without lag
- ✅ Trail length creates visible effect from 0 to 100
- ✅ Speed range works smoothly from 0 to 50
- ✅ Center spawn indicator dot scales correctly with spawn radius
- ✅ Star color changes across full spectrum (0-360°)
- ✅ Vertical sliders positioned in bottom-left corner
- ✅ Spaceship interior overlay visible and doesn't obscure controls
- ✅ Performance remains smooth at default settings
- ✅ Visual aesthetic matches specification (black background, customizable star colors)
- ✅ Responsive design works on desktop and mobile
- ✅ Code is clean, commented, and maintainable

## Testing Checklist
- [ ] Test all slider ranges (min, max, mid values)
- [ ] Test speed slider specifically at 0 (stationary particles)
- [ ] Verify center dot scales correctly with spawn radius (0-100px)
- [ ] Test color slider across full spectrum (all hues)
- [ ] Verify vertical slider layout in bottom-left corner
- [ ] Confirm spaceship overlay doesn't block controls
- [ ] Test window resize functionality
- [ ] Test on multiple browsers
- [ ] Test on mobile devices (touch controls for vertical sliders)
- [ ] Performance test with 1000 particles
- [ ] Visual verification of trail lengths
- [ ] Spawn radius verification at different values
- [ ] Speed verification from 0 (stopped) to 50 (fastest)

---

**Version**: 1.0  
**Last Updated**: January 2026  
**Status**: Specification Complete - Ready for Development
