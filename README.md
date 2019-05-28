# shb-vue-upload

## install

npm install shb-vue-upload

## use
### main.js
```javascript
import Vue from 'vue'
import App from './App.vue'
import VueUpload from 'shb-vue-upload'

Vue.use(VueUpload)
new Vue({
  render: h => h(App),
}).$mount('#app')
```
### app.vue
```javascript
<template>
  <div id="app">
    <vue-upload
      title="选择图片"
      active='/fileupload'
      name='SHB_FU_Input'
      @uploaded='showUploaded'
      @progress='onProgress'
      @error="error"
      class="upload"
      >
        <input type="button" value="upload" />
      </vue-upload>
  </div>
</template>
<script>
export default {
  name: 'app',
  components: {
  },
  methods: {
    showUploaded (res) {
      // console.log('showUploaded',res)
    },
    onProgress (cur, all) {
      // console.log(cur, all)
    },
    error (e) {
      // console.log('upload error ',e)
    }
  }
}
</script>
```
## 参数
| 参数        | 说明          |  类型  | 默认值 |
| --------    | -----:        | ----: |-----: |
| title        | 显示标题      |   String    | 浏览 |
| avtive       | 后台上传接口      |   String    | 不可为空 |
| name        | 后台接口获取参数名      |   String    | 不可为空|
| className | 自定义样式 | String | '' |
| accept | 文件类型，详见html file | String | '' |
| timeout | 超时时间（为0时没有超时限制） | Number | 0 |

## 回调方法
| 方法名        | 说明          |
| --------    | -----:        |
| uploaded        | 上传成功，返回后台接口返回数据（数据为text格式）      |
| error |错误回调，返回 {status，statusText} |
| progress | 上传进度 cur，totle |