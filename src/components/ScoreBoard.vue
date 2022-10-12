<template>
  <section class="score">
    <div class="play question-item">
      <p> Player <strong> {{ this.winCount }} </strong></p>
    </div>
    <span> x </span>
    <div class="comput question-item">
      <p>Computer <strong> {{ this.loseCount }} </strong></p>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    winCount: Number,
    loseCount: Number,
  },
};
</script>

<style lang="scss">
.section.score {
  border-bottom: 1px solid black;
  padding: 24px;
  font-size: 18px;

  display: flex;
  align-items: center;
  justify-content: center;

  span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid black;
  }
}
</style>
