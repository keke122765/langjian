<template>
  <div :style="{ padding: '12px 44px 12px 12px' }">
    <h4 :style="{ marginBottom: '20px' }">{{ title }}</h4>
    <v-chart
      height="254"
      :data="data"
      :forceFit="true"
      :padding="['auto', 'auto', '40', '50']">
      <v-tooltip />
      <v-axis />
      <v-bar position="x*y" color="#13c2c2" />
    </v-chart>
  </div>
</template>

<script>
const data = []
for (let i = 0; i < 15; i += 1) {
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
    title: {
      type: String,
      default: ''
    }
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
