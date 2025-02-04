# ol-context-menu

> A contextmenu extension for OpenLayers.

Right click on the map to open the contextmenu.

<script setup>
import ContextMenuDemo from "@demos/ContextMenuDemo.vue"
</script>
<ClientOnly>
<ContextMenuDemo />
</ClientOnly>

## Usage

Add context menu to map

```vue
<template>
  <ol-map
    :loadTilesWhileAnimating="true"
    :loadTilesWhileInteracting="true"
    style="height:400px"
    ref="map"
  >
    <ol-view
      :center="center"
      :rotation="rotation"
      :zoom="zoom"
      :projection="projection"
      ref="view"
    />

    <ol-tile-layer>
      <ol-source-osm />
    </ol-tile-layer>

    <ol-context-menu :items="contextMenuItems" />

    <ol-vector-layer>
      <ol-source-vector ref="markers"> </ol-source-vector>
      <ol-style>
        <ol-style-icon :src="marker" :scale="0.1"></ol-style-icon>
      </ol-style>
    </ol-vector-layer>
  </ol-map>
</template>

<script>
import { ref, inject } from "vue";

import marker from "@/assets/marker.png";
export default {
  setup() {
    const center = ref([40, 40]);
    const projection = ref("EPSG:4326");
    const zoom = ref(8);
    const rotation = ref(0);

    const contextMenuItems = ref([]);

    const markers = ref(null);
    const view = ref(null);

    const Feature = inject("ol-feature");
    const Geom = inject("ol-geom");

    contextMenuItems.value = [
      {
        text: "Center map here",
        classname: "some-style-class", // add some CSS rules
        callback: (val) => {
          view.value.setCenter(val.coordinate);
        }, // `center` is your callback function
      },
      {
        text: "Add a Marker",
        classname: "some-style-class", // you can add this icon with a CSS class
        // instead of `icon` property (see next line)
        icon: marker, // this can be relative or absolute
        callback: (val) => {
          console.log(val);
          let feature = new Feature({
            geometry: new Geom.Point(val.coordinate),
          });
          markers.value.source.addFeature(feature);
        },
      },
      "-", // this is a separator
    ];

    return {
      center,
      projection,
      zoom,
      rotation,
      contextMenuItems,
      marker,
      markers,
      view,
    };
  },
};
</script>
```

## Properties

### eventType

- **Type**: `String`
- **Default**: `contextmenu`

The listening event type (You could use 'click', 'dblclick')

### defaultItems

- **Type**: `Boolean`
- **Default**: `true`

Whether the default items (which are: Zoom In/Out) are enabled

### width

- **Type**: `Number`
- **Default**: `150`

The menu's width

### items

- **Type**: `Array`
- **Default**: `[]`

An array of object|string
