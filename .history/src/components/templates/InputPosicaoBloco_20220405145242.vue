<template>
  <div class="sf-row">
    <div class="sf-col-12 sf-form-group">
      <span
        class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
      >
        Bordas
        <hr class="sf-width-100 sf-ml-4" />
      </span>
    </div>
    <div class="sf-col-auto sf-form-group">
      <input
        id="start"
        type="radio"
        name="positionDivs"
        v-model="valor"
        value="start"
        checked
      />
      <label for="start"> Esquerda</label>
    </div>

    <div class="sf-col-auto sf-form-group">
      <input
        type="radio"
        id="center"
        name="positionDivs"
        v-model="valor"
        value="center"
      />
      <label for="center"> Centro</label>
    </div>

    <div class="sf-col-auto sf-form-group">
      <input
        type="radio"
        id="end"
        name="positionDivs"
        v-model="valor"
        value="end"
      />
      <label for="end"> Final</label>
    </div>
    <div class="sf-col-auto sf-form-group">
      <input
        type="radio"
        id="between"
        name="positionDivs"
        v-model="valor"
        value="between"
      />
      <label for="between"> Laterais</label>
    </div>
    <div class="sf-col-auto sf-form-group">
      <input
        type="radio"
        id="around"
        name="positionDivs"
        v-model="valor"
        value="around"
      />
      <label for="around"> Meio</label>
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
