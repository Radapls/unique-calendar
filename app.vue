<template>
  <section class="clock-container">

    <svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
      <!-- Circular Path -->
      <path id="circlePath" d="M 10, 50 a 40,40 0 1,1 80,0 40,40 0 1,1 -80,0" fill="none" ref="circlePath" />

      <!-- Moving Circle -->
      <circle id="monthCircle" ref="monthCircle" cx="2" cy="46%" r="2" stroke-width="10" stroke="red"
        stroke-dasharray="360" stroke-dashoffset="360" fill="red" />

      <circle cx="50%" cy="50%" r="41" stroke="black" stroke-width="10" stroke-dasharray="0" stroke-dashoffset="360"
        fill="none" />


      <!-- Months -->
      <text>
        <textPath v-for="(month, index) in months" :key="month" :href="'#circlePath'" :startOffset="offsets[index]">
          {{ month }}
        </textPath>
      </text>
    </svg>

    <button @click="moveCircleToRandomMonth">Move to Random Month</button>
  </section>

</template>

<script>
import '~/assets/css/main.css'
export default {
  data() {
    return {
      months: ["Jan", "Feb", "Mar", "Apr", "May", "Jun", "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"],
      offsets: [],
      monthCirclePositions: [
        { cx: "2", cy: "46" },
        { cx: "4.5", cy: "33.5" },
        { cx: "11", cy: "21" },
        { cx: "20", cy: "12" },
        { cx: "31.9", cy: "5.2" },
        { cx: "44.5", cy: "2" },
        { cx: "58", cy: "2.2" },
        { cx: "71.5", cy: "6.5" },
        { cx: "81.8", cy: "13.5" },
        { cx: "91.2", cy: "24" },
        { cx: "96.5", cy: "36" },
        { cx: "98", cy: "49" },

      ]
    };
  },
  mounted() {
    this.calculateOffsets();
  },
  methods: {
    calculateOffsets() {
      const path = this.$refs.circlePath;

      if (!path) return;

      const pathLength = path.getTotalLength();

      const monthsCount = this.months.length;

      const offsetStep = pathLength / monthsCount;

      this.offsets = this.months.map((_, index) => index * offsetStep / 1.90);
    },
    moveCircleToMonth(month) {
      const circle = this.$refs.monthCircle;
      if (!circle) return;

      const monthIndex = this.months.indexOf(month);
      if (monthIndex === -1) return;

      const newOffset = this.monthCirclePositions[monthIndex];

      circle.setAttribute("cx", newOffset.cx);
      circle.setAttribute("cy", newOffset.cy);
    },
    getRandomMonth() {
      const randomIndex = Math.floor(Math.random() * this.months.length);
      return this.months[randomIndex];
    },
    moveCircleToRandomMonth() {
      const randomMonth = this.getRandomMonth();
      this.moveCircleToMonth(randomMonth);
    }
  }
};
</script>


<style>
@import url("~/assets/css/main.css");

.clock-container {
  background-color: lightblue;
  width: 100vw;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

svg {
  max-height: 950px;
}

textPath {
  font-size: 3px;
  fill: white;
  font-weight: bold;
  font-family: sans-serif;
  text-transform: uppercase;
  cursor: pointer;
  transition: 0.2s;
}

textPath:hover {
  fill: red;
}

button {
  position: absolute;
  top: 50%;
}
</style>