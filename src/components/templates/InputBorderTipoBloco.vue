<template>
  <div class="sf-col-10 sf-form-group">
    <label class="sf-label">Tipo de Borda</label>
    <select
      id="inputBorderTipo"
      v-model="control"
      :items="items"
      @change="onSelectType"
      class="sf-text-capitalize"
    >
      <option
        v-bind:key="item.value"
        :value="item.value"
        :disabled="item.disable"
        v-for="item of items"
      >
        {{ item.label }}
      </option>
    </select>
  </div>
</template>

<script>
export default {
  props: {
    valorInput: String,
  },
  data: () => {
    return {
      items: [
        { label: "Nenhum Valor", value: "initial", disable: true },
        { label: "Solid", value: "solid" },
        { label: "Dashed", value: "dashed" },
      ],
    };
  },
  computed: {
    control: {
      get() {
        return this.valorInput;
      },
      set(valorInput) {
        this.$emit("callback", valorInput);
      },
    },
  },
  methods: {
    onSelectType(event) {
      this.$emit("change", event);
    },
  },
};
</script>

<style></style>
