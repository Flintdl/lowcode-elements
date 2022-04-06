<template>
  <div class="sf-col-10 sf-form-group">
    <label class="sf-label">Tipo de Borda</label>
    <select id="inputBorderTipo" v-model="valor" class="sf-text-capitalize">
      <option hidden>Escolha uma Borda</option>
      <option value="solid">Solid</option>
      <option value="dashed">Dashed</option>
    </select>
  </div>
</template>

<script>
export default {
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
};
</script>

<style></style>
