<template>
  <div class="sf-col-3 sf-form-group">
    <label class="sf-label">Espaçamento</label>
    <input
      type="number"
      min="1"
      max="12"
      value="2"
      id="inputGap"
      v-model="chave"
      class="filter-states sf-text-capitalize"
      placeholder="Quantidade de Blocos"
    />
  </div>
</template>

<script>
export default {
  props: {
    chave: Number,
  },
};
</script>

<style></style>
