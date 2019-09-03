<template>
  <div class="letter shadow mr-3">
    <span class="pb-1" v-if="open">{{value}}</span>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: ""
    },
    open: {
      type: Boolean,
      default: false
    }
  }
};
</script>

<style>
.letter {
  width: 70px;
  height: 80px;
  font-size: 45px;

  display: flex;
  align-items: center;
  justify-content: center;
  flex-grow: 0;
}
</style>