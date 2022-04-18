<template>
  <div
    :ref="idBlocoTexto"
    class="sf-dialog-content sf-position-fixed sf-bg-white sf-border-radius"
  >
    <div
      @mousedown="moveDialog"
      class="sf-dialog-header sf-border-primary sf-bg-third sf-border-radius-top sf-cursor-grab sf-border-bottom sf-py-2 sf-px-4"
    >
      <span
        class="sf-display-flex sf-align-items-center sf-justify-content-between sf-width-100 sf-font-weight-bold sf-text-md sf-text-white"
      >
        Texto ID {{ idBlocoTexto }}
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
      <input type="text" v-model="valor" />
    </div>
  </div>
</template>

<script>
// Shadow

export default {
  props: {
    cardId: String,
    idBlocoTexto: String,
    textoDescrito: String,
    valorTeste: String,
  },
  computed: {
    valor: {
      get() {
        return this.valorTeste;
      },
      set(valorTeste) {
        this.$emit("callbackTeste", valorTeste);
      },
    },
  },
  components: {},
  data: function () {
    return {
      iconVerify: false,
    };
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
              previewDrop.style.left = "0px";
            }, 7);
            document.body.appendChild(previewDrop);
          }
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
        if (
          divResize.computedStyleMap().get("left").value <=
          document.body.offsetLeft - 50
        ) {
          divResize.style.top = "initial";
          divResize.style.bottom = "0";
          divResize.style.left = "0";
          divResize.style.width = "clamp(150px, 500px, 900px)";
          divResize.style.height = "100%";
        } else {
          divResize.style.width = "clamp(150px, 50ch, 900px)";
          divResize.style.bottom = "initial";
          divResize.style.height = "fit-content";
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
    padding-top: 1.5rem;
    padding-bottom: 1.5rem;
    height: 100%;
  }
}
</style>
