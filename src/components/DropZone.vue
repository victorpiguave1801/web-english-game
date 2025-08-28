<script>
export default {
  props: {
    targetColor: String,
    result: String,
  },
  emits: ['validate'],
  computed: {
    zoneClass() {
      if (this.result === 'correct') return 'border-green-500 text-green-600 bg-green-100'
      if (this.result === 'wrong') return 'border-red-500 text-red-600 bg-red-100'
      return 'border-gray-400 text-gray-600 bg-white'
    },
  },
  methods: {
    onDrop(e) {
      const droppedColor = e.dataTransfer.getData('text/plain')
      this.$emit('validate', droppedColor)
    },
  },
}
</script>
<template>
  <div
    class="w-56 h-72 border-4 rounded border-dashed border-gray-400 flex items-center justify-center text-gray-600"
    :class="zoneClass"
    @dragover.prevent
    @drop="onDrop"
  >
    Drop Here
  </div>
</template>
