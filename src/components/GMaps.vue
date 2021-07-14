<template>
  <div>
    <div class="GMap"/>
    <v-btn @click="deleteSelectedShape">Clear Shape</v-btn>
    <v-btn @click="getPolygon">Get Path</v-btn>
  </div>

</template>

<script>
import gmapsInit from '../utils/gmaps';

export default {
  name: 'GMap',
  data() {
    return {
      google: null,
      drawingManager: null,
      selectedShape: null
    }
  },
  methods: {
    getPolygon() {
      console.log(this.selectedShape)
    },
    clearSelection() {
      if (this.selectedShape) {
        this.selectedShape.setEditable(false)
        this.selectedShape = null
      }
    },
    setSelection(shape) {
      this.clearSelection();
      this.selectedShape = shape;
      shape.setEditable(true);
    },
    deleteSelectedShape() {
      if (this.selectedShape) {
        this.selectedShape.setMap(null);
        // To show:
        this.drawingManager.setOptions({
          drawingControl: true
        });
      }
    },
    overlayDone(e) {
      var polygonBounds = e.overlay.getPath();
      var testArray = [];
      for (var a = 0; a < polygonBounds.length; a++) {
        testArray.push(polygonBounds.getAt(a).lat(), polygonBounds.getAt(a).lng());
      }
      console.log(testArray)

      // Switch back to non-drawing mode after drawing a shape.
      this.drawingManager.setDrawingMode(null);
      // To hide:
      this.drawingManager.setOptions({
        drawingControl: false
      });

      // Add an event listener that selects the newly-drawn shape when the user
      // mouses down on it.
      var newShape = e.overlay;
      newShape.type = e.type;
      this.google.maps.event.addListener(newShape, 'click', function () {
        this.setSelection(newShape);
      });
      this.setSelection(newShape);
    }
  },
  async mounted() {
    try {
      this.google = await gmapsInit();
      var google = this.google


      this.map = new this.google.maps.Map(this.$el.children[0], {
        zoom: 10,
        center: new google.maps.LatLng(22.344, 114.048),
        mapTypeId: google.maps.MapTypeId.ROADMAP,
        disableDefaultUI: true,
        zoomControl: true
      });

      var polyOptions = {
        strokeWeight: 0,
        fillOpacity: 0.45,
        editable: true
      };
      // Creates a drawing manager attached to the map that allows the user to draw polygons only
      this.drawingManager = new google.maps.drawing.DrawingManager({
        drawingMode: google.maps.drawing.OverlayType.POLYGON,
        drawingControlOptions: {
          drawingModes: [
            google.maps.drawing.OverlayType.POLYGON
          ]
        },
        markerOptions: {
          draggable: true
        },
        polylineOptions: {
          editable: true
        },
        rectangleOptions: polyOptions,
        circleOptions: polyOptions,
        polygonOptions: polyOptions,
        map: this.map
      });

      var polygonOptions = this.drawingManager.get('polygonOptions');
      polygonOptions.fillColor = 'red';
      this.drawingManager.set('polygonOptions', polygonOptions);

      google.maps.event.addListener(this.drawingManager, 'overlaycomplete', this.overlayDone)

      // google.maps.event.addListener(this.drawingManager, 'overlaycomplete', function(e) {
      //   if (e.type !== google.maps.drawing.OverlayType.MARKER) {
      //     // Switch back to non-drawing mode after drawing a shape.
      //     drawingMan.setDrawingMode(null);
      //     // To hide:
      //     drawingMan.setOptions({
      //       drawingControl: false
      //     });
      //
      //     // Add an event listener that selects the newly-drawn shape when the user
      //     // mouses down on it.
      //     var newShape = e.overlay;
      //     newShape.type = e.type;
      //     google.maps.event.addListener(newShape, 'click', function() {
      //       this.setSelection(newShape);
      //     });
      //     this.setSelection(newShape);
      //   }
      // });
      //
      // // Clear the current selection when the drawing mode is changed, or when the
      // // map is clicked.
      // google.maps.event.addListener(this.drawingManager, 'drawingmode_changed', this.clearSelection);
      // google.maps.event.addListener(this.map, 'click', this.clearSelection);
      // google.maps.event.addDomListener(document.getElementById('delete-button'), 'click', deleteSelectedShape);

    } catch (error) {
      console.error(error);
    }
  },
  watch: {

  }
};
</script>

<style>
html,
body {
  margin: 0;
  padding: 0;
}

div > .GMap {
  width: 100vw;
  height: 70vh;
}
</style>
