<template>
  <div class="sf-col-12 sf-form-group">
    <label class="sf-label">Inserir Texto</label>
    <div
      class="sf-display-flex sf-align-items-center sf-justify-content-between sf-text-preview sf-border-2 sf-border-radius sf-border-dashed sf-px-3 sf-py-2 sf-form-group"
      v-for="(texto, i) of textLabel"
      v-bind:key="i"
      @blur="editarTexto"
      :id="texto.id"
      contenteditable="true"
    >
      {{ texto.texto }}
      <i
        @click="removeTexto"
        contenteditable="false"
        class="mdi mdi-close sf-text-lg sf-cursor-pointer sf-text-danger sf-dropdown-action"
      ></i>
    </div>
    <!-- v-model="valor" -->
    <input
      type="text"
      ref="novoTexto"
      class="sf-text-capitalize sf-form-group"
      placeholder="Novo Texto"
    />
    <button class="sf-btn sf-btn-primary" @click="insertTexto">
      Novo Texto
    </button>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      textLabel: [],
    };
  },
  props: {
    valorInput: Array,
  },
  methods: {
    insertTexto() {
      var date = new Date().getTime();
      var novoTexto = { id: date, texto: this.$refs.novoTexto.value };
      this.textLabel.push(novoTexto);
      this.$refs.novoTexto.value = "";
      this.$refs.novoTexto.focus();
    },
    editarTexto(e) {
      var objIndex = this.textLabel.findIndex(
        (obj) => obj.id == e.currentTarget.id
      );
      this.textLabel[objIndex].texto = e.currentTarget.innerText;
    },
    removeTexto(e) {
      this.textLabel = this.textLabel.filter(function (returnableObjects) {
        return (
          returnableObjects.texto !== e.currentTarget.parentElement.innerText
        );
      });
    },
    renderTexto() {},
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
};
</script>

<style></style>
