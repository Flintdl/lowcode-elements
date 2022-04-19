<template>
  <div
    class="sf-position-relative sf-p-3 sf-tooltip-actions-hover"
    :style="[cssDefinido, styleComputed]"
    :ref="idBloco"
  >
    <div v-if="textObject.length">
      <p
        class="sf-text-white sf-text-center"
        v-for="(texto, i) of textObject"
        v-bind:key="i"
        :style="fontCss"
      >
        {{ texto.texto }}
      </p>
    </div>
    <!-- v-on:callbackTextoInsereBloco="insereTexto" -->
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
      v-on:callbackAc="getValorCampoAc"
      :class="componentMinimized"
    />
    <div class="sf-tooltip-actions sf-position-absolute">
      <ul class="sf-row">
        <li class="sf-col-4 sf-text-center">
          <label :for="idBloco" class="sf-cursor-pointer">
            <i
              class="mdi mdi-image sf-text-lg sf-text-white sf-dropdown-action"
            ></i>
            <input
              :id="idBloco"
              :ref="'input' + idBloco"
              type="file"
              class="sf-display-none"
              @change="showOptions($event, 'imagem')"
            />
          </label>
        </li>
        <li class="sf-col-4 sf-text-center">
          <i
            @click="showOptions($event, 'text')"
            class="mdi mdi-card-text sf-text-lg sf-text-white sf-dropdown-action"
          ></i>
        </li>
        <li class="sf-col-4 sf-text-center">
          <i
            @click="showOptions($event, 'options')"
            class="mdi mdi-cog sf-text-lg sf-text-white sf-dropdown-action"
          ></i>
        </li>
      </ul>
    </div>

    <!-- <div
      class="sf-height-position sf-position-absolute sf-display-flex sf-flex-column sf-px-2"
    >
      <i
        class="mdi mdi-arrow-up sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        {{ altura }}
        px
      </span>
      <i
        class="mdi mdi-arrow-down sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </div> -->
    <!-- <div
      class="sf-width-position sf-width-100 sf-position-absolute sf-display-flex sf-align-items-center sf-px-2"
    >
      <i
        class="mdi mdi-arrow-left sf-text-md sf-text-white sf-dropdown-action"
      ></i>
      <hr class="sf-width-100 sf-mr-2" />
      <span class="sf-text-white sf-font-weight-extra-bold sf-text-center">
        {{ larguraPixel }}px
      </span>
      <hr class="sf-width-100 sf-ml-2" />
      <i
        class="mdi mdi-arrow-right sf-text-md sf-text-white sf-dropdown-action"
      ></i>
    </div> -->
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
      fontFamilyData: this.fontFamilyData,
      valorfontSizeData: this.valorfontSizeData,
      estiloFontData: this.estiloFontData,
      fontPropsMargin: {
        marginLeft: null,
        marginTop: null,
        marginRight: null,
        marginBottom: null,
      },
      textObject: [],
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
    if (localStorage.getItem("textLabelBloco"))
      this.textObject = JSON.parse(localStorage.getItem("textLabelBloco"));
  },
  methods: {
    showOptions: function (e, params) {
      switch (params) {
        case "imagem":
          var filesSelected = e.currentTarget.files;
          var backgroundDiv =
            e.currentTarget.parentElement.parentElement.parentElement
              .parentElement.parentElement;
          if (filesSelected.length > 0) {
            var fileToLoad = filesSelected[0];
            var fileReader = new FileReader();
            var srcData;
            fileReader.onload = function (fileLoadedEvent) {
              srcData = fileLoadedEvent.target.result; // <--- data: base64
              var img = document.createElement("img");
              img.src = srcData;
              img.setAttribute("alt", "Imagem Teste");
              img.style.width = "50%";
              img.style.maxHeight = "90%";
              backgroundDiv.appendChild(img);
            };
          }
          fileReader.readAsDataURL(fileToLoad);
          break;
        case "options":
          this.component = "DialogBlocoConfiguracao";
          break;
      }
    },
    closeDialog: function (closeDialog) {
      this.component = closeDialog;
    },
    minimizedDialog: function (minimizedDialog) {
      this.componentMinimized = minimizedDialog;
    },
    removeTexto: function (removeTexto) {
      this.textObject = this.textObject.filter(function (returnableObjects) {
        return returnableObjects.id !== removeTexto.id;
      });
      localStorage.setItem("textLabelBloco", JSON.stringify(this.textObject));
    },
    editaTexto: function (editaTexto) {
      var objIndex = this.textObject.findIndex(
        (obj) => obj.id == editaTexto.id
      );
      if (this.textObject[objIndex]) {
        this.textObject[objIndex].texto = editaTexto.texto;
        localStorage.setItem("textLabelBloco", JSON.stringify(this.textObject));
      }
    },
    insereTexto: function (insereTexto) {
      var objIndex = this.textObject.find((obj) => obj.id === insereTexto.id);
      if (!objIndex) {
        this.textObject.push(insereTexto);
        localStorage.setItem("textLabelBloco", JSON.stringify(this.textObject));
      }
    },
    fontProps(font) {
      this.estiloFontData = font.style;
      this.fontFamilyData = font.family;
      this.valorfontSizeData = font.size;
      this.fontPropsMargin.marginTop = font.fontMargin.marginTop + "px";
      this.fontPropsMargin.marginRight = font.fontMargin.marginRight + "px";
      this.fontPropsMargin.marginBottom = font.fontMargin.marginBottom + "px";
      this.fontPropsMargin.marginLeft = font.fontMargin.marginLeft + "px";
    },
    getValorCampoAc(valor) {
      this.shadowLateral = valor.shadowLateral;
      this.shadowBottom = valor.shadowBottom;
      this.shadowExpanse = valor.shadowExpanse;
      this.shadowCor = valor.shadowCor;
    },
    created() {
      this.$watch("textObject", () => {
        console.log("Alterou", this.textObject);
        // this.textObject
      });
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
    fontCss() {
      var marginProps = this.fontPropsMargin;
      console.log(marginProps);
      return {
        "font-style": this.estiloFontData,
        "font-family": this.fontFamilyData,
        "font-size": this.valorfontSizeData + "rem",
        margin: `${marginProps.marginTop} ${marginProps.marginRight} ${marginProps.marginBottom} ${marginProps.marginLeft}`,
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
