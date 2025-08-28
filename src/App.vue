<script>
import { ref } from 'vue'
import ColorCard from './components/ColorCard.vue'
import DropZone from './components/DropZone.vue'
import ModalMessage from './components/ModalMessage.vue'
import LifePlayer from './components/LifePlayer.vue'
import confetti from 'canvas-confetti'
export default {
  setup() {
    const ListFullColor = [
      { title: 'Red', hex: '#FF0000' },
      { title: 'Blue', hex: '#0000FF' },
      { title: 'Green', hex: '#00FF00' },
      { title: 'Yellow', hex: '#FFFF00' },
      { title: 'Orange', hex: '#FFA500' },
      { title: 'Purple', hex: '#800080' },
      { title: 'Pink', hex: '#FFC0CB' },
      { title: 'Brown', hex: '#8B4513' },
      { title: 'Black', hex: '#000000' },
      { title: 'White', hex: '#FFFFFF' },
      { title: 'Gray', hex: '#808080' },
      { title: 'Light Blue', hex: '#ADD8E6' },
      { title: 'Dark Green', hex: '#006400' },
      { title: 'Gold', hex: '#FFD700' },
      { title: 'Silver', hex: '#C0C0C0' },
      { title: 'Cyan', hex: '#00FFFF' },
      { title: 'Magenta', hex: '#FF00FF' },
      { title: 'Beige', hex: '#F5F5DC' },
      { title: 'Lime', hex: '#32CD32' },
      { title: 'Navy', hex: '#000080' },
    ]
    const remainingColors = ref([...ListFullColor])
    const list_color = ref([])
    const targetColor = ref('')
    const feedback = ref(null)
    const result = ref(null)
    const started = ref(false)
    const speak = (text) => {
      const utterance = new SpeechSynthesisUtterance(text)
      utterance.lang = 'en-US'
      speechSynthesis.speak(utterance)
    }

    const generateRound = () => {
      if (remainingColors.value.length < 3) {
        list_color.value = []
        targetColor.value = ''
        feedback.value = 'Game complete'
        return
      }
      list_color.value = remainingColors.value.sort(() => 0.5 - Math.random()).slice(0, 3)
      targetColor.value = list_color.value[Math.floor(Math.random() * 3)].title
      speak(`Drag the ${targetColor.value} card to the box`)
    }

    const repeatSpeak = () => {
      speak(`Drag the ${targetColor.value} card to the box`)
    }

    const speakColor = () => {
      speak(targetColor.value)
    }
    const showModal = ref(false)
    const lives = ref(3)

    const messagePrincipal = ref('')
    const messageSecond = ref('')
    const checkAnswer = (selected) => {
      if (selected === targetColor.value) {
        feedback.value = `✅ Correct! That’s ${selected}`
        result.value = 'correct'
        remainingColors.value = remainingColors.value.filter((c) => c.title !== selected)
        messagePrincipal.value = ' Well done!'
        messageSecond.value = 'You matched the correct color!'
        showModal.value = true

        confetti({
          particleCount: 100,
          spread: 70,
          origin: { y: 0.6 },
        })
      } else {
        feedback.value = `❌ Wrong! You dropped ${selected}`
        result.value = 'wrong'
        lives.value = Math.max(0, lives.value - 1)
      }

      if (lives.value === 0) {
        feedback.value = '💔 Game Over! Restarting...'
        setTimeout(() => {
          lives.value = 3
          remainingColors.value = [...list_color]
          generateRound()
        }, 3000)
      }

      setTimeout(() => {
        celebrate()
        result.value = null
        showModal.value = false
        generateRound()
      }, 2000)
    }

    const celebrate = () => {
      const utterance = new SpeechSynthesisUtterance('Great job!')
      utterance.lang = 'en-US'
      speechSynthesis.speak(utterance)
    }

    generateRound()
    return {
      list_color,
      targetColor,
      result,
      feedback,
      remainingColors,
      checkAnswer,
      started,
      repeatSpeak,
      showModal,
      messagePrincipal,
      messageSecond,
      lives,
      speakColor,
    }
  },
  components: {
    ColorCard,
    DropZone,
    ModalMessage,
    LifePlayer,
  },
}
</script>

<template>
  <main class="min-h-screen bg-gray-100 p-4 flex flex-col items-center justify-center space-y-6">
    <LifePlayer :lives="lives" />
    <ModalMessage
      :show="showModal"
      :message-principal="messagePrincipal"
      :message-second="messageSecond"
    />
    <h1 class="text-2xl font-bold text-center">Welcome English Color</h1>
    <div v-if="list_color.length" class="text-xl font-semibold">
      Drag the <span @click="speakColor" class="text-blue-600">{{ targetColor }}</span> card to the
      box
    </div>
    <button
      class="bg-blue-600 text-white px-4 py-2 rounded shadow hover:bg-blue-700 transition text-lg"
      @click="repeatSpeak"
    >
      Repeat Instruction
    </button>

    <div class="flex flex-col sm:flex-row gap-8 items-center justify-center w-full max-w-4xl">
      <!---Layout Izquierda-->
      <div class="flex flex-wrap sm:flex-row gap-4 sm:w-2/3 justify-center">
        <ColorCard
          v-for="card in list_color"
          :key="card.title"
          :title="card.title"
          :color="card.hex"
        />
      </div>
      <div class="flex justify-center">
        <DropZone :title="targetColor" :result="result" @validate="checkAnswer" />
      </div>
    </div>
  </main>
</template>

<style scoped></style>
