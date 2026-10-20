# Cognigy Vehicle Area Selector

Custom message plugin for Cognigy Webchat v3. It overlays six selectable areas on a top-view vehicle image:

- front_left / Frontal izquierdo
- front_right / Frontal derecho
- middle_left / Medio izquierdo
- middle_right / Medio derecho
- rear_left / Detrás izquierdo
- rear_right / Detrás derecho

## Build

```bash
npm ci
npm run build
```

The generated file is:

```text
dist/vehicle-area-selector.webchat-plugin.js
```

## Trigger from a Cognigy Say Node

Put this object in **Say Node > Options > Data**. Replace `imageUrl` with a direct HTTPS URL to an image file that your browser can load and whose license permits your use.

```json
{
  "_plugin": {
    "type": "vehicle-area-selector",
    "imageUrl": "https://YOUR-CDN.example.com/car-top-view.png",
    "title": "Selecciona la zona del vehículo",
    "instruction": "Pulsa sobre la zona afectada para continuar.",
    "imageAlt": "Vista superior de un vehículo",
    "showLabels": true,
    "disabledAfterSelection": true
  }
}
```

## Returned input

If the user selects the front-left area, the plugin sends the visible text `Frontal izquierdo` and this structured data:

```json
{
  "action": "vehicle_area_selected",
  "component": "vehicle-area-selector",
  "selection": {
    "id": "front_left",
    "label": "Frontal izquierdo",
    "row": 1,
    "column": 1
  },
  "dictionary": {
    "front_left": true
  }
}
```

In the next Flow turn, inspect `input.data` in the Interaction Panel. The intended values are available under:

```text
input.data.selection.id
input.data.selection.label
input.data.dictionary
```

A robust routing condition is:

```text
{{input.data && input.data.action === "vehicle_area_selected" && input.data.selection.id === "front_left"}}
```

You can copy the complete dictionary to context with a Set Context node:

```text
Context key: vehicleAreaSelection
Value: {{input.data}}
```

## Custom area dictionary

The default areas can be overridden from the Say Node by passing exactly six entries:

```json
{
  "_plugin": {
    "type": "vehicle-area-selector",
    "imageUrl": "https://YOUR-CDN.example.com/car-top-view.png",
    "areas": [
      { "id": "front_left", "label": "Frontal izquierdo", "row": 1, "column": 1 },
      { "id": "front_right", "label": "Frontal derecho", "row": 1, "column": 2 },
      { "id": "middle_left", "label": "Medio izquierdo", "row": 2, "column": 1 },
      { "id": "middle_right", "label": "Medio derecho", "row": 2, "column": 2 },
      { "id": "rear_left", "label": "Detrás izquierdo", "row": 3, "column": 1 },
      { "id": "rear_right", "label": "Detrás derecho", "row": 3, "column": 2 }
    ]
  }
}
```

## Deployment

1. Upload `dist/vehicle-area-selector.webchat-plugin.js` to an HTTPS static host.
2. Ensure the response has a JavaScript content type and is reachable from the users' browsers.
3. Add the bundle URL to the Webchat v3 Endpoint's **Webchat Plugins** setting for Demo Webchat testing, or load it in the hosted Webchat embedding configuration.
4. Add the Say Node data payload shown above.
5. Test a click and inspect `input.text` and `input.data` in the Interaction Panel.

## Image licensing note

The Hum2D page supplied for this implementation states that its image may be used only for personal, non-commercial projects. This package therefore does not redistribute that image. Obtain a license suitable for the intended client use, or upload another approved image to your own CDN and use its direct HTTPS file URL.
