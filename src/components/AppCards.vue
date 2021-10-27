<template>
  <div class="row">
    <!-- V-FOR CARDS FROM PROPS -->
    <div v-for="(item, idx) in items" :key="idx" class="card bg-white">
      <div class="card-heading">
        <span class="t-primary fs-5 fw-r">{{item.category}}</span>
        <div @click="$emit('deleteCategory', item)" class="delete">
          <img src="@/assets/images/close.png" alt="Удалить задачу" />
        </div>
      </div>
      <div class="card-body">
        <!-- TASKS -->
        <span v-if="!item.tasks.length" class="fs-4">Задач пока нет</span>
        <ul>
          <li v-for="(task, idx2) in item.tasks" :key="idx2">
            <div @click="task.completed = !task.completed; $emit('save', items)">
              <div class="checkbox b-primary" :class="{'checkbox-completed': task.completed}"></div>
              <span class="text fs-4" :class="{'text-completed': task.completed}">{{task.name}}</span>
            </div>
            <div @click="$emit('deleteTask',items[idx], items[idx].tasks[idx2]);" class="delete">—</div>
          </li>
        </ul>

        <!-- ADDING NEW TASK -->
        <div class="card-actions">
          <app-btn @click.native="$emit('createTask', item)" text="+ новая задача" primary />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppBtn from "@/components/AppBtn";

export default {
  components: { AppBtn },
  props: {
    items: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
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
    padding-bottom: 90px;
    position: relative;
    box-shadow: 0 2px 20px rgba(0, 0, 0, 0.288);

    &-heading {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding: 15px 15px 0 15px;
      .delete {
        cursor: pointer;
        img {
          width: 14px;
          height: 14px;
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
            margin-left: 10px;
            font-size: 18px;
          }

          .text {
            word-break: break-all;
          }
          div {
            display: flex;
            align-items: center;
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

@media (max-width: 660px) {
  .row {
    .card {
      margin-right: 0;
      max-width: 100%;
    }
  }
}
</style>