<template>
  <div>
    <Transition name="bounce">
      <KeepAlive>
        <component
          v-if="componentText"
          v-bind:is="componentText"
          :idBlocoTexto="textoID"
          :textoDescrito="textoDescricao"
          :fontProps="fontProps"
          v-on:callbackProps="fontProps"
          v-on:callbackCloseDialog="closeDialog"
          style="top: 5%; left: 25%"
        />
      </KeepAlive>
    </Transition>
    <p
      @blur="editarTexto"
      class="sf-m-0 sf-text-truncate sf-width-100"
      ref="text-label-edit"
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
      componentText: null,
      componentTextID: null,
      textoDescrito: null,
    };
  },
  props: {
    textoDescricao: String,
    valorObject: String,
    textoID: String,
    textLabel: Object,
  },

  components: {
    DialogTextConfiguracao,
  },
  methods: {
    configuraTexto(target) {
      target = target.currentTarget;
      this.componentText = "DialogTextConfiguracao";
      this.componentTextID = target.parentElement.parentElement.id;
      this.textoDescrito = target.parentElement.parentElement.innerText;
    },
    editarTexto(event) {
      this.$emit("callbackEditar", event.currentTarget);
    },
    removeTexto(event) {
      this.$emit("callbackRemover", event.currentTarget);
    },
    compartilhaTexto(e) {
      var compartilhaTexto = {
        id: e.currentTarget.parentElement.parentElement.id,
        texto: this.$refs["text-label-edit"].innerText,
      };
      this.$emit("compartilhaTexto", compartilhaTexto);
    },
    fontProps(font) {
      this.$emit("callbackFontProps", font);
    },
    closeDialog: function (closeDialog) {
      this.componentText = closeDialog;
    },
  },
};
</script>

<style>
.bounce-enter-active {
  animation: bounce-in 0.3s;
}
.bounce-leave-active {
  animation: bounce-in 0.2s reverse;
}
@keyframes bounce-in {
  0% {
    transform: scale(0.6);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>
