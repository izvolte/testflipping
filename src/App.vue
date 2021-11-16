<template>
  <div id="app">
    <Calendar :date="date" @update="onUpdateDate"/>
  </div>
</template>

<script>
import {Calendar} from "@/components/Calendar";

export default {
  name: 'App',
  components: {Calendar},
  data: () => ({
    date: "06.01.2017" // "MM.DD.YYYY"
  }),
  methods: {
    onUpdateDate(date){
      this.date = date
    }
  }
};
</script>
