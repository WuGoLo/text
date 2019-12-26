---
title: TinyMCE富文本
date: 2019-12-26 10:30:03
tags: 富文本编辑器
---

## TinyMCE富文本编辑器

TinyMCE对于要求比较复杂的富文本适用，但是简单的还是不用为好，当然熟悉之后随便用。

### 第一种写法

直接上vue的配置代码

1、首先执行`npm install tinymec -S`安装依赖文件；

2、从node_modules文件夹找到tinymec文件下的tinymec.min.js、skins文件夹。放到static文件夹下，单独建立一个tinymec文件夹里。然后再index.html文件中引入tinymec.min.js文件；

3、可以开始简历组件了：

单独建立一个组件文件夹，文件夹里是： 

index.vue【组件文件】、

toolbar.js【组件的菜单功能项配置】、

plugins.js【组件的插件，其实也是一些功能项配置】

index.vue

~~~~vue
<template>
  <div class="tinymce-container editor-container">
    <textarea :id="tinymceId" class="tinymce-textarea" />
    <div class="editor-custom-btn-container">
      <editorImage color="#1890ff" class="editor-upload-btn" @successCBK="imageSuccessCBK" />
    </div>
  </div>
</template>

<script>
import editorImage from './components/EditorImage'
import plugins from './plugins'
import toolbar from './toolbar'

export default {
  name: 'Tinymce',
  components: { editorImage },
  props: {
    id: {
      type: String,
      default: function() {
        return 'vue-tinymce-' + +new Date() + ((Math.random() * 1000).toFixed(0) + '')
      }
    },
    value: {
      type: String,
      default: ''
    },
    toolbar: {
      type: Array,
      required: false,
      default() {
        return []
      }
    },
    menubar: {
      type: String,
      default: 'file edit insert view'
      // default: 'file edit insert view format'
    },
    height: {
      type: Number,
      required: false,
      default: 360
    }
  },
  data() {
    return {
      hasChange: false,
      hasInit: false,
      tinymceId: this.id,
      fullscreen: false,
      languageTypeList: {
        'zh': 'zh_CN'
      },
      temp: 0,
    }
  },
  watch: {
    value(val) {
      if (!this.hasChange && this.hasInit) {
        this.$nextTick(() =>
          window.tinymce.get(this.tinymceId).setContent(val || ''))
      }
    }
  },
  mounted() {
    this.initTinymce()
    // 获取vue项目布局的主要内容部分的最外层盒子，监听滚动事件
    this.box = document.querySelector('.content-container')
    this.box.addEventListener('scroll', this.getscroll)
  },
  methods: {
    initTinymce() {
      const _this = this
      window.tinymce.init({
        language: 'zh_CN',
        selector: `#${this.tinymceId}`,
        height: this.height,
        body_class: 'panel-body ',
        object_resizing: false,
        toolbar: this.toolbar.length > 0 ? this.toolbar : toolbar,
        menubar: this.menubar,
        plugins: plugins,
        end_container_on_empty_block: true,
        powerpaste_word_import: 'clean',
        code_dialog_height: 450,
        code_dialog_width: 1000,
        advlist_bullet_styles: 'square',
        advlist_number_styles: 'default',
        imagetools_cors_hosts: ['www.tinymce.com', 'codepen.io'],
        default_link_target: '_blank',
        link_title: false,
        nonbreaking_force_tab: true, // inserting nonbreaking space &nbsp; need Nonbreaking Space Plugin
        init_instance_callback: editor => {
          if (_this.value) {
            editor.setContent(_this.value)
          }
          _this.hasInit = true
          editor.on('NodeChange Change KeyUp SetContent', () => {
            this.hasChange = true
            this.$emit('input', editor.getContent())
          })
        },
        setup(editor) {
          editor.on('FullscreenStateChanged', (e) => {
            _this.fullscreen = e.state
          })
        }
      })
    },
    // 下拉菜单弹出层无法跟随文本域移动, 只有操作dom了
    getscroll(e) {
      // 因为文本编辑器顶端距离浏览器顶端是240像素
      let origin = 240, currentPop = null;
      let parentboxTop = e.target.scrollTop;
      let popupbox = document.querySelectorAll('.mce-menu')
      for (let i = 0; i < popupbox.length; i++) {
        let state = popupbox[i].style.display;
        if(state != 'none') {
          currentPop = popupbox[i]
        }
      }
      if(currentPop != null) {
        let top = origin - parentboxTop;
        currentPop.style.top = top + 'px';
      }
      this.temp ++
    },
    destroyTinymce() {
      const tinymce = window.tinymce.get(this.tinymceId)
      if (this.fullscreen) {
        tinymce.execCommand('mceFullScreen')
      }

      if (tinymce) {
        tinymce.destroy()
      }
    },
    setContent(value) {
      window.tinymce.get(this.tinymceId).setContent(value)
    },
    getContent() {
      window.tinymce.get(this.tinymceId).getContent()
    },
    imageSuccessCBK(arr) {
      const _this = this
      arr.forEach(v => {
        window.tinymce.get(_this.tinymceId).insertContent(`<img class="wscnph" src="${v.url}" >`)
      })
    }
  },
  activated() {
    this.initTinymce()
  },
  deactivated() {
    this.destroyTinymce()
    this.box.removeEventListener('scroll', this.getscroll)
  },
  destroyed() {
    this.destroyTinymce()
    this.box.removeEventListener('scroll', this.getscroll)
  },
}
</script>

<style scoped>
.tinymce-container {
  position: relative;
  line-height: normal;
}
.tinymce-container>>>.mce-fullscreen {
  z-index: 10000;
}
.tinymce-textarea {
  visibility: hidden;
  z-index: -1;
}
.editor-custom-btn-container {
  position: absolute;
  right: 4px;
  top: 4px;
  /*z-index: 2005;*/
}
.fullscreen .editor-custom-btn-container {
  z-index: 10000;
  position: fixed;
}
.editor-upload-btn {
  display: inline-block;
}
</style>

~~~~

toolbar.js

~~~js
// 这个地址展示了一些可选的功能项
// Detail list see https://www.tinymce.com/docs/advanced/editor-control-identifiers/#toolbarcontrols

// const toolbar = [ 'hr bullist numlist  insertdatetime  table emoticons forecolor backcolor fullscreen']
const toolbar = [ ' bold forecolor backcolor aligncenter alignjustify alignleft alignright insertdatetime removeformat']

export default toolbar
~~~

plugins.js

~~~js
// 插件查询请看这俩链接
// Detail plugins list see https://www.tinymce.com/docs/plugins/
// Custom builds see https://www.tinymce.com/download/custom-builds/
// 对应index.vue的init中的plugins，一些功能插件
const plugins = ['mentions advlist anchor autosave code codesample colorpicker colorpicker contextmenu directionality emoticons fullscreen hr image imagetools insertdatetime  lists media nonbreaking noneditable pagebreak paste preview print save searchreplace spellchecker tabfocus table template textcolor textpattern visualblocks visualchars wordcount']

export default plugins
~~~

#### 上传本地图片功能

项目中如果需要上传图片功能，可以看官方文档，下边是另一种办法

在开始的Tinymce的组件文件夹中，新建一个文件夹，其中放置一个element上传图片的组件

editorImage.vue

~~~vue
<template>
  <div class="upload-container">
    <el-button style="height:26px;" :style="{background:color,borderColor:color}" icon="el-icon-upload" size="mini" type="primary" @click="images()">
      图片上传
    </el-button>
    <el-dialog title="图片上传" class="upload-dialog" :showClose="false" :visible.sync="dialogVisible" :close-on-click-modal="false" v-if="imagesshow">
      <el-upload
        :multiple="true"
        :file-list="fileList"
        :show-file-list="true"
        :on-remove="handleRemove"
        :on-success="handleSuccess"
        :http-request="handleUpload"
        :before-upload="beforeUpload"
        class="editor-slide-upload"
        action="https://httpbin.org/post"
        list-type="picture-card"
        >
        <i class="el-icon-plus"></i>
      </el-upload>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary4" size="small" @click="handleCancel">
          取消
        </el-button>
        <el-button type="primary" size="small" style="margin-left:50px;" @click="handleSubmit">
          确认
        </el-button>
      </span>  
    </el-dialog>
  </div>
</template>

<script>
import { Upload } from '@/api/dynamicList.js'
export default {
  name: 'EditorSlideUpload',
  props: {
    color: {
      type: String,
      default: '#1890ff'
    }
  },
  data() {
    return {
      imagesshow:false,
      dialogVisible: false,
      listObj: {},
      fileList: []
    }
  },
  methods: {
    images(){
      this.dialogVisible = true;
      this.imagesshow = true;
    },
    checkAllSuccess() {
      return Object.keys(this.listObj).every(item => this.listObj[item].hasSuccess)
      console.log(this.listObj)
    },
    handleSubmit() {
      const arr = Object.keys(this.listObj).map(v => this.listObj[v])
      if (!this.checkAllSuccess()) {
        this.$message('请等待所有图片上传成功 或 出现了网络问题，请刷新页面重新上传！')
        return
      }
      this.$emit('successCBK', arr)
      this.listObj = {}
      this.fileList = []
      this.dialogVisible = false;
      this.imagesshow = false;
      
    },
    handleSuccess(response, file) {
      console.log(response)
      console.log(file)
      const uid = file.uid
      const objKeyArr = Object.keys(this.listObj)
      for (let i = 0, len = objKeyArr.length; i < len; i++) {
        if (this.listObj[objKeyArr[i]].uid === uid) {
          this.listObj[objKeyArr[i]].url = response.data.fileUrl
          this.listObj[objKeyArr[i]].hasSuccess = true
          return
        }
      }
    },
    handleRemove(file) {
      console.log(file)
      const uid = file.uid
      const objKeyArr = Object.keys(this.listObj)
      for (let i = 0, len = objKeyArr.length; i < len; i++) {
        if (this.listObj[objKeyArr[i]].uid === uid) {
          delete this.listObj[objKeyArr[i]]
          return
        }
      }
    },
    beforeUpload(file) {
      console.log('-----beforeUpload-----'+file)
      console.log(file)
      const _self = this
      const _URL = window.URL || window.webkitURL
      const fileName = file.uid
      this.listObj[fileName] = {}
      return new Promise((resolve, reject) => {
        const img = new Image()
        img.src = _URL.createObjectURL(file)
        img.onload = function() {
          _self.listObj[fileName] = { hasSuccess: false, uid: file.uid, width: this.width, height: this.height }
        }
        resolve(true)
      })
    },
    handleUpload(item) {
      let fd = new FormData();
      fd.append("file", item.file, item.fileName);
        Upload(fd).then(res => {
          console.log('-----handleUpload-----'+item)
          console.log(item)
          if (res.data.code !== 1) {
            this.$message.error(res.data.msg);
          } else {
            this.handleSuccess(res.data,item.file)
          }
        })
    },
    handleCancel(response, file){
      this.fileList = []
      this.listObj = {}
      this.dialogVisible = false;
      this.imagesshow = false;
      console.log("-----handleCancel-----"+response)
      console.log(response)
      console.log(file)
    }
  }
}
</script>

<style scoped>
.editor-slide-upload {
  margin-bottom: 20px;
}
.editor-slide-upload >>> .el-upload--picture-card {
  width: 100%;
}
.upload-dialog /deep/ .el-dialog {
  width: 51%;
}
.el-upload--picture-card:hover, .el-upload:focus {
    border-color: #009A52;
    color: #009A52;
}
</style>

~~~

### 第二种写法

需要先下载依赖组件包

1、运行

`npm install tinymec -S`

`npm install @tinymce/tinymce-vue -S `

2、复制tinymec包中的相关文件

- 需要复制tinymec/skins文件夹到static/tinymec文件下。
- 而且需要引入中文语言文件zh_CN.js

3、新建组件，代码如下：

Tinymce.vue

~~~vue
<template>
  <div id="tinymce-editor">
    <editor v-model="myValue" :init="init"></editor>
  </div>
</template>

<script>
// 引入npm的包，前三个是包主要入口
import Editor from '@tinymce/tinymce-vue'
import tinymce from 'tinymce/tinymce'
import 'tinymce/themes/modern/theme'
// 这些都是tinymce包下边的plugins里边的功能插件，
// 需要什么就引入
import 'tinymce/plugins/image'
import 'tinymce/plugins/media'
import 'tinymce/plugins/table'
import 'tinymce/plugins/lists'
import 'tinymce/plugins/contextmenu'
import 'tinymce/plugins/wordcount'
import 'tinymce/plugins/colorpicker'
import 'tinymce/plugins/textcolor'
export default {
  components: { Editor },
  props: {
    //传入一个value，使组件支持v-model绑定
    value: {
      type: String,
      default: ''
    },
    plugins: {
      type: [String, Array],
      default: 'lists image media table textcolor wordcount contextmenu'
    },
    toolbar: {
      type: [String, Array],
      default: 'undo redo |  formatselect | bold italic | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | lists image media table | removeformat'
    }
  },
  data() {
    return {
      //初始化配置
      init: {
        // 引入static中的中文语言
        language_url: '/static/tinymce4.7.5/langs/zh_CN.js',
        language: 'zh_CN',
        // 引入skin皮肤路径
        skin_url: '/static/tinymce4.7.5/skins/lightgray',
        height: 300,
        plugins: this.plugins,
        toolbar: this.toolbar,
        branding: false,
        menubar: 'file edit',
        //此处为图片上传处理函数，这个直接用了base64的图片形式上传图片，
        //如需ajax上传可参考https://www.tiny.cloud/docs/configure/file-image-upload/#images_upload_handler
        images_upload_handler: (blobInfo, success, failure) => {
          const img = 'data:image/jpeg;base64,' + blobInfo.base64()
          success(img)
        }
      },
      myValue: this.value
    }
  },
  mounted() {

  },
  methods: {
    //可以添加一些自己的自定义事件，如清空内容
    clear() {
      this.myValue = ''
    },
  },
  watch: {
    value(newValue) {
      this.myValue = newValue
    },
    myValue(newValue) {
      this.$emit('input', newValue)
    }
  }
}

</script>
<style scoped>
</style>
~~~

应用

demo.vue

~~~vue
// 简单写了，引入组件
import Tinymce from '@/compoents/tinyce'
// 声明组件
components: { Tinymce }
// 应用组件，可以直接用v-model，也可以用:value="content"然后watch监听value。这里看自己思路了
<Tinymce v-model="content" />
~~~



