<template>
  <modal
    title="Modal with form"
    @close="closeWindow">
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- email -->
        <div class="form-item" :class="{ errorInput: $v.email.$error }">
          <label>Email:</label>
          <p class="errorText" v-if="!$v.email.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.email.email"> Email is not correct!</p>
          <input
            v-model="email"
            :class="{ error: $v.email.$error }"
            @change="$v.email.$touch()">
        </div>

        <div class="form-item" :class="{ errorInput: $v.password.$error }">
          <label class="form__label">Password</label>
          <p class="errorText" v-if="!$v.password.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.password.password"> Password is not correct!</p>
          <input 
            type="password"
            v-model="password"
            :class="{ error: $v.password.$error }"
            @change="$v.password.$touch()">
        </div>

        <div class="form-item" :class="{ errorInput: $v.repeatPassword.$error }">
          <label class="form__label">Check Password</label>
          <p class="errorText" v-if="!$v.repeatPassword.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.repeatPassword.repeatPassword"> Password is not correct!</p>
          <p class="errorText" v-if="!$v.repeatPassword.sameAsPassword"> Passwords must be identical.</p>
          <input 
            type="password"
            v-model="repeatPassword"
            :class="{ error: $v.repeatPassword.$error }"
            @change="$v.repeatPassword.$touch()">
        </div>
        <!-- button -->
        <button class="btn btnPrimary" :disabled="$v.$invalid" >Submit!</button>
      </form>
    </div>
  </modal>
</template>

<script>
import { required, minLength, sameAs, email } from 'vuelidate/lib/validators'

import modal from '@/components/UI/Modal.vue'


export default {
  components: { modal },
  data () {
    return {
      email: '',
      password: '',
      repeatPassword: ''
    }
  },
  validations: {
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      required,
      minLength: minLength(6),
      sameAsPassword: sameAs('password')
    }
  },
  methods: {
    onSubmit () {
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password,
        }
        console.log(user)

        // DONE!
        this.repeatPassword = ''
        this.password = ''
        this.email = ''
        this.$v.$reset()
        this.$emit('close')
      }
    },
    closeWindow() {
      this.$emit('close')
      this.password = ''
      this.email = ''
      this.repeatPassword = ''
    }
  }
}
</script>

<style lang="scss">
.form-item .errorText {
  display: none;
  margin-bottom: 8px;
  font-size: 13.4px;
  color: #de4040;
}
.form-item {
  &.errorInput .errorText {
    display: block;
  }
}
input.error {
  border-color: #de4040;
}

button:disabled {
  opacity: 0.5;
}

</style>
