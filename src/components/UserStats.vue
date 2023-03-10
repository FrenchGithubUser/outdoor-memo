<template>
  <div class="user-stats">
    <q-btn
      label="Statistiques"
      color="primary"
      no-caps
      @click="popupOpened = true"
    />

    <q-dialog v-model="popupOpened">
      <div class="popup" id="stats-popup">
        <div class="today section shadow-3">
          <span class="title">Ajourd'hui :</span>
          <div class="subsection primary">{{ doneToday }} réponses</div>
          <div class="subsection positive">{{ correctToday }} justes</div>
        </div>
        <div class="total section shadow-3">
          <span class="title">Total :</span>
          <div class="subsection primary">{{ doneTotal }} réponses</div>
          <div class="subsection positive">{{ correctTotal }} justes</div>
        </div>
      </div>
    </q-dialog>
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'UserStats',
  props: {},
  data() {
    return {
      popupOpened: false,
      stats: {},
      doneToday: 0,
      correctToday: 0,
      doneTotal: 0,
      correctTotal: 0,
    }
  },
  created() {
    this.emitter.on('newAnswer', this.newAnswer)
    this.stats = JSON.parse(localStorage.getItem('userStats'))

    if (!this.stats) {
      this.stats = {
        items: [],
      }
    }

    this.stats.items.forEach((item) => {
      this.updateDisplayedStats(item)
    })
  },
  methods: {
    newAnswer(answer) {
      if (!answer) return
      this.stats.items.unshift(answer)
      this.updateDisplayedStats(answer)
      localStorage.setItem('userStats', JSON.stringify(this.stats))
    },
    updateDisplayedStats(item) {
      this.doneTotal++
      if (item.status) {
        this.correctTotal++
      }
      if (this.isToday(new Date(item.date))) {
        this.doneToday++
        if (item.status) {
          this.correctToday++
        }
      }
    },
    isToday(date) {
      var today = new Date()
      return (
        date.getDate() === today.getDate() &&
        date.getMonth() === today.getMonth() &&
        date.getFullYear() === today.getFullYear()
      )
    },
  },
  beforeUnmount() {
    this.emitter.off('newAnswer', this.newAnswer())
  },
})
</script>

<style lang="scss" scoped></style>
<style lang="scss">
#stats-popup {
  display: flex;
  .section {
    display: flex;
    flex-direction: column;
    margin: 5px;
    padding: 10px;
    text-align: center;
    border-radius: 10px;
    font-weight: bold;
    .title {
      font-size: 1.2em;
      margin-bottom: 10px;
    }
    .positive {
      color: $positive;
    }
    .primary {
      color: $primary;
    }
  }
}
</style>
