<template>
  <div class="sf-col-12 sf-form-group">
    <sup>*Alterações são feitas ao sair do campo</sup> <br />
    <label class="sf-label"> Inserir Texto </label>
    <div v-if="textLabel">
      <TextoCustomizacao
        class="sf-display-flex sf-align-items-center sf-justify-content-between sf-text-preview sf-border-2 sf-border-radius sf-border-dashed sf-px-3 sf-py-2 sf-form-group"
        v-for="(texto, i) of textLabel"
        :textoDescricao="texto.texto"
        :textoID="String(texto.id)"
        v-on:callbackEditar="editarTexto"
        v-on:callbackConfigurar="configuraTexto"
        v-on:callbackRemover="removeTexto"
        v-on:compartilhaTexto="compartilhaTexto"
        :componentText="componentText"
        :id="texto.id"
        v-bind:key="i"
      />
    </div>
    <hr class="sf-mb-4" />
    <!-- v-model="valor" -->
    <div class="sf-row">
      <div class="sf-col">
        <input
          type="text"
          ref="novoTexto"
          class="sf-text-capitalize"
          @keyup.enter="insertTexto"
          placeholder="Novo Texto"
        />
      </div>
      <div class="sf-col-auto sf-display-flex sf-justify-content-right">
        <button
          ref="botaoInsere"
          class="sf-btn sf-btn-primary sf-ml-auto"
          @click="insertTexto"
        >
          Novo Texto
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import TextoCustomizacao from "@/components/templates/Dialog/InputsDialog/TextoCustomizacao.vue";

export default {
  data: function () {
    return {
      textLabel: [],
      componentText: null,
      componentTextID: null,
      textoDescrito: null,
    };
  },
  props: {
    valorInput: Array,
  },
  components: {
    TextoCustomizacao,
  },
  beforeMount() {
    if (localStorage.getItem("textLabel")) {
      this.textLabel = JSON.parse(localStorage.getItem("textLabel"));
    }
  },
  methods: {
    insertTexto() {
      var date = new Date().getTime();
      var novoTexto = { id: date, texto: this.$refs.novoTexto.value };
      this.textLabel.push(novoTexto);
      this.$refs.novoTexto.value = "";
      this.$refs.novoTexto.focus();
      localStorage.setItem("textLabel", JSON.stringify(this.textLabel));
    },
    editarTexto(target) {
      var objIndex = this.textLabel.findIndex(
        (obj) => obj.id == target.parentElement.id
      );
      this.textLabel[objIndex].texto = target.innerText;
      localStorage.setItem("textLabel", JSON.stringify(this.textLabel));
      var editaTexto = {
        id: target.parentElement.id,
        texto: target.innerText,
      };
      this.$emit("callbackTextoEditaBloco", editaTexto);
    },
    removeTexto(target) {
      console.log(target);
      this.textLabel = this.textLabel.filter(function (returnableObjects) {
        return (
          returnableObjects.id !=
          target.parentElement.parentElement.getAttribute("id")
        );
      });
      localStorage.setItem("textLabel", JSON.stringify(this.textLabel));
      var removeTexto = {
        id: target.parentElement.parentElement.getAttribute("id"),
        texto: target.parentElement.parentElement.innerText,
      };
      this.$emit("callbackTextoRemoveBloco", removeTexto);
    },
    compartilhaTexto(target) {
      var novoTexto = {
        id: target.parentElement.parentElement.getAttribute("id"),
        texto: target.parentElement.parentElement.innerText,
      };
      this.$emit("callbackTextoInsere", novoTexto);
    },
    configuraTexto(target) {
      this.componentText = "DialogTextConfiguracao";
      this.componentTextID = target.parentElement.parentElement.id;
      this.textoDescrito = target.parentElement.parentElement.innerText;
    },
  },
  computed: {
    valor: {
      get() {
        return this.valorInput;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
};
</script>

<style></style>
