<template>
  <header class="header">
    <div class="header-container">
      <!-- Mobile: Hamburger -->
      <div class="mobile-menu-icon" @click="toggleMenu">
        <img src="/menu.png" alt="Menu" />
      </div>

      <!-- Logo (desktop: left, mobile: center) -->
      <div class="logo">
        <img src="/logo.png" alt="Houzeo Logo" />
      </div>

      <!-- Desktop: Nav + Contact + CTA -->
      <div class="right-content">
        <nav class="nav-links">
          <span>Product ▼</span>
          <span>How Houzeo Works</span>
          <span>Reviews</span>
          <span>Buy ▼</span>
          <span>Sell ▼</span>
          <span>Pricing</span>
          <span>Resources ▼</span>
          <span>Sign In</span>
        </nav>
        <div class="contact-cta">
          <div class="contact-info">
            <div class="phone-number">(844) 448-0110</div>
            <div class="hours">(9am to 6pm CST, Mon–Sat)</div>
          </div>
          <button class="cta-button">Start Free Listing</button>
        </div>
      </div>

      <!-- Mobile: Profile -->
      <div class="mobile-profile-icon">
        <img src="/profile.svg" alt="Profile" />
      </div>
    </div>

    <!-- Mobile Dropdown -->
    <transition name="slide-fade">
      <div v-if="isMenuOpen" class="mobile-dropdown-menu">
        <span>Product</span>
        <span>How Houzeo Works</span>
        <span>Reviews</span>
        <span>Buy</span>
        <span>Sell</span>
        <span>Pricing</span>
        <span>Resources</span>
        <span>Sign In</span>
      </div>
    </transition>
  </header>
</template>

<script>
export default {
  data() {
    return {
      isMenuOpen: false,
    };
  },
  methods: {
    toggleMenu() {
      this.isMenuOpen = !this.isMenuOpen;
    },
  },
};
</script>

<style scoped>
.header {
  background-color: white;
  border-bottom: 1px solid #e5e7eb;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  width: 100%;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.header-container {
  max-width: 1400px;
  margin: 0 auto;
  padding: 12px 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  position: relative;
}

/* Logo aligned to left by default */
.logo {
  display: flex;
  align-items: center;
}

.logo img {
  height: 28px;
}

/* Desktop: Navigation and CTA */
.right-content {
  display: flex;
  align-items: center;
  gap: 24px;
}

.nav-links {
  display: flex;
  gap: 20px;
  font-size: 13px;
  color: #333;
}

.nav-links span {
  cursor: pointer;
  transition: color 0.3s ease;
}

.nav-links span:hover {
  color: #2563eb;
}

.contact-cta {
  display: flex;
  align-items: center;
  gap: 16px;
}

.contact-info {
  text-align: right;
  font-size: 13px;
  color: #333;
  line-height: 1.2;
}

.contact-info .hours {
  font-size: 10px;
  color: #888;
}

.cta-button {
  background-color: #2563eb;
  color: white;
  border: none;
  padding: 8px 16px;
  font-size: 13px;
  font-weight: 500;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s ease;
}

.cta-button:hover {
  background-color: #1d4ed8;
}

/* Mobile only: hide right content, show icons */
.mobile-menu-icon,
.mobile-profile-icon {
  display: none;
  align-items: center;
  cursor: pointer;
}

.mobile-menu-icon img,
.mobile-profile-icon img {
  width: 24px;
  height: 24px;
}

/* Mobile Dropdown */
.mobile-dropdown-menu {
  display: flex;
  flex-direction: column;
  background-color: #ffffff;
  padding: 12px 24px;
  border-top: 1px solid #e5e7eb;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
}

.mobile-dropdown-menu span {
  padding: 10px 0;
  border-bottom: 1px solid #f0f0f0;
  font-size: 14px;
  color: #333;
  cursor: pointer;
}

.mobile-dropdown-menu span:last-child {
  border-bottom: none;
}

/* Transition animation */
.slide-fade-enter-active,
.slide-fade-leave-active {
  transition: all 0.3s ease;
}
.slide-fade-enter-from,
.slide-fade-leave-to {
  transform: translateY(-10px);
  opacity: 0;
}

/* Responsive */
@media (max-width: 1024px) {
  .right-content {
    display: none;
  }

  .mobile-menu-icon,
  .mobile-profile-icon {
    display: flex;
  }

  .header-container {
    justify-content: space-between;
  }

  .logo {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
  }
}
</style>
