<script lang="ts">
  import { onMount } from 'svelte';
  import mapboxgl from 'mapbox-gl';
  import 'mapbox-gl/dist/mapbox-gl.css';
  import { Polyline } from '$lib/Polyline';
  import stationDataJson from './resources/station_data.json' assert { type: 'json' };

  let mapContainer: HTMLDivElement;
  let currentHour = 0; // Track current hour
  let map: mapboxgl.Map; // Make map accessible in component scope

  // Function to filter station data by hour
  function filterStationsByHour(hour: number): GeoJSON.FeatureCollection {
    return {
      type: "FeatureCollection" as const,
      features: stationDataJson.features.map(f => ({
        type: "Feature" as const,
        geometry: {
          type: "Point" as const,
          coordinates: f.geometry.coordinates
        },
        properties: f.properties
      })).filter(feature => feature.properties?.hour === hour)
    };
  }

  // Function to update map data
  function updateMapData(hour: number) {
    if (!map) return;
    const source = map.getSource('stations') as mapboxgl.GeoJSONSource;
    if (source) {
      source.setData(filterStationsByHour(hour));
    }
  }

  // Watch for hour changes
  $: if (map && currentHour !== undefined) {
    updateMapData(currentHour);
  }

  // Modify the stationData declaration
  const stationData = stationDataJson as GeoJSON.FeatureCollection;

  // Add this after stationData
  const subwayLines: GeoJSON.FeatureCollection = {
    "type": "FeatureCollection",
    "features": [
      {
        "type": "Feature",
        "properties": { "line": "1", "color": "#FED103" },
        "geometry": {
          "type": "LineString",
          "coordinates": [
            [-79.5282, 43.7964], // VAUGHAN METROPOLITAN CENTRE
            [-79.5216, 43.7699], // HIGHWAY 407
            [-79.5076, 43.7612], // PIONEER VILLAGE
            [-79.5013, 43.7743], // YORK UNIVERSITY
            [-79.4909, 43.7655], // FINCH WEST
            [-79.4769, 43.7502], // DOWNSVIEW PARK
            [-79.4623, 43.7499], // SHEPPARD WEST
            [-79.4503, 43.7345], // WILSON
            [-79.4476, 43.7245], // YORKDALE
            [-79.4444, 43.7156], // LAWRENCE WEST
            [-79.4359, 43.7096], // GLENCAIRN
            [-79.4359, 43.6997], // EGLINTON WEST
            [-79.4158, 43.6840], // ST. CLAIR WEST
            [-79.4065, 43.6747], // DUPONT
            [-79.4030, 43.6677], // SPADINA
            [-79.3987, 43.6683], // ST. GEORGE
            [-79.3936, 43.6670], // MUSEUM
            [-79.3901, 43.6598], // QUEEN'S PARK
            [-79.3883, 43.6545], // ST. PATRICK
            [-79.3866, 43.6505], // OSGOODE
            [-79.3856, 43.6473], // ST. ANDREW
            [-79.3806, 43.6453], // UNION
            [-79.3777, 43.6486], // KING
            [-79.3793, 43.6526], // QUEEN
            [-79.3807, 43.6565], // DUNDAS
            [-79.3828, 43.6605], // COLLEGE
            [-79.3839, 43.6654], // WELLESLEY
            [-79.3854, 43.6700], // BLOOR-YONGE
            [-79.3889, 43.6765], // ROSEDALE
            [-79.3914, 43.6820], // SUMMERHILL
            [-79.3938, 43.6872], // ST. CLAIR
            [-79.3966, 43.6981], // DAVISVILLE
            [-79.3986, 43.7052], // EGLINTON
            [-79.4026, 43.7259], // LAWRENCE
            [-79.4060, 43.7445], // YORK MILLS
            [-79.4103, 43.7615], // SHEPPARD-YONGE
            [-79.4124, 43.7681], // NORTH YORK CENTRE
            [-79.4146, 43.7805]  // FINCH
          ]
        }
      },
      {
        "type": "Feature",
        "properties": { "line": "2", "color": "#00923F" },
        "geometry": {
          "type": "LineString",
          "coordinates": [
            [-79.5365, 43.6369], // KIPLING
            [-79.5238, 43.6453], // ISLINGTON
            [-79.5110, 43.6484], // ROYAL YORK
            [-79.4942, 43.6516], // OLD MILL
            [-79.4836, 43.6498], // JANE
            [-79.4752, 43.6516], // RUNNYMEDE
            [-79.4663, 43.6536], // HIGH PARK
            [-79.4594, 43.6550], // KEELE
            [-79.4526, 43.6569], // DUNDAS WEST
            [-79.4425, 43.6591], // LANSDOWNE
            [-79.4357, 43.6609], // DUFFERIN
            [-79.4258, 43.6628], // OSSINGTON
            [-79.4183, 43.6645], // CHRISTIE
            [-79.4113, 43.6663], // BATHURST
            [-79.4030, 43.6677], // SPADINA
            [-79.3987, 43.6683], // ST. GEORGE
            [-79.3925, 43.6706], // BAY
            [-79.3854, 43.6700], // BLOOR-YONGE
            [-79.3766, 43.6726], // SHERBOURNE
            [-79.3680, 43.6746], // CASTLE FRANK
            [-79.3578, 43.6768], // BROADVIEW
            [-79.3527, 43.6784], // CHESTER
            [-79.3456, 43.6798], // PAPE
            [-79.3379, 43.6810], // DONLANDS
            [-79.3301, 43.6824], // GREENWOOD
            [-79.3230, 43.6840], // COXWELL
            [-79.3139, 43.6861], // WOODBINE
            [-79.3018, 43.6884], // MAIN STREET
            [-79.2878, 43.6955], // VICTORIA PARK
            [-79.2756, 43.7111], // WARDEN
            [-79.2655, 43.7323]  // KENNEDY
          ]
        }
      },
      {
        "type": "Feature",
        "properties": { "line": "4", "color": "#8C288C" },
        "geometry": {
          "type": "LineString",
          "coordinates": [
            [-79.4103, 43.7615], // SHEPPARD-YONGE
            [-79.3857, 43.7669], // BAYVIEW
            [-79.3764, 43.7690], // BESSARION
            [-79.3642, 43.7711], // LESLIE
            [-79.3454, 43.7756]  // DON MILLS
          ]
        }
      }
    ]
  };

  onMount(() => {
    map = new mapboxgl.Map({
      container: mapContainer,
      style: 'mapbox://styles/mapbox/dark-v11',
      center: [-79.3128, 43.6864],
      zoom: 13,
      pitch: 0,
      antialias: true,
      accessToken: 'pk.eyJ1IjoicGVhY2hlc2dvYmJsciIsImEiOiJjbTdyMzQxMXQxNGNmMmpwdXJrYWd0c3M4In0.rkr_jHMuuHPlbCgGAk_q8w'
    });

    map.on('load', () => {
      // Create polylines for subway lines
      subwayLines.features.forEach(feature => {
        if (!feature.properties?.line || !feature.properties?.color) return;
        
        const polyline = new Polyline({
          id: `line-${feature.properties.line}`,
          points: (feature.geometry as GeoJSON.LineString).coordinates as [number, number][],
          layout: {
            'line-join': 'round',
            'line-cap': 'round'
          },
          paint: {
            'line-color': feature.properties.color,
            'line-width': 4,
            'line-blur': 0.5
          }
        });
        polyline.addTo(map);
      });

      // Add stations source with filtered data
      map.addSource('stations', {
        type: 'geojson',
        data: filterStationsByHour(currentHour)
      });

      // Add a layer for the stations
      map.addLayer({
        'id': 'station-delays',
        'type': 'circle',
        'source': 'stations',
        'paint': {
          // Circle radius based on likelihood of delay
          'circle-radius': [
            'interpolate',
            ['linear'],
            ['get', 'likelihood_of_delay'],
            0, 5,    // Minimum size for 0% likelihood
            1, 50    // Maximum size for 100% likelihood
          ],
          // Circle color based on average delay
          'circle-color': [
            'interpolate',
            ['linear'],
            ['get', 'avg_delay'],
            0, '#00ff00',
            9, '#ff0000'
          ],
          'circle-opacity': 0.7,
          'circle-stroke-width': 1,
          'circle-stroke-color': '#ffffff'
        }
      });

      // Add popup on hover
      const popup = new mapboxgl.Popup({
        closeButton: false,
        closeOnClick: false
      });

      map.on('mouseenter', 'station-delays', (e: mapboxgl.MapMouseEvent & { features?: mapboxgl.MapboxGeoJSONFeature[] }) => {
        if (!e.features?.length) return;
        
        map.getCanvas().style.cursor = 'pointer';
        
        const feature = e.features[0];
        const point = feature.geometry as GeoJSON.Point;
        const coordinates: [number, number] = [point.coordinates[0], point.coordinates[1]];
        
        const properties = feature.properties;
        if (!properties) return;
        
        const description = `
          <strong>${properties.station}</strong><br>
          Hour: ${properties.hour}:00<br>
          Average Delay: ${properties.avg_delay} minutes<br>
          Likelihood of Delay: ${(properties.likelihood_of_delay * 100).toFixed(1)}%
        `;
        
        popup
          .setLngLat(coordinates)
          .setHTML(description)
          .addTo(map);
      });

      map.on('mouseleave', 'station-delays', () => {
        map.getCanvas().style.cursor = '';
        popup.remove();
      });
    });

    return () => map.remove();
  });
</script>

<div class="container">
  <h1 class="text-2xl mb-4">Toronto Transit Delays by Station</h1>
  
  <!-- Add time slider -->
  <div class="time-control mb-4">
    <label for="time-slider" class="block text-sm mb-2">
      Time: {currentHour.toString().padStart(2, '0')}:00
    </label>
    <input
      id="time-slider"
      type="range"
      min="0"
      max="23"
      bind:value={currentHour}
      class="w-full"
    />
  </div>

  <div class="legend mb-4">
    <p class="text-sm mb-2">Circle size represents likelihood of delay</p>
    <p class="text-sm">Circle color represents average delay (green = low, red = high)</p>
  </div>
  <div bind:this={mapContainer} class="map-container"></div>
</div>

<style>
  .container {
    padding: 1rem;
  }

  .map-container {
    width: 100%;
    height: 600px;
    border-radius: 4px;
    border: 1px solid #ccc;
  }

  .legend {
    background: rgba(255, 255, 255, 0.9);
    padding: 0.5rem;
    border-radius: 4px;
  }

  :global(.mapboxgl-map) {
    width: 100%;
    height: 100%;
  }

  :global(.mapboxgl-popup) {
    max-width: 300px;
  }

  :global(.mapboxgl-popup-content) {
    padding: 1rem;
    font-size: 14px;
  }

  .time-control {
    background: rgba(255, 255, 255, 0.9);
    padding: 1rem;
    border-radius: 4px;
  }

  input[type="range"] {
    width: 100%;
    height: 8px;
    border-radius: 4px;
    appearance: none;
    background: #ddd;
    outline: none;
    opacity: 0.7;
    transition: opacity 0.2s;
  }

  input[type="range"]:hover {
    opacity: 1;
  }

  input[type="range"]::-webkit-slider-thumb {
    appearance: none;
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: #4a5568;
    cursor: pointer;
  }

  input[type="range"]::-moz-range-thumb {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    background: #4a5568;
    cursor: pointer;
  }
</style>