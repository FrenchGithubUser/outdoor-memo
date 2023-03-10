<template>
  <div class="flash-card">
    <div class="image-container shadow-3">
      <img
        :src="item.media[0].web_path.replace('/small', '')"
        v-if="answerStatus === null"
      />
      <div class="answer-status" v-else>
        <div class="true" v-if="answerStatus">Bonne réponse !</div>
        <div class="false" v-else>
          Mauvaise réponse !
          <div class="hint">(t'es nul !)</div>
        </div>
        <div class="response">
          <div>Nom : {{ item.bird_name }}</div>
          <div>Nom latin : {{ item.latin_name }}</div>
        </div>
        <q-btn
          label="Ok"
          color="primary"
          no-caps
          @click="acknowledgeResponse"
        />
      </div>
    </div>
    <q-input
      bg-color="blue-grey-1"
      v-model="input"
      label="Nom"
      class="answer-input"
      @keyup.enter="checkAnswer"
      outlined
    >
    </q-input>
    <q-btn
      class="btn"
      label="Valider"
      color="primary"
      no-caps
      @click="checkAnswer"
    />
  </div>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'FlashCard',
  props: {
    item: { type: Object },
  },
  data() {
    return {
      input: '',
      answerStatus: null,
    }
  },
  created() {},
  methods: {
    acknowledgeResponse() {
      this.emitter.emit('newAnswer', {
        status: this.answerStatus,
        id: this.item.id,
        date: new Date().toUTCString(),
        latin_name: this.item.latin_name,
        media: [
          {
            web_path: this.item.media[0].web_path,
          },
        ],
      })
      console.log(this.item)
      this.$emit('next')
    },
    checkAnswer() {
      if (this.input === '') return
      let answer = this.input.toLowerCase()
      let responseFr = this.item.bird_name.toLowerCase()
      let responseLatin = this.item.latin_name.toLowerCase()
      if (answer === responseFr || answer === responseLatin) {
        this.answerStatus = true
      } else {
        this.answerStatus = false
      }
    },
  },
  watch: {
    item(newval) {
      console.log(newval.bird_name)
      console.log(newval.latin_name)
      this.answerStatus = null
      this.input = ''
    },
  },
})
</script>

<style lang="scss" scoped>
.flash-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  .image-container {
    padding: 7px;
    border-radius: 15px;
    display: flex;
    justify-content: center;
    align-items: center;
    width: 500px;
    height: 500px;
    img {
      border-radius: 15px;
      margin: 3px;
      width: 100%;
      height: 100%;
      object-fit: contain;
    }
    .answer-status {
      text-align: center;
      font-size: 2em;
      font-weight: bold;
      .false {
        color: $negative;
        .hint {
          font-size: 0.5em;
        }
      }
      .true {
        color: $positive;
      }
      .response {
        margin: 15px 0px;
        font-weight: normal;
        font-size: 0.65em !important;
      }
    }
  }
  .answer-input {
    margin: 10px;
    width: 300px;
  }
}
</style>
