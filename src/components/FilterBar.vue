<template>
  <div class="search-filter-wrapper">
    <!-- Search Bar -->
    <div class="search-bar-container">
      <div class="search-bar">
        <input
          type="text"
          v-model="searchText"
          placeholder="Austin, TX"
          class="search-input"
        />
        <button v-if="searchText" @click="clearSearch" class="clear-button">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <line x1="18" y1="6" x2="6" y2="18"></line>
            <line x1="6" y1="6" x2="18" y2="18"></line>
          </svg>
        </button>
        <button class="search-button">
          <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
            <circle cx="11" cy="11" r="8"></circle>
            <line x1="21" y1="21" x2="16.65" y2="16.65"></line>
          </svg>
        </button>
      </div>

      <!-- Mobile Filters Button -->
      <button class="mobile-filters-button">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <line x1="4" y1="21" x2="4" y2="14" />
          <line x1="4" y1="10" x2="4" y2="3" />
          <line x1="12" y1="21" x2="12" y2="12" />
          <line x1="12" y1="8" x2="12" y2="3" />
          <line x1="20" y1="21" x2="20" y2="16" />
          <line x1="20" y1="12" x2="20" y2="3" />
          <line x1="1" y1="14" x2="7" y2="14" />
          <line x1="9" y1="8" x2="15" y2="8" />
          <line x1="17" y1="16" x2="23" y2="16" />
        </svg>
        <span class="badge">2</span>
      </button>
    </div>

    <!-- Filter Buttons -->
    <div class="filter-buttons">
      <button class="filter-button for-sale-button">For Sale <span class="arrow">▼</span></button>
      <button class="filter-button">Price <span class="arrow">▼</span></button>
      <button class="filter-button desktop-only">Beds & Baths <span class="arrow">▼</span></button>
      <button class="filter-button desktop-only">Property Type <span class="arrow">▼</span></button>
      <button class="filter-button icon-button filters-button desktop-only">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <line x1="4" y1="21" x2="4" y2="14" />
          <line x1="4" y1="10" x2="4" y2="3" />
          <line x1="12" y1="21" x2="12" y2="12" />
          <line x1="12" y1="8" x2="12" y2="3" />
          <line x1="20" y1="21" x2="20" y2="16" />
          <line x1="20" y1="12" x2="20" y2="3" />
          <line x1="1" y1="14" x2="7" y2="14" />
          <line x1="9" y1="8" x2="15" y2="8" />
          <line x1="17" y1="16" x2="23" y2="16" />
        </svg>
        Filters
      </button>
      <button class="filter-button icon-button saved-button desktop-only">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 20h9"></path>
          <path d="M16.5 3.5a2.121 2.121 0 0 1 3 3L7 19l-4 1 1-4L16.5 3.5z"></path>
        </svg>
        Saved
      </button>
      <button
  v-if="isMobile"
  class="filter-button save-search-button">
  Save Search
</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const searchText = ref('Austin, TX');

const clearSearch = () => {
  searchText.value = '';
};

import {  onMounted, onBeforeUnmount } from 'vue';

const isMobile = ref(window.innerWidth <= 768);

const handleResize = () => {
  isMobile.value = window.innerWidth <= 768;
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onBeforeUnmount(() => {
  window.removeEventListener('resize', handleResize);
});

</script>

<style>
.search-filter-wrapper {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap:10px;
  padding: 16px 150px 16px 40px;
  background-color: white;
}

.search-bar-container {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-left: 15px; /* Reduced space between search and filters */
  flex-grow: 2;
}

.search-bar {
  display: flex;
  align-items: center;
  border: 2px solid #0070c9;
  border-radius: 9999px;
  padding: 4px 12px;
  max-width: 440px;
  width: 100%;
  background-color: white;
}

.search-input {
  border: none;
  outline: none;
  flex-grow: 1;
  padding: 6px 8px;
  font-size: 14px;
}

.clear-button {
  background: none;
  border: none;
  cursor: pointer;
  color: #999;
  margin-right: 6px;
  display: flex;
  align-items: center;
}

.clear-button:hover {
  color: #666;
}

.search-button {
  background-color: #0070c9;
  border: none;
  padding: 6px;
  border-radius: 9999px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}

.search-button:hover {
  background-color: #005fa3;
}

.filter-buttons {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}

.filter-button {
  display: flex;
  align-items: center;
  padding: 8px 16px;
  border: 1px solid #ccc;
  border-radius: 8px;
  background-color: white;
  font-size: 14px;
  color: #333;
  cursor: pointer;
  min-width: 110px;
}

.filter-button:hover {
  border-color: #888;
}

.for-sale-button {
  font-weight: 500;
  border: 2px solid #0070c9;
  color: #0070c9;
}

.filters-button {
  padding: 8px 16px;
}

.arrow {
  margin-left: 10px;
  font-size: 10px;
}

.icon-button {
  gap: 6px;
}

.saved-button {
  border-radius: 9999px;
  padding-left: 16px;
  padding-right: 16px;
}

.save-search-button {
  background-color: #0070c9;
  color: white;
  border-radius: 9999px;
  font-weight: 500;
}

.mobile-filters-button {
  display: none;
  position: relative;
  background-color: #0070c9;
  border: none;
  border-radius: 9999px;
  padding: 10px;
  color: white;
}

.mobile-filters-button .badge {
  position: absolute;
  top: -4px;
  right: -4px;
  background-color: red;
  color: white;
  font-size: 10px;
  padding: 2px 6px;
  border-radius: 999px;
}

/* Responsive Rules */
@media (max-width: 768px) {
  .desktop-only {
    display: none !important;
  }

  .mobile-only {
    display: inline-flex !important;
  }

  .search-filter-wrapper {
    flex-direction: column;
    gap: 10px;
    padding: 16px;
  }

  .search-bar-container {
    width: 100%;
  }

  .filter-buttons {
    width: 100%;
    justify-content: space-between;
  }

  .mobile-filters-button {
    display: inline-flex;
  }

  .filter-button {
    flex-grow: 1;
    justify-content: center;
  }

  .for-sale-button, .filter-button.wide {
    min-width: unset;
  }

  .save-search-button {
    flex-grow: 1;
    justify-content: center;
  }
}
</style>
