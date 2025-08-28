<script>
export default {
  props: {
    title: String,
    color: String,
  },
  methods: {
    onDragStart(event) {
      event.dataTransfer.setData('text/plain', this.title)
      event.dataTransfer.effectAllowed = 'move'
      event.dataTransfer.setDragImage(event.target, 0, 0)
    },
    speakColor(title) {
      const utterance = new SpeechSynthesisUtterance(title)
      utterance.lang = 'en-US'
      speechSynthesis.speak(utterance)
    },
  },
}
</script>
<template>
  <div
    :style="{ backgroundColor: color }"
    class="w-40 h-64 rounded shadow-md cursor-grab flex items-center justify-center transition-transform"
    draggable="true"
    @dragstart="onDragStart"
    @click="speakColor(title)"
  >
    <span class="drop-shadow text-white font-bold">
      {{ title }}
    </span>
  </div>
</template>
