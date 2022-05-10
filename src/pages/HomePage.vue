<template>
  <q-page class="home-page">
    <div class="text-center text-white q-py-xl">
      <h1 class="q-my-none home-page__title">MESSAGE ENCRYPTION</h1>
      <p class="q-mt-none home-page__subtitle text-grey-6">Encriptar e Desencriptar Mensagens RSA</p>
    </div>

    <div class="container relative-position">
      <div class="flex justify-between q-pb-lg">
        <q-input class="home-page__input" disable color="primary" label-color="white" filled v-model="values.valueP" label="Valor P" type="number">
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
        <q-input class="home-page__input" disable color="primary" label-color="white" filled v-model="values.valueQ" label="Valor Q" type="number" >
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
        <q-input class="home-page__input" disable color="primary" label-color="white" filled v-model="values.valueD" label="Valor D" type="number" >
          <template v-slot:prepend>
            <q-icon name="confirmation_number" color="white" />
          </template>
        </q-input>
      </div>
      <div class="text-center home-page__container-buttons column q-mt-xl">
        <q-btn class="q-px-xl q-mb-md" color="primary" outline  label="Gerar novamente" @click="setValues" />
        <q-btn class="q-px-xl" color="secondary" text-color="white" label="Executar" @click="openEncryptionModal" />
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

const primeNumber = [3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73 , 79, 83, 89, 97];

export default {
  data() {
    return {
      infoModal: false,
      encryptionModal: false,
      values: {
        valueP: 0,
        valueQ: 0,
        valueD: 0
      }
    }
  },

  components: {
    ModalInfo,
    EncryptionModal
  },

  created () {
    this.setValues()
  },

  methods: {
    openInfoModal () {
      this.infoModal = true
    },

    openEncryptionModal () {
      this.encryptionModal = true
    },

    generateRandomValue () {
      const position = Math.floor(Math.random() * (primeNumber.length - 4)) + 4

      return primeNumber[position]
    },

    generateLimitedValue () {
      const position = Math.floor(Math.random() * (8))

      return primeNumber[position]
    },

    calculateMDC (num1, num2) {
      let resto;

      do {
          resto = num1 % num2;

          num1 = num2;
          num2 = resto;

      } while (resto != 0);

      return num1;
    },

    setValues () {
      this.values.valueP = this.generateRandomValue()
      this.values.valueQ = this.generateRandomValue()

      do {
        this.values.valueQ = this.generateRandomValue()
      } while(this.values.valueP === this.values.valueQ)

      const num1 = (this.values.valueP - 1) * (this.values.valueQ - 1)

      let controller = true
      do {
        const num2 = this.generateLimitedValue()
        const mdc = this.calculateMDC(num1, num2)

        if(mdc === 1) {
          if(num1 > num2) {
            controller = false
            this.values.valueD = num2
          }
        }
      } while (controller)
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

  &__container-buttons {
    max-width: 280px;
    margin: 0 auto;
  }

  &__information-button {
    position: absolute;
    bottom: 12px;
    right: 12px;
    border-radius: 16px 0 16px 0;
  }
}
</style>
