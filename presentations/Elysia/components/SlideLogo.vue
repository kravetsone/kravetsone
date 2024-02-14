<script setup lang="ts">
const props = defineProps<{
    title: "ExpressJS"
}>()

const logos: Record<typeof props["title"], string> = {
    ExpressJS: "/express-logo.png"
}
</script>

<template>
    <div class="flex flex-row items-center font-bold">
        <img width="50px" :src="logos[props.title]" />
        <span class="text-xl">{{ props.title }}</span>
    </div>
</template>