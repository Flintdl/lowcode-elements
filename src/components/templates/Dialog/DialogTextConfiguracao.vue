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
        Configurações de Texto - Nº: {{ idBlocoTexto }}
        <div>
          <i
            @click="closeAction"
            class="mdi mdi-close sf-text-lg sf-cursor-pointer sf-text-white sf-dropdown-action"
          ></i>
        </div>
      </span>
    </div>
    <div class="sf-dialog-body">
      <div class="sf-row sf-px-4 sf-py-3">
        <div class="sf-col-12 sf-form-group">
          <p
            class="sf-my-2 sf-text-lg sf-position-relative sf-border-radius sf-px-3 sf-py-5 sf-bg-muted sf-text-center"
          >
            <label
              style="top: 0; left: 0.5rem"
              class="sf-position-absolute sf-text-sm sf-display-block"
            >
              <sub><i>Preview</i></sub>
            </label>
            <span
              :style="{
                'font-style': fontProps.style,
                'text-decoration': fontProps.decoration,
                'text-transform': fontProps.transform,
                'font-family': fontProps.family,
              }"
            >
              {{ textoDescrito }}
            </span>
          </p>
        </div>
        <div
          class="sf-col-12 sf-mb-3 sf-justify-content-between sf-display-flex sf-flex-wrap"
        >
          <label class="sf-label">Tamanho da Fonte</label>
          <span class="sf-font-weight-bold sf-text-muted">
            {{ fontProps.size }} Rem
          </span>
          <div
            class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
          >
            <input
              type="range"
              min="0"
              max="10"
              step="0.5"
              @input="
                porcentagemRange($event);
                onSelectType();
              "
              v-model="fontProps.size"
              class="sf-px-0 sf-text-capitalize input-range-animation"
            />
            <div class="h4-container">
              <div class="h4-subcontainer">
                <h4>0<span></span></h4>
              </div>
            </div>
          </div>
        </div>
        <div class="sf-col-12 sf-mb-3">
          <span
            class="sf-text-nowrap sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
          >
            Configurações de Fonte
            <hr class="sf-width-100 sf-ml-4"
          /></span>
        </div>
        <div class="sf-col-12 sf-mb-3">
          <label class="sf-label">Tipo de Fonte</label>
          <select
            v-model="fontProps.family"
            @change="onSelectType"
            class="sf-text-capitalize"
          >
            <option
              v-bind:key="item.value"
              :value="item.value"
              :disabled="item.disable"
              v-for="item of fontFamily"
            >
              {{ item.label }}
            </option>
          </select>
        </div>
        <div class="sf-col-12 sf-mb-3">
          <label class="sf-label">Decorar Fonte</label>
          <div class="sf-display-flex">
            <li
              v-for="(texto, i) of fontDecoration"
              v-bind:key="i"
              class="sf-display-flex sf-cursor-pointer"
              ref="decorationFocus"
              :title="texto.value"
              @click="onSelectDecoration($event, texto.value)"
            >
              <i
                :class="
                  'mdi mdi-' +
                  texto.mdi +
                  ' ' +
                  texto.size +
                  ' sf-px-3 sf-py-1 sf-border-radius sf-mr-2 sf-bg-muted ' +
                  texto.initial
                "
              ></i>
            </li>
          </div>
        </div>
        <div class="sf-col sf-mb-3">
          <label class="sf-label">Transformar Fonte</label>
          <div class="sf-display-flex">
            <li
              v-for="(texto, i) of fontTransform"
              v-bind:key="i"
              class="sf-display-flex sf-cursor-pointer"
              ref="transformFocus"
              :title="texto.value"
              @click="onSelectTransform($event, texto.value)"
            >
              <i
                :class="
                  'mdi mdi-' +
                  texto.mdi +
                  ' ' +
                  texto.size +
                  ' sf-px-3 sf-py-1 sf-border-radius sf-mr-2 sf-bg-muted ' +
                  texto.initial
                "
              ></i>
            </li>
          </div>
        </div>
        <div class="sf-col-4 sf-mb-3">
          <label class="sf-label sf-text-right sf-display-block"
            >Estilo da Fonte</label
          >
          <div class="sf-display-flex sf-justify-content-end">
            <li
              v-for="(texto, i) of items"
              v-bind:key="i"
              class="sf-display-flex sf-cursor-pointer"
              ref="styleFocus"
              :title="texto.value"
              @click="onSelectStyle($event, texto.value)"
            >
              <i
                :class="
                  'mdi mdi-' +
                  texto.mdi +
                  ' ' +
                  texto.size +
                  ' sf-px-3 sf-py-1 sf-border-radius sf-bg-muted ' +
                  texto.margin +
                  ' ' +
                  texto.initial
                "
              ></i>
            </li>
          </div>
        </div>
        <div class="sf-col-12 sf-mb-3">
          <span
            class="sf-mb-3 sf-text-nowrap sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
          >
            Espaçamento Fonte
            <hr class="sf-width-100 sf-ml-4"
          /></span>
          <div class="sf-row">
            <div class="sf-col-12">
              <div
                class="sf-container-bulk sf-display-grid sf-px-2 sf-py-4 sf-border-bulk sf-border-2 sf-border-radius"
              >
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="fontProps.fontMargin.marginTop + 'px'"
                    @input="onSelectType"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectType"
                    class="input-range-tickes-bulk"
                    v-model="fontProps.fontMargin.marginTop"
                  />
                </div>
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="fontProps.fontMargin.marginRight + 'px'"
                    @input="onSelectType"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectType"
                    class="input-range-tickes-bulk"
                    v-model="fontProps.fontMargin.marginRight"
                  />
                </div>
                <div
                  ref="sf-container-bulk__itens-preview"
                  class="sf-container-bulk__itens sf-py-3 sf-px-5 sf-border-radius sf-border-info sf-border-2 sf-text-center"
                >
                  <i
                    class="mdi mdi-link-variant sf-text-info sf-text-xl sf-text-white sf-dropdown-action"
                  ></i>
                </div>
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="fontProps.fontMargin.marginBottom + 'px'"
                    @input="onSelectType"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectType"
                    class="input-range-tickes-bulk"
                    v-model="fontProps.fontMargin.marginBottom"
                  />
                </div>
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="fontProps.fontMargin.marginLeft + 'px'"
                    @input="onSelectType"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectType"
                    class="input-range-tickes-bulk"
                    v-model="fontProps.fontMargin.marginLeft"
                  />
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// Shadow

export default {
  data: function () {
    return {
      iconVerify: false,
      items: [
        {
          mdi: "format-clear",
          value: "normal",
          size: "sf-text-lg",
          margin: "sf-mr-2",
          initial: "sf-text-info",
        },
        {
          mdi: "format-italic",
          value: "italic",
          size: "sf-text-lg",
        },
      ],
      fontFamily: [
        { label: "Nunito Sans", value: "'Nunito Sans', sans-serif" },
        { label: "Times New Roman", value: "'Times New Roman', Times, serif" },
        { label: "Arial", value: "Arial, sans-serif" },
        { label: "Monospace", value: "monospace, sans-serif" },
        { label: "Helvetica", value: "Helvetica, sans-serif" },
        { label: "Verdana", value: "'Verdana', sans-serif" },
        { label: "Tahoma", value: "'Tahoma', sans-serif" },
        { label: "Trebuchet", value: "'Trebuchet', sans-serif" },
        { label: "Georgia", value: "'Georgia', serif" },
        { label: "Garamond", value: "Garamond, serif" },
        { label: "Courier New", value: "'Courier New', monospace" },
        { label: "Brush Script MT", value: "'Brush Script MT', cursive" },
      ],
      fontDecoration: [
        {
          mdi: "format-clear",
          value: "initial",
          size: "sf-text-lg",
          initial: "sf-text-info",
        },
        { mdi: "format-underline", value: "underline", size: "sf-text-lg" },
        { mdi: "format-overline", value: "overline", size: "sf-text-lg" },
        {
          mdi: "format-strikethrough-variant",
          value: "line-through",
          size: "sf-text-lg",
        },
        {
          mdi: "format-letter-matches",
          value: "underline overline",
          size: "sf-text-lg",
        },
      ],
      fontTransform: [
        {
          mdi: "format-clear",
          value: "initial",
          size: "sf-text-lg",
          initial: "sf-text-info",
        },
        {
          mdi: "format-size",
          value: "capitalize",
          size: "sf-text-lg",
        },
        {
          mdi: "format-letter-case-upper",
          value: "uppercase",
          size: "sf-text-lg",
        },
        {
          mdi: "format-letter-case-lower",
          value: "lowercase",
          size: "sf-text-lg",
        },
      ],
      fontProps: {
        id: this.idBlocoTexto,
        style: "normal",
        decoration: "initial",
        transform: "initial",
        family: "'Nunito Sans', sans-serif",
        size: "1",
        fontMargin: {
          marginTop: "0",
          marginRight: "0",
          marginBottom: "0",
          marginLeft: "0",
        },
      },
    };
  },
  props: {
    cardId: String,
    idBlocoTexto: String,
    textoDescrito: String,
  },
  methods: {
    onSelectType() {
      this.$emit("callbackProps", this.fontProps);
    },
    onSelectDecoration(e, decoration) {
      var refsGet = this.$refs.decorationFocus;
      for (let index = 0; index < refsGet.length; index++) {
        const element = refsGet[index];
        if (element.querySelector("i").classList.contains("sf-text-info"))
          element.querySelector("i").classList.remove("sf-text-info");
      }
      e.currentTarget.querySelector("i").classList.add("sf-text-info");

      this.fontProps.decoration = decoration;
      this.$emit("callbackProps", this.fontProps);
    },
    onSelectStyle(e, style) {
      var refsGet = this.$refs.styleFocus;
      for (let index = 0; index < refsGet.length; index++) {
        const element = refsGet[index];
        if (element.querySelector("i").classList.contains("sf-text-info"))
          element.querySelector("i").classList.remove("sf-text-info");
      }
      e.currentTarget.querySelector("i").classList.add("sf-text-info");

      this.fontProps.style = style;
      this.$emit("callbackProps", this.fontProps);
    },
    onSelectTransform(e, transform) {
      var refsGet = this.$refs.transformFocus;
      for (let index = 0; index < refsGet.length; index++) {
        const element = refsGet[index];
        if (element.querySelector("i").classList.contains("sf-text-info"))
          element.querySelector("i").classList.remove("sf-text-info");
      }
      e.currentTarget.querySelector("i").classList.add("sf-text-info");

      this.fontProps.transform = transform;
      this.$emit("callbackProps", this.fontProps);
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

        if (divResize.classList.contains("sf-drop-dialog")) {
          divResize.classList.remove("sf-drop-dialog");
          divResize.classList.remove("sf-drop-dialog-left");
        }

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
          divResize.className += " sf-drop-dialog sf-drop-dialog-left";
        } else {
          divResize.classList.remove("sf-drop-dialog");
          divResize.classList.remove("sf-drop-dialog-left");
          return;
        }
      }
    },
    closeAction() {
      this.$emit("callbackCloseDialog", null);
      this.$emit("callbackMinimized", "");
    },
    porcentagemRange(e) {
      var h4Capture = e.currentTarget.nextElementSibling;
      h4Capture = h4Capture.querySelector("h4");
      h4Capture.innerHTML = e.currentTarget.value + "<span></span>";
      e.currentTarget.style.filter =
        "hue-rotate(" + e.currentTarget.value + "deg)";
      h4Capture.style.cssText = `transform: translateX(calc(-50% - 20px))
      }); left: calc(${e.currentTarget.value}% * 10)`;
      h4Capture.style.cssText = `transform: translateX(-50%)
      }); left: calc(${e.currentTarget.value}% * 10)`;
      h4Capture.style.filter = "hue-rotate(" + e.currentTarget.value + "deg)";
    },
  },
};
</script>

<style lang="scss" scoped>
.sf-container-bulk {
  .sf-container-bulk__itens {
    input {
      width: 70%;
      margin: auto;
      padding: 0;
      background-color: #f0f4fb;
      border: none;
      height: 34px;
    }
    &:nth-child(1) {
      grid-column: 1;
      grid-row: 1;
    }
    &:nth-child(2) {
      grid-column: 4;
      grid-row: 1;
    }
    &:nth-child(3) {
      grid-column: 2 / span 2;
      margin: -1rem 0;
      grid-row: 2;
    }
    &:nth-child(4) {
      grid-column: 1;
      grid-row: 3;
    }
    &:nth-child(5) {
      grid-column: 4;
      grid-row: 3;
    }
  }
}
</style>
