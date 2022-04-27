<template>
  <div class="sf-col-12">
    <div class="sf-row">
      <div class="sf-col sf-display-flex sf-mb-3" v-if="imagemData.length">
        <i
          @click="colGridSelect = 'sf-col-6'"
          :class="[
            colGridSelect === 'sf-col-6' && 'sf-text-info ',
            ' mdi mdi-view-module sf-mr-3 sf-cursor-pointer sf-text-xl sf-px-2 sf-bg-muted sf-border-radius',
          ]"
        ></i>
        <i
          @click="colGridSelect = 'sf-col-12'"
          :class="[
            colGridSelect === 'sf-col-12' && 'sf-text-info ',
            ' mdi mdi-view-list sf-cursor-pointer sf-text-xl sf-px-2 sf-bg-muted sf-border-radius',
          ]"
        ></i>
      </div>
      <div
        :class="[
          !imagemData.length && 'sf-ml-auto',
          'sf-col-auto sf-display-flex sf-mb-3',
        ]"
      >
        <label
          :for="'nova-imagem' + idBloco"
          class="sf-display-flex sf-align-items-center sf-bg-muted sf-border sf-border-radius sf-px-2 sf-overflow-hidden sf-text-center sf-text-muted sf-width-100 sf-cursor-pointer"
        >
          <i
            class="mdi mdi-image-plus sf-text-lg sf-mr-3 sf-dropdown-action"
          ></i>
          Nova imagem
        </label>
      </div>
    </div>
    <div v-if="imagemData.length">
      <div class="sf-row">
        <ImagemCustomizacao
          v-bind:key="i"
          v-for="(imagem, i) of imagemData"
          :class="colGridSelect + ' sf-display-flex'"
          :srcImagem="imagem.dataBase"
          :idImagem="String(imagem.id)"
          v-on:callbackRemoveImagem="removeImagem"
          v-on:callbackPropsImage="propsImageEmit"
          v-on:callbackCompartilhaImagem="compartilhaImagem"
          :idImageModal="String(imagem.id)"
          :tituloImagem="imagem.titulo"
        />
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
        <div class="sf-row">
          <div class="sf-col-12 sf-mb-3">
            <label for="tituloInput" class="sf-label">Titulo da Imagem</label>
            <input
              ref="tituloInput"
              id="tituloInput"
              type="text"
              placeholder="Ex. Logo da Empresa X"
            />
            <span
              class="sf-display-flex sf-align-items-center sf-text-white sf-bg-danger sf-p-2 sf-border-radius sf-mt-2"
              v-if="mensagemError"
            >
              <i
                class="mdi mdi-alert-circle sf-text-lg sf-mr-3 sf-text-white sf-dropdown-action"
              ></i>

              {{ mensagemError }}
            </span>
          </div>
        </div>
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
      colGridSelect: "sf-col-6",
      mensagemError: null,
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
      if (this.$refs.tituloInput.value.trim().length >= 5) {
        this.mensagemError = null;
        var date = new Date().getTime();
        var novaImagem = {
          id: date,
          titulo: this.$refs.tituloInput.value,
          dataBase: this.$refs.imagemPreview.getAttribute("src"),
        };

        this.imagemData.push(novaImagem);
        this.$refs.novaImagem.value = "";
        this.$refs.tituloInput.value = "";
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
      } else {
        this.mensagemError = "Digite pelo menos 5 caracteres";
      }
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
      var removeImagem = {
        id: target.parentElement.parentElement.getAttribute("id"),
      };
      this.$emit("callbackRemoveImagem", removeImagem);
    },
    compartilhaImagem(elementCompartilha) {
      this.$emit("callbackCompartilhaImagem", elementCompartilha);
    },
    propsImageEmit(imagemProps) {
      this.$emit("callbackPropsImage", imagemProps);
    },
  },
};
</script>

<style lang="scss">
// input:invalid {
//   animation: shake 300ms;
// }
// @keyframes shake {
//   25% {
//     transform: translateX(4px);
//   }
//   50% {
//     transform: translateX(-4px);
//   }
//   100% {
//     transform: translateX(4px);
//   }
// }
</style>
