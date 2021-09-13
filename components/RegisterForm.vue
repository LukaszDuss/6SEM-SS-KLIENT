<template>
  <div class="flex justify-center items-center mt-52">
    <div class="w-full max-w-xs">
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
        <div class="w-full flex justify-center font-bold text-gray-600">
          Create your account
        </div>

        <div class="identity-input my-4">
          <label
            for="identity"
            class="block text-gray-600 text-sm font-bold mb-2"
          >
            Email</label
          >
          <input
            class="
              shadow
              appearance-none
              rounded-lg
              w-full
              py-2
              px-3
              text-gray-700
              mb-3
              leading-tight
              focus:outline-none
              focus:shadow-outline
            "
            type="text"
            required
            placeholder="Email"
            v-model="email"
          />
          <span class="text-xs text-red-700"></span>
        </div>

        <div class="password-input mb-6">
          <label class="block text-gray-600 text-sm font-bold mb-2"
            >Password</label
          >

          <input
            aria-describedby="passwordHelp"
            v-model="password"
            class="
              shadow
              appearance-none
              rounded-lg
              w-full
              py-2
              px-3
              text-gray-700
              mb-3
              leading-tight
              focus:outline-none
              focus:shadow-outline
            "
            required
            type="password"
            placeholder="*******"
          />

          <span class="text-xs text-green-500" v-if="success"> User created</span>
          <span class="text-xs text-red-500" v-if="exist">
            Already a user
          </span>
          <span class="text-xs text-red-500" v-if="blank">
            Password can't be empty!
          </span>
        </div>

        <div class="flex items-center justify-between">
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
            @click="getUser()"
          >
            Sign Up
          </button>
          <a
            class="
              inline-block
              align-baseline
              font-bold
              text-sm text-green-500
              hover:text-green-800
            "
            href="/login"
          >
            Already a user?
          </a>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'RegisterForm',
  data() {
    return {
      success: false,
      exist: false,
      blank: false,
      email: '',
      password: '',
    }
  },
  methods: {
    async getUser() {
      this.exist = false
      this.blank = false
      this.success = false
      axios
        .post('http://localhost:3002/user', {
          data: {
            name: this.email,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then((response) => {
          if (response.data) {
            this.exist = true
          } else {
            if (this.password) {
              this.createUser()
            }
          }
        })
    },

    async createUser() {
      axios
        .post('http://localhost:3002/user/create', {
          data: {
            name: this.email,
            pass: this.password,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then(() => {
          this.success = true
        })
    },
  },
}
</script>