<script setup>
import { computed } from 'vue'
import { useStyleTag } from '@vueuse/core'

useStyleTag(computed(() =>
    ['slide', 'presenter'].includes($renderContext.value) && $page.value === $slidev.nav.currentPage ? `
.v-popper__popper.v-popper--theme-twoslash {
  display: block !important;
}` : ''))
</script>

<template>
    <div />
</template>