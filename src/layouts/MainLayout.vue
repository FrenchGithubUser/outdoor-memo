<template>
  <q-layout view="lHh Lpr lFf">
    <div class="header">
      <div class="right">
        <UserStats />
      </div>
    </div>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import UserStats from 'src/components/UserStats.vue'
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'MainLayout',

  components: {
    UserStats,
  },
  data() {
    return {}
  },
  created() {},
  methods: {},
})
</script>

<style lang="scss" scoped>
.header {
  margin: 5px 10px;
  margin-bottom: -20px;
  .right {
    text-align: right;
  }
}
</style>
