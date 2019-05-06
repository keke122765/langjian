<template>
  <div :style="{ padding: '24px' }">
    <v-chart
      height="400"
      :data="data"
      :forceFit="true"
      :padding="['auto', 'auto', '40', '50']">
      <v-tooltip />
      <v-axis />
      <v-bar position="x*y" color="#13c2c2" :size="15"/>
    </v-chart>
  </div>
</template>

<script>
const data = []
for (let i = 0; i < 30; i += 1) {
  data.push({
    x: `${i + 1}`,
    y: Math.floor(Math.random() * 1000) + 200
  })
}
const tooltip = [
  'x*y',
  (x, y) => ({
    name: x,
    value: y
  })
]
const scale = [{
  dataKey: 'x',
  min: 2
}, {
  dataKey: 'y',
  title: '时间',
  min: 1,
  max: 22
}]

export default {
  name: 'Bar',
  props: {
  },
  data () {
    this.colors = ['#c4ccd3','#2f4554', '#61a0a8',
        '#d48265', '#91c7ae','#749f83', 
        '#ca8622', '#bda29a','#6e7074',
        '#546570', '#c4ccd3']
    return {
      data,
      scale,
      tooltip
    }
  }
}
</script>
