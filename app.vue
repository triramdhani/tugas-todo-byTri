<template>
  <div class="list-task">
    <header>
      <h1 class="text-3xl font-bold">Todo List</h1>
    </header>
    <div v-show="isEmpty">
      <p class="font-semibold">Belum Ada Task</p>
    </div>
    <div v-for="(task, index) in tasks" :key="index">
      <div
        class="item-task d-flex align-items-start border-bottom pt-3 pb-4"
        v-bind="key"
      >
        <input
          id="task"
          type="checkbox"
          name="status"
          class="me-2 mt-2"
          :checked="task.isDone"
          v-model="tasks[index].isDone"
        />
        <div class="d-flex flex-column">
          <div class="title-task mb-1" :class="[task.isDone ? 'line-through' : 'null']">
            {{ task.title }}
          </div>
          <div class="description-task small text-muted">
            {{ task.description }}
          </div>
          <div>
            <button @click="(e) => deleteTask(task)">Delete</button>
          </div>
        </div>
      </div>
    </div>

    <div class="action py-2">
      <a
        v-if="!isCreating"
        href="#"
        class="underline text-blue-500 cursor-pointer"
        @click="isCreating = !isCreating"
        >Add Task</a
      >
      <div v-else class="add-card">
        <div class="card mb-2">
          <div class="card-body d-flex flex-column p-0">
            <input
              v-model="titleValue"
              placeholder="Title"
              type="text"
            />
            <textarea
              v-model="descriptionValue"
              class="form-control  border-0 small"
              placeholder="Description"
              rows="3"
            ></textarea>
          </div>
        </div>
        <div class="button-wrapper d-flex">
          <button class="btn btn-primary me-2" @click="addTask">Save</button>
          <button
            class="btn btn-outline-secondary"
            @click="isCreating = !isCreating"
          >
            Cancel
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Daftar task
      tasks: [
        {
          title: "Task 1",
          description: "ini deskripsi",
          isDone: false,
        },
      ],
      isCreating: false,
      // state for handle form input
      titleValue: ``,
      descriptionValue: ``,
      isEmpty: true,
    };
  },
  mounted() {
    const tasks = this.tasks.length === 0;
    !tasks ? (this.isEmpty = false) : (this.tasks = true);
  },
  methods: {
    addTask() {
      // console.log(this.titleValue);
      const data = {
        title: this.titleValue,
        description: this.descriptionValue,
        isDone: false,
      };
      this.tasks.push(data);


      // hide input form && clear form
      this.isCreating = !this.isCreating
      this.titleValue = ``;
      this.descriptionValue = ``;
    },
    deleteTask(dltTask){
      // better to use id task
      let currentTask = this.tasks ;
      let newTasks = currentTask.filter(task => task.title !== dltTask.title)

      this.tasks = newTasks

    },
    async taskToEmpty(){
      this.isEmpty = true
    }
  },
  watch : {
    tasks(newValue , oldValue){
      if(newValue.length === 0 ){
        this.taskToEmpty()
      }
    }
  }
};
</script>
