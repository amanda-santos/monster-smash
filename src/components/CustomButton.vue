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
    class="flex cursor-pointer items-center rounded-base border-2 border-border bg-main px-4 py-2 font-mono text-sm font-semibold uppercase text-text shadow-light transition-all hover:translate-x-boxShadowX hover:translate-y-boxShadowY hover:shadow-none dark:border-darkBorder dark:shadow-dark dark:hover:shadow-none"
    @click="handleClick"
  >
    <slot></slot>
  </button>
</template>

<style scoped></style>
