<template>
  <modal
    title="Modal with form + Validate"
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
        <!-- button -->
        <button class="btn btnPrimary">Submit!</button>
      </form>
        <button @click="$emit('openCreateWindow')">I need account</button>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'

import modal from '@/components/UI/Modal.vue'


export default {
  components: { modal },
  data () {
    return {
      email: '',
      password: ''
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
  },
  methods: {
    onSubmit () {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          password: this.password,
        }
        console.log(user)

        // DONE!
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
    },    
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

</style>
