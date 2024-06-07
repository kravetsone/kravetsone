<template>
    <Ray class="h-[150px] top-0 left-0 z-[100] opacity-25 dark:opacity-[.55] pointer-events-none" static />
    <!-- an example footer for pages -->
    <footer v-if="$nav.currentLayout !== 'cover' && $nav.currentLayout !== 'full'"
        class="absolute top-0 left-0 right-0 p-2 opacity-25">
        {{ $nav.currentPage }} / {{ $nav.total }}
    </footer>
</template>