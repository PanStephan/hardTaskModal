<template>
  <modal
    title="Modal with form + Validate"
    @close="closeWindow">
    <!-- body -->
    <div slot="body">
      <form @submit.prevent="onSubmit">

        <!-- name -->
        <div class="form-item" :class="{ errorInput: $v.name.$error }">
          <label>Name:</label>
          <p class="errorText" v-if="!$v.name.required"> Filed is required! </p>
          <p class="errorText" v-if="!$v.name.minLength"> Name must have at least {{ $v.name.$params.minLength.min }} !</p>
          <input
            v-model="name"
            :class="{ error: $v.name.$error }"
            @change="$v.name.$touch()">
        </div>

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
        <button @click="openCreateWindow" >I need account</button>
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
      name: '',
      email: '',
      password: '',
      
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
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
          name: this.name,
          email: this.email,
          password: this.password,
        }
        console.log(user)

        // DONE!
        this.password = ''
        this.name = ''
        this.email = ''
        this.$v.$reset()
        this.$emit('close')
      }
    },
    closeWindow() {
      this.$emit('close')
      this.password = ''
      this.name = ''
      this.email = ''
    },    
    openCreateWindow () {
      this.$emit('close')
      this.modalCreate = !this.modalCreate
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

</style>
