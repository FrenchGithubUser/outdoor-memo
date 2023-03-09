<template>
  <div class="flash-card">
    <div class="images shadow-3">
      <img
        v-for="image in item.media"
        :key="image.web_path"
        :src="image.web_path"
      />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'FlashCard',
  props: {
    item: { type: Object },
  },
  created() {},
  methods: {},
})
</script>

<style lang="scss" scoped>
.flash-card {
  .images {
    padding: 7px;
    border-radius: 15px;
    img {
      border-radius: 15px;
      margin: 3px;
    }
  }
}
</style>
