<template functional>
  <div class="day" :class="props.position">
    <div v-if="props.day === 1" class="day__title">
      {{props.month}}
    </div>
    {{props.day}}
  </div>
</template>

