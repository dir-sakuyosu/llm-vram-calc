<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import CalcResult from './components/CalcResult.vue'

const inputParams = ref(0)
const selectPrecision = ref(0)
const inputPrecision = ref(0)
const selectContextSize = ref(0)
const inputContextSize = ref(0)

const commonTag = computed(() => {
  const query = 'option[value="' + inputPrecision.value + '"]'
  const elem = document.querySelector(query)
  // const elem = null
  if (elem === null) {
    return `${inputParams.value}b`
  }
  return `${inputParams.value}b_${elem.innerHTML}`
})

const onChangeSelectPrecision = () => {
  inputPrecision.value = selectPrecision.value
}
const onChangeInputPrecision = () => {
  selectPrecision.value = inputPrecision.value
}
const onChangeSelectContextSize = () => {
  inputContextSize.value = selectContextSize.value
}
const onChangeInputContextSize = () => {
  selectContextSize.value = inputContextSize.value
}

onMounted(() => {
  inputParams.value = 12
  selectPrecision.value = 4
  inputPrecision.value = 4
  selectContextSize.value = 4096
  inputContextSize.value = 4096
})
</script>

<template>
  <div class="container">
    <div class="content">
      <h3>LLMの設定を入力することで必要なVRAMサイズを簡易推定します</h3>
    </div>
    <div class="field is-horizontal">
      <div class="field-label is-normal">
        <label class="label">パラメータ数 (B)</label>
      </div>
      <div class="field-body">
        <div class="field has-addons">
          <div class="control">
            <input class="input" type="text" v-model="inputParams" />
          </div>
          <div class="control">
            <label class="label field-text">B</label>
          </div>
        </div>
      </div>
    </div>
    <div class="field is-horizontal">
      <div class="field-label is-normal">
        <label class="label">精度 (bit)</label>
      </div>
      <div class="field-body">
        <div class="field has-addons">
          <div class="control">
            <div class="select">
              <select v-model="selectPrecision" @change="onChangeSelectPrecision">
                <option :value="0"></option>
                <option :value="2">Q2_K_M</option>
                <option :value="3">Q3_K_M</option>
                <option :value="4">Q4_K_M</option>
                <option :value="5">Q5_K_M</option>
                <option :value="6">Q6_K_M</option>
                <option :value="7">Q7_K_M</option>
                <option :value="8">Q8_K_M</option>
                <option :value="16">fp16</option>
              </select>
            </div>
          </div>
          <div class="control">
            <input
              class="input"
              type="text"
              v-model="inputPrecision"
              @change="onChangeInputPrecision"
            />
          </div>
          <div class="control">
            <label class="label field-text">bit</label>
          </div>
        </div>
      </div>
    </div>
    <div class="field is-horizontal">
      <div class="field-label is-normal">
        <label class="label">コンテキストサイズ</label>
      </div>
      <div class="field-body">
        <div class="field has-addons">
          <div class="control">
            <div class="select">
              <select v-model="selectContextSize" @change="onChangeSelectContextSize">
                <option :value="0"></option>
                <option :value="1024">1k</option>
                <option :value="2048">2k</option>
                <option :value="4096">4k</option>
                <option :value="8192">8k</option>
                <option :value="16384">16k</option>
                <option :value="32768">32k</option>
                <option :value="65536">64k</option>
              </select>
            </div>
          </div>
          <div class="control">
            <input
              class="input"
              type="text"
              v-model="inputContextSize"
              @change="onChangeInputContextSize"
            />
          </div>
          <div class="control">
            <label class="label field-text"></label>
          </div>
        </div>
      </div>
    </div>
    <hr class="hr" />
    <div class="field is-horizontal">
      <div class="field-label is-normal">
        <label class="label">一般的なタグ</label>
      </div>
      <div class="field-body">
        <div class="field has-addons">
          <div class="control">
            <label class="label field-text">{{ commonTag }}</label>
          </div>
        </div>
      </div>
    </div>
    <CalcResult
      :params="inputParams"
      :precision="inputPrecision"
      :context-size="inputContextSize"
    ></CalcResult>
  </div>
</template>

<style scoped></style>
