<template>
  <footer v-if="$slidev.nav.currentPage !== $slidev.nav.total" class="common-footer">
    <span>
      ContainerDays & AI Context Singapore 2026 · 
      Securing AI Coding Agents with Kubernetes Sandboxes ·
      Ryan Djurovich ·
      <a href="https://ryan0x44.com">ryan0x44.com</a>
    </span>
    <span class="footer-meta">
      {{ $slidev.nav.currentPage }} of {{ $slidev.nav.total }}
    </span>
  </footer>
</template>
