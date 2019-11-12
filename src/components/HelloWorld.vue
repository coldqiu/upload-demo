<template>
 <div class="hello">
   <button @click="uppyUploadHandle">click upload</button>
   <span>断点续传 <a href="https://github.com/tus">Tus</a>的<a href="https://github.com/tus/tus-js-client">js客户端</a>的在Vue的使用</span>
 </div>
</template>

<script>
import Uppy from '@uppy/core'
import Dashboard from '@uppy/dashboard'
// import XHRUpload from '@uppy/xhr-upload' // eslint-disable-line
import Tus from '@uppy/tus'

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  created () {
    this.initTest()
  },
  methods: {
    uppyUploadHandle () {
      this.uppy.getPlugin('Dashboard').openModal()
    },
    initTest () {
      this.uppy = Uppy({
        restrictions: {
          maxFileSize: null,
          maxNumberOfFiles: null,
          minNumberOfFiles: null,
          allowedFileTypes: null
        }
      })
        .use(Dashboard, {
          id: 'Dashboard',
          inline: false,
          closeModalOnClickOutside: true,
          showProgressDetails: true,
          hidePauseResumeButton: false,
          locale: {
            strings: {
              addMoreFiles: '添加更多文件',
              removeFile: '移除文件',
              uploading: '上传中',
              complete: '完成',
              uploadFailed: '上传失败',
              paused: '暂停',
              retry: '重试',
              cancel: '关闭',
              retryUpload: '重试上传',
              pauseUpload: '暂停上载',
              resumeUpload: '恢复上传',
              cancelUpload: '取消上传',
              replaceTargetContent: false
            }
          }
        })
        .use(Tus, {
          endpoint: 'https://master.tus.io/files/'
          // headers: {'Tus-Resumable': '1.0.0'}
        })
      // http://192.168.0.13:8080/upload.html
      // https://api2.transloadit.com
      // http://d1a9235622.imwork.net:46894/upload
      this.uppy.on('complete', (result) => {
        console.log('Upload complete! We’ve uploaded these files sss:', result.successful)
        console.log('failed files:', result.failed)
      })
    }
  },
  destroyed () {
    this.uppy.close()
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  /*@import '~@uppy/core/dist/style.css';*/
  /*@import '~@uppy/dashboard/dist/style.css';*/
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
