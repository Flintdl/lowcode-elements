<template>
  <div class="sf-col-4 sf-form-group">
    <label class="sf-label">Quantidade</label>
    <input
      type="number"
      value="1"
      min="1"
      max="30"
      id="inputQuantidade"
      v-model="valor"
      class="filter-states sf-text-capitalize"
      placeholder="Quantidade de Blocos"
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
