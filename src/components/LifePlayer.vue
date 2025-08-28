<template>
  <div class="flex gap-1">
    <span v-for="n in 3" :key="n" class="text-red-500 text-xl">
      {{ n <= lives ? '❤️' : '🤍' }}
    </span>
  </div>
</template>
<script>
export default {
  props: {
    lives: Number,
  },
}
</script>
