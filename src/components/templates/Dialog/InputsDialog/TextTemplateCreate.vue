<template>
  <div class="sf-col-12 sf-form-group">
    <sup v-if="textLabel.length">*Alterações são feitas ao sair do campo</sup>
    <br v-if="textLabel.length" />
    <label class="sf-label" v-if="textLabel.length"> Inserir Texto </label>
    <div v-if="textLabel">
      <TextoCustomizacao
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
      />
    </div>
    <hr class="sf-mb-4" v-if="textLabel.length" />
    <!-- v-model="valor" -->
    <div class="sf-row">
      <div class="sf-col-12 sf-mb-3">
        <label class="sf-label">Tipo de Texto</label>
        <ul class="sf-display-flex sf-justify-content-between">
          <li
            v-for="(texto, i) of typeOfTag"
            v-bind:key="i"
            class="sf-display-flex sf-cursor-pointer"
            ref="decorationTypeOfTag"
            :title="texto.value"
            @click="onSelectTypeOfTag($event, texto.value)"
          >
            <i
              :class="
                'mdi mdi-' +
                texto.mdi +
                ' ' +
                texto.margin +
                ' sf-px-3 sf-py-1 sf-border-radius sf-text-lg sf-text-muted ' +
                texto.class
              "
            ></i>
          </li>
        </ul>
      </div>
      <div class="sf-col">
        <input
          type="text"
          ref="novoTexto"
          @keyup.enter="insertTexto"
          :minlength="invalid && 5"
          :placeholder="'Novo Texto ' + tipo_de_texto"
        />
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
    </div>
  </div>
</template>

<script>
import TextoCustomizacao from "@/components/templates/Dialog/InputsDialog/TextoCustomizacao.vue";

export default {
  data: function () {
    return {
      tipo_de_texto: "H1",
      typeOfTag: [
        {
          mdi: "format-header-1",
          value: "h1",
          margin: "sf-mr-2",
          class: "sf-bg-disable",
        },
        {
          mdi: "format-header-2",
          value: "h2",
          margin: "sf-mr-2",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-header-3",
          value: "h3",
          margin: "sf-mr-2",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-header-4",
          margin: "sf-mr-2",
          value: "h4",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-header-5",
          margin: "sf-mr-2",
          value: "h5",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-header-6",
          margin: "sf-mr-2",
          value: "h6",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-paragraph",
          value: "p",
          class: "sf-bg-muted",
        },
      ],
      typeOfTagData: "h1",
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
          if (props)
            props.forEach((element) => {
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
        var novoTexto = {
          id: date,
          texto: this.$refs.novoTexto.value,
          tag: this.typeOfTagData,
        };

        this.textLabel.push(novoTexto);
        this.$refs.novoTexto.value = "";
        this.$refs.novoTexto.focus();

        var lista = JSON.parse(localStorage.getItem("textLabel")) || [];

        var conteinerAtual = this.idBloco;

        let achou = false;
        lista.forEach((element) => {
          if (element.container_id == conteinerAtual) {
            if (!element.props) {
              element.push({
                props: [novoTexto],
              });
            } else {
              element.props.push(novoTexto);
            }
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
    onSelectTypeOfTag(e, typeoftag) {
      this.tipo_de_texto = typeoftag;
      var refsGet = this.$refs.decorationTypeOfTag;
      for (let index = 0; index < refsGet.length; index++) {
        const element = refsGet[index];
        if (
          element.querySelector("i").classList.contains("sf-text-info") ||
          element.querySelector("i").classList.contains("sf-bg-disable")
        ) {
          element.querySelector("i").classList.remove("sf-text-info");
          element.querySelector("i").classList.remove("sf-bg-disable");
          element.querySelector("i").classList.add("sf-bg-muted");
        }
      }
      e.currentTarget.querySelector("i").classList.remove("sf-bg-muted");
      e.currentTarget.querySelector("i").classList.add("sf-bg-disable");
      e.currentTarget.querySelector("i").classList.add("sf-text-info");

      this.typeOfTagData = typeoftag;
      this.$refs.novoTexto.focus();
      this.$emit("callbackProps", this.fontProps);
    },
    fontProps(font) {
      this.$emit("callbackFontProps", font);
    },
  },
};
</script>

<style lang="scss">
// .sf-texto-customizacao {
//   &:hover {
//     border-color: #e7515a !important;
//   }
// }
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
