<template>
  <div>
    <div
      class="ham burger-link-mobile sf-display-flex sf-display-md-none"
      @click="menu"
    >
      <i class="mdi mdi-menu sf-text-xlg sf-text-muted sf-dropdown-action"></i>
    </div>
    <nav class="sf-menu">
      <div class="sf-menu-header">
        <div
          @click="menu"
          class="hamburger-link sf-display-none sf-display-md-block"
        >
          <i
            class="mdi mdi-menu sf-text-xlg sf-text-muted sf-dropdown-action"
          ></i>
        </div>
      </div>
      <div class="sf-menu-body">
        <ul class="sf-menu-ul">
          <li class="sf-menu-li sf-px-4">
            <div class="sf-row">
              <div class="sf-col-12 sf-form-group">
                <span
                  class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
                >
                  Template
                  <hr class="sf-width-100 sf-ml-4" />
                </span>
              </div>
              <InputGapTemplate
                :valorInput="inputGap"
                v-on:callback="getValorCampo"
              />
              <InputQuantidadeTemplate
                :valorInput="inputQuantidade"
                v-on:callback="getValorCampoQuantidade"
              />
              <InputQuantLinhasTemplate
                :valorInput="inputQuantidadeLinha"
                v-on:callback="getValorCampoQuantidadeLinha"
              />
              <InputLarguraBloco
                :valorInput="inputRange"
                v-on:callback="getValorLarguraBloco"
              />
              <InputAlturaBloco
                :valorInput="valorHeight"
                v-on:callback="getValorAlturaBloco"
              />
            </div>
            <div class="sf-row">
              <div class="sf-col-12 sf-form-group">
                <span
                  class="sf-text-md sf-font-weight-bold sf-text-muted sf-display-flex sf-align-items-center"
                >
                  Bordas
                  <hr class="sf-width-100 sf-ml-4" />
                </span>
              </div>
              <InputBorderRadiusBloco
                :valorInput="inputBorderRadius"
                v-on:callback="getValorRadiusBloco"
              />
              <InputBorderBloco
                :valorInput="inputBorder"
                v-on:callback="getValorBorderBloco"
              />
              <InputBorderCorBloco
                :valorInput="inputBorderColor"
                v-on:callback="getValorBorderCorBloco"
              />
              <InputBorderTipoBloco
                :valorInput="inputBorderTipo"
                v-on:callback="getValorBorderTipoBloco"
              />
            </div>
            <InputPosicaoBloco
              :valorInput="checkValue"
              v-on:callback="getValorPosicaoBloco"
            />
            <div class="sf-row">
              <div class="sf-col-auto sf-form-group">
                <label class="sf-label">Cor</label>
                <input
                  type="color"
                  id="inputColor"
                  v-model="inputColor"
                  class="sf-p-1 sf-text-capitalize"
                  placeholder="Quantidade de Blocos"
                />
              </div>
            </div>
            <svg
              viewBox="0 0 200 200"
              class="DottedControlBackground__Svg-sc-1pjdrzl-0 kIXwoT"
            >
              <line
                x1="0"
                y1="100"
                x2="200"
                y2="100"
                class="DottedControlBackground__Axis-sc-1pjdrzl-1 dDNeSt"
              ></line>
              <line
                x1="100"
                y1="0"
                x2="100"
                y2="200"
                class="DottedControlBackground__Axis-sc-1pjdrzl-1 dDNeSt"
              ></line>
              <g>
                <circle
                  cx="16.666666666666664"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="16.666666666666664"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="33.33333333333333"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="50"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="66.66666666666666"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="83.33333333333334"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="116.66666666666667"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="133.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="150"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="166.66666666666669"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="16.666666666666664"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="33.33333333333333"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="50"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="66.66666666666666"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="83.33333333333334"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="116.66666666666667"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="133.33333333333331"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="150"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="166.66666666666669"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
                <circle
                  cx="183.33333333333331"
                  cy="183.33333333333331"
                  r="1"
                  fill="var(--color-gray-300)"
                  fill-opacity="0.5"
                ></circle>
              </g>
            </svg>
          </li>
        </ul>
      </div>
    </nav>
    <div class="sf-content-page">
      <HomeView
        :valor="inputRange"
        :inputGap="inputGap"
        :valorHeight="valorHeight"
        :quantidade="parseInt(inputQuantidade)"
        :quantidadeLinha="inputQuantidadeLinha"
        :inputBorderRadius="inputBorderRadius"
        :checkValue="checkValue"
        :inputColor="inputColor"
        :inputBorder="inputBorder"
        :inputBorderColor="inputBorderColor"
        :inputBorderTipo="inputBorderTipo"
      />
    </div>
    <div class="ga-mascara-fundo" @click="mask"></div>
  </div>
</template>

<script>
import HomeView from "@/views/HomeView.vue";
import InputGapTemplate from "@/components/templates/InputGapTemplate.vue";
import InputQuantidadeTemplate from "@/components/templates/InputQuantidadeTemplate.vue";
import InputQuantLinhasTemplate from "@/components/templates/InputQuantLinhasTemplate.vue";
import InputLarguraBloco from "@/components/templates/InputLarguraBloco.vue";
import InputAlturaBloco from "@/components/templates/InputAlturaBloco.vue";
import InputBorderRadiusBloco from "@/components/templates/InputBorderRadiusBloco.vue";
import InputBorderBloco from "@/components/templates/InputBorderBloco.vue";
import InputPosicaoBloco from "@/components/templates/InputPosicaoBloco.vue";
import InputBorderCorBloco from "@/components/templates/InputBorderCorBloco.vue";
import InputBorderTipoBloco from "@/components/templates/InputBorderTipoBloco.vue";
import image from "@/assets/logo-soften.webp";

export default {
  name: "MenuTemplate",
  data: function () {
    return {
      inputQuantidade: (this.inputQuantidade = "2"),
      inputQuantidadeLinha: (this.inputQuantidadeLinha = "2"),
      inputBorderRadius: (this.inputBorderRadius = "6"),
      inputRange: (this.inputRange = "100"),
      valorHeight: (this.valorHeight = "100"),
      inputGap: (this.inputGap = "4"),
      checkValue: (this.checkValue = "center"),
      inputColor: (this.inputColor = "#7e34e3"),
      inputBorder: (this.inputBorder = "0"),
      inputBorderColor: (this.inputBorderColor = ""),
      inputBorderTipo: (this.inputBorderTipo = ""),
      image: image,
    };
  },
  components: {
    HomeView,
    InputGapTemplate,
    InputQuantidadeTemplate,
    InputQuantLinhasTemplate,
    InputLarguraBloco,
    InputAlturaBloco,
    InputPosicaoBloco,
    InputBorderRadiusBloco,
    InputBorderBloco,
    InputBorderCorBloco,
    InputBorderTipoBloco,
  },
  beforeMount() {
    if (window.innerWidth >= 768) {
      if (localStorage.getItem("menu") === "fechado") {
        document.body.classList.add("menu-fechado");
      } else {
        document.body.classList.remove("menu-fechado");
      }
    }

    (function () {
      var current = window.location.pathname.split("")[0];
      if (current === "") return;
      var menuItems = document.querySelectorAll(".sf-menu-list-link");
      for (var i = 0, len = menuItems.length; i < len; i++) {
        if (menuItems[i].getAttribute("href").indexOf(current) !== -1) {
          menuItems[i].className += " active-nolink";
          menuItems[i].nextElementSibling.className += " active-nolink";
        }
      }
    })();
  },
  methods: {
    menu() {
      if (window.innerWidth >= 768) {
        document.body.classList.toggle("menu-fechado");
        if (document.body.classList.contains("menu-fechado")) {
          localStorage.setItem("menu", "fechado");
        } else {
          localStorage.removeItem("menu");
        }
      } else {
        document.body.classList.toggle("menu-mobile-aberto");
      }
    },
    mask() {
      document.body.classList.remove("menu-mobile-aberto");
    },
    getValorCampo(valor) {
      this.inputGap = valor;
    },
    getValorCampoQuantidade(valor) {
      this.inputQuantidade = valor;
    },
    getValorCampoQuantidadeLinha(valor) {
      this.inputQuantidadeLinha = valor;
    },
    getValorLarguraBloco(valor) {
      this.inputRange = valor;
    },
    getValorAlturaBloco(valor) {
      this.valorHeight = valor;
    },
    getValorRadiusBloco(valor) {
      this.inputBorderRadius = valor;
    },
    getValorBorderBloco(valor) {
      this.inputBorder = valor;
    },
    getValorBorderCorBloco(valor) {
      this.inputBorderColor = valor;
    },
    getValorBorderTipoBloco(valor) {
      this.inputBorderTipo = valor;
    },
    getValorPosicaoBloco(valor) {
      this.checkValue = valor;
    },
  },
};

// function dropdown(menu) {
//   var dropdowns = document.getElementsByClassName("sf-dropdown-menu");
//   var i;
//   for (i = 0; i < dropdowns.length; i++) {
//     var openDropdown = dropdowns[i];
//     if (openDropdown.classList.contains("show")) {
//       openDropdown.classList.remove("show");
//     }
//   }
//   document.querySelector(menu).classList.toggle("show");
// }
</script>

<style lang="scss">
@import "@/assets/scss/style.scss";
@import "@/assets/scss/standard.scss";
@import "@/assets/scss/menu.scss";
</style>
