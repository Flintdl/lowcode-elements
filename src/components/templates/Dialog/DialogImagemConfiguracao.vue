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
          width: "100",
          height: "200",
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
