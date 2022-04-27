<template>
  <div class="sf-col sf-form-group">
    <label class="sf-label">Background do Bloco</label>
    <ul class="sf-colors-list sf-display-flex sf-mb-2 sf-align-items-center">
      <TransitionGroup name="list-color" class="sf-display-flex">
        <li
          v-for="(color, i) of colorsList"
          v-bind:key="i"
          class="sf-colors-itens sf-cursor-pointer sf-mr-2"
        >
          <span
            :style="'background-color:' + color.hexadecimal"
            :data-color="color.hexadecimal"
            :title="'Hexadecimal: ' + color.hexadecimal"
            @click="setValueColor"
            :ref="color.id"
            class="sf-colors-itens-input sf-display-block sf-p-3 sf-position-relative"
          >
            <label
              class="sf-remove-color sf-position-absolute"
              @click="removeColorArray"
            >
              <i class="mdi mdi-close sf-text-sm sf-text-standard"></i>
            </label>
            <label
              class="sf-edit-color sf-position-absolute"
              @click="editColorArray"
            >
              <i class="mdi mdi-pencil sf-text-sm sf-text-standard"></i>
            </label>
          </span>
        </li>
      </TransitionGroup>
      <li
        v-if="verifyButton"
        @click="insertColorArray"
        class="sf-colors-itens sf-colors-itens-new sf-cursor-pointer"
      >
        <span
          class="sf-colors-itens-input sf-display-block sf-px-2 sf-py-1 sf-position-relative"
        >
          <i class="mdi mdi-plus sf-text-lg sf-text-standard"></i>
        </span>
      </li>
      <li
        class="sf-colors-itens sf-colors-itens-new sf-ml-auto sf-position-relative"
      >
        <div
          v-if="clipboardVerify"
          ref="clipboardMensagem"
          class="sf-clipboard-message sf-py-1 sf-display-block sf-border-radius sf-text-white sf-text-center sf-position-absolute"
        >
          Copiado
        </div>
        <label
          class="sf-text-md sf-py-1 sf-px-2 sf-border-radius sf-text-default sf-bg-muted sf-font-weight-extra-bold sf-cursor-pointer"
          style="height: fit-content; width: 100px"
          @click="clipboardCopy"
        >
          Hex: {{ valorInput }}
        </label>
      </li>
    </ul>
    <input
      type="color"
      id="inputColor"
      v-model="valor"
      ref="novaCorBloco"
      class="sf-p-1 sf-text-capitalize"
    />
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      verifyButton: true,
      clipboardVerify: false,
      colorsList: [],
    };
  },
  props: {
    valorInput: String,
  },
  computed: {
    valor: {
      get() {
        var inp = this.valorInput;
        return inp;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
  beforeMount() {
    if (localStorage.getItem("coresPresetBloco")) {
      this.colorsList = JSON.parse(localStorage.getItem("coresPresetBloco"));
    }
  },
  methods: {
    insertColorArray() {
      if (this.colorsList.length < 5) {
        if (this.colorsList.length <= 0) {
          var date = new Date().getTime();
          var novaCor = {
            id: date,
            hexadecimal: this.$refs.novaCorBloco.value,
          };
          this.colorsList.push(novaCor);
        } else {
          var objIndex = this.colorsList.find(
            (obj) => obj.hexadecimal === this.$refs.novaCorBloco.value
          );
          if (!objIndex) {
            date = new Date().getTime();
            novaCor = { id: date, hexadecimal: this.$refs.novaCorBloco.value };
            this.colorsList.push(novaCor);
          }
        }
        localStorage.setItem(
          "coresPresetBloco",
          JSON.stringify(this.colorsList)
        );
      }
    },
    editColorArray(e) {
      var objIndexCompare = this.colorsList.find(
        (obj) => obj.hexadecimal === this.$refs.novaCorBloco.value
      );
      if (!objIndexCompare) {
        var objIndex = this.colorsList.find(
          (obj) =>
            obj.hexadecimal ==
            e.currentTarget.parentElement.getAttribute("data-color")
        );
        objIndex.hexadecimal = this.$refs.novaCorBloco.value;
        localStorage.setItem(
          "coresPresetBloco",
          JSON.stringify(this.colorsList)
        );
      }
    },
    removeColorArray(e) {
      this.colorsList = this.colorsList.filter(function (returnableObjects) {
        return (
          returnableObjects.hexadecimal !==
          e.currentTarget.parentElement.getAttribute("data-color")
        );
      });
      localStorage.setItem("coresPresetBloco", JSON.stringify(this.colorsList));
    },
    setValueColor(e) {
      this.$emit("callback", e.currentTarget.getAttribute("data-color"));
    },
    clipboardCopy() {
      this.clipboardVerify = true;
      navigator.clipboard.writeText(this.valorInput);
      setTimeout(() => {
        this.clipboardVerify = false;
      }, 1500);
    },
    porcentagemRange(e, tipoFuncao) {
      var conversorQuantidade;
      switch (tipoFuncao) {
        case "width":
          conversorQuantidade = "";
          break;
        case "height":
          conversorQuantidade = "/ 15";
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
  created() {
    this.$watch("colorsList", () => {
      if (this.colorsList.length >= 5) {
        this.verifyButton = false;
      } else {
        this.verifyButton = true;
      }
    });
  },
};
</script>

<style lang="scss" scoped>
.sf-clipboard-message {
  background-color: #5dc25b;
  top: calc(-100% - 24px);
  width: 100%;
  left: 0;

  &:after {
    content: "";
    position: absolute;
    display: inline-block;
    border: 7px solid transparent;
    border-top: 8px solid #5dc25b;
    border-bottom: 0 none;
    bottom: -7px;
    left: 50%;
    transform: translateX(-50%);
  }
}
.sf-remove-color,
.sf-edit-color {
  background-color: #dcdee7;
  border: 2px solid #dcdee7;
  border-radius: 50px;
  right: -8px;
  top: -6px;
  padding: 0.1rem;
  height: 22px;
  display: none;
  align-items: center;
  justify-content: center;
}
.sf-edit-color {
  right: inherit;
  left: -8px;
}
.sf-colors-list {
  .sf-colors-itens {
    .sf-colors-itens-input {
      border-radius: 50px;
      border: 2px solid #dcdee7;
      box-shadow: 1px 2px 11px 0 rgba(0, 0, 0, 0.1);
      &:hover {
        .sf-remove-color,
        .sf-edit-color {
          display: flex;
        }
      }
    }
    &.sf-colors-itens-new {
      span {
        padding: 0.063rem 0.375rem !important;
      }
    }
  }
}
.list-color-enter-active {
  animation: bounce-in-list 0.3s;
}
.list-color-leave-active {
  animation: bounce-in-list 0.2s reverse;
}
@keyframes bounce-in-list {
  0% {
    transform: scale(0.6);
  }
  50% {
    transform: scale(1.05);
  }
  100% {
    transform: scale(1);
  }
}
</style>
