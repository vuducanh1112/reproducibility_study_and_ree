<!-- global-bottom.vue -->
<template>
  <footer 
    v-if="$nav.currentLayout !== 'cover'" 
    class="absolute bottom-0 left-0 right-0 p-3 flex justify-between items-center text-[10px] text-gray-400 opacity-60 uppercase tracking-widest"
  >
    <!-- Left: Title -->
    <div>{{ $slidev.configs.title }}</div>

    <!-- Center: Author(s) -->
    <div>{{ $slidev.configs.author }}</div>

    <!-- Right: Page Number -->
    <div>
      <SlideCurrentNo /> / <SlidesTotal />
    </div>
  </footer>
</template>
