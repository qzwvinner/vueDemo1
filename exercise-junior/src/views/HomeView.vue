<template>
    <div class="charts">
      <EchartsBar :data="cycleData" />
      <input class="charts_input" type="number" maxlength="3" :min="0" :max="100" v-model="cycleData.residue">
    </div>
</template>

<script>
import EchartsBar from '../components/EchartsBar.vue'

export default {
  name: 'HomeView',
  components: {
    EchartsBar
  },
  data() {
    return {
      cycleData: {
        residue: 60,
        total: 100
      }
    }
  }
}
</script>
<style scoped lang='less'>
.charts_input{
  margin: 30px;
  width: 250px;
}
</style>
