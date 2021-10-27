<template>
  <div class="modal-wrap">
    <form class="modal bg-white">
      <h2 class="fs-6 text-primary">
        Введите имя
        <slot></slot>
      </h2>

      <input v-model="value" class="b-primary text-primary fs-4" type="text" />

      <div class="actions">
        <app-btn @click.native="create()" text="Создать" primary />
        <app-btn @click.native="deny()" text="Отменить" secondary />
      </div>
    </form>
  </div>
</template>

<script>
import AppBtn from "@/components/AppBtn";

export default {
  components: { AppBtn },
  data: () => ({
    value: "",
  }),
  mounted() {
    document.body.addEventListener("keyup", (e) => {
      if (e.keyCode === 27) {
        this.deny();
      } else if (e.keyCode === 13) {
        this.create();
      }
    });
  },
  methods: {
    deny() {
      this.value = "";
      this.$emit("deny");
    },
    create() {
      if (this.value.length > 1) {
        this.$emit("accept", this.value);
        this.value = "";
      }
    },
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

    input {
      margin: 10px 0 40px 0;
      padding: 10px;
      border-radius: 5px;
    }
    .actions {
      button:first-child {
        margin-right: 10px;
      }
    }
  }
}
</style>