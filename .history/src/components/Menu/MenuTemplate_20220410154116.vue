<template>
  <div>
    <div
      class="ham burger-link-mobile sf-display-flex sf-display-md-none"
      @click="menu"
    >
      <i class="mdi mdi-menu sf-text-xlg sf-text-muted sf-dropdown-action"></i>
    </div>
    <nav class="sf-menu">
      <div class="sf-menu-header">
        <div
          @click="menu"
          class="hamburger-link sf-display-none sf-display-md-block"
        >
          <i
            class="mdi mdi-menu sf-text-xlg sf-text-muted sf-dropdown-action"
          ></i>
        </div>
      </div>
      <div class="sf-menu-body">
        <div class="sf-row sf-px-4" style="visibility: visible">
          <div
            v-for="tab of tabs"
            v-bind:key="tab.label"
            class="sf-col-3 sf-text-center sf-form-group"
          >
            <div
              @click="showContent"
              :dataRef="tab.ref"
              class="sf-btn-primary sf-cursor-pointer sf-p-2 sf-text-white sf-font-weight-bold sf-border-radius"
            >
              {{ tab.label }}
            </div>
          </div>
        </div>
        <ul class="sf-menu-ul sf-menu-content open" ref="container-collapse">
          <li class="sf-menu-li sf-px-4">
            <div class="sf-col-12 sf-form-group">
              <span
                class="sf-text-nowrap sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
              >
                Cor do Container
                <hr class="sf-width-100 sf-ml-4" />
              </span>
            </div>
            <InputContainerColor
              :valorInput="inputContainerColor"
              v-on:callback="getValorContainerCor"
            />
          </li>
        </ul>
        <ul class="sf-menu-ul sf-menu-content" ref="template-collapse">
          <li class="sf-menu-li sf-px-4">
            <div class="sf-row">
              <div class="sf-col-12 sf-form-group">
                <span
                  class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
                >
                  Template
                  <hr class="sf-width-100 sf-ml-4" />
                </span>
              </div>
              <InputGapTemplate
                :valorInput="inputGap"
                v-on:callback="getValorCampo"
              />
              <InputQuantidadeTemplate
                :valorInput="inputQuantidade"
                v-on:callback="getValorCampoQuantidade"
              />
              <InputQuantLinhasTemplate
                :valorInput="inputQuantidadeLinha"
                v-on:callback="getValorCampoQuantidadeLinha"
              />
              <InputLarguraBloco
                :valorInput="inputRange"
                v-on:callback="getValorLarguraBloco"
              />
              <InputAlturaBloco
                :valorInput="valorHeight"
                v-on:callback="getValorAlturaBloco"
              />
            </div>
            <div class="sf-row">
              <div class="sf-col-4 sf-form-group">
                <label class="sf-label">Cor</label>
                <input
                  type="color"
                  id="inputColor"
                  v-model="inputColor"
                  class="sf-p-1 sf-text-capitalize"
                  placeholder="Quantidade de Blocos"
                />
              </div>
            </div>
          </li>
        </ul>
        <ul class="sf-menu-ul sf-menu-content" ref="border-collapse">
          <li class="sf-menu-li sf-px-4">
            <div class="sf-row">
              <div class="sf-col-12 sf-form-group">
                <span
                  class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
                >
                  Bordas
                  <hr class="sf-width-100 sf-ml-4" />
                </span>
              </div>
              <InputBorderRadiusBloco
                :valorInput="inputBorderRadius"
                v-on:callback="getValorRadiusBloco"
              />
              <InputBorderBloco
                :valorInput="inputBorder"
                v-on:callback="getValorBorderBloco"
              />
              <InputBorderCorBloco
                :valorInput="inputBorderColor"
                v-on:callback="getValorBorderCorBloco"
              />
              <InputBorderTipoBloco
                :valorInput="inputBorderTipo"
                v-on:callback="getValorBorderTipoBloco"
              />
            </div>
          </li>
        </ul>
        <ul class="sf-menu-ul sf-menu-content" ref="position-collapse">
          <li class="sf-menu-li sf-px-4">
            <InputPosicaoBloco
              :valorInput="checkValue"
              v-on:callback="getValorPosicaoBloco"
            />
          </li>
        </ul>
      </div>
    </nav>
    <div class="sf-content-page sf-p-0">
      <div
        class="sf-background-resize sf-mx-auto resizable"
        :style="containerCss"
      >
        <div class="resizer">
          <i class="mdi mdi-arrow-split-vertical sf-text-lg"></i>
        </div>
        <HomeView
          :valor="inputRange"
          :inputGap="inputGap"
          :valorHeight="valorHeight"
          :quantidade="parseInt(inputQuantidade)"
          :quantidadeLinha="parseInt(inputQuantidadeLinha)"
          :inputBorderRadius="inputBorderRadius"
          :checkValue="checkValue"
          :inputColor="inputColor"
          :inputBorder="inputBorder"
          :inputBorderColor="inputBorderColor"
          :inputBorderTipo="inputBorderTipo"
        />
      </div>
    </div>
    <div class="ga-mascara-fundo" @click="mask"></div>
  </div>
</template>

<script>
import HomeView from "@/views/HomeView.vue";
import InputGapTemplate from "@/components/templates/InputGapTemplate.vue";
import InputQuantidadeTemplate from "@/components/templates/InputQuantidadeTemplate.vue";
import InputQuantLinhasTemplate from "@/components/templates/InputQuantLinhasTemplate.vue";
import InputLarguraBloco from "@/components/templates/InputLarguraBloco.vue";
import InputAlturaBloco from "@/components/templates/InputAlturaBloco.vue";
import InputBorderRadiusBloco from "@/components/templates/InputBorderRadiusBloco.vue";
import InputBorderBloco from "@/components/templates/InputBorderBloco.vue";
import InputPosicaoBloco from "@/components/templates/InputPosicaoBloco.vue";
import InputBorderCorBloco from "@/components/templates/InputBorderCorBloco.vue";
import InputBorderTipoBloco from "@/components/templates/InputBorderTipoBloco.vue";
import InputContainerColor from "@/components/templates/InputContainerColor.vue";
import image from "@/assets/logo-soften.webp";

export default {
  name: "MenuTemplate",
  data: function () {
    return {
      inputQuantidade: (this.inputQuantidade = "2"),
      inputQuantidadeLinha: (this.inputQuantidadeLinha = "2"),
      inputBorderRadius: (this.inputBorderRadius = "6"),
      inputRange: (this.inputRange = "100"),
      valorHeight: (this.valorHeight = "100"),
      inputGap: (this.inputGap = "4"),
      checkValue: (this.checkValue = "center"),
      inputColor: (this.inputColor = "#34E589"),
      inputBorder: (this.inputBorder = "0"),
      inputBorderColor: (this.inputBorderColor = ""),
      inputBorderTipo: (this.inputBorderTipo = ""),
      inputContainerColor: (this.inputContainerColor = "#FCFCFC"),
      image: image,

      tabs: [
        { label: "Container", ref: "container-collapse" },
        { label: "Template", ref: "template-collapse" },
        { label: "Bordas", ref: "border-collapse" },
        { label: "Posições", ref: "position-collapse" },
      ],
    };
  },
  components: {
    HomeView,
    InputGapTemplate,
    InputQuantidadeTemplate,
    InputQuantLinhasTemplate,
    InputLarguraBloco,
    InputAlturaBloco,
    InputPosicaoBloco,
    InputBorderRadiusBloco,
    InputBorderBloco,
    InputBorderCorBloco,
    InputBorderTipoBloco,
    InputContainerColor,
  },
  beforeMount() {
    function attr() {
      var p = document.querySelector(".sf-background-resize");
      var resizer = document.querySelector(".resizer");
      resizer.addEventListener("mousedown", initDrag, false);

      var startX, startWidth;

      function initDrag(e) {
        startX = e.clientX;
        startWidth = parseInt(
          document.defaultView.getComputedStyle(p).width,
          10
        );
        document.documentElement.addEventListener("mousemove", doDrag, false);
        document.documentElement.addEventListener("mouseup", stopDrag, false);
      }

      function doDrag(e) {
        p.style.width = startWidth - e.clientX + startX + "px";
      }

      function stopDrag() {
        document.documentElement.removeEventListener(
          "mousemove",
          doDrag,
          false
        );
        document.documentElement.removeEventListener(
          "mouseup",
          stopDrag,
          false
        );
      }
    }

    setTimeout(() => {
      attr();
    }, 1000);

    if (window.innerWidth >= 768) {
      if (localStorage.getItem("menu") === "fechado") {
        document.body.classList.add("menu-fechado");
      } else {
        document.body.classList.remove("menu-fechado");
      }
    }

    (function () {
      var current = window.location.pathname.split("")[0];
      if (current === "") return;
      var menuItems = document.querySelectorAll(".sf-menu-list-link");
      for (var i = 0, len = menuItems.length; i < len; i++) {
        if (menuItems[i].getAttribute("href").indexOf(current) !== -1) {
          menuItems[i].className += " active-nolink";
          menuItems[i].nextElementSibling.className += " active-nolink";
        }
      }
    })();
  },
  computed: {
    containerCss() {
      return {
        "background-color": this.inputContainerColor,
        height: "100vh",
      };
    },
  },
  methods: {
    showContent(e) {
      var referenciaItem = this.$refs[e.currentTarget.getAttribute("dataRef")];
      for (let i = 0; i < this.tabs.length; i++) {
        const element = this.tabs[i];
        if (this.$refs[element.ref]) {
          if (this.$refs[element.ref].classList.contains("open"))
            this.$refs[element.ref].classList.remove("open");
        }
      }
      if (referenciaItem) {
        referenciaItem.classList.toggle("open");
        if (!referenciaItem.classList.contains("open")) {
          referenciaItem.classList.remove("open");
        } else {
          referenciaItem.classList.add("open");
        }
      }
    },
    menu() {
      if (window.innerWidth >= 768) {
        document.body.classList.toggle("menu-fechado");
        if (document.body.classList.contains("menu-fechado")) {
          localStorage.setItem("menu", "fechado");
        } else {
          localStorage.removeItem("menu");
        }
      } else {
        document.body.classList.toggle("menu-mobile-aberto");
      }
    },
    mask() {
      document.body.classList.remove("menu-mobile-aberto");
    },
    getValorCampo(valor) {
      this.inputGap = valor;
    },
    getValorCampoQuantidade(valor) {
      this.inputQuantidade = valor;
    },
    getValorCampoQuantidadeLinha(valor) {
      this.inputQuantidadeLinha = valor;
    },
    getValorLarguraBloco(valor) {
      this.inputRange = valor;
    },
    getValorAlturaBloco(valor) {
      this.valorHeight = valor;
    },
    getValorRadiusBloco(valor) {
      this.inputBorderRadius = valor;
    },
    getValorBorderBloco(valor) {
      this.inputBorder = valor;
    },
    getValorBorderCorBloco(valor) {
      this.inputBorderColor = valor;
    },
    getValorBorderTipoBloco(valor) {
      this.inputBorderTipo = valor;
    },
    getValorContainerCor(valor) {
      this.inputContainerColor = valor;
    },
    getValorPosicaoBloco(valor) {
      this.checkValue = valor;
    },
  },
};

// function dropdown(menu) {
//   var dropdowns = document.getElementsByClassName("sf-dropdown-menu");
//   var i;
//   for (i = 0; i < dropdowns.length; i++) {
//     var openDropdown = dropdowns[i];
//     if (openDropdown.classList.contains("show")) {
//       openDropdown.classList.remove("show");
//     }
//   }
//   document.querySelector(menu).classList.toggle("show");
// }
</script>

<style lang="scss">
.sf-background-resize {
  padding: 3.6rem 2rem;
  width: 100%;
}

.sf-background-resize.resizable {
  position: relative;
  min-width: 20vw;
  max-width: 100%;
}

.sf-background-resize .resizer {
  box-shadow: 1px 2px 11px 0 rgba(0, 0, 0, 0.1);
  width: 30px;
  transform: translateY(-50%);
  height: 30px;
  border-radius: 50px;
  position: absolute;
  background-color: #d9d9d9;
  left: -15px;
  top: 50%;
  z-index: 999;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #272727;
  cursor: pointer;
}

@import "@/assets/scss/style.scss";
@import "@/assets/scss/standard.scss";
@import "@/assets/scss/menu.scss";
</style>
