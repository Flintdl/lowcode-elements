<template>
  <div class="sf-position-relative sf-imagem-customizacao-hover">
    <Teleport to="body" v-if="previewModal">
      <div
        @click="previewModal = null"
        class="modal sf-cursor-pointer sf-display-flex sf-align-items-center sf-justify-content-center"
      >
        <img :src="previewModal" alt="teste" />
      </div>
    </Teleport>
    <Transition name="bounce">
      <KeepAlive>
        <component
          v-if="componentText"
          v-bind:is="componentText"
          :idBlocoImagem="idImagem"
          :imagemDataBase="imagemDataBase"
          :tituloImagem="tituloImagem"
          v-on:callbackCloseDialog="closeDialog"
          v-on:callbackPropsImage="propsImageEmit"
          style="top: 5%; left: 25%"
        />
      </KeepAlive>
    </Transition>
    <div
      class="sf-imagem-customizacao sf-display-flex sf-text-preview sf-border sf-border-radius sf-px-2 sf-py-2 sf-form-group sf-width-100 sf-position-relative sf-options-float-image-content sf-align-items-center"
      ref="parentElementID"
      :id="idImagem"
    >
      <div
        class="sf-imagem-content sf-display-flex sf-justify-content-center sf-border-radius"
      >
        <img :src="srcImagem" :ref="idImageModal" class="sf-display-block" />
      </div>

      <div
        class="sf-overflow-hidden sf-titulo-imagem sf-display-flex sf-flex-wrap sf-pl-3 sf-align-items-center"
      >
        <label class="sf-text-truncate-two-lines sf-width-100">
          {{ tituloImagem }}
        </label>
      </div>
      <div
        class="sf-options-float-image sf-bg-muted sf-border-radius sf-border-radius-right-left-0 sf-justify-content-between sf-align-items-center sf-position-absolute sf-px-2"
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
          @click="compartilhaImagem"
          contenteditable="false"
          class="mdi mdi-send sf-text-lg sf-cursor-pointer sf-text-info sf-dropdown-action"
        ></i>
      </div>
    </div>
  </div>
</template>

<script>
import DialogImagemConfiguracao from "@/components/templates/Dialog/DialogImagemConfiguracao.vue";
export default {
  data: function () {
    return {
      componentImagemID: null,
      imagemDataBase: null,
      textoDescrito: null,
      previewModal: null,
      componentText: null,
    };
  },
  props: {
    valorObject: String,
    srcImagem: String,
    idImagem: String,
    idImageModal: String,
    tituloImagem: String,
  },
  components: {
    DialogImagemConfiguracao,
  },
  methods: {
    previewModalAction(idModal) {
      this.previewModal = null;
      this.previewModal = this.$refs[idModal].src;
    },
    configuraTexto() {
      this.componentText = "DialogImagemConfiguracao";
      this.componentImagemID = this.$refs.parentElementID.id;
      this.imagemDataBase = this.$refs[this.idImageModal].src;
    },
    removeImagem(event) {
      this.$emit("callbackRemoveImagem", event.currentTarget);
    },
    compartilhaImagem() {
      var compartilhaImagem = {
        id: this.$refs.parentElementID.id,
        dataBase: this.$refs[this.idImageModal].src,
        titulo: this.tituloImagem,
      };
      this.$emit("callbackCompartilhaImagem", compartilhaImagem);
    },
    fontProps(font) {
      this.$emit("callbackFontProps", font);
    },
    closeDialog: function (closeDialog) {
      this.componentText = closeDialog;
    },
    propsImageEmit(imagemProps) {
      this.$emit("callbackPropsImage", imagemProps);
    },
  },
};
</script>

<style lang="scss">
.sf-imagem-customizacao {
  > .sf-imagem-content {
    overflow: hidden;
    max-width: 50px;
    min-width: 50px;
    > img {
      width: fit-content;
      max-height: 50px;
      height: 60px;
    }
  }
}

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
    width: 10rem;
    bottom: 0;
    right: 0%;
  }
}
.sf-imagem-customizacao-hover {
  &:hover {
    .sf-imagem-customizacao {
      border-color: #e7515a !important;
    }
    .sf-options-float-image-content {
      .sf-options-float-image {
        display: flex;
      }
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
