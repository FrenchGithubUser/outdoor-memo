<template>
  <div class="flash-cards">
    <FlashCard :item="currentItem" v-if="currentItem.id" />
    <q-input
      bg-color="blue-grey-1"
      v-model="input"
      class="answer-input"
      @keyup.enter="search"
      outlined
    >
    </q-input>

    <div class="btns">
      <q-btn
        class="btn"
        label="Précédent"
        color="primary"
        no-caps
        @click="previous"
      />
      <q-btn
        class="btn"
        label="Suivant"
        color="primary"
        no-caps
        @click="next"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'
import FlashCard from './FlashCard.vue'

export default defineComponent({
  name: 'EssentialLink',
  components: { FlashCard },
  props: {},
  data() {
    return { items: [], currentItem: {} }
  },
  created() {
    this.getData()
  },
  methods: {
    next() {
      this.currentItem = this.items[0]
      this.items.shift()
    },
    getData() {
      axios
        .get(
          'https://api.allorigins.win/get?url=' +
            encodeURIComponent(
              'https://www.faune-france.org/index.php?m_id=1351&content=gallery_pictures&backlink=skip&sp_tg=1&frmPage=all&mp_item_per_page=1&mp_current_page=1&frmAuthor=0&frmSpecies=0&frmCanton=0&frmCantons='
            )
        )
        .then((data) => {
          this.items = JSON.parse(data.data.contents).data
          this.next()
        })
    },
  },
})
</script>

<style lang="scss" scoped>
.flash-cards {
  display: flex;
  flex-direction: column;
  align-items: center;
  .answer-input {
    margin: 10px;
  }
  .btns {
    .btn {
      margin: 5px;
    }
  }
}
</style>
