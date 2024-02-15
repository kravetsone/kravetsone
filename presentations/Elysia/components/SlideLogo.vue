<script setup lang="ts">
const props = defineProps<{
    framework: "ExpressJS" | "KoaJS" | "FastifyJS" | "ElysiaJS",
    title: string
}>()

const logos: Record<typeof props["framework"], string> = {
    ExpressJS: "/express-logo.png",
    KoaJS: "/koa-logo.png",
    FastifyJS: "/fastify-logo.png",
    ElysiaJS: "/elysia-logo.png"
}

</script>

<template>
    <div class="flex flex-row items-center font-bold">
        <img width="50px" :src="logos[props.framework]" />
        <span class="text-xl">{{ props.framework }} - </span>
        <span class="ml-1 text-elysia-purple">
            {{ props.title }}
        </span>
    </div>
</template>