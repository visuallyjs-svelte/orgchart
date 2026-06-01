### Summary
This demo shows an organizational chart builder. It demonstrates how to create hierarchical layouts and interactive employee profiles.

### Components Used
- `SurfaceProvider`: Context provider.
- `SurfaceComponent`: The main org chart canvas.
- `ControlsComponent`: UI for zooming and navigation.
- `MiniviewComponent`: A thumbnail view for large charts.
- `InspectorComponent`: (In `components/InspectorComponent.svelte`) For viewing and editing employee details.

### Component Options
#### `SurfaceComponent`
- `renderOptions`: Custom rendering for person nodes.
- `viewOptions`: Configures the hierarchical view and interaction (e.g., selection callback).
- `url`: Path to the employee dataset.
- `className`: CSS class for the canvas.

#### `ControlsComponent`
- `undoRedo`: false.
- `clear`: false.

### Stylesheet Requirement
The `visuallyjs.css` stylesheet is required.

```css
@import "@visuallyjs/browser-ui/css/visuallyjs.css";
```
