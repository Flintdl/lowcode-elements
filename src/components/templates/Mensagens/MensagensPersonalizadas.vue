<template>
  <div class="mensagem-box">
    <div
      class="mensagem"
      v-for="mensagem of mensagens"
      v-bind:key="mensagem"
    ></div>
  </div>
</template>

<script>
export default {
  props: {
    mensagens: Object,
  },
  methods: {
    messageBox(status, mensagemText) {
      // cria um novo elemento div
      // e dá à ele conteúdo
      var divNova = document.createElement("div");
      var boxMensagem = document.querySelector(".mensagem-box");
      // adiciona o novo elemento criado e seu conteúdo ao DOM

      if (status == "successRegister") {
        var conteudoNovo = document.createTextNode(mensagemText);
        divNova.classList.add("success");
      }

      if (status == "success") {
        conteudoNovo = document.createTextNode(mensagemText);
        divNova.classList.add("success");
      } else if (status == "error") {
        conteudoNovo = document.createTextNode(mensagemText);
        divNova.classList.add("error");
      } else if (status == "serverError") {
        conteudoNovo = document.createTextNode(mensagemText);
      }
      divNova.appendChild(conteudoNovo);
      if (!document.body.classList.contains("mensagem")) {
        document.body.appendChild(divNova);
      }
      divNova.classList.add("mensagem");
      boxMensagem.appendChild(divNova);
      setTimeout(function () {
        divNova.remove();
      }, 3000);
    },
  },
};
</script>

<style></style>
