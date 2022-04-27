<template>
  <div
    class="sf-position-relative sf-p-3 sf-tooltip-actions-hover"
    :style="[cssDefinido, styleComputed]"
    :ref="idBloco"
  >
    <div class="sf-row" v-if="textObject.length || imagemObject.length">
      <div
        data-bind-grid
        class="sf-col-12"
        v-bind:key="texto.id + i"
        v-for="(texto, i) of textObject"
      >
        <component
          class="sf-text-white sf-text-center"
          data-bind-grid-tag
          style="margin: 0"
          :is="texto.tag"
          :ref="'Text_' + texto.id"
        >
          {{ texto.texto }}
        </component>
      </div>
      <div
        data-bind-grid
        class="sf-col-12"
        v-bind:key="imagem.id + i"
        v-for="(imagem, i) of imagemObject"
      >
        <img
          class="sf-display-block"
          data-bind-grid-tag
          :src="imagem.dataBase"
          :title="imagem.titulo"
          width="100%"
          :ref="'Img_' + imagem.id"
        />
      </div>
    </div>
    <!-- v-on:callbackTextoInsereBloco="insereTexto" -->
    <Transition name="bounce">
      <KeepAlive>
        <component
          :cardId="idBloco"
          v-bind:is="component"
          :idBloco="'component_' + idBloco"
          v-on:callback="closeDialog"
          v-on:callbackMinimized="minimizedDialog"
          v-on:callbackTextoInsereBloco="insereTexto"
          v-on:callbackTextoEditaBloco="editaTexto"
          v-on:callbackTextoRemoveBloco="removeTexto"
          v-on:callbackFontProps="fontProps"
          v-on:callbackPropsImage="propsImageEmit"
          v-on:callbackCompartilhaImagem="insereImagem"
          v-on:callbackRemoveImagem="removeImagem"
          v-on:callbackAc="getValorCampoAc"
          :class="componentMinimized"
        />
      </KeepAlive>
    </Transition>
    <div class="sf-tooltip-actions sf-position-absolute">
      <ul class="sf-row">
        <li class="sf-col-4 sf-text-center">
          <i
            @click="showOptions"
            class="mdi mdi-cog sf-text-lg sf-text-white sf-dropdown-action"
          ></i>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import DialogBlocoConfiguracao from "@/components/templates/Dialog/DialogBlocoConfiguracao.vue";

export default {
  data() {
    return {
      backgroundImage: null,
      component: null,
      componentMinimized: null,
      shadowLateral: this.shadowLateral,
      shadowBottom: this.shadowBottom,
      shadowExpanse: this.shadowExpanse,
      shadowCor: this.shadowCor,
      textObject: [],
      imagemObject: [],
    };
  },
  props: {
    cssDefinido: Object,
    tamanho: String,
    idBloco: String,
    altura: String,
    background: String,
  },
  components: {
    DialogBlocoConfiguracao,
  },
  beforeMount() {
    if (localStorage.getItem("textLabelBloco")) {
      var listaGet = JSON.parse(localStorage.getItem("textLabelBloco")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var props = element.props;
          props.forEach((element) => {
            this.textObject.push(element);
          });
        }
      });
    }
    if (localStorage.getItem("imagemDataBloco")) {
      var listaGetImagem =
        JSON.parse(localStorage.getItem("imagemDataBloco")) || [];
      var conteinerAtualImagem = this.idBloco;
      listaGetImagem.forEach((element) => {
        if (element.container_id == conteinerAtualImagem) {
          var props = element.imagemData;
          props.forEach((element) => {
            this.imagemObject.push(element);
          });
        }
      });
    }
  },
  methods: {
    showOptions: function () {
      this.component = "DialogBlocoConfiguracao";
    },
    closeDialog: function (closeDialog) {
      this.component = closeDialog;
    },
    minimizedDialog: function (minimizedDialog) {
      this.componentMinimized = minimizedDialog;
    },
    removeTexto: function (removeTexto) {
      var listaGet = JSON.parse(localStorage.getItem("textLabelBloco")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          element.props = element.props.filter(function (returnableObjects) {
            return returnableObjects.id !== removeTexto.id;
          });
          if (element.props)
            localStorage.setItem("textLabelBloco", JSON.stringify(listaGet));
          this.textObject = element.props;
        }
      });
    },
    editaTexto: function (editaTexto) {
      var listaGet = JSON.parse(localStorage.getItem("textLabelBloco")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          var props = element.props;
          var objIndex = props.findIndex((obj) => obj.id == editaTexto.id);
          if (props[objIndex]) {
            props[objIndex].texto = editaTexto.texto;
            localStorage.setItem("textLabelBloco", JSON.stringify(listaGet));
          }
          if (this.textObject[objIndex]) {
            this.textObject[objIndex].texto = editaTexto.texto;
          }
        }
      });
    },
    insereTexto: function (insereTexto) {
      var objIndex = this.textObject.find((obj) => obj.id === insereTexto.id);
      if (!objIndex) {
        this.textObject.push(insereTexto);

        var lista = JSON.parse(localStorage.getItem("textLabelBloco")) || [];
        var conteinerAtual = this.idBloco;
        let achou = false;
        lista.forEach((element) => {
          if (element.container_id == conteinerAtual) {
            element.props.push(insereTexto);
            achou = true;
          }
        });

        if (!achou) {
          lista.push({
            container_id: conteinerAtual,
            props: [insereTexto],
          });
        }

        localStorage.setItem("textLabelBloco", JSON.stringify(lista));

        // localStorage.setItem("textLabelBloco", JSON.stringify(this.textObject));
      }
    },
    fontProps(font) {
      this.textObject.forEach((element) => {
        if (font.id === String(element.id)) {
          var elementFind = this.$refs["Text_" + element.id];
          elementFind[0].style.cssText = `
          font-style: ${font.style};
          text-decoration: ${font.decoration};
          text-transform: ${font.transform};
          font-family: ${font.family};
          font-size: ${font.size}rem;
          margin: ${font.fontMargin.marginTop}px ${font.fontMargin.marginRight}px ${font.fontMargin.marginBottom}px ${font.fontMargin.marginLeft}px;`;
        }
      });
    },
    insereImagem: function (insereImagem) {
      var objIndex = this.imagemObject.find(
        (obj) => obj.id === insereImagem.id
      );
      if (!objIndex) {
        this.imagemObject.push(insereImagem);

        var lista = JSON.parse(localStorage.getItem("imagemDataBloco")) || [];
        var conteinerAtual = this.idBloco;
        let achou = false;
        lista.forEach((element) => {
          if (element.container_id == conteinerAtual) {
            element.imagemData.push(insereImagem);
            achou = true;
          }
        });

        if (!achou) {
          lista.push({
            container_id: conteinerAtual,
            imagemData: [insereImagem],
          });
        }

        localStorage.setItem("imagemDataBloco", JSON.stringify(lista));
      }
    },
    removeImagem: function (removeImagem) {
      var listaGet = JSON.parse(localStorage.getItem("imagemDataBloco")) || [];
      var conteinerAtual = this.idBloco;
      listaGet.forEach((element) => {
        if (element.container_id == conteinerAtual) {
          element.imagemData = element.imagemData.filter(function (
            returnableObjects
          ) {
            return returnableObjects.id !== removeImagem.id;
          });
          if (element.imagemData)
            localStorage.setItem("imagemDataBloco", JSON.stringify(listaGet));
          this.imagemObject = element.imagemData;
        }
      });
    },
    propsImageEmit(imagem) {
      console.log(imagem);
      this.imagemObject.forEach((element) => {
        if (imagem.id === String(element.id)) {
          var elementFind = this.$refs["Img_" + element.id];
          elementFind[0].style.cssText = `
          border-radius: ${imagem.styles.borderRadius}px;
          width: ${imagem.styles.width}%;
          height: ${
            imagem.styles.height > 0 ? imagem.styles.height : "fit-content"
          }px;
          `;
          if (imagem.styles.margin.auto === false) {
            elementFind[0].style.cssText += `
              margin: ${imagem.styles.margin.marginTop}px 
              ${imagem.styles.margin.marginRight}px 
              ${imagem.styles.margin.marginBottom}px 
              ${imagem.styles.margin.marginLeft}px;
          `;
          } else {
            elementFind[0].style.cssText += `
              margin: ${imagem.styles.margin.marginTop}px auto ${imagem.styles.margin.marginBottom}px;
          `;
          }
        }
      });
    },
    getValorCampoAc(valor) {
      this.shadowLateral = valor.shadowLateral;
      this.shadowBottom = valor.shadowBottom;
      this.shadowExpanse = valor.shadowExpanse;
      this.shadowCor = valor.shadowCor;
    },
  },
  computed: {
    styleComputed() {
      return {
        "background-color": this.background,
        "box-shadow": `
          ${this.shadowLateral ? this.shadowLateral : "1"}px
          ${this.shadowBottom ? this.shadowBottom : "2"}px
          ${this.shadowExpanse ? this.shadowExpanse : "11"}px 0
          ${this.shadowCor ? this.shadowCor : "rgba(0, 0, 0, .1)"} `,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.sf-height-position,
.sf-width-position {
  font-size: 1.4ch;
}
.sf-height-position {
  transform: translateY(-50%);
  left: 0;
  top: 50%;
}
.sf-width-position {
  transform: translateX(-50%);
  bottom: 0;
  left: 50%;
}
</style>
