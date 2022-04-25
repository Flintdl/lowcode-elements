<template>
  <div class="sf-col-12 sf-form-group">
    <label class="sf-label"> Inserir Imagem </label>
    <div v-if="imagemData">
      <!-- <TextoCustomizacao
        class="sf-texto-customizacao sf-display-flex sf-align-items-center sf-justify-content-between sf-text-preview sf-border-2 sf-border-radius sf-border-dashed sf-px-3 sf-py-2 sf-form-group"
        v-for="(texto, i) of textLabel"
        :textoDescricao="texto.texto"
        :textoTag="texto.tag"
        :textoID="String(texto.id)"
        v-on:callbackEditar="editarTexto"
        v-on:callbackRemover="removeTexto"
        v-on:compartilhaTexto="compartilhaTexto"
        v-on:callbackFontProps="fontProps"
        :componentText="componentText"
        :id="texto.id"
        v-bind:key="i"
      /> -->
      <img
        v-bind:key="i"
        :src="imagem.dataBase"
        v-for="(imagem, i) of imagemData"
      />
    </div>
    <hr class="sf-mb-4" v-if="imagemData.length" />
    <!-- v-model="valor" -->
    <div class="sf-row">
      <div class="sf-col sf-display-flex">
        <input
          type="file"
          ref="novaImagem"
          id="nova-imagem"
          class="sf-display-none"
          @change="imagemConvert"
        />
        <label
          for="nova-imagem"
          class="sf-border-radius sf-border sf-width-100 sf-p-2 sf-text-truncate"
          >{{ imagemDataTemp ? imagemDataTemp : "Insira uma imagem" }}</label
        >
      </div>
      <div class="sf-col-auto sf-display-flex sf-justify-content-right">
        <button
          ref="botaoInsere"
          class="sf-btn sf-btn-primary sf-ml-auto"
          @click="insertTexto"
        >
          Adicionar
        </button>
      </div>
      <div class="sf-col-12 sf-mt-3" v-if="imagemDataTemp">
        <div class="sf-display-flex sf-p-2 sf-border-2 sf-border-radius">
          <img
            width="100%"
            class="sf-border-radius"
            :alt="imagemDataTemp"
            ref="imagemPreview"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import TextoCustomizacao from "@/components/templates/Dialog/InputsDialog/TextoCustomizacao.vue";

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
    // TextoCustomizacao,
  },
  methods: {
    imagemConvert(e) {
      var filesSelected = e.currentTarget.files;
      if (filesSelected.length > 0) {
        var fileToLoad = filesSelected[0];
        this.imagemDataTemp = fileToLoad.name;
        var fileReader = new FileReader();
        var imagemPreview;
        fileReader.addEventListener("load", function (fileLoadedEvent) {
          imagemPreview = fileLoadedEvent.target.result;
        });
        this.$refs.imagemPreview.src = imagemPreview;
      }
      fileReader.readAsDataURL(fileToLoad);
    },
    insertTexto() {
      var date = new Date().getTime();
      var novaImagem = {
        id: date,
        dataBase: this.$refs.imagemPreview.src,
      };
      console.log(this.$refs.imagemPreview.src);

      this.imagemData.push(novaImagem);
      this.$refs.novaImagem.value = "";
      this.$refs.novaImagem.focus();

      var lista = JSON.parse(localStorage.getItem("textLabel")) || [];

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

      localStorage.setItem("textLabel", JSON.stringify(lista));
    },
    editarTexto(target) {
      var listaGet = JSON.parse(localStorage.getItem("textLabel")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var props = element.props;
          var objIndex = props.findIndex(
            (obj) => obj.id == target.parentElement.id
          );
          if (props[objIndex]) {
            props[objIndex].texto = target.innerText;
            localStorage.setItem("textLabel", JSON.stringify(listaGet));
          }
          if (this.textLabel[objIndex]) {
            this.textLabel[objIndex].texto = target.innerText;
          }
        }
      });
      var editaTexto = {
        id: target.parentElement.id,
        texto: target.innerText,
      };
      this.$emit("callbackTextoEditaBloco", editaTexto);
    },
    removeTexto(target) {
      var listaGet = JSON.parse(localStorage.getItem("textLabel")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          element.props = element.props.filter(function (returnableObjects) {
            return (
              returnableObjects.id !=
              target.parentElement.parentElement.getAttribute("id")
            );
          });
          if (element.props)
            localStorage.setItem("textLabel", JSON.stringify(listaGet));
          this.textLabel = element.props;
        }
      });
      var removeTexto = {
        id: target.parentElement.parentElement.getAttribute("id"),
        texto: target.parentElement.parentElement.innerText,
      };
      this.$emit("callbackTextoRemoveBloco", removeTexto);
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
    fontProps(font) {
      this.$emit("callbackFontProps", font);
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
