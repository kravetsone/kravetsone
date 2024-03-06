<script setup lang="ts">
const props = defineProps<{
    framework: "ExpressJS" | "KoaJS" | "FastifyJS" | "ElysiaJS",
    title: string
}>()

</script>

<template>
    <div class="flex flex-row items-center font-bold gap-2">
        <div class="text-3xl">
            <skill-icons-expressjs-light text-1xl v-if="props.framework === 'ExpressJS'" />
            <simple-icons-koa v-if="props.framework === 'KoaJS'" />
            <simple-icons-fastify v-if="props.framework === 'FastifyJS'" />
            <img src="/elysia-logo.png" width="40px" v-if="props.framework === 'ElysiaJS'" />
        </div>

        <div class="flex flex-row items-center gap-1">
            <span class="text-xl">{{ props.framework }} - </span>
            <span class="text-elysia-purple">
                {{ props.title }}
            </span>
        </div>
    </div>
</template>