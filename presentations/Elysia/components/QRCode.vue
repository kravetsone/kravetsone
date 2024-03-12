<script setup lang="ts">
import QrCodeС from 'vue-qr-code-styling';

const props = defineProps<{
    size: number,
    value: string
}>()
</script>

<template>
    <QrCodeС :size="props.size" :value="props.value" />
</template>
