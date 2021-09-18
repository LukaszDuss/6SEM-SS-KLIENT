<template>
  <div class="flex justify-center bg-gray-100 items-center mt-52">
    <div class="w-full max-w-xs">
      <div
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
          Log in to your account
        </div>

        <div class="identity-input my-4">
          <label
            for="identity"
            class="block text-gray-600 text-sm font-bold mb-2"
          >
            Email</label
          >
          <input
            id="identity"
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
            type="text"
            placeholder="Email"
            aria-describedby="emailHelp"
            v-model="userInfo.email"
          />
          <span class="text-xs text-red-700" id="emailHelp"></span>
        </div>

        <div class="password-input mb-6">
          <label
            for="identity"
            class="block text-gray-600 text-sm font-bold mb-2"
            >Password</label
          >

          <input
            aria-describedby="passwordHelp"
            v-model="userInfo.password"
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
            id="password"
            type="password"
            placeholder="*******"
          />

          <span v-if="badCreds" class="text-xs text-red-700" id="passwordHelp"
            >Wrong user or password</span
          >
        </div>

        <div class="flex items-center justify-between">
          <button
            href="/tasks"
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
            @click="loginUser()"
            type="submit"
          >
            Log In
          </button>
          <a
            class="
              inline-block
              align-baseline
              font-bold
              text-sm text-green-500
              hover:text-green-800
            "
            href="/register"
          >
            Create account
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'LoginForm',
  data() {
    return {
      badCreds: false,
      userInfo: {
        email: '',
        password: '',
      },
    }
  },
  methods: {
    async loginUser() {
      axios
        .post('http://localhost:3002/user/login', {
          data: {
            name: this.userInfo.email,
            pass: this.userInfo.password,
          },
        })
        .catch((error) => {
          console.log(error)
          this.errored = true
        })
        .then((response) => {
          if (response.status != 201) {
            this.badCreds = true
          } else {
            localStorage.token = response.data
            this.$router.push('/tasks')
          }
        })
    },
  },
}
</script>