<template>
  <div>
    <component
      v-bind:is="componentText"
      :idBlocoTexto="textoID"
      :textoDescrito="textoDescricao"
    />
    <p
      @blur="editarTexto"
      class="sf-m-0 sf-text-truncate sf-width-100"
      contenteditable="true"
    >
      {{ textoDescricao }}
    </p>
    <div class="sf-display-flex sf-align-items-center">
      <i
        @click="configuraTexto"
        contenteditable="false"
        class="mdi mdi-cog sf-mr-3 sf-text-lg sf-cursor-pointer sf-text-default sf-dropdown-action"
      ></i>
      <i
        @click="removeTexto"
        contenteditable="false"
        class="mdi mdi-close sf-mr-3 sf-text-lg sf-cursor-pointer sf-text-danger sf-dropdown-action"
      ></i>
      <i
        @click="compartilhaTexto"
        contenteditable="false"
        class="mdi mdi-send sf-text-lg sf-cursor-pointer sf-text-info sf-dropdown-action"
      ></i>
    </div>
  </div>
</template>

<script>
import DialogTextConfiguracao from "@/components/templates/Dialog/DialogTextConfiguracao.vue";
export default {
  data: function () {
    return {
      textLabel: [],
      componentTextID: null,
      textoDescrito: null,
    };
  },
  props: {
    textoDescricao: String,
    componentText: String,
    valorObject: String,
    textoID: String,
  },
  beforeMount() {
    if (localStorage.getItem("textLabel")) {
      this.textLabel = JSON.parse(localStorage.getItem("textLabel"));
    }
  },
  components: {
    DialogTextConfiguracao,
  },
  methods: {
    editarTexto(e) {
      console.log(e);
      this.$emit("callbackEditar", e.currentTarget);
    },
    configuraTexto(e) {
      console.log(e);
      this.$emit("callbackConfigurar", e.currentTarget);
    },
    removeTexto(e) {
      console.log(e);
      this.$emit("callbackRemover", e.currentTarget);
    },
    compartilhaTexto(e) {
      console.log(e);
      this.$emit("callbackCompartilhar", e.currentTarget);
    },
  },
};
</script>

<style></style>
