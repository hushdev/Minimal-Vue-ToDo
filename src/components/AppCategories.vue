<template>
  <div class="categories">
    <!-- MODAL FOR TASKS -->
    <transition name="fade">
      <app-delete-modal v-if="isDeleteTaskModalShown" @accept="deleteTask" @deny="deny">
        <h2 class="fs-6 text-primary">Вы уверены что хотите удалить задачу из списка?</h2>
      </app-delete-modal>

      <!-- MODAL FOR CATEGORIES -->
      <app-delete-modal v-if="isDeleteCategoryModalShown" @accept="deleteCategory" @deny="deny">
        <h2 class="fs-6 text-primary">Вы уверены что хотите удалить категорию?</h2>
      </app-delete-modal>

      <app-create-modal
        v-if="isCreateCategoryModalShown"
        @accept="createCategory"
        @deny="deny"
      >категории</app-create-modal>

      <app-create-modal v-if="isCreateTaskModalShown" @accept="createTask" @deny="deny">задачи</app-create-modal>
    </transition>

    <!-- NEW CATEGORY BUTTON -->
    <app-btn
      v-if="items.length < 3"
      @click.native="isCreateCategoryModalShown = true"
      text="+ новая категория"
      primary
      class="new-category"
    />

    <!-- TEXT FOR CASE OF EMPTY DATA -->
    <p
      v-if="items.length < 1"
      class="welcome-text fs-5 text-secondary"
    >Список пуст, добавьте категории и задачи...</p>

    <div v-if="items.length" class="row">
      <!-- CARD WITH CATEGORIES -->
      <div v-for="(item, idx) in items" :key="idx" class="card">
        <div class="card-heading">
          <span class="t-primary fs-5 fw-r">{{item.category}}</span>
          <div @click="openDeleteCategoryModal(item)" class="delete">
            <img src="@/assets/images/close_big.png" alt="Удалить задачу" />
          </div>
        </div>
        <div class="card-body">
          <!-- TASKS -->
          <span v-if="!item.tasks.length">Задач пока нет</span>
          <ul>
            <li v-for="(task, idx2) in item.tasks" :key="idx2">
              <div @click="task.completed = !task.completed">
                <div class="checkbox b-primary" :class="{'checkbox-completed': task.completed}"></div>
                <span class="text" :class="{'text-completed': task.completed}">{{task.name}}</span>
              </div>
              <div @click="openDeleteTaskModal(items[idx], items[idx].tasks[idx2])" class="delete">
                <img src="@/assets/images/close.png" alt="Удалить задачу" />
              </div>
            </li>
          </ul>

          <!-- ADDING NEW TASK -->
          <div class="card-actions">
            <app-btn @click.native="openCreateTaskModal(item)" text="+ новая задача" primary />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppDeleteModal from "@/components/modals/AppDeleteModal";
import AppCreateModal from "@/components/modals/AppCreateModal";
import AppBtn from "@/components/AppBtn";

export default {
  components: { AppBtn, AppDeleteModal, AppCreateModal },
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

      //clear data
      this.categoryWhereDelete = null;
      this.taskToDelete = null;
    },

    deleteCategory() {
      //change modal flag to closed
      this.isDeleteCategoryModalShown = false;

      //filter tasks and delete selected one
      this.items = this.items.filter((item) => item !== this.categoryToDelete);
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
    box-shadow: 0 0 3px #6c6c6c;
    width: 210px;
  }
  .row {
    display: flex;
    justify-content: flex-start;
    align-items: stretch;
    flex-wrap: wrap;
    .card {
      max-width: 300px;
      width: 100%;
      border-radius: 5px;
      margin-top: 15px;
      margin-right: 15px;
      padding-bottom: 60px;
      position: relative;
      box-shadow: 0 3px 20px #919191;
      &-heading {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 15px 15px 0 15px;
        .delete {
          cursor: pointer;
          img {
            width: 18px;
            height: 18px;
            transform: translateX(4px);
          }
        }
      }

      &-body {
        padding: 15px;
        width: calc(100% - 30px);
        ul {
          list-style-type: none;
          li {
            display: flex;
            justify-content: space-between;
            margin-top: 15px;
            &:first-child {
              margin-top: 0;
            }
            .delete {
              cursor: pointer;
              img {
                width: 10px;
                height: 10px;
              }
            }
            div {
              display: flex;
              align-items: flex-start;
              cursor: pointer;
              .checkbox {
                display: block;
                min-width: 13px;
                min-height: 13px;
                background: transparent;
                border-radius: 50%;
                margin-right: 5px;
              }
              .checkbox-completed {
                background: #000000;
              }
              .text-completed {
                text-decoration: line-through;
              }
            }
          }
        }
      }
      &-actions {
        position: absolute;
        bottom: 15px;
        left: 15px;
      }
    }
  }
}

@media (max-width: 660px) {
  .categories {
    .row {
      flex-direction: column;
      .card {
        margin-right: 0;
        max-width: 100%;
      }
    }
  }
}
</style>