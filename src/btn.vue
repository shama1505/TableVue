<template lang="html">
  <div id="control">
    <button type="button" name="button" v-on:click="counter += 1">добавить таблицу</button>
    <my-table v-for="i in counter"></my-table>
  </div>
</template>

<script>
import App from './App.vue'

export default {
  data() {
    return {
      counter: 0
    }
  },
  components: {
    "my-table":App,
  }
}
</script>

<style lang="css">
button{
  padding-left: 2px;
  margin: 5px;
  border-radius: 10px;
  border: none;
}
button{
  border:1px solid black;
}
</style>
