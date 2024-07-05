<template>
  <CardBox
    shadow="never"
    :title="data.title || '-'"
    class="paragraph-source-card cursor mb-8 paragraph-source-card-height"
    :class="data.is_active ? '' : 'disabled'"
    :showIcon="false"
  >
    <template #icon>
      <AppAvatar class="mr-12 avatar-light" :size="22"> {{ index + 1 + '' }}</AppAvatar>
    </template>
    <div class="active-button primary">{{ data.similarity?.toFixed(3) }}</div>
    <template #description>
      <el-scrollbar height="150">
        <MdPreview ref="editorRef" editorId="preview-only" :modelValue="data.content" />
      </el-scrollbar>
    </template>
    <template #footer>
      <div class="footer-content flex-between">
        <el-text>
          <el-icon>
            <Document />
          </el-icon>
          {{ data?.document_name }}
          <el-icon  style="padding-left: 3px;" color="blue" @click="getRegulationFileInfo(data?.document_name)">
            <Download />
          </el-icon>
        </el-text>
        
        <div class="flex align-center" style="line-height: 32px;">
          <AppAvatar class="mr-8" shape="square" :size="18">
            <img src="@/assets/icon_document.svg" style="width: 58%" alt="" />
          </AppAvatar>

          <span class="ellipsis"> {{ data?.dataset_name }}</span>
        </div>
      </div>
    </template>
  </CardBox>
</template>

<script setup lang="ts">
import fetchExternalData from '@/api/customize'
const props = defineProps({
  data: {
    type: Object,
    default: () => {}
  },
  index: {
    type: Number,
    default: 0
  }
})

const isMobile = () => {
  let flag = navigator.userAgent.match(/(phone|pad|pod|iPhone|iPod|ios|iPad|Android|Mobile|BlackBerry|IEMobile|MQQBrowser|JUC|Fennec|wOSBrowser|BrowserNG|WebOS|Symbian|Windows Phone)/i)
  return flag;
}

// 普通函数
function getRegulationFileInfo(document_name: any) {
  // alert(document_name);
  // window.open('https://www.baidu.com', '_blank')
  // window.open('https://www.example.com');
  //暴露这个函数
  fetchExternalData.downLoadFile(document_name).then((r1)=>{
  
    console.log(r1?.value?.data)
    if (r1?.value?.data) {
      if(isMobile()){
        window.location.href = "http://wx.gszq.com:9070/sys/attachment/sys_att_main/sysAttMain.do?method=view&viewer=mobilehtmlviewer&fdId="+r1?.value?.data.fdid
      }else{
        window.open("https://oanew.gszq.com/sys/attachment/sys_att_main/sysAttMain.do?method=view&fdId="+r1?.value?.data.fdid)
      }
    }else{
      alert("文件不存在,请联系管理员")
    } 
  })
} 
</script>
<style lang="scss" scoped>
.paragraph-source-card-height {
  height: 260px;
}
@media only screen and (max-width: 768px) {
  .paragraph-source-card-height {
      height: 285px;
    }
}
</style>
