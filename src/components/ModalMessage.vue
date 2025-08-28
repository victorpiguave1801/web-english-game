<template>
  <div
    v-if="show"
    class="fixed inset-0 bg-black opacity-50 flex items-center justify-center justify z-50"
  >
    <div class="bg-white rounded-lg p-6 shadow text-center animate-bounce">
      <h2 class="">{{ messagePrincipal }}</h2>
      <p>{{ messageSecond }}</p>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    messagePrincipal: String,
    show: Boolean,
    messageSecond: String,
  },
}
</script>
