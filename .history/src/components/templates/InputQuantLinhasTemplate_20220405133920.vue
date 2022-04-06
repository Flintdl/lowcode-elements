<template>
  <div class="sf-col-5 sf-form-group">
    <label class="sf-label">Quantidade por Linhas</label>
    <input
      type="number"
      min="1"
      max="12"
      value="1"
      id="inputQuantidadeLinha"
      v-model="valor"
      class="filter-states sf-text-capitalize"
      placeholder="Quantidade Linha"
    />
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
