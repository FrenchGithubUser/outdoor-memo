<template>
  <div class="flash-cards">
    <CategorySelectors
      @selection-updated="categoriesUpdated"
      :loading="loading"
    />
    <FlashCard :item="currentItem" v-if="currentItem.id" @next="next" />
    <div class="btns">
      <q-btn
        class="btn"
        label="Précédent"
        color="primary"
        :disable="itemsDone.length === 0"
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
import FlashCard from 'components/FlashCard.vue'
import CategorySelectors from 'components/CategorySelectors.vue'

export default defineComponent({
  name: 'FlashCards',
  components: { FlashCard, CategorySelectors },
  props: {},
  data() {
    return {
      items: [],
      itemsDone: [],
      currentItem: {},
      loading: true,
      apiBaseUrl:
        'https://www.faune-france.org/index.php?m_id=1351&content=gallery_pictures&backlink=skip&frmPage=all&mp_item_per_page=16&mp_current_page=1&frmAuthor=0&frmSpecies=0&frmCanton=0',
    }
  },
  created() {
    this.getData()
  },
  methods: {
    next() {
      this.itemsDone.unshift({ ...this.currentItem })
      this.items.shift()
      this.currentItem = this.items[0]
    },
    previous() {
      this.items.unshift({ ...this.currentItem })
      this.currentItem = this.itemsDone[0]
      this.itemsDone.shift()
    },
    categoriesUpdated(payload) {
      console.log(payload)
      this.getData(payload.species, payload.canton)
    },
    getData(species = '1', canton = '') {
      this.loading = true
      axios
        .get(
          'https://api.allorigins.win/get?url=' +
            encodeURIComponent(
              this.apiBaseUrl + '&sp_tg=' + species + '&frmCantons=' + canton
            )
        )
        .then((data) => {
          console.log(data.data === '')
          if (data.data === '') {
            this.$q.notify('Aucune donnée, utilisation des données précédentes')
          } else {
            this.items = []
            this.itemsDone = []
            this.items = JSON.parse(data.data.contents).data

            this.currentItem = this.items[0]
          }
          this.loading = false
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

  .btns {
    text-align: center;
    margin-top: 15px;
    .btn {
      margin: 10px;
    }
  }
}
</style>
