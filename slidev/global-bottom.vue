<!-- global-bottom.vue -->
<template>
  <footer
    v-if="$nav.currentLayout !== 'cover'"
    class="absolute bottom-0 left-0 right-0 h-8 flex items-center justify-between px-6 border-t border-gray-100 text-[10px] text-gray-400 tracking-wide"
  >
    <div class="font-semibold text-gray-500 uppercase tracking-widest">{{ $slidev.configs.title }}</div>
    <div class="font-mono">Duc | AnhDuc.Vu@paluno.uni-due.de</div>
    <div class="font-mono tabular-nums">
      <SlideCurrentNo /> / <SlidesTotal />
    </div>
  </footer>
</template>