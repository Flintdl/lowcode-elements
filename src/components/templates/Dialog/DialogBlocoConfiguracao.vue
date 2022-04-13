<template>
  <div
    :ref="idBloco"
    class="sf-dialog-content sf-position-fixed sf-bg-white sf-border-radius"
  >
    <div
      @mousedown="moveDialog"
      class="sf-dialog-header sf-bg-primary sf-border-radius-top sf-cursor-grab sf-border-bottom sf-py-2 sf-px-3"
    >
      <span
        class="sf-display-flex sf-align-items-center sf-justify-content-between sf-width-100 sf-font-weight-bold sf-text-md sf-text-white"
      >
        Bloco {{ cardId }}
        <i
          @click="closeAction"
          class="mdi mdi-close sf-text-lg sf-cursor-pointer sf-text-white sf-dropdown-action"
        ></i>
      </span>
    </div>
    <div class="sf-dialog-body sf-py-2 sf-px-3">
      <div class="sf-btn-select-content" style="visibility: visible">
        <div class="sf-display-flex sf-flex-wrap">
          <div
            v-for="tab of tabsDialog"
            v-bind:key="tab.label"
            class="sf-text-center"
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
        class="sf-menu-ul sf-menu-content"
        :id="'container-collapse_' + cardId"
        :ref="'container-collapse_' + cardId"
      >
        Arroz1
      </div>
      <div
        class="sf-menu-ul sf-menu-content"
        :id="'template-collapse_' + cardId"
        :ref="'template-collapse_' + cardId"
      >
        Arroz 2
      </div>
      <div
        class="sf-menu-ul sf-menu-content"
        :id="'border-collapse_' + cardId"
        :ref="'border-collapse_' + cardId"
      >
        Arroz3
      </div>
      <div
        class="sf-menu-ul sf-menu-content"
        :id="'position-collapse_' + cardId"
        :ref="'position-collapse_' + cardId"
      >
        Arroz 4
      </div>
    </div>
    <div class="sf-dialog-footer sf-py-2 sf-px-3 sf-border-top">
      Dialog Footer
    </div>
    <!-- <svg id="svg">
      <line ref="line" id="line" />
    </svg> -->
    <div ref="line" class="line"></div>
  </div>
</template>

<script>
export default {
  props: {
    cardId: String,
    idBloco: String,
  },
  data: function () {
    return {
      tabsDialog: [
        {
          label: "Container",
          ref: `container-collapse_${this.cardId}`,
          container: "initial",
        },
        { label: "Template", ref: `template-collapse_${this.cardId}` },
        { label: "Bordas", ref: `border-collapse_${this.cardId}` },
        { label: "Posições", ref: `position-collapse_${this.cardId}` },
      ],
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
      }

      function stopDrag() {
        document.documentElement.removeEventListener("mousemove", doDrag);
        document.documentElement.removeEventListener("mouseup", stopDrag);
        target.classList.remove("sf-cursor-grabbing");
        target.classList.add("sf-cursor-grab");
      }
    },
    closeAction() {
      this.$emit("callback", null);
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
