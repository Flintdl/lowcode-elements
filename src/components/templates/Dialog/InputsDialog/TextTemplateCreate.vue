<template>
  <div class="sf-col-12 sf-form-group">
    <sup>*Alterações são feitas ao sair do campo</sup> <br />
    <label class="sf-label"> Inserir Texto </label>
    <div v-if="textLabel">
      <TextoCustomizacao
        class="sf-texto-customizacao sf-display-flex sf-align-items-center sf-justify-content-between sf-text-preview sf-border-2 sf-border-radius sf-border-dashed sf-px-3 sf-py-2 sf-form-group"
        v-for="(texto, i) of textLabel"
        :textoDescricao="texto.texto"
        :textoID="String(texto.id)"
        v-on:callbackEditar="editarTexto"
        v-on:callbackRemover="removeTexto"
        v-on:compartilhaTexto="compartilhaTexto"
        v-on:callbackFontProps="fontProps"
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
          @keyup.enter="insertTexto"
          :minlength="invalid && 5"
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
    TextoCustomizacao,
  },
  beforeMount() {
    if (localStorage.getItem("textLabel")) {
      var listaGet = JSON.parse(localStorage.getItem("textLabel")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var props = element.props;
          props.forEach((element) => {
            console.log(element);
            this.textLabel.push(element);
          });
        }
      });
    }
  },
  methods: {
    insertTexto() {
      if (this.$refs.novoTexto.value.length >= 5) {
        if (this.$refs.novoTexto.classList.contains("sf-border-danger"))
          this.$refs.novoTexto.classList.remove("sf-border-danger");
        this.invalid = false;

        var date = new Date().getTime();
        var novoTexto = { id: date, texto: this.$refs.novoTexto.value };

        this.textLabel.push(novoTexto);
        this.$refs.novoTexto.value = "";
        this.$refs.novoTexto.focus();

        var lista = JSON.parse(localStorage.getItem("textLabel")) || [];

        var conteinerAtual = this.idBloco;

        let achou = false;
        lista.forEach((element) => {
          if (element.container_id == conteinerAtual) {
            element.props.push(novoTexto);
            achou = true;
          }
        });

        if (!achou) {
          lista.push({
            container_id: conteinerAtual,
            props: [novoTexto],
          });
        }

        localStorage.setItem("textLabel", JSON.stringify(lista));
      } else {
        this.invalid = true;
        this.$refs.novoTexto.classList.add("sf-border-danger");
        setTimeout(() => {
          this.invalid = false;
        }, 200);
      }
    },
    editarTexto(target) {
      var listaGet = JSON.parse(localStorage.getItem("textLabel")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var props = element.props;
          console.log(props);
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
          console.log(element.props);
          this.textLabel = element.props;
        }
      });

      // this.textLabel = this.textLabel.filter(function (returnableObjects) {
      //   return (
      //     returnableObjects.id !=
      //     target.parentElement.parentElement.getAttribute("id")
      //   );
      // });
      // localStorage.setItem("textLabel", JSON.stringify(this.textLabel));
      var removeTexto = {
        id: target.parentElement.parentElement.getAttribute("id"),
        texto: target.parentElement.parentElement.innerText,
      };
      this.$emit("callbackTextoRemoveBloco", removeTexto);
    },
    compartilhaTexto(element) {
      var novoTexto = {
        id: element.id,
        texto: element.texto,
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
