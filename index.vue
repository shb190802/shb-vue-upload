<template>
  <span class="shb-vue-upload" :class="className" @click="selectFile">
    {{showText}}
    <slot></slot>
    <form id="shb-vue-form"
     ref="shb-vue-form"
     method="post"
     enctype='multipart/form-data'
    >
      <input
      type="file"
      :accept='accept'
      :name='name'
      @change="doUpload"
      ref="shb-vue-file"/>
    </form>
  </span>
</template>
<script>
/* eslint-disable */
import { setTimeout } from 'timers';
export default {
  name: 'VueUpload',
  props: {
    className: {
      type: String,
      default: ''
    },
    title: {
      type: String,
      default: '浏览'
    },
    name: {
      type: String,
      default: 'file'
    },
    accept: {
      type: String,
      default: ''
    },
    active: {
      type: String,
      default: '',
      required: true
    },
    timeout: {
      type: Number,
      default: 0
    }
  },
  computed: {
    showText () {
      return this.$slots.default ? '' : this.title
    }
  },
  methods: {
    selectFile () {
      this.$refs['shb-vue-file'].click()
    },
    doUpload () {
      let form = new FormData(this.$refs['shb-vue-form'])
      var xhr = new XMLHttpRequest()
      var isTimeout = false
      xhr.open('POST',this.active,true)
      xhr.onreadystatechange = () => {
        if(xhr.readyState === 4 && !isTimeout){
          let status = xhr.status
          if (status === 200 || status === 301 || status === 302) {
            this.$emit('uploaded', xhr.responseText)
          } else {
            this.$emit('error', {
              status: xhr.status,
              statusText: xhr.statusText
            })
          }
        }
      }
      xhr.onprogress = (e) => {
        this.$emit('progress',e.loaded,e.total)
      }
      if (this.timeout) {
        setTimeout(() => {
          isTimeout = true
          this.$emit('error', {
              status: 'timeout',
              statusText: 'timeout'
            })
        },this.timeout)
      }
      xhr.send(form)
    }
  }
}
</script>
<style>
.shb-vue-upload {
  display: inline;
}
#shb-vue-form {
  display: none;
}
</style>
