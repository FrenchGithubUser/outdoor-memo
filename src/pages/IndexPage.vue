<template>
  <div class="index">
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
  justify-content: center;
  height: 100vh;
}
</style>
