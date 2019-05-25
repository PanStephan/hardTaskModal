<template>
  <div class="wrapper">
    <div class="wrapper-content">

      <section>
        <div class="container">

          <!-- modal with validate -->
          <button class="btn btnPrimary" @click="modalValidate = !modalValidate">Show modal with form + validate</button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false"/>

          <modalCreate v-show="modalCreate" @openCreateWindow="openCreateWindow"/>

        </div>
      </section>


    </div>
  </div>
</template>

<script>
import modals from '@/components/UI/Modal.vue'
import modalValidate from '@/components/ModalValidate.vue'
import modalCreate from '@/components/ModalCreate.vue'

export default {
  components: { modals, modalValidate, modalCreate },
  data () {
    return {
      modalValidate: false,
      modalCreate: false
    }
  },
  methods: {
    openCreateWindow () {
      this.$emit('openCreateWindow', this.modalCreate)
    }
  }
}
</script>
