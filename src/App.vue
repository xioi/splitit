<script setup lang="ts">
  import { NButton, NInput, NSpace, NInputNumber } from 'naive-ui'
  import { ref } from 'vue'

  const origin = ref(""), processed = ref("")
  
  const maxCharacters = ref(50)

  function splitParagraph(text: string) {
    let splited = text.split(/([,.，。！？])/g), result = ""
    let splited2 = []
    for (let i = 0; i < splited.length / 2 -1; i++) { // 在保留标点符号的情况下把标点符号黏到句子后面
      console.log(i)
      splited2[i] = splited[i*2] + splited[i*2+1]
    }

    let count = 0
    splited2.forEach(split => { // 重新粘贴每个句子
      result += split
      count += split.length

      if(count >= maxCharacters.value) { // 如果超过了需要断句的长度
        result += "\n\n"
        count = 0
      }
    })

    return result
  }

  function splitit() {
    let ori = origin.value, result = ""

    let splitedNewlines = ori.split(/\n{1,}/) // 分割各个段落

    splitedNewlines.forEach(para => {
      result += splitParagraph(para) + "\n\n\n\n"
    })

    processed.value = result
  }
</script>

<template>
  <div id="content">
    <h1>splitit</h1>
    <n-space vertical>
      <n-input v-model:value="origin" type="textarea" placeholder="在这里输入要分段的文字"/>
      <n-space style="float: right;">每句话最多的字符数：<n-input-number v-model:value="maxCharacters" /></n-space>
      <n-button style="float: right;" @click="splitit">分割文本</n-button>
      <n-input v-model:value="processed" type="textarea" placeholder="这里是处理过的文字" :autosize="{minRows: 10}" />
    </n-space>
  </div>
</template>

<style scoped>
  #content {
    max-width: 720px;
    margin: auto;
    padding: 20px;
  }
</style>