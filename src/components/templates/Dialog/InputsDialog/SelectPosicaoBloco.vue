<template>
  <div class="sf-col-12 sf-mb-3">
    <label class="sf-label">Alinhamento</label>
    <ul class="sf-display-flex">
      <li
        v-for="(posicao, i) of typeOfAlinhamento"
        v-bind:key="i"
        class="sf-display-flex sf-cursor-pointer"
        ref="posicaoTypeRef"
        :title="posicao.value"
        @click="onSelectTypeOfTag($event, posicao.value)"
      >
        <i
          :class="
            'mdi mdi-' +
            posicao.mdi +
            ' ' +
            posicao.margin +
            ' sf-px-3 sf-py-1 sf-border-radius sf-text-lg sf-text-muted ' +
            posicao.class
          "
        ></i>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      typeOfAlinhamento: [
        {
          mdi: "format-align-justify",
          value: "0 auto",
          margin: "sf-mr-2",
          class: "sf-bg-disable sf-text-info",
        },
        {
          mdi: "format-align-right",
          value: "0 0 0 auto",
          margin: "sf-mr-2",
          class: "sf-bg-muted",
        },
        {
          mdi: "format-align-left",
          value: "0 auto 0 0",
          margin: "sf-mr-2",
          class: "sf-bg-muted",
        },
      ],
    };
  },
  props: {
    valorInput: String,
  },
  methods: {
    onSelectTypeOfTag(e, valorPosicao) {
      var refsGet = this.$refs.posicaoTypeRef;
      for (let index = 0; index < refsGet.length; index++) {
        const element = refsGet[index];
        if (
          element.querySelector("i").classList.contains("sf-text-info") ||
          element.querySelector("i").classList.contains("sf-bg-disable")
        ) {
          element.querySelector("i").classList.remove("sf-text-info");
          element.querySelector("i").classList.remove("sf-bg-disable");
          element.querySelector("i").classList.add("sf-bg-muted");
        }
      }
      e.currentTarget.querySelector("i").classList.remove("sf-bg-muted");
      e.currentTarget.querySelector("i").classList.add("sf-bg-disable");
      e.currentTarget.querySelector("i").classList.add("sf-text-info");
      this.$emit("callback", valorPosicao);
    },
  },
};
</script>

<style></style>
