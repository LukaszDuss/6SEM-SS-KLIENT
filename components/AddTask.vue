<template>
  <div class=" w-full justify-center items-center">
    <div v-if="errored" class="">
      <Error />
    </div>
    <div v-else class="w-full justify-center mt-52 ">
      <div class=" justify-center items-center w-full  px-8">
        <form
          class="
            bg-white
            shadow-md
            rounded-lg
            px-8
            pt-6
            pb-8
            mb-4
            border-t-4 border-green-400
          "
          @submit.prevent
        >
          <div class="w-full justify-center font-bold text-gray-600 ">
            Create new task
          </div>

          <div class="identity-input my-4 ">
            <input
              class="
                shadow
                appearance-none
                rounded-lg
                w-full
                py-2
                px-3
                text-gray-700
                leading-tight
                focus:outline-none
                focus:shadow-outline
              "
              type="text"
              placeholder="Task name..."
              v-model="task"
            />
            <span v-if="success" class="text-xs text-green-500 w-full  mb-3  "
              >Task added!</span
            >
          </div>

          <div class=" items-center justify-center">
            <button
              class="
                bg-green-600
                hover:bg-black
                text-white
                font-bold
                py-2
                px-4
                rounded-lg
                focus:outline-none
                focus:shadow-outline
              "
              type="submit"
              @click="addTask()"
            >
              Add task
            </button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

 <script>
import axios from 'axios'
export default {
  data() {
    return {
      task: null,
      success: false,
      loading: false,
      errored: false,
    }
  },
  methods: {
    async addTask() {
      if (this.task) {
        axios
          .post('http://localhost:3002/tasks', {
            data: {
              task: this.task,
              status: false,
            },
          })
          .catch((error) => {
            console.log(error)
            this.errored = true
          })
          .finally((response) => {
            this.success = true
          })
      }
    },
  },
}
</script>
 
