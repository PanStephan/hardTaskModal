<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <!-- modal with validate -->
          <button class="btn btnPrimary" @click="login = !login">log in</button>
          <login v-show="login" @close="login = false" @openCreateWindow="openCreateWindow"/>

          <registration v-show="registration" @close="registration = false"/>

        </div>
      </section>


    </div>
  </div>
</template>

<script>
import modals from '@/components/UI/Modal.vue'
import login from '@/components/Login.vue'
import registration from '@/components/Registration.vue'

export default {
  components: { modals, login, registration },
  data () {
    return {
      login: false,
      registration: false
    }
  },
  methods: {
    openCreateWindow () {
      this.login = false,
      this.registration = !this.registration
    }
  }
}
</script>
