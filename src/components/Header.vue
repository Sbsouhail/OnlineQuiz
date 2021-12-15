<template>
  <nav class="navbar navbar-dark bg-dark">
    <div class="container-fluid">
      <a class="pointer navbar-brand" @click="restart">Fancy Quiz App</a>
      <div class="navbar-text">
        Answers:{{ numTotal }} | Correct:{{ numCorrect }} | Qestions:{{ size }}
      </div>
    </div>
  </nav>
</template>
<script>
export default {
  props: {
    size: Number,
    numCorrect: Number,
    numTotal: Number,
  },
  methods: {
    restart() {
      location.reload();
    },
  },
};
</script>
<style scoped>
.pointer {
  cursor: pointer;
}
</style>