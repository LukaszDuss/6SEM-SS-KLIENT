<template>
  <div class="p-4">
    <!-- ERROR MSG -->
    <div v-if="errored" class=""><Error /></div>

    <!-- TASK LIST -->
    <div v-else class="w-full">
      <!-- LOADING MSG -->
      <div class="h-screen" v-if="loading"><Loading /></div>
      <div v-else>
        <ul>
          <li
            v-for="(task, index) in tasks"
            :index="index"
            :key="task.id"
            class=""
          >
            <!-- status -->
            <div
              class="
                my-4
                w-auto
                flex
                bg-gray-100
                rounded-xl
                items-center
                shadow-md
                border-l-8 border-t
                justify-between
                p-2
              "
              v-bind:class="{
                'border-green-500 ': task.status,
                'border-red-500': !task.status,
              }"
            >
              <button
                class="flex items-center py-2 focus:outline-none"
                @submit.prevent
                @click="updateStatus(index, task)"
              >
                <Zondicon
                  v-if="task.status"
                  icon="checkmark-outline"
                  class="flex w-5 h-5 text-green-800 fill-current"
                />

                <Zondicon
                  v-else
                  icon="close-outline"
                  class="flex w-5 h-5 text-red-800 fill-current"
                />
              </button>
              <!-- task -->
              <input
                v-if="task.edit"
                type="text"
                v-model="task.task"
                class="px-2 flex font-bold w-3/5"
              />
              <div v-else class="px-2 flex font-bold w-3/5">
                {{ task.task }}
              </div>
              <div class="flex flex-row">
                <a
                  class="flex items-center px-1 focus:outline-none"
                  @click="updateTask(index, task)"
                  @submit.prevent
                >
                  <Zondicon
                    v-if="task.edit"
                    icon="checkmark"
                    class="flex w-4 h-4 fill-current"
                  />
                  <div v-else class="flex w-4 h-4"></div>
                </a>
                <a
                  class="flex items-center px-1 focus:outline-none"
                  @click="editTask(index)"
                  @submit.prevent
                >
                  <Zondicon
                    v-if="task.edit"
                    icon="block"
                    class="flex w-4 h-4 fill-current"
                  />
                  <Zondicon
                    v-else
                    icon="compose"
                    class="flex w-4 h-4 fill-current"
                  />
                </a>
                <button
                  class="flex items-center py-2 focus:outline-none"
                  @submit.prevent
                  @click="deleteTask(index, task.id)"
                >
                  <Zondicon icon="trash" class="flex w-4 h-4 fill-current" />
                </button>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import Zondicon from 'vue-zondicons'
import axios from 'axios'
export default {
  data() {
    return {
      tasks: [],
      loading: true,
      errored: false,
    }
  },
  async mounted() {
    if(localStorage.token){

      axios
        .get('http://localhost:3002/tasks', {
          headers: {
            'sessionToken': localStorage.token,
          },
        })
        .then((response) => {
          this.tasks = response.data
          this.tempTasks = response.data
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .finally(() => {
          this.loading = false
        })
    } else {
      this.errored = true
    }
  },
  methods: {
    async deleteTask(index, id) {
      axios
        .delete('http://localhost:3002/tasks', {
          data: {
            id: id,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then((response) => {
          this.$delete(this.tasks, index)
        })
    },

    async updateStatus(index, task) {
      let id = task.id
      let status = !task.status
      axios
        .put('http://localhost:3002/tasks/status', {
          data: {
            id: id,
            status: status,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then(() => (this.tasks[index].status = !task.status))
    },

    async updateTask(index, task) {
      let id = task.id
      let newTask = task.task
      axios
        .put('http://localhost:3002/tasks', {
          data: {
            id: id,
            task: newTask,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then(() => (this.tempTasks[index].edit = false))
    },

    editTask: function (index) {
      let oryg = this.tasks[index].task
      this.tasks[index].edit = !this.tasks[index].edit
      this.tasks[index].task = oryg
    },
  },
  components: {
    Zondicon,
  },
}
</script>
