<template>
  <div class="sf-col sf-form-group">
    <label class="sf-label">Cor</label>
    <input
      type="color"
      id="inputBorderColor"
      v-model="valor"
      class="sf-p-1 sf-text-capitalize"
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
