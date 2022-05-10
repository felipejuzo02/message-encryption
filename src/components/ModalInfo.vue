<template>
  <q-dialog v-bind="$attrs" persistent transition-show="scale" transition-hide="scale">
    <q-card class="bg-secondary text-white modal-info column justify-between">
      <q-card-section>
        <div class="text-h6">Instruções</div>
        <div class="q-mt-lg">
          <p>- Os valores das chaves serão criados de forma aleatória</p>
          <p>- Você pode re sortear caso queira</p>
          <p>- Agora clique em executar e digite seu texto</p>
          <p>- Agora clique em executar</p>
          <p>- Digite seu texto (Normal ou encriptado)</p>
          <p>- Clique em Encriptar/Desencriptar</p>
          <p>- Você pode realizar a inversão caso necessário</p>
          <p>- Caso queira você pode copiar o texto, tanto normal quanto encriptado</p>
        </div>
      </q-card-section>

      <q-card-actions align="right" class="bg-white text-teal">
        <q-btn flat label="Entendi" v-close-popup />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<style lang="scss">
.modal-info {
  width: 500px;
  height: 500px;
}
</style>

