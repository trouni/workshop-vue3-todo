<script setup>
defineProps({
  checked: Boolean
})
</script>

<template>
  <div :class="['checkbox', { checked }]"></div>
</template>

<style lang="scss" scoped>
$check-color: #41b883;

.checkbox {
  display: flex;
  align-self: center;
  border: solid 2px $check-color;
  border-radius: 50%;
  width: 2rem;
  height: 2rem;
  margin: 0 1rem;
  cursor: pointer;

  &:hover {
    background-color: rgba($check-color, 0.2);
    transform: scale(1.1);
  }

  &.checked {
    background-color: $check-color;
  }

  &.checked:after {
    content: "";
    display: inline-block;
    transform: rotate(45deg) translate(-35%, 40%);
    height: 1rem;
    width: 1rem;
    margin-left: 60%;
    border-bottom: 5px solid white;
    border-right: 5px solid white;
  }
}
</style>