<template>
  <div class="sf-col-10 sf-form-group">
    <label class="sf-label">Tipo de Borda</label>
    <input
      type="number"
      min="1"
      max="12"
      value="2"
      id="inputGap"
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
