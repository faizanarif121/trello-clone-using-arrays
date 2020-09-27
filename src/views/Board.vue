<template>
  <div class="cards">
    <card v-for="(task, index) in tasks" :key="index">
      <template slot="header">
        <div class="header">
          <editable-input
            :key="index"
            :value="task.title"
            :index="index"
            @save-changes="saveTitle"
            :showDeleteAction="false"
          />
          <button @click="removeTask(index)">
            <svg fill="none" width="16" height="16">
              <use :xlink:href="'#delete-icon'" />
            </svg>
          </button>
        </div>
      </template>
      <template slot="tasks">
        <div class="container">
          <editable-input
            v-for="(childTask, key) in task.childTasks"
            :key="index + key"
            :value="childTask"
            :index="key"
            :parentId="index"
            @save-changes="saveSubtask"
            @remove-item="removeSubtask"
          />
          <button class="add-task-btn" @click="addNewTask(index)">
            Add new Task
          </button>
        </div>
      </template>
    </card>
    <button class="add-card-btn" @click="addNewCard()">Add new Card</button>
  </div>
</template>

<script>
export default {
  name: 'Board',
  data: () => ({
    tasks: [
      {
        title: 'First task',
        childTasks: ['first', 'second', 'third'],
      },
      {
        title: 'Second task',
        childTasks: ['first', 'second'],
      },
      {
        title: 'Third task',
        childTasks: ['One'],
      },
    ],
  }),
  methods: {
    addNewCard() {
      this.tasks.push({
        title: 'Add title',
        childTasks: ['Add Subtask'],
      })
    },
    addNewTask(taskIndex) {
      const isExist = this.tasks.indexOf(taskIndex)
      if (isExist) {
        this.tasks[taskIndex].childTasks.push('Subtask')
      }
    },
    saveTitle(payload) {
      let newArray = [...this.tasks]
      newArray[payload.childId].title = payload.value
      this.tasks = [...newArray]
    },
    saveSubtask(payload) {
      let newArray = [...this.tasks]
      newArray[payload.parentId].childTasks[payload.childId] = payload.value
      this.tasks = [...newArray]
    },
    removeTask(index) {
      let newArray = [...this.tasks]
      newArray.splice(index, 1)
      this.tasks = [...newArray]
    },
    removeSubtask(payload) {
      let newArray = [...this.tasks]
      newArray[payload.parentId].childTasks.splice(payload.childId, 1)
      this.tasks = [...newArray]
    },
  },
  components: {
    Card: () => import('@/components/Card.vue'),
    EditableInput: () => import('@/components/EditableInput.vue'),
  },
}
</script>
<style lang="css" scoped>
.cards {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
}
@media (max-width: 768px) {
  .cards {
    grid-template-columns: 2fr;
  }
}
@media (max-width: 575px) {
  .cards {
    grid-template-columns: 1fr;
  }
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: baseline;
}
.add-task-btn,
.add-card-btn {
  color: #cdd1c4;
  height: 40px;
  border-radius: 5px;
  margin-top: 5px;
  padding: 5px;
  background-color: #30323d;
  cursor: pointer;
}
.add-task-btn {
  width: 100%;
}
.add-card-btn {
  margin-top: 0;
}
</style>
