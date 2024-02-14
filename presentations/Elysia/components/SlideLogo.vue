<script setup lang="ts">
const props = defineProps<{
    framework: "ExpressJS" | "KoaJS" | "FastifyJS",
    title: string
}>()

const logos: Record<typeof props["framework"], string> = {
    ExpressJS: "/express-logo.png",
    KoaJS: "/koa-logo.png",
    FastifyJS: "/fastify-logo.png"
}
// TODO: поправить размеры и отступы
const widths: Record<typeof props["framework"], string> = {
    ExpressJS: "50px",
    KoaJS: "43px",
    FastifyJS: "50px"
}

</script>

<template>
    <div class="flex flex-row items-center font-bold">
        <img :width="widths[props.framework]" :src="logos[props.framework]" />
        <span class="text-xl">{{ props.framework }} - </span>
        <span class="ml-1 text-elysia-purple">
            {{ props.title }}
        </span>
    </div>
</template>