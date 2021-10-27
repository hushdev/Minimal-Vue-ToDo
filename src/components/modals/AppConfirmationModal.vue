<template>
  <div class="modal-wrap">
    <form class="modal bg-white">
      <slot></slot>
      <span class="text fs-4">Это действие невозможно отменить</span>
      <div class="actions">
        <app-btn @click.native="$emit('accept')" text="Да, удалить" primary />
        <app-btn @click.native="$emit('deny')" text="Отменить" secondary />
      </div>
    </form>
  </div>
</template>

<script>
import AppBtn from "@/components/AppBtn";

export default {
  components: { AppBtn },
  mounted() {
    document.body.addEventListener("keyup", (e) => {
      if (e.keyCode === 27) {
        this.$emit("deny");
      } else if (e.keyCode === 13) {
        this.$emit("accept");
      }
    });
  },
};
</script>

<style lang="scss" scoped>
.modal-wrap {
  width: calc(100vw - 30px);
  height: 100vh;
  position: fixed;
  top: 0;
  left: 0;
  background: #f0f0f0;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 100;
  padding: 15px;

  .modal {
    display: flex;
    flex-direction: column;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 2px 20px rgba(0, 0, 0, 0.288);

    .text {
      margin: 10px 0 30px 0;
    }
    .actions {
      button:first-child {
        margin-right: 10px;
      }
      button {
        margin-top: 10px;
      }
    }
  }
}
</style>