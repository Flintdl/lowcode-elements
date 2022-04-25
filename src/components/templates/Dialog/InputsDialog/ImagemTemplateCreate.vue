<template>
  <div class="sf-col-12">
    <div v-if="imagemData.length">
      <div class="sf-row">
        <ImagemCustomizacao
          v-bind:key="i"
          v-for="(imagem, i) of imagemData"
          class="sf-col-4 sf-display-flex"
          :srcImagem="imagem.dataBase"
          :idImagem="String(imagem.id)"
          v-on:callbackRemover="removeImagem"
          :idImageModal="String(imagem.id)"
        />
        <div class="sf-col-4 sf-display-flex">
          <label
            :for="'nova-imagem' + idBloco"
            class="sf-display-flex sf-flex-column sf-justify-content-center sf-bg-muted sf-border sf-border-radius sf-px-3 sf-py-2 sf-form-group sf-overflow-hidden sf-text-center sf-text-muted sf-width-100 sf-cursor-pointer"
            style="height: 74px"
          >
            <i class="mdi mdi-image-plus sf-text-lg sf-dropdown-action"></i>
          </label>
        </div>
      </div>
    </div>
    <div v-if="!imagemData.length">
      <div class="sf-row">
        <div class="sf-col-3 sf-display-flex">
          <label
            :for="'nova-imagem' + idBloco"
            class="sf-display-flex sf-flex-column sf-justify-content-center sf-bg-muted sf-border sf-border-radius sf-px-3 sf-py-2 sf-form-group sf-overflow-hidden sf-text-center sf-text-muted sf-width-100 sf-cursor-pointer"
            style="height: 74px"
          >
            <i class="mdi mdi-image-plus sf-text-lg sf-dropdown-action"></i>
          </label>
        </div>
      </div>
    </div>
    <hr class="sf-mb-4" v-if="imagemData.length && invalid" />
    <!-- v-model="valor" -->
    <div class="sf-row">
      <div class="sf-col sf-display-flex">
        <input
          type="file"
          ref="novaImagem"
          :id="'nova-imagem' + idBloco"
          class="sf-display-none"
          @change="imagemConvert"
        />
      </div>
      <div class="sf-col-12 sf-form-group" :style="!invalid && 'display: none'">
        <div class="sf-display-flex sf-p-2 sf-border-2 sf-border-radius">
          <img
            width="100%"
            class="sf-border-radius"
            :alt="imagemDataTemp"
            ref="imagemPreview"
          />
        </div>
      </div>
      <div
        class="sf-col-12 sf-display-flex sf-justify-content-end sf-form-group"
        v-if="invalid"
      >
        <button
          ref="botaoInsere"
          class="sf-btn sf-btn-outline-danger sf-mr-3"
          @click="removeInsert"
        >
          Remover
        </button>
        <button
          ref="botaoInsere"
          class="sf-btn sf-btn-primary"
          @click="insertImagem"
        >
          Adicionar
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import ImagemCustomizacao from "@/components/templates/Dialog/InputsDialog/ImagemCustomizacao.vue";

export default {
  data: function () {
    return {
      imagemData: [],
      imagemDataTemp: null,
      imagemDataBase: null,
      invalid: false,
    };
  },
  props: {
    componentText: null,
    componentTextID: null,
    textoDescrito: null,
    valorInput: Array,
    idBloco: String,
  },
  components: {
    ImagemCustomizacao,
  },
  beforeMount() {
    if (localStorage.getItem("imagemData")) {
      var listaGet = JSON.parse(localStorage.getItem("imagemData")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var imagem = element.imagemData;
          if (imagem)
            imagem.forEach((element) => {
              this.imagemData.push(element);
            });
        }
      });
    }
  },
  methods: {
    imagemConvert(e) {
      this.invalid = true;
      var filesSelected = e.currentTarget.files;
      if (filesSelected.length > 0) {
        var fileToLoad = filesSelected[0];
        this.imagemDataTemp = fileToLoad.name;
        var fileReader = new FileReader();
        var imagemPreview = this.$refs.imagemPreview;
        fileReader.addEventListener("load", function (fileLoadedEvent) {
          imagemPreview.setAttribute("src", fileLoadedEvent.target.result);
        });
      }
      fileReader.readAsDataURL(fileToLoad);
    },
    removeInsert() {
      this.$refs.novaImagem.value = "";
      this.$refs.imagemPreview.removeAttribute("src");
      this.imagemDataTemp = null;
      this.invalid = false;
    },
    insertImagem() {
      var date = new Date().getTime();
      var novaImagem = {
        id: date,
        dataBase: this.$refs.imagemPreview.getAttribute("src"),
      };

      this.imagemData.push(novaImagem);
      this.$refs.novaImagem.value = "";
      this.$refs.imagemPreview.removeAttribute("src");
      this.imagemDataTemp = null;
      this.invalid = false;

      var lista = JSON.parse(localStorage.getItem("imagemData")) || [];

      var conteinerAtual = this.idBloco;

      let achou = false;
      lista.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          element.imagemData.push(novaImagem);
          achou = true;
        }
      });

      if (!achou) {
        lista.push({
          container_id: conteinerAtual,
          imagemData: [novaImagem],
        });
      }

      localStorage.setItem("imagemData", JSON.stringify(lista));
    },
    removeImagem(target) {
      var listaGet = JSON.parse(localStorage.getItem("imagemData")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          element.imagemData = element.imagemData.filter(function (
            returnableObjects
          ) {
            return (
              String(returnableObjects.id) !=
              target.parentElement.parentElement.getAttribute("id")
            );
          });
          if (element.imagemData)
            localStorage.setItem("imagemData", JSON.stringify(listaGet));
          this.imagemData = element.imagemData;
        }
      });
      // var removeTexto = {
      //   id: target.parentElement.parentElement.getAttribute("id"),
      //   texto: target.parentElement.parentElement.innerText,
      // };
      // this.$emit("callbackTextoRemoveBloco", removeTexto);
    },
    compartilhaTexto(elementCompartilha) {
      var typeTag;
      this.textLabel.forEach((element) => {
        if (elementCompartilha.id === String(element.id)) {
          typeTag = element.tag;
        }
      });
      var novoTexto = {
        id: elementCompartilha.id,
        texto: elementCompartilha.texto,
        tag: typeTag,
      };
      this.$emit("callbackTextoInsere", novoTexto);
    },
  },
};
</script>

<style lang="scss">
.sf-texto-customizacao {
  &:hover {
    border-color: #e7515a !important;
  }
}
input:invalid {
  animation: shake 300ms;
}
@keyframes shake {
  25% {
    transform: translateX(4px);
  }
  50% {
    transform: translateX(-4px);
  }
  100% {
    transform: translateX(4px);
  }
}
</style>
