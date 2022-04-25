<template>
  <div class="sf-col sf-form-group">
    <label class="sf-label">Cor do Shadow</label>
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
      id="inputShadowColor"
      v-model="valor"
      ref="novaCor"
      class="sf-p-1 sf-text-capitalize"
    />
  </div>
</template>

<script>
export default {
  props: {
    valorInput: String,
  },
  data: function () {
    return {
      verifyButton: true,
      clipboardVerify: false,
      colorsList: [],
    };
  },
  computed: {
    valor: {
      get() {
        return this.valorInput;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
  beforeMount() {
    if (localStorage.getItem("coresPreset")) {
      this.colorsList = JSON.parse(localStorage.getItem("coresPreset"));
    }
  },
  methods: {
    insertColorArray() {
      if (this.colorsList.length < 5) {
        if (this.colorsList.length <= 0) {
          var date = new Date().getTime();
          var novaCor = { id: date, hexadecimal: this.$refs.novaCor.value };
          this.colorsList.push(novaCor);
        } else {
          var objIndex = this.colorsList.find(
            (obj) => obj.hexadecimal === this.$refs.novaCor.value
          );
          if (!objIndex) {
            date = new Date().getTime();
            novaCor = { id: date, hexadecimal: this.$refs.novaCor.value };
            this.colorsList.push(novaCor);
          }
        }
        localStorage.setItem("coresPreset", JSON.stringify(this.colorsList));
      }
    },
    editColorArray(e) {
      var objIndexCompare = this.colorsList.find(
        (obj) => obj.hexadecimal === this.$refs.novaCor.value
      );
      if (!objIndexCompare) {
        var objIndex = this.colorsList.find(
          (obj) =>
            obj.hexadecimal ==
            e.currentTarget.parentElement.getAttribute("data-color")
        );
        objIndex.hexadecimal = this.$refs.novaCor.value;
        localStorage.setItem("coresPreset", JSON.stringify(this.colorsList));
      }
    },
    removeColorArray(e) {
      this.colorsList = this.colorsList.filter(function (returnableObjects) {
        return (
          returnableObjects.hexadecimal !==
          e.currentTarget.parentElement.getAttribute("data-color")
        );
      });
      localStorage.setItem("coresPreset", JSON.stringify(this.colorsList));
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
