<template>
  <q-page class="home-page">
    <div class="text-center text-white q-py-xl">
      <h1 class="q-my-none home-page__title">MESSAGE ENCRYPTION</h1>
      <p class="q-mt-none home-page__subtitle text-grey-6">Encriptar e Desencriptar Mensagens RSA</p>
    </div>

    <div class="container relative-position">
      <div class="flex justify-between q-pb-lg">
        <q-input class="home-page__input" color="primary" label-color="white" filled v-model="values.valueP" label="Valor P" type="number">
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
        <q-input class="home-page__input" color="primary" label-color="white" filled v-model="values.valueQ" label="Valor Q" type="number">
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
        <q-input class="home-page__input" color="primary" label-color="white" filled v-model="values.valueD" label="Valor D" type="number">
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
      </div>
      <div class="full-width text-center q-mt-xl">
        <q-btn :disable="hasAllValues" class="q-px-xl" color="secondary" text-color="white" label="Executar" @click="openEncryptionModal" />
      </div>
    </div>

    <q-btn class="home-page__information-button" outline size="12px" icon-right="info" color="primary" label="Instruções" @click="openInfoModal" />
  </q-page>

  <encryption-modal v-model="encryptionModal" :values="values" />
  <modal-info v-model="infoModal" />
</template>

<script>
import ModalInfo from '../components/ModalInfo.vue'
import EncryptionModal from '../components/EncryptionModal.vue'

export default {
  data() {
    return {
      infoModal: false,
      encryptionModal: false,
      values: {
        valueP: 17,
        valueQ: 11,
        valueD: 7
      }
    }
  },

  components: {
    ModalInfo,
    EncryptionModal
  },

  computed: {
    hasAllValues () {
      let hasValues = false

      Object.values(this.values).forEach((el) => {
        hasValues = !el
      })

      return hasValues
    }
  },

  methods: {
    openInfoModal () {
      this.infoModal = true
    },

    openEncryptionModal () {
      this.encryptionModal = true
    }
  }
}
</script>

<style lang="scss">
.home-page {
  &__title {
    font-family: 'Bebas Neue', cursive, 'Cairo';
    font-size: 4rem;
  }

  &__subtitle {
    font-family: 'Bebas Neue', cursive, 'Cairo';
    font-size: 1.3rem;
    margin-top: -20px;
    letter-spacing: 0.15rem;
  }

  &__input input {
    color: $white;
  }

  &__information-button {
    position: absolute;
    bottom: 12px;
    right: 12px;
    border-radius: 16px 0 16px 0;
  }
}
</style>
