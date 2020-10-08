<template>
  <div>
    <input
      ref="uploadImg"
      accept="image/jpeg"
      type="file"
      @change="handleImgFileChange"
      hidden
      webkitdirectory
      directory
    />
    <button @click="upload">上传图片</button>

    <img-lazy
      :imgList="imgList"
      :itemHeight="itemHeight"
      style="height: 600px"
    ></img-lazy>
  </div>
</template>

<script>
import imgLazy from './imgLazy.vue'

export default {
  name: 'HelloWorld',
  data() {
    return {
      imgList: [],
      itemHeight: 200,
    }
  },
  components: { imgLazy },
  methods: {
    handleImgFileChange(e) {
      if (!e.target.files.length) {
        return
      }
      const imgList = Array.from(e.target.files).filter((item) => {
        return item.type === 'image/jpeg'
      })
      this.imgList = imgList
      console.log(imgList)
      e.target.value = ''
    },
    upload() {
      this.$refs.uploadImg.click()
    },
  },
}
</script>

<style scoped>
</style>
