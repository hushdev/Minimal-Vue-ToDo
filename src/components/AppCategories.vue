<template>
  <div class="categories">
    <transition name="fade">
      <!-- MODAL TO DELETE TASKS -->
      <app-confirmation-modal v-if="isDeleteTaskModalShown" @accept="deleteTask" @deny="deny">
        <h2 class="fs-6 text-primary">Вы уверены что хотите удалить задачу из списка?</h2>
      </app-confirmation-modal>

      <!-- MODAL TO DELETE CATEGORIES -->
      <app-confirmation-modal
        v-if="isDeleteCategoryModalShown"
        @accept="deleteCategory"
        @deny="deny"
      >
        <h2 class="fs-6 text-primary">Вы уверены что хотите удалить категорию?</h2>
      </app-confirmation-modal>

      <!-- MODAL TO CREATE CATEGORY -->
      <app-create-modal
        v-if="isCreateCategoryModalShown"
        @accept="createCategory"
        @deny="deny"
      >категории</app-create-modal>

      <!-- MODAL TO CREATE TASK -->
      <app-create-modal v-if="isCreateTaskModalShown" @accept="createTask" @deny="deny">задачи</app-create-modal>
    </transition>

    <!-- TEXT FOR CASE OF EMPTY DATA -->
    <p
      v-if="items.length < 1"
      class="welcome-text fs-4 text-secondary"
    >Список пуст, добавьте категории и задачи...</p>

    <!-- CARDS WITH CATEGORIES -->
    <app-cards
      :items="items"
      @createTask="openCreateTaskModal"
      @deleteTask="openDeleteTaskModal"
      @deleteCategory="openDeleteCategoryModal"
      @save="saveIntoLocalStorage"
    />

    <!-- NEW CATEGORY BUTTON -->
    <app-btn
      v-if="items.length < 3"
      @click.native="isCreateCategoryModalShown = true"
      text="+ новая категория"
      primary
      class="new-category"
    />
  </div>
</template>

<script>
import AppCards from "@/components/AppCards";
import AppConfirmationModal from "@/components/modals/AppConfirmationModal";
import AppCreateModal from "@/components/modals/AppCreateModal";
import AppBtn from "@/components/AppBtn";

export default {
  components: { AppBtn, AppConfirmationModal, AppCreateModal, AppCards },
  data: () => ({
    isDeleteTaskModalShown: false,
    isDeleteCategoryModalShown: false,
    isCreateCategoryModalShown: false,
    isCreateTaskModalShown: false,
    categoryWhereDelete: null,
    categoryToDelete: null,
    categoryToModify: null,
    taskToDelete: null,
    items: [],
  }),

  mounted() {
    try {
      const data = JSON.parse(localStorage.getItem("items"));
      if (data === undefined || data === null) {
        return;
      }
      this.items = data;
    } catch (e) {
      throw e;
    }
  },
  methods: {
    openDeleteTaskModal(category, task) {
      //change modal flag to opened
      this.isDeleteTaskModalShown = true;

      //set category where task must be deleted
      this.categoryWhereDelete = category;

      //set task to be deleted
      this.taskToDelete = task;
    },

    openDeleteCategoryModal(category) {
      //change modal flag to opened
      this.isDeleteCategoryModalShown = true;

      //set category where task must be deleted
      this.categoryToDelete = category;
    },

    openCreateTaskModal(category) {
      this.isCreateTaskModalShown = true;
      this.categoryToModify = category;
    },

    createCategory(value) {
      //create new Object
      const item = {
        category: value,
        tasks: [],
      };
      this.items.push(item);

      this.saveIntoLocalStorage(this.items);

      //hide modal
      this.isCreateCategoryModalShown = false;
    },

    createTask(value) {
      //creating new task
      const item = {
        name: value,
        completed: false,
      };

      this.items.forEach((el) => {
        if (el === this.categoryToModify) {
          //filter tasks and push selected one
          el.tasks.push(item);
        }
      });

      this.saveIntoLocalStorage(this.items);

      //hide modal
      this.isCreateTaskModalShown = false;
    },

    deleteTask() {
      //change modal flag to closed
      this.isDeleteTaskModalShown = false;

      //find category where task needs to be deleted
      this.items.forEach((item) => {
        if (item === this.categoryWhereDelete) {
          //filter tasks and delete selected one
          item.tasks = item.tasks.filter((task) => task != this.taskToDelete);
        }
      });

      this.saveIntoLocalStorage(this.items);

      //clear data
      this.categoryWhereDelete = null;
      this.taskToDelete = null;
    },

    deleteCategory() {
      //change modal flag to closed
      this.isDeleteCategoryModalShown = false;

      //filter tasks and delete selected one
      this.items = this.items.filter((item) => item !== this.categoryToDelete);

      this.saveIntoLocalStorage(this.items);
    },

    deny() {
      //modal flag to closed
      this.isDeleteTaskModalShown = false;
      this.isDeleteCategoryModalShown = false;
      this.isCreateCategoryModalShown = false;
      this.isCreateTaskModalShown = false;

      //clear data
      this.categoryWhereDelete = null;
      this.taskToDelete = null;
      this.categoryToDelete = null;
      this.categoryToModify = null;
    },

    //save all progress to Web Local Storage
    saveIntoLocalStorage(value) {
      localStorage.setItem("items", JSON.stringify(value));
    },
  },
};
</script>

<style lang="scss" scoped>
.categories {
  padding: 0 0 60px 0;
  button.new-category {
    position: fixed;
    left: calc(50%);
    bottom: 20px;
    transform: translateX(-50%);
    z-index: 10;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.589);
    width: 210px;
  }
}
</style>