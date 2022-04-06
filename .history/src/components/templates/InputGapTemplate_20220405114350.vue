<template>
  <div class="sf-col-3 sf-form-group">
    <label class="sf-label">Espaçamento</label>
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
    valorInput: Number,
  },
  computed: {
    valor: {
      get() {
        var a = this.valorInput;
        console.log(String(a));
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
