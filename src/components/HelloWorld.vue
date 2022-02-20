<template>
  <div class="hello">
    <div></div>
    <progress id="bar" :value="counter" max="100"></progress>
    <div><!--{{ roundCounter }} %--></div>
    <button v-if="!plugged" @click="plug">Take a break</button>
    <button v-if="plugged" @click="unplug">Start working</button>
    <button @click="resetZero">I'm exhausted</button>
    <button @click="resetCent">I'm ready</button>

    <div>
      <label for="book">Work (minutes) :</label>
      <select
        id="book"
        placeholder="Select a book"
        v-model="selected_uncharging_duration"
      >
        <option disabled value="" selected=""></option>
        <option
          :value="duration.value"
          v-for="duration in durations"
          :key="duration.id"
          >{{ duration.value }}</option
        >
      </select>
    </div>

    <div>
      <label for="book">Break (minutes) :</label>
      <select
        id="book"
        placeholder="Select a book"
        v-model="selected_charging_duration"
      >
        <option disabled value="" selected=""></option>
        <option
          :value="duration.value"
          v-for="duration in durations"
          :key="duration.id"
          >{{ duration.value }}</option
        >
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function () {
    return {
      counter: 100,
      roundCounter: 100,
      plugged: true,
      selected_charging_duration: 10,
      selected_uncharging_duration: 30,
      durations: [
        { id: 1, value: 5 },
        { id: 2, value: 10 },
        { id: 3, value: 15 },
        { id: 4, value: 30 },
        { id: 5, value: 60 }
      ]
    }
  },
  methods: {
    plug () {
      this.plugged = true
      clearInterval(this.timer)
      this.timer = null
      var bar = document.getElementById('bar')
      bar.style.color = 'yellow'
      this.timer = setInterval(() => {
        if (this.counter < 100) {
          this.counter += 100 / (this.selected_charging_duration * 60)
          this.roundCounter = Math.round(this.counter)
        }
      }, 1000)
    },
    unplug () {
      this.plugged = false
      clearInterval(this.timer)
      this.timer = null
      this.timer = setInterval(() => {
        if (this.counter > 0) {
          this.counter -= 100 / (this.selected_uncharging_duration * 60)
          this.roundCounter = Math.round(this.counter)
        }
      }, 1000)
    },
    resetCent () {
      this.counter = 100
      this.roundCounter = 100
    },
    resetZero () {
      this.counter = 0
      this.roundCounter = 0
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

progress {
  border: 0;
  height: 18px;
  border-radius: 9px;
}
progress::-webkit-progress-bar {
  border: 0;
  height: 18px;
  border-radius: 9px;
  background-color: lightgray;
}
progress::-webkit-progress-value {
  border: 0;
  height: 18px;
  border-radius: 9px;
  background-color: green;
}
progress::-moz-progress-bar {
  border: 0;
  height: 18px;
  border-radius: 9px;
  background-color: green;
}
</style>
