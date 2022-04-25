<template>
  <q-dialog v-bind="$attrs" persistent transition-show="scale" transition-hide="scale" full-width>
    <q-card class="text-white modal-encryption column justify-between relative-position">
      <q-card-section>
        <div class="text-h6">Digite o texto</div>
        <div class="q-mt-lg" v-if="normalMode">
          <div>
            <p>Texto normal</p>
            <q-input class="modal-encryption__input" v-model="normalText" filled type="textarea" />
          </div>
          <div class="text-center q-my-sm">
             <q-btn flat icon="sync_alt" @click="invertMode" />
          </div>
          <div>
            <div class="row justify-between">
              <p>Texto encryptado</p>
              <q-btn v-if="encryptText" flat icon="copy_all" @click="copyToClipboard" />
            </div>
            <q-input :disable="normalMode" class="modal-encryption__input" v-model="encryptText" filled type="textarea" />
          </div>
        </div>

        <div v-else class="q-mt-lg">
          <div>
            <p>Texto encryptado</p>
            <q-input class="modal-encryption__input" v-model="encryptText" filled type="textarea" />
          </div>
          <div class="text-center q-my-sm">
             <q-btn flat icon="sync_alt" @click="invertMode" />
          </div>
          <div>
            <div class="row justify-between">
              <p>Texto normal</p>
              <q-btn v-if="normalText" flat icon="copy_all" @click="copyToClipboard" />
            </div>
            <q-input :disable="!normalMode" class="modal-encryption__input" v-model="normalText" filled type="textarea" />
          </div>
        </div>

        <q-btn class="q-px-xl q-my-sm modal-encryption__main-button" color="secondary" text-color="white" :label="encryptionTextButton" @click="execute" />
      </q-card-section>

      <q-card-actions align="right" class="bg-white text-teal">
        <q-btn flat label="Fechar" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
import { copyToClipboard } from 'quasar'

export default {
  data() {
    return {
      normalText: '',
      encryptText: '',
      normalMode: true,
      othersValues: {
        valueN: 0,
        valueZ: 0,
        valueE: 0
      }
    }
  },

  props: {
    values: {
      type: Object,
      default: () => ({})
    }
  },

  computed: {
    encryptionTextButton () {
      return this.normalMode ? 'Encriptar' : 'Desencriptar'
    }
  },

  methods: {
    execute () {
      this.normalMode ? this.setEncryptText() : this.setNormalText()
    },

    copyToClipboard () {
      copyToClipboard(this.normalMode ? this.encryptText : this.normalText)
        .then(() => {
          this.$q.notify({
            message: 'Texto copiado com sucesso!',
            type: 'positive'
          })
        })
        .catch(() => {
          this.$q.notify({
            message: 'Erro ao copiar o texto',
            type: 'negative'
          })
        })
    },

    setEncryptText () {
      const { valueP, valueQ, valueD } = this.values

      let text = [...this.normalText]
      let asciiCode = []

      text.forEach(number => asciiCode.push(number.charCodeAt()))

      this.othersValues.valueN = valueP * valueQ
      this.othersValues.valueZ = (valueP - 1) * (valueQ - 1)

      for(let i = 0; i <= 1000; i++) {
        if((i * valueD) % this.othersValues.valueZ === 1) {
          this.othersValues.valueE = i
          break;
        }
      }

      const { valueE, valueN } = this.othersValues

      let encryptArray = []

      asciiCode.forEach(original => {
        const originalText = BigInt(original)
        const textE = BigInt(valueE)

        const teste = (originalText ** textE) % BigInt(valueN)

        encryptArray.push(String.fromCharCode(parseFloat(teste)))
      })


      this.clearFields()
      encryptArray.forEach(teste => this.encryptText += teste.toString() )
    },

    setNormalText () {
      const { valueP, valueQ, valueD } = this.values

      let text = [...this.encryptText]
      let asciiCode = []

      this.othersValues.valueN = valueP * valueQ
      this.othersValues.valueZ = (valueP - 1) * (valueQ - 1)

      for(let i = 0; i <= 1000; i++) {
        if((i * valueD) % this.othersValues.valueZ === 1) {
          this.othersValues.valueE = i
          break;
        }
      }

      const { valueN } = this.othersValues

      text.forEach(number => asciiCode.push(number.charCodeAt()))

      let cornoDoLeo = []

      asciiCode.forEach(decimal => {
        const decimalText = BigInt(decimal)
        const textD = BigInt(valueD)

        const teste = (decimalText ** textD) % BigInt(valueN)

        cornoDoLeo.push(String.fromCharCode(parseFloat(teste)))
      })

      this.clearFields()
      cornoDoLeo.forEach(teste => this.normalText += teste.toString() )

    },

    invertMode () {
      this.normalMode = !this.normalMode
    },

    clearFields () {
      this.normalMode ? this.encryptText = '' : this.normalText = ''
    }
  }
}
</script>

<style lang="scss">
.modal-encryption {
  max-width: 120px;
  height: 600px;
  background: $dark-gradient;

  &__input textarea {
    color: $white;
  }

  &__main-button {
    position: absolute;
    bottom: -40px;
    left: 50%;
    transform: translateX(-50%);
  }
}
</style>

