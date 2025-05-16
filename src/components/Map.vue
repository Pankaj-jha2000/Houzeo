<template>
  <div class="real-estate-container">
    <div
  class="map-container"
  v-show="!isMobile || (isMobile && !isMapHidden)"
>
  <div id="map" ref="mapContainer"></div>
  <div class="map-controls">
    <button @click="zoomIn" class="map-btn"><i class="map-icon">+</i></button>
    <button @click="zoomOut" class="map-btn"><i class="map-icon">-</i></button>
  </div>
  <div class="keyboard-shortcuts">Keyboard shortcuts</div>
</div>
    <div
  class="listings-container"
  v-show="!isMobile || (isMobile && isMapHidden)"
>
  <div class="listings-header">
    <h2>{{ selectedCity }} real estate & homes for sale</h2>
    <div class="listings-meta">
      <span>{{ properties.length.toLocaleString() }} Homes</span>
      <div class="sort-by">
        Sort by:
        <select v-model="sortOption" class="sort-select">
          <option value="new">New Listing</option>
          <option value="price-high">Price (High to Low)</option>
          <option value="price-low">Price (Low to High)</option>
        </select>
      </div>
    </div>
  </div>
      <button
      v-if="isMobile"
      @click="toggleMap"
      class="map-toggle-button"
    >
      {{ isMapHidden ? 'Map' : 'List' }}
    </button>

      
      <div class="listings-grid">
        <div v-for="(property, index) in properties" :key="index" class="property-card">
          <div class="property-image-wrapper">
          <div class="property-image">
            <img :src="property.image" alt="Property image">
              <div class="image-footer">
               <div class="image-indicators">
    <span v-for="n in 5" :key="n" :class="{ small: n === 5 }"></span>
  </div>
      <img src="/triangle.png" alt="MLS Logo" class="mls-logo" />
</div>
            <div class="property-badge">{{ property.daysListed }} days on Houzeo</div>
            <button class="favorite-btn" @click="toggleFavorite(index)">
  <img :src="property.isFavorite ? 'red1.png' : 'heart.png'" alt="Favorite" class="heart-image" />
</button>

            </div>

          </div>
          <div class="property-details">
            <div class="property-type">
              <span class="property-tag">{{ property.type }} For Sale</span>
              <span class="views-count">{{ property.views }}</span>
            </div>

            <div class="property-details-row">
  <div class="property-price">${{ formatPrice(property.price) }}</div>
  <div class="property-specs">
    <span><span class="spec-number">{{ property.beds }}</span> Beds</span>
    <span><span class="spec-number">{{ property.baths }}</span> Baths</span>
    <span><span class="spec-number">{{ property.sqft }}</span> sqft</span>
    
  </div>

</div>

            <div class="property-address">
            <span class="bold">{{ property.street }}</span>
            <span class="normal">, {{ property.city }}, {{ property.state }} {{ property.zip }}</span>
            </div>
            <div class="property-broker">{{ property.broker }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import mapboxgl from 'mapbox-gl'
import 'mapbox-gl/dist/mapbox-gl.css'


export default {
  name: 'Map',
  data(){

    return {
      isMapHidden: window.innerWidth <= 768, // true on mobile
      isMobile: window.innerWidth <= 768,
      
      map: null,
      mobileMapOptions: {
      center: [-97.7431, 30.2672], // Example: Austin, TX
      zoom: 10
    },
    desktopMapOptions: {
      center: [-97.7431, 30.2672], // Keep same or different
      zoom: 12
    },


      selectedCity: 'Austin, TX',
      sortOption: 'new',
      properties: [
        {
          id: 1,
          type: 'House',
          price: 3349000,
          beds: 4,
          baths: 3,
          sqft: 998,
          street: '2856 Meadow Park Ave', 
          city: 'Henderson',
          state: 'NV',
          zip: 89052,
          broker: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
          image: '/1.png',
          daysListed: 6,
          views: '2.3K',
          isFavorite: false,
          coordinates: [-96.7970, 32.7767]
        },
        {
          id: 2,
          type: 'Condo',
          price: 3349000,
          beds: 4,
          baths: 3,
          sqft: 998,
          street: '2856 Meadow Park Ave', 
          city: 'Henderson',
          state: 'NV',
          zip: 89052,        
          broker: 'Sotheby\'s International Realty',
          image: '/2.png',
          daysListed: 12,
          views: '2.3K',
          isFavorite: false,
          coordinates: [-122.4194, 37.7749]
        },
        {
          id: 3,
          type: 'Multi-family home',
          price: 3349000,
          beds: 4,
          baths: 3,
          sqft: 998,
          street: '2856 Meadow Park Ave', 
          city: 'Henderson',
          state: 'NV',
          zip: 89052,          
          broker: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
          image: '/3.png',
          daysListed: 8,
          views: '2.3K',
          isFavorite: false,
          coordinates: [-87.6298, 41.8781]
        },
        {
          id: 4,
          type: 'House',
          price: 3349000,
          beds: 4,
          baths: 3,
          sqft: 998,
          street: '2856 Meadow Park Ave', 
          city: 'Henderson',
          state: 'NV',
          zip: 89052,   
          broker: 'Nashville (Real Tracs Mid) MLS-TN as distributed by MLS GRID',
          image: '/4.png',
          daysListed: 19,
          views: '2.3K',
          isFavorite: false,
          coordinates: [-118.2437, 34.0522]
        }
      ],
      markers: []
    };
  },
  mounted() {
    this.initializeMap();
    this.checkMobile();
    window.addEventListener('resize', this.checkMobile);

  },
  beforeDestroy() {
  window.removeEventListener('resize', this.checkMobile);
  },

  methods: {
    initializeMap() {
      const mapOptions = this.isMobile ? this.mobileMapOptions : this.desktopMapOptions;

      // Replace with your Mapbox access token
      mapboxgl.accessToken = 'pk.eyJ1IjoicGFua2FqMTU5OWpoYSIsImEiOiJjbWFvejlodXMwYWJwMmlzY3ZwcnozcTc1In0._6nMoMg4Jpa4g89xDoTeeQ';
      
      this.map = new mapboxgl.Map({
        container: this.$refs.mapContainer,
        style: 'mapbox://styles/mapbox/streets-v11',
        center: mapOptions.center,
        zoom: mapOptions.zoom
      });

      this.map.on('load', () => {
        this.addMarkers();
      });

    },
    addMarkers() {
      // Clear existing markers
      this.markers.forEach(marker => marker.remove());
      this.markers = [];

      // Add markers for each property
      this.properties.forEach(property => {
        // Create a marker element
        const el = document.createElement('div');
        el.className = 'map-marker';
        el.innerHTML = `
        <div class="marker-inner">
        <svg class="marker-icon" viewBox="0 0 24 24" fill="white" xmlns="http://www.w3.org/2000/svg">
        <path d="M3 11L12 2L21 11H17V20H7V11H3Z"/>
        </svg>
  </div>`;
        
        // Add click event to the marker
        el.addEventListener('click', () => {
          this.selectProperty(property.id);
        });
        
        // Create and add the marker to the map
        const marker = new mapboxgl.Marker(el)
          .setLngLat(property.coordinates)
          .addTo(this.map);
          
        this.markers.push(marker);
      });
      
      // Add additional sample markers across the US
      const sampleLocations = [
        [-74.0060, 40.7128], // New York
        [-80.1918, 25.7617], // Miami
        [-122.3321, 47.6062], // Seattle
        [-104.9903, 39.7392], // Denver
        [-93.2650, 44.9778], // Minneapolis
        [-84.3880, 33.7490], // Atlanta
        [-71.0589, 42.3601], // Boston
        [-95.3698, 29.7604], // Houston
        [-115.1398, 36.1699], // Las Vegas
        [-86.1581, 39.7684], // Indianapolis
        [-78.6382, 35.7796], // Raleigh
        [-112.0740, 33.4484], // Phoenix
      ];
      
      sampleLocations.forEach(coords => {
  const el = document.createElement('div');
  el.className = 'map-marker';
  el.innerHTML = `
    <div class="marker-inner">
      <svg class="marker-icon" viewBox="0 0 24 24" fill="white" xmlns="http://www.w3.org/2000/svg">
        <path d="M3 11L12 2L21 11H17V20H7V11H3Z"/>
      </svg>
    </div>
  `;

  const marker = new mapboxgl.Marker(el)
    .setLngLat(coords)
    .addTo(this.map);

  this.markers.push(marker);
});

    },
    selectProperty(id) {
      // Scroll to the property in the listing
      const propertyIndex = this.properties.findIndex(p => p.id === id);
      if (propertyIndex !== -1) {
        const propertyElements = document.querySelectorAll('.property-card');
        if (propertyElements[propertyIndex]) {
          propertyElements[propertyIndex].scrollIntoView({ behavior: 'smooth' });
        }
      }
    },
    zoomIn() {
      this.map.zoomIn();
    },
    zoomOut() {
      this.map.zoomOut();
    },
    toggleFavorite(index) {
      this.properties[index].isFavorite = !this.properties[index].isFavorite;
    },
    formatPrice(price) {
      return price.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ",");
    },
    toggleMap() {
      this.isMapHidden = !this.isMapHidden;
      this.$nextTick(() => {
      if (this.showMap && this.map) {
        this.map.resize();
      }
    });

    },
    checkMobile() {
      this.isMobile = window.innerWidth <= 768;
      this.isMapHidden = this.isMobile; // hide map if switching to mobile

    }
  }
};
</script>

<style>
.real-estate-container {
  display: flex;
  height: 100vh;
  font-family: Arial, sans-serif;
}

.map-container {
  position: relative;
  flex: 1;
  height: 100%;
  min-width: 50%;
}

#map {
  width: 100%;
  height: 100%;
}

.map-controls {
  position: absolute;
  top: 10px;
  right: 10px;
  display: flex;
  flex-direction: column;
  gap: 5px;
}

.map-btn {
  width: 30px;
  height: 30px;
  background: white;
  border: 1px solid #ccc;
  border-radius: 4px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.map-icon {
  font-style: normal;
  font-size: 18px;
}

.keyboard-shortcuts {
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  background: rgba(255, 255, 255, 0.8);
  padding: 5px 10px;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
}

.map-toggle-button {
  position: fixed;
  bottom: 20px;
  left: 50%;
  transform: translateX(-50%);
  background-color: #007BFF;
  color: white;
  padding: 10px 25px;
  border: none;
  border-radius: 25px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  z-index: 1000;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.real-estate-container.mobile {
  flex-direction: column;
}

@media (max-width: 768px) {

  .map-container {
    height: 600px;
  }

  #map{
    height: 300px;
  }
  

  .real-estate-container {
    flex-direction: column;
  }

  .map-container,
  .listings-container {
    width: 100%;
    height: calc(100vh - 60px); /* leave space for toggle button */
  }
}



.listings-container {
  position: relative;
  flex: 1;
  overflow-y: auto;
  padding: 20px;
  background-color: #f9f9f9;
}

.listings-header {
  margin-bottom: 20px;
}

.listings-header h2 {
  margin: 0 0 10px 0;
  font-size: 24px;
}

.listings-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  color: #666;
}

.sort-by {
  display: flex;
  align-items: center;
  gap: 5px;
}

.sort-select {
  border: none;
  background: transparent;
  color: #0066cc;
  font-weight: bold;
  cursor: pointer;
}


.heart-image {
  width: 20px;
  height: 20px;
}


.image-footer {
  position: absolute;
  bottom: 8px;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.image-indicators-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  gap: 8px;
}

.image-indicators {
  display: flex;
  gap: 6px;
  justify-content: center;
}

.image-indicators span {
  width: 8px;
  height: 8px;
  background-color: white;
  border-radius: 50%;
  opacity: 0.7;
}

.image-indicators .small {
  width: 5px;
  height: 5px;
}

.mls-logo {
  
  position: absolute;
  bottom: 10px;
  right: 20px;
  height: 25px;
  
}

.property-image {
  position: relative;
  width: 100%;
  overflow: hidden;
}



.listings-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
}

.property-card {
  background: white;
  border-radius: 8px;
  overflow: hidden;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}







.property-tag {
  background-color: #e6f0ff;
  color: black; /* light blue */
  padding: 4px 10px;
  border-radius: 12px;
  border-color: black;
  font-size: 14px;
  font-weight: 400;
}

.property-badge {
  position: absolute;
  top: 10px;
  left: 10px;
  background: white;
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 12px;
  font-weight: bold;
}

.favorite-btn {
  position: absolute;
  top: 10px;
  right: 10px;
  background: white;
  border: none;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.heart-icon {
  font-style: normal;
  color: #ccc;
}

.heart-icon.favorited {
  color: #ff385c;
}

.property-details {
  padding: 15px;
}

.property-type {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.property-tag {
  background-color: white;
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 4px 10px;
  border-radius: 15px;
  font-size: 14px;
  font-weight: 400;
    border: 1px solid grey; /* black border */

}

.property-tag::before {
  content: "";
  display: inline-block;
  width: 10px;
  height: 10px;
  background-color: #00cc99;
  border-radius: 50%;
}



.views-count {
  display: flex;
  align-items: center;
  gap: 5px;
  color: #666;
}

.views-count::before {
  content: "";
  display: inline-block;
  width: 16px;
  height: 16px;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23666' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z'%3E%3C/path%3E%3Ccircle cx='12' cy='12' r='3'%3E%3C/circle%3E%3C/svg%3E");
  background-size: contain;
}

.property-price {
  color: #0B5AA5;
  font-size: 18px;
  font-weight: bold;
}

.property-details-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.property-specs span {
  margin-left: 8px;
  
}

.spec-number {
  color: #0B5AA5;
  font-weight: 600;
}
.property-address {
  margin-top: 1em;
  font-size: 16px;
  margin-bottom: 1em;

}

.property-address .bold {
  font-weight: 600;
  color: #333; /* Darker color */
  font-size: 14px;

}

.property-address .normal {
  font-weight: 400;
  font-size: 14px;
  color: #555;
}


.property-broker {
  font-size: 14px;
  color: #666;
}

.map-marker {
  width: 30px;
  height: 30px;
  position: relative;
  cursor: pointer;
}

.marker-inner {
  width: 30px;
  height: 30px;
  background-color: #0066cc;
  border-radius: 50% 50% 50% 0;
  transform: rotate(-45deg);
  position: relative;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: center;
  justify-content: center;
}

.marker-icon {
  transform: rotate(45deg); /* Counter the parent rotation */
  width: 16px;
  height: 16px;
  z-index: 2;
}

</style>