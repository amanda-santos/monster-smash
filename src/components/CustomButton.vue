<script setup lang="ts">
const props = defineProps<{
  onClick?: () => void
}>()

const handleClick = () => {
  props.onClick?.()
}
</script>

<template>
  <button
    role="button"
    class="flex cursor-pointer items-center rounded-base border-2 border-border bg-main px-4 py-2 font-mono text-sm font-semibold uppercase text-text shadow-light transition-all enabled:hover:translate-x-boxShadowX enabled:hover:translate-y-boxShadowY enabled:hover:shadow-none disabled:cursor-not-allowed disabled:bg-zinc-300 dark:border-darkBorder dark:shadow-dark"
    @click="handleClick"
  >
    <slot></slot>
  </button>
</template>

<style scoped></style>
