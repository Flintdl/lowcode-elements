<template>
  <div class="sf-col sf-form-group">
    <label class="sf-label">Cor do Shadow</label>
    <input
      type="color"
      id="inputShadowColor"
      v-model="valor"
      class="sf-p-1 sf-text-capitalize"
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
