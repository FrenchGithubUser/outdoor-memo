<template>
  <div class="index">
    <div class="title">Outdoor Memo</div>
    <FlashCards />
  </div>
</template>

<script>
import FlashCards from 'src/components/FlashCards.vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  components: { FlashCards },
})
</script>

<style lang="scss" scoped>
.index {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  .title {
    font-size: 2em;
    font-weight: bold;
    color: $primary;
    margin-bottom: 15px;
    margin-top: 20px;
  }
}
</style>
