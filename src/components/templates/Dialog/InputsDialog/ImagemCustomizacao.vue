<template>
  <div class="sf-position-relative">
    <Teleport to="body" v-if="previewModal">
      <div
        @click="previewModal = null"
        class="modal sf-cursor-pointer sf-display-flex sf-align-items-center sf-justify-content-center"
      >
        <img :src="previewModal" alt="teste" />
      </div>
    </Teleport>
    <!-- <Transition name="bounce">
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
    </Transition> -->
    <div
      class="sf-texto-customizacao sf-display-flex sf-text-preview sf-border-2 sf-border-radius sf-border-dashed sf-px-3 sf-py-2 sf-form-group sf-width-100 sf-position-relative sf-options-float-image-content"
      :id="idImagem"
    >
      <img
        :src="srcImagem"
        :ref="idImageModal"
        class="sf-display-block sf-m-auto"
        style="width: fit-content; max-height: 80px; max-width: 100%"
      />
      <div
        class="sf-options-float-image sf-bg-muted sf-border-radius-bottom sf-justify-content-between sf-align-items-center sf-position-absolute sf-px-2"
      >
        <i
          @click="previewModalAction(idImageModal)"
          contenteditable="false"
          class="mdi mdi-eye sf-mr-1 sf-text-lg sf-cursor-pointer sf-text-muted sf-dropdown-action"
        ></i>
        <i
          @click="configuraTexto"
          contenteditable="false"
          class="mdi mdi-cog sf-mr-1 sf-text-lg sf-cursor-pointer sf-text-default sf-dropdown-action"
        ></i>
        <i
          @click="removeImagem"
          contenteditable="false"
          class="mdi mdi-close sf-mr-1 sf-text-lg sf-cursor-pointer sf-text-danger sf-dropdown-action"
        ></i>
        <i
          @click="compartilhaTexto"
          contenteditable="false"
          class="mdi mdi-send sf-text-lg sf-cursor-pointer sf-text-info sf-dropdown-action"
        ></i>
      </div>
    </div>
  </div>
</template>

<script>
// import DialogTextConfiguracao from "@/components/templates/Dialog/DialogTextConfiguracao.vue";
export default {
  data: function () {
    return {
      componentText: null,
      componentTextID: null,
      textoDescrito: null,
      previewModal: null,
    };
  },
  props: {
    valorObject: String,
    srcImagem: String,
    idImagem: String,
    idImageModal: String,
  },

  components: {
    // DialogTextConfiguracao,
  },
  methods: {
    previewModalAction(idModal) {
      this.previewModal = null;
      this.previewModal = this.$refs[idModal].src;
    },
    configuraTexto(target) {
      target = target.currentTarget;
      this.componentText = "DialogTextConfiguracao";
      this.componentTextID = target.parentElement.parentElement.id;
      this.textoDescrito = target.parentElement.parentElement.innerText;
    },
    editarTexto(event) {
      this.$emit("callbackEditar", event.currentTarget);
    },
    removeImagem(event) {
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

<style lang="scss">
.modal {
  position: fixed;
  background-color: #00000070;
  z-index: 999;
  top: 0;
  left: 0%;
  width: 100%;
  height: 100%;
  img {
    max-width: 50%;
    max-height: 70vh;
  }
}
.sf-options-float-image-content {
  .sf-options-float-image {
    display: none;
    bottom: 0;
    left: 0;
    width: 100%;
  }
  &:hover {
    .sf-options-float-image {
      display: flex;
    }
  }
}

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
