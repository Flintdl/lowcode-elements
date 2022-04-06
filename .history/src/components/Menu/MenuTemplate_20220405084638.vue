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
        <a class="sf-logo-link" href="/view.html">
          <img class="logo" :src="image" alt="Low Code Elements" />
        </a>
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
        <ul class="sf-menu-ul">
          <li class="sf-menu-li sf-px-4">
            <div class="sf-row">
              <div class="sf-col-3 sf-form-group">
                <label class="sf-label">Espaçamento</label>
                <input
                  type="number"
                  min="1"
                  max="12"
                  value="2"
                  id="inputGap"
                  v-model="inputGap"
                  class="filter-states sf-text-capitalize"
                  placeholder="Quantidade de Blocos"
                />
              </div>
              <div class="sf-col-4 sf-form-group">
                <label class="sf-label">Quantidade</label>
                <input
                  type="number"
                  value="1"
                  id="inputQuantidade"
                  v-model="inputQuantidade"
                  class="filter-states sf-text-capitalize"
                  placeholder="Quantidade de Blocos"
                />
              </div>
              <div class="sf-col-5 sf-form-group">
                <label class="sf-label">Quantidade por Linhas</label>
                <input
                  type="number"
                  min="1"
                  max="12"
                  value="1"
                  id="inputQuantidadeLinha"
                  v-model="inputQuantidadeLinha"
                  class="filter-states sf-text-capitalize"
                  placeholder="Quantidade Linha"
                />
              </div>
              <div class="sf-col-12 sf-form-group">
                <label class="sf-label">Largura do Bloco</label>
                <div class="sf-input-group">
                  <input
                    type="range"
                    min="10"
                    max="100"
                    value="10"
                    id="inputRange"
                    v-model="inputRange"
                    class="sf-border-right-0 sf-border-radius-right-0 sf-text-capitalize"
                  />
                  <div class="sf-input-group-append">
                    <div
                      class="sf-display-flex sf-align-items-center sf-justify-content-center sf-border-left-0 sf-border-radius sf-border-radius-left-0 sf-px-2 sf-text-white sf-font-weight-bold"
                    >
                      {{ inputRange }}%
                    </div>
                  </div>
                </div>
              </div>
              <div class="sf-col-12 sf-form-group">
                <label class="sf-label">Altura Bloco</label>
                <input
                  type="range"
                  min="10"
                  max="500"
                  id="valorHeight"
                  v-model="valorHeight"
                  class="filter-states sf-text-capitalize"
                />
              </div>
              <div class="sf-col-12 sf-form-group">
                <label class="sf-label">Altura Bloco</label>
                <div>
                  <input
                    id="huey"
                    type="radio"
                    name="drone"
                    v-model="checkValue"
                    value="left"
                    checked
                  />
                  <label for="left"> Left</label>
                </div>

                <div>
                  <input
                    type="radio"
                    id="dewey"
                    name="drone"
                    v-model="checkValue"
                    value="center"
                  />
                  <label for="center"> Center</label>
                </div>

                <div>
                  <input
                    type="radio"
                    id="louie"
                    name="drone"
                    v-model="checkValue"
                    value="right"
                  />
                  <label for="right"> Right</label>
                </div>
              </div>
            </div>
          </li>
        </ul>
      </div>
    </nav>
    <div class="sf-content-page">
      <HomeView
        :valor="parseInt(inputRange)"
        :inputGap="parseInt(inputGap)"
        :valorHeight="parseInt(valorHeight)"
        :quantidade="parseInt(inputQuantidade)"
        :quantidadeLinha="parseInt(inputQuantidadeLinha)"
        :checkValue="checkValue"
      />
    </div>
    <div class="ga-mascara-fundo" @click="mask"></div>
  </div>
</template>

<script>
import HomeView from "@/views/HomeView.vue";
import image from "@/assets/logo-soften.webp";

export default {
  name: "MenuTemplate",
  data: function () {
    return {
      inputQuantidade: (this.inputQuantidade = 1),
      inputQuantidadeLinha: (this.inputQuantidadeLinha = 1),
      inputRange: (this.inputRange = 100),
      valorHeight: (this.valorHeight = 100),
      inputGap: (this.inputGap = 4),
      checkValue: this.checkValue,
      image: image,
    };
  },
  components: {
    HomeView,
  },
  beforeMount() {
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
  methods: {
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
@import "@/assets/scss/style.scss";
@import "@/assets/scss/standard.scss";
@import "@/assets/scss/menu.scss";
</style>
