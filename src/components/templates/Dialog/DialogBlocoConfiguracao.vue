<template>
  <div
    :ref="idBloco"
    class="sf-dialog-content sf-position-fixed sf-bg-white sf-border-radius"
  >
    <div
      @mousedown="moveDialog"
      class="sf-dialog-header sf-border-primary sf-bg-third sf-border-radius-top sf-cursor-grab sf-border-bottom sf-py-2 sf-px-4"
    >
      <span
        class="sf-display-flex sf-align-items-center sf-justify-content-between sf-width-100 sf-font-weight-bold sf-text-md sf-text-white"
      >
        Bloco {{ cardId }}
        <div>
          <i
            @click="minimizeAction"
            class="mdi sf-mr-3 sf-text-lg sf-cursor-pointer sf-text-white sf-dropdown-action"
            :class="iconVerify ? 'mdi-window-maximize' : 'mdi-window-minimize'"
          ></i>
          <i
            @click="closeAction"
            class="mdi mdi-close sf-text-lg sf-cursor-pointer sf-text-white sf-dropdown-action"
          ></i>
        </div>
      </span>
    </div>
    <div class="sf-dialog-body">
      <div class="sf-btn-select-content" style="visibility: visible"></div>
      <div class="sf-display-flex">
        <div
          v-for="tab of tabsDialog"
          v-bind:key="tab.label"
          class="sf-text-center sf-width-100"
        >
          <div
            @click="showContent"
            :dataRef="tab.ref"
            class="sf-btn-select-dialog sf-cursor-pointer sf-py-2 sf-px-4 sf-text-white sf-font-weight-bold"
            :class="tab.container ? 'sf-btn-primary-hover' : 'sf-btn-primary'"
          >
            {{ tab.label }}
          </div>
        </div>
      </div>
    </div>

    <div
      class="sf-menu-ul sf-menu-content open sf-pb-0"
      :id="'container-collapse_' + cardId"
      :ref="'container-collapse_' + cardId"
    >
      <div class="sf-row sf-px-4">
        <BoxShadowBlocoLateral
          :valorInput="shadowObject.shadowLateral"
          v-on:callback="getShadowLateral"
        />
        <BoxShadowBlocoBottom
          :valorInput="shadowObject.shadowBottom"
          v-on:callback="getShadowBottom"
        />
        <BoxShadowBloco
          :valorInput="shadowObject.shadowExpanse"
          v-on:callback="getShadowExpanse"
        />
        <BoxShadowBlocoCor
          :valorInput="shadowObject.shadowCor"
          v-on:callback="getShadowCor"
        />
      </div>
      <div class="sf-dialog-footer sf-py-3 sf-px-4 sf-border-top">
        <button
          @click="resetShadow"
          class="sf-btn sf-text-sm sf-px-3 sf-py-0 sf-btn-primary"
        >
          Resetar Shadow
        </button>
      </div>
    </div>
    <div
      class="sf-menu-ul sf-menu-content sf-pb-0"
      :id="'template-collapse_' + cardId"
      :ref="'template-collapse_' + cardId"
    >
      <div class="sf-row sf-px-4">
        <TextTemplateCreate
          :valorInput="textObject.texto"
          v-on:callback="getTextCreated"
        />
      </div>
      <div class="sf-dialog-footer sf-py-3 sf-px-4 sf-border-top">
        <button
          @click="resetShadow"
          class="sf-btn sf-text-sm sf-px-3 sf-py-0 sf-btn-primary"
        >
          Resetar Texto
        </button>
      </div>
    </div>
    <div
      class="sf-menu-ul sf-menu-content sf-pb-0"
      :id="'border-collapse_' + cardId"
      :ref="'border-collapse_' + cardId"
    >
      Arroz3
    </div>
    <div
      class="sf-menu-ul sf-menu-content sf-pb-0"
      :id="'position-collapse_' + cardId"
      :ref="'position-collapse_' + cardId"
    >
      Arroz 4
    </div>

    <!-- <svg id="svg">
      <line ref="line" id="line" />
    </svg> -->
  </div>
</template>

<script>
// Shadow
import BoxShadowBlocoLateral from "@/components/templates/Dialog/InputsDialog/BoxShadowBlocoLateral.vue";
import BoxShadowBlocoBottom from "@/components/templates/Dialog/InputsDialog/BoxShadowBlocoBottom.vue";
import BoxShadowBloco from "@/components/templates/Dialog/InputsDialog/BoxShadowBloco.vue";
import BoxShadowBlocoCor from "@/components/templates/Dialog/InputsDialog/BoxShadowBlocoCor.vue";
// Text
import TextTemplateCreate from "@/components/templates/Dialog/InputsDialog/TextTemplateCreate.vue";

export default {
  props: {
    cardId: String,
    idBloco: String,
  },
  components: {
    // Shadow
    BoxShadowBlocoLateral,
    BoxShadowBlocoBottom,
    BoxShadowBloco,
    BoxShadowBlocoCor,
    // Text
    TextTemplateCreate,
  },
  data: function () {
    return {
      iconVerify: false,
      tabsDialog: [
        {
          label: "Shadows",
          ref: `container-collapse_${this.cardId}`,
          container: "initial",
        },
        { label: "Textos", ref: `template-collapse_${this.cardId}` },
        { label: "Imagens", ref: `border-collapse_${this.cardId}` },
        { label: "Efeitos", ref: `position-collapse_${this.cardId}` },
      ],
      shadowObject: {
        shadowLateral: (this.shadowLateral = "0"),
        shadowBottom: (this.shadowBottom = "0"),
        shadowExpanse: (this.shadowExpanse = "0"),
        shadowCor: (this.shadowCor = "transparent"),
      },
      textObject: [],
    };
  },
  mounted() {
    // console.log(this.idBloco);
    // console.log(this.$refs[this.idBloco]);
    // console.log(this.$refs[this.idBloco].parentElement);
    // var linha = document.createElement("span");
    // linha;
  },
  methods: {
    moveDialog(e) {
      var divResize = e.currentTarget.parentElement;
      var target = e.currentTarget;
      target.classList.remove("sf-cursor-grab");
      target.classList.add("sf-cursor-grabbing");

      var startX, startY, startLeft, startTop;

      startX = e.clientX;
      startY = e.clientY;
      startLeft = parseInt(
        document.defaultView.getComputedStyle(divResize).left,
        10
      );
      startTop = parseInt(
        document.defaultView.getComputedStyle(divResize).top,
        10
      );

      document.documentElement.addEventListener("mousemove", doDrag);
      document.documentElement.addEventListener("mouseup", stopDrag);

      function doDrag(e) {
        divResize.style.left = startLeft + e.clientX - startX + "px";
        divResize.style.top = startTop + e.clientY - startY + "px";
        if (
          divResize.computedStyleMap().get("top").value >=
          document.body.offsetTop + document.body.offsetHeight - 50
        ) {
          var previewDrop = document.createElement("div");
          previewDrop.style.height = "50px";
          previewDrop.classList.add("preview-Drop");
          previewDrop.style.borderRadius = "100% 100% 0 0";
          previewDrop.style.width = "100%";
          previewDrop.style.bottom = "0";
          previewDrop.style.backgroundColor = "#fa807280";
          previewDrop.style.position = "fixed";
          previewDrop.style.zIndex = "500";
          document.body.appendChild(previewDrop);
        } else {
          if (document.querySelector(".preview-Drop")) {
            document
              .querySelector(".preview-Drop")
              .remove(document.querySelector(".preview-Drop"));
          }
        }
      }

      function stopDrag() {
        document.documentElement.removeEventListener("mousemove", doDrag);
        document.documentElement.removeEventListener("mouseup", stopDrag);
        target.classList.remove("sf-cursor-grabbing");
        target.classList.add("sf-cursor-grab");
        console.log(divResize.computedStyleMap().get("top").value);
        console.log(document.body.offsetTop + document.body.offsetHeight - 50);
        console.log(
          divResize.computedStyleMap().get("top").value >=
            document.body.offsetTop + document.body.offsetHeight - 50
        );
        if (
          divResize.computedStyleMap().get("top").value >=
          document.body.offsetTop + document.body.offsetHeight - 50
        ) {
          divResize.style.top = "initial";
          divResize.style.bottom = "0";
          divResize.style.left = "0";
          divResize.style.width = "100%";
        } else {
          divResize.style.width = "clamp(150px, 50ch, 900px)";
          divResize.style.bottom = "initial";
          return;
        }
      }
    },
    closeAction() {
      this.$emit("callback", null);
      this.$emit("callbackMinimized", "");
    },
    minimizeAction(e) {
      var elementPai =
        e.currentTarget.parentElement.parentElement.parentElement.parentElement;
      if (elementPai.classList.contains("sf-minimized-dialog")) {
        this.$emit("callbackMinimized", "");
        this.iconVerify = false;
      } else {
        this.$emit("callbackMinimized", "sf-minimized-dialog");
        this.iconVerify = true;
      }
    },
    showContent(e) {
      var referenciaItem = this.$refs[e.currentTarget.getAttribute("dataRef")];
      var referenciaAllButtons = document.querySelectorAll(
        `.sf-btn-select-dialog[dataRef]`
      );

      var referenciaItemCss = e.currentTarget;

      for (let i = 0; i < this.tabsDialog.length; i++) {
        const element = this.tabsDialog[i];
        console.log(element);
        if (this.$refs[element.ref] !== referenciaItem) {
          if (this.$refs[element.ref].classList.contains("open"))
            this.$refs[element.ref].classList.remove("open");
        }
      }

      for (let i = 0; i < referenciaAllButtons.length; i++) {
        const element = referenciaAllButtons[i];
        element.classList.remove("sf-btn-primary-hover");
        element.classList.add("sf-btn-primary");
      }

      if (referenciaItem) {
        referenciaItem.classList.toggle("open");
        if (!referenciaItem.classList.contains("open")) {
          referenciaItemCss.classList.remove("sf-btn-primary-hover");
          referenciaItemCss.classList.add("sf-btn-primary");
          referenciaItem.classList.remove("open");
        } else {
          referenciaItemCss.classList.remove("sf-btn-primary");
          referenciaItemCss.classList.add("sf-btn-primary-hover");
          referenciaItem.classList.add("open");
        }
      }
    },
    // Shadow Input
    getShadowLateral(valor) {
      this.shadowObject.shadowLateral = valor;
      this.$emit("callbackAc", this.shadowObject);
    },
    getShadowBottom(valor) {
      this.shadowObject.shadowBottom = valor;
      this.$emit("callbackAc", this.shadowObject);
    },
    getShadowExpanse(valor) {
      this.shadowObject.shadowExpanse = valor;
      this.$emit("callbackAc", this.shadowObject);
    },
    getShadowCor(valor) {
      this.shadowObject.shadowCor = valor;
      this.$emit("callbackAc", this.shadowObject);
    },
    resetShadow() {
      this.shadowObject.shadowLateral = "0";
      this.shadowObject.shadowBottom = "0";
      this.shadowObject.shadowExpanse = "0";
      this.shadowObject.shadowCor = "transparent";
      this.$emit("callbackAc", this.shadowObject);
    },
    // Shadow Input
    // Text Input
    getTextCreated(valor) {
      console.log(valor);
      var Arroz = { texto: valor };
      console.log(Arroz);
      this.textObject.push(Arroz);
      console.log(this.textObject);
    },
    // Text Input
  },
};
</script>

<style lang="scss" scoped>
#svg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: -1;
}

// #line {
//   stroke-width: 2px;
//   stroke: rgb(0, 0, 0);
// }
.line {
  height: 2px;
  width: 1px;
  background: salmon;
  position: fixed;
  -moz-transform-origin: 0% 0%;
  -webkit-transform-origin: 0% 0%;
  transform-origin: 0% 0%;
}
.sf-menu-content {
  display: none !important;
  &.open {
    display: block !important;
    background-color: #f0f0f48c;
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
    height: 100%;
  }
}
</style>
