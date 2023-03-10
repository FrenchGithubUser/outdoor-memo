<template>
  <div class="category-selectors">
    <div class="selects">
      <q-select
        outlined
        v-model="selectedSpecies"
        :options="species"
        label="Espèce"
        class="selector"
      />
      <q-select
        outlined
        v-model="selectedCanton"
        :options="cantons"
        label="Canton"
        class="selector"
      />
    </div>
    <q-btn
      label="Chercher"
      color="primary"
      :loading="loading"
      no-caps
      @click="selectionUpdated"
    />
  </div>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'CategorySelectors',
  components: {},
  props: { loading: { type: Boolean } },
  data() {
    return {
      species: [
        { label: 'Oiseaux', id: 1 },
        { label: 'Mamifères', id: 3 },
      ],
      cantons: [],
      selectedSpecies: {},
      selectedCanton: { label: 'Tous', code: '' },
    }
  },
  created() {
    this.selectedSpecies = this.species[0]
    axios
      .get(
        'https://api.allorigins.win/get?url=' +
          encodeURIComponent(
            'https://www.faune-france.org/index.php?m_id=1351&content=gallery_cantons&backlink=skip&frmPage=1&frmPage=1&sp_tg=1'
          )
      )
      .then((data) => {
        let cantons = JSON.parse(data.data.contents)
        Object.keys(cantons).forEach((key) => {
          this.cantons.push({ label: cantons[key]['value'], code: key })
        })
        this.cantons.unshift({ label: 'Tous', code: '' })
      })
  },
  methods: {
    selectionUpdated() {
      this.$emit('selection-updated', {
        canton: this.selectedCanton.code,
        species: this.selectedSpecies.id,
      })
    },
  },
})
</script>

<style lang="scss" scoped>
.category-selectors {
  margin-bottom: 15px;
  text-align: center;
  .selects {
    display: flex;
    .selector {
      width: 200px;
      margin: 5px;
    }
  }
}
</style>
