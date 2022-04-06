<template>
  <div class="sf-col-3 sf-form-group">
    <label class="sf-label">Quantidade</label>
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
