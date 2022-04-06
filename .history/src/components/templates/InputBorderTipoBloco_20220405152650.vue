<template>
  <div class="sf-col-10 sf-form-group">
    <label class="sf-label">Tipo de Borda</label>
    <select
      id="inputBorderTipo"
      v-model="control"
      :items="items"
      item-text="label"
      item-value="value"
      @change="onSelectType"
      class="sf-text-capitalize"
    >
      <option v-bind:key="item + 1" v-for="item of items">
        {{ item.value }}
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
        { label: "Solid", value: "solid" },
        { label: "Dashed", value: "dashed" },
      ],
    };
  },
  computed: {
    control: {
      get() {
        var inp = this.valorInput;
        return inp;
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
