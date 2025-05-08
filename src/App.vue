<template>
  <header class="header">
    <section class="main">
      <h1>
        <img src="./assets/images/logo.webp" />
        辟谣报告汇总
      </h1>
      <h2>数据源于微信公众平台运营中心</h2>
    </section>
  </header>
  <main>
    <t-space direction="vertical">
      <t-alert theme="info">
        <template #message>
          开源：
          <t-link
            theme="primary"
            underline
            href="https://github.com/uxiaohan/WxPiYao"
            target="_blank"
          >
            WxPiYao
          </t-link>
          ，技术支持：
          <t-link theme="primary" underline href="https://www.vvhan.com" target="_blank">
            韩小韩博客
          </t-link>
        </template>
      </t-alert>
      <t-alert
        theme="success"
        message="不造谣！不传谣！数据源于 微信公众平台运营中心 辟谣报告汇总，权威！"
      />
    </t-space>
    <t-skeleton :loading="!dataList.length" animation="flashed" style="margin-top: 16px">
      <t-collapse expand-mutex borderless>
        <t-collapse-panel v-for="i in dataList" :key="i.title" :value="i.title" :header="i.title">
          <t-card
            :title="_.title"
            :bordered="false"
            hover-shadow
            v-for="_ in i.list"
            :key="_.title"
          >
            {{ _.content }}
          </t-card>
        </t-collapse-panel>
      </t-collapse>
    </t-skeleton>
  </main>
</template>
<script lang="ts" setup>
import { ref, onMounted } from 'vue'
import { NotifyPlugin } from 'tdesign-vue-next'

const dataList = ref<any>([])
const getList = async () => {
  const res = await fetch('https://wx-piyao-api.4ce.cn/api')
  const resJson = await res.json()
  if (!resJson.success)
    return NotifyPlugin.error({
      title: 'Error',
      content: '数据获取失败，请稍后再试！',
    })
  NotifyPlugin.success({
    title: 'Yes',
    content: '最新数据已更新！',
  })
  dataList.value = resJson.data.map((item: any) => {
    return { title: item.name, list: JSON.parse(item.content) }
  })
}
onMounted(() => {
  getList()
})
</script>
