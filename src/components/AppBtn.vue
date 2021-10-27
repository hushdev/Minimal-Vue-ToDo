<template>
  <button
    class="fs-4 fw-r text-white"
    :class="[{'bg-primary': primary}, {'b-primary': primary}, {'bg-secondary': secondary}, {'b-secondary': secondary}]"
  >{{text}}</button>
</template>

<script>
export default {
  props: {
    primary: {
      type: Boolean,
      required: false,
    },
    secondary: {
      type: Boolean,
      required: false,
    },
    text: {
      type: String,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
button {
  padding: 10px;
  // background: transparent;
  cursor: pointer;
  transition: 0.2s;
  border-radius: 5px;
}

button.bg-primary:hover {
  background: transparent;
  color: #000000;
}

button.bg-secondary:hover {
  background: transparent;
  color: #6c6c6c;
}
</style>