<template>
  <div
    :ref="idBlocoImagem"
    class="sf-dialog-content sf-position-fixed sf-bg-white sf-border-radius"
  >
    <div
      @mousedown="moveDialog"
      class="sf-dialog-header sf-border-primary sf-bg-third sf-border-radius-top sf-cursor-grab sf-border-bottom sf-py-2 sf-px-4"
    >
      <span
        class="sf-display-flex sf-align-items-center sf-justify-content-between sf-width-100 sf-font-weight-bold sf-text-md sf-text-white"
      >
        Configurações de Imagem - Nº: {{ idBlocoImagem }}
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
        <div class="sf-col-8 sf-mt-3 sf-mb-4 sf-mx-auto">
          <div
            class="sf-bg-muted sf-p-2 sf-border-radius sf-display-flex sf-align-items-center sf-justify-content-center"
          >
            <img
              :style="imagemPropsCSS"
              :src="imagemDataBase"
              :title="tituloImagem"
              loading="lazy"
              width="100%"
            />
          </div>
        </div>
        <div class="sf-col-12 sf-mt-3 sf-form-group">
          <label for="input-label-title" class="sf-label"
            >Título da Imagem</label
          >
          <textarea
            id="input-label-title"
            type="text"
            rows="1"
            :value="tituloImagem"
            disabled
          />
        </div>
        <div
          class="sf-col-12 sf-mb-3 sf-justify-content-between sf-display-flex sf-flex-wrap"
        >
          <label class="sf-label">Curva da Borda</label>
          <span class="sf-font-weight-bold sf-text-muted">
            {{ imagemProps.styles.borderRadius }} Pixels
          </span>
          <div
            class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
          >
            <input
              type="range"
              min="0"
              v-model="imagemProps.styles.borderRadius"
              max="50"
              @input="
                porcentagemRange($event);
                onSelectProps();
              "
              class="sf-px-0 sf-text-capitalize input-range-animation"
            />
            <div class="h4-container">
              <div class="h4-subcontainer">
                <h4>0<span></span></h4>
              </div>
            </div>
          </div>
        </div>
        <div
          class="sf-col-12 sf-mb-3 sf-justify-content-between sf-display-flex sf-flex-wrap"
        >
          <label class="sf-label">Tamanho da Imagem</label>
          <span class="sf-font-weight-bold sf-text-muted">
            {{ imagemProps.styles.width }} %
          </span>
          <div
            class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
          >
            <input
              type="range"
              min="0"
              v-model="imagemProps.styles.width"
              max="100"
              @input="
                porcentagemRange($event, 'width');
                onSelectProps();
              "
              class="sf-px-0 sf-text-capitalize input-range-animation"
            />
            <div class="h4-container">
              <div class="h4-subcontainer">
                <h4>0<span></span></h4>
              </div>
            </div>
          </div>
        </div>
        <div
          class="sf-col-12 sf-mb-3 sf-justify-content-between sf-display-flex sf-flex-wrap"
        >
          <label class="sf-label">Altura da Imagem</label>
          <span class="sf-font-weight-bold sf-text-muted">
            {{ imagemProps.styles.height }} Pixels
          </span>
          <div
            class="sf-display-flex sf-flex-wrap sf-width-100 sf-align-items-center"
          >
            <input
              type="range"
              min="0"
              v-model="imagemProps.styles.height"
              max="500"
              @input="
                porcentagemRange($event, 'height');
                onSelectProps();
              "
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
            class="sf-mb-3 sf-text-nowrap sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
          >
            Espaçamento Imagem
            <hr class="sf-width-100 sf-ml-4"
          /></span>
          <div class="sf-row">
            <div class="sf-col-12">
              <label class="sf-label">Margin Automatica</label>
            </div>
            <div class="sf-col-12 sf-display-flex sf-mb-3">
              <label class="switch">
                <input
                  type="checkbox"
                  :checked="imagemProps.styles.margin.auto"
                  v-model="imagemProps.styles.margin.auto"
                  @change="onSelectProps"
                />
                <span class="slider round"></span>
              </label>
            </div>
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
                    :value="imagemProps.styles.margin.marginTop + 'px'"
                    @input="onSelectProps"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectProps"
                    class="input-range-tickes-bulk"
                    v-model="imagemProps.styles.margin.marginTop"
                  />
                </div>
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="imagemProps.styles.margin.marginRight + 'px'"
                    @input="onSelectProps"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectProps"
                    :disabled="imagemProps.styles.margin.auto"
                    class="input-range-tickes-bulk"
                    v-model="imagemProps.styles.margin.marginRight"
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
                    :value="imagemProps.styles.margin.marginBottom + 'px'"
                    @input="onSelectProps"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectProps"
                    class="input-range-tickes-bulk"
                    v-model="imagemProps.styles.margin.marginBottom"
                  />
                </div>
                <div class="sf-container-bulk__itens sf-p-2">
                  <input
                    min="0"
                    max="100"
                    type="text"
                    disabled="true"
                    :value="imagemProps.styles.margin.marginLeft + 'px'"
                    @input="onSelectProps"
                    class="sf-text-center sf-text-md sf-mb-1 sf-font-weight-bold"
                  />
                  <input
                    type="range"
                    @input="onSelectProps"
                    :disabled="imagemProps.styles.margin.auto"
                    class="input-range-tickes-bulk"
                    v-model="imagemProps.styles.margin.marginLeft"
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
      imagemProps: {
        id: this.idBlocoImagem,
        styles: {
          borderRadius: "0",
          width: "50",
          height: "fit-content",
          margin: {
            auto: true,
            marginTop: "0",
            marginRight: "0",
            marginBottom: "0",
            marginLeft: "0",
          },
        },
        // fontMargin: {
        //   marginTop: "0",
        //   marginRight: "0",
        //   marginBottom: "0",
        //   marginLeft: "0",
        // },
      },
    };
  },
  props: {
    cardId: String,
    idBlocoImagem: String,
    textoDescrito: String,
    imagemDataBase: String,
    tituloImagem: String,
  },
  computed: {
    imagemPropsCSS() {
      return {
        "border-radius": this.imagemProps.styles.borderRadius + "px",
      };
    },
  },
  mounted() {
    this.$emit("callbackPropsImage", this.imagemProps);
  },
  methods: {
    onSelectProps() {
      this.$emit("callbackPropsImage", this.imagemProps);
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
    porcentagemRange(e, tipoFuncao) {
      var conversorQuantidade;
      switch (tipoFuncao) {
        case "width":
          conversorQuantidade = "";
          break;
        case "height":
          conversorQuantidade = "/ 5";
          break;
        default:
          conversorQuantidade = "* 2";
          break;
      }
      var h4Capture = e.currentTarget.nextElementSibling;
      h4Capture = h4Capture.querySelector("h4");
      h4Capture.innerHTML = e.currentTarget.value + "<span></span>";
      e.currentTarget.style.filter =
        "hue-rotate(" + e.currentTarget.value + "deg)";
      h4Capture.style.cssText = `transform: translateX(calc(-50% - 20px))
      }); left: calc(${e.currentTarget.value}% ${conversorQuantidade})`;
      h4Capture.style.cssText = `transform: translateX(-50%)
      }); left: calc(${e.currentTarget.value}% ${conversorQuantidade})`;
      h4Capture.style.filter = "hue-rotate(" + e.currentTarget.value + "deg)";
    },
  },
};
</script>

<style lang="scss" scoped>
.switch {
  position: relative;
  display: inline-block;
  width: 50px;
  height: 24px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #e7515a;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 16px;
  width: 16px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: 0.4s;
  transition: 0.4s;
}

input:checked + .slider {
  background-color: #00b751;
}

input:focus + .slider {
  box-shadow: 0 0 1px #00b751;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
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
