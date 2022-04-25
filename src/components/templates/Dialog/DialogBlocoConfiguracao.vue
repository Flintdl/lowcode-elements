<template>
  <div
    :ref="idBloco"
    @mousedown="zIndexDialog"
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
            @click="dropAction"
            class="mdi sf-mr-3 sf-text-lg sf-cursor-pointer sf-text-white sf-dropdown-action"
            :class="iconVerifyDrop ? 'mdi-dock-right' : 'mdi-dock-window'"
          ></i>
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

    <Transition name="bounce">
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
    </Transition>
    <div
      class="sf-menu-ul sf-menu-content sf-pb-0"
      :id="'template-collapse_' + cardId"
      :ref="'template-collapse_' + cardId"
    >
      <div class="sf-row sf-px-4">
        <!-- :valorInput="textObject.texto"
          v-on:callback="getTextCreated" -->
        <TextTemplateCreate
          v-on:callbackTextoInsere="insereTextCreated"
          v-on:callbackTextoEditaBloco="editaTextCreated"
          v-on:callbackTextoRemoveBloco="removeTextCreated"
          v-on:callbackFontProps="fontProps"
          :idBloco="idBloco"
        />
      </div>
    </div>
    <div
      class="sf-menu-ul sf-menu-content sf-pb-0"
      :id="'border-collapse_' + cardId"
      :ref="'border-collapse_' + cardId"
    >
      <div class="sf-row sf-px-4">
        <ImagemTemplateCreate :idBloco="idBloco" />
      </div>
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
// Imagem
import ImagemTemplateCreate from "@/components/templates/Dialog/InputsDialog/ImagemTemplateCreate.vue";

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
    // Imagem
    ImagemTemplateCreate,
  },
  data: function () {
    return {
      iconVerify: false,
      iconVerifyDrop: false,
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
        shadowCor: (this.shadowCor = "#000000"),
      },
      // textObject: [],
    };
  },
  methods: {
    zIndexDialog(e) {
      var divZindexTarget = e.currentTarget;
      var divZindex = document.querySelectorAll(".sf-dialog-content");
      for (let i = 0; i < divZindex.length; i++) {
        const element = divZindex[i];
        element.style.zIndex = 998;
      }
      divZindexTarget.style.zIndex = 999;
    },
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

        if (divResize.classList.contains("sf-drop-dialog-content")) {
          divResize.classList.remove("sf-drop-dialog-content");
          divResize.classList.remove("sf-drop-dialog-left");
        }

        if (divResize.classList.contains("sf-drop-dialog-content-active")) {
          if (
            divResize.computedStyleMap().get("left").value <=
            document.body.offsetLeft - 50
          ) {
            if (!document.querySelector(".preview-Drop")) {
              var previewDrop = document.createElement("div");
              previewDrop.style.height = "100vh";
              previewDrop.style.left = "-50px";
              previewDrop.classList.add("preview-Drop");
              previewDrop.style.borderRadius = "0px";
              previewDrop.style.transition = "left .07s linear";
              previewDrop.style.width = "50px";
              previewDrop.style.bottom = "0";
              previewDrop.style.backgroundColor = "#e9206399";
              previewDrop.style.position = "fixed";
              previewDrop.style.zIndex = "500";
              setTimeout(() => {
                previewDrop.style.left = "0";
              }, 7);
              document.body.appendChild(previewDrop);
            }
          } else if (
            divResize.computedStyleMap().get("left").value +
              divResize.computedStyleMap().get("width").value >=
            document.body.offsetLeft + document.body.offsetWidth - 50
          ) {
            document.querySelector(".sf-content-page").style.marginRight =
              "30rem";
            if (!document.querySelector(".preview-drop-dashed")) {
              let div = document.createElement("div");
              div.className =
                "preview-drop-dashed sf-display-flex sf-align-items-center sf-justify-content-center";
              div.style.width = "30rem";
              div.style.height = "100vh";
              div.style.border = "2px dashed salmon";
              div.style.transition = "right .15s ease";
              div.style.right = "-30rem";
              div.style.position = "fixed";
              div.style.top = "0";
              div.innerText = "Solte o Elemento";

              document
                .querySelector(".sf-content-page")
                .parentNode.insertBefore(
                  div,
                  document.querySelector(".sf-content-page").nextSibling
                );
              setTimeout(() => {
                div.style.right = "0rem";
              }, 7);
            }
          } else {
            document.querySelector(".sf-content-page").style.marginRight =
              "initial";
            if (document.querySelector(".preview-drop-dashed")) {
              document
                .querySelector(".preview-drop-dashed")
                .remove(document.querySelector(".preview-drop-dashed"));
            } else if (document.querySelector(".preview-Drop")) {
              document
                .querySelector(".preview-Drop")
                .remove(document.querySelector(".preview-Drop"));
            }
          }
        }
      }

      function stopDrag() {
        document.documentElement.removeEventListener("mousemove", doDrag);
        document.documentElement.removeEventListener("mouseup", stopDrag);
        target.classList.remove("sf-cursor-grabbing");
        target.classList.add("sf-cursor-grab");

        var caseDrop;
        if (divResize.classList.contains("sf-drop-dialog-content-active")) {
          if (
            divResize.computedStyleMap().get("left").value <=
            document.body.offsetLeft - 50
          ) {
            caseDrop = "leftCase";
          } else if (
            divResize.computedStyleMap().get("left").value +
              divResize.computedStyleMap().get("width").value >=
            document.body.offsetLeft + document.body.offsetWidth - 50
          ) {
            caseDrop = "rightCase";
          } else if (
            divResize.computedStyleMap().get("left").value >=
            document.body.offsetLeft + document.body.offsetWidth / 2 - 50
          ) {
            divResize.computedStyleMap().get("left").value >=
              document.body.offsetLeft - 50;
            caseDrop = "CancelLeftCase";
          } else if (
            divResize.computedStyleMap().get("left").value +
              divResize.computedStyleMap().get("width").value <=
            document.body.offsetLeft + document.body.offsetWidth - 50
          ) {
            caseDrop = "CancelrightCase";
          }
        }
        switch (caseDrop) {
          case "leftCase":
            divResize.className +=
              " sf-drop-dialog-content sf-drop-dialog-left";
            break;
          case "CancelLeftCase":
            divResize.classList.remove("sf-drop-dialog");
            divResize.classList.remove("sf-drop-dialog-left");
            break;
          case "rightCase":
            divResize.style.top = "initial";
            divResize.style.bottom = "0";
            divResize.style.left = "initial";
            divResize.style.right = "0";
            divResize.style.height = "100%";
            document.querySelector(".sf-content-page").style.marginRight =
              "30rem";
            break;
          case "CancelrightCase":
            document.querySelector(".sf-content-page").style.marginRight =
              "initial";
            divResize.style.bottom = "initial";
            divResize.style.height = "fit-content";
            break;
        }
      }
    },
    closeAction() {
      this.$emit("callback", null);
      this.$emit("callbackMinimized", "");
    },
    dropAction(e) {
      var elementPai =
        e.currentTarget.parentElement.parentElement.parentElement.parentElement;
      if (elementPai.classList.contains("sf-drop-dialog-content-active")) {
        elementPai.classList.remove("sf-drop-dialog-content-active");
        this.iconVerifyDrop = false;
      } else {
        elementPai.classList.add("sf-drop-dialog-content-active");
        this.iconVerifyDrop = true;
      }
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
        if (this.$refs[element.ref].classList.contains("open"))
          this.$refs[element.ref].classList.remove("open");
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
      this.shadowObject.shadowCor = "#000000";
      this.$emit("callbackAc", this.shadowObject);
    },
    insereTextCreated(valor) {
      this.$emit("callbackTextoInsereBloco", valor);
    },
    editaTextCreated(valor) {
      this.$emit("callbackTextoEditaBloco", valor);
    },
    removeTextCreated(valor) {
      this.$emit("callbackTextoRemoveBloco", valor);
    },
    fontProps(font) {
      this.$emit("callbackFontProps", font);
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
  max-height: 500px;
  overflow-x: hidden;
  overflow-y: auto;
  &.open {
    display: block !important;
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
    height: 100%;
  }
}
.sf-drop-dialog-content-active {
  .sf-menu-content {
    max-height: calc(100% - 87px) !important;
  }
}
</style>
