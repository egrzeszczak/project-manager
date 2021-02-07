<template>
  <div
    class="card"
    :id="card.id"
    :draggable="card.isDraggable"
    @dragstart="$emit('carddragstart', $event)"
    @dragend="$emit('carddragend', $event)"
  >
    {{ card.content }}
  </div>
</template>

<script>
export default {
  props: ["card"],
};
</script>

<style lang="scss" scoped>
@import url("https://fonts.googleapis.com/css2?family=Comfortaa:wght@300;400;700&display=swap");

$color-background: #caf3cb;
$color-board-1: #a3c19c;
$color-board-2: #99c99d;
$color-board-3: #97b788;
$color-board-4: #779a83;
$color-font: #5c5757;
$color-label: #eefeee;

.card {
  display: grid;
  align-content: center;
  justify-content: center;

  background: $color-label;
  color: $color-font;

  margin: 0 0.75rem 0.75rem 0.75rem;
  min-height: 50px;
  border-radius: 0.75rem;

  word-break: normal;

  font-family: "Comfortaa";
  font-size: 12px;
}
</style>
