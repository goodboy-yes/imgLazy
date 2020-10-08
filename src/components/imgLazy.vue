<template>
  <div ref="scroller" class="scroller" @scroll="handleScroll">
    <div ref="container" class="container">
      <div
        class="imgItem"
        v-for="(item, index) in showImgList"
        :key="index"
        ref="imgItem"
      >
        <img :src="item.imgUrl" :style="`height:${itemHeight}px`" class="img" />
        <p ref="title" class="title" :title="item.imgFile.name">
          {{ item.imgFile.name }}
        </p>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      lastTime: 0,
      timeout: 20,
      showIndex: 50,
      showImgList: [],
      update: true,
    }
  },
  props: {
    imgList: {
      type: Array,
      default: [],
    },
    itemHeight: {
      type: Number,
      default: 200,
    },
  },
  mounted() {
    this.handleScroll()
  },
  computed: {
    allImgList() {
      return this.imgList.map((item) => {
        return {
          imgUrl: URL.createObjectURL(item),
          imgFile: item,
        }
      })
    },
  },
  watch: {
    imgList() {
      this.$refs.scroller.scrollTop = 0
      this.showIndex = 50
      this.handleScroll()
    },
  },
  methods: {
    handleScroll() {
      let currentTime = new Date()
      if (currentTime - this.lastTime > this.timeout) {
        this.updateVisibleData(this.$refs.scroller.scrollTop)
        this.lastTime = currentTime
      }
    },

    updateVisibleData(scrollTop = 0) {
      if (
        this.$refs.container.offsetHeight <
        scrollTop + this.$refs.scroller.offsetHeight + this.itemHeight * 2
      ) {
        if (this.update === true) {
          if (this.showIndex < this.imgList.length) {
            this.showImgList = this.allImgList.slice(0, this.showIndex)
            this.showIndex += 50
            this.update = false
            setTimeout(() => {
              this.showImgList.forEach((item, index) => {
                let imgw = document.getElementsByClassName('imgItem')[index]
                  .offsetWidth
                document.getElementsByClassName('title')[index].style.width =
                  imgw + 'px'
              })
              this.update = true
            }, 250)
          }
        }
      }
    },
  },
}
</script>
<style scoped >
.scroller {
  overflow-y: auto;
}
.container {
  display: flex; /*显示模式设置为弹性盒子*/
  flex-wrap: wrap; /*进行强制换行*/
}
.container:after {
  /*对最后一个伪元素进行最大限度伸缩*/
  content: ' ';
  flex-grow: 999999999999999999;
}
.imgItem {
  margin: 2px;
  flex-grow: 1; /*进行按比例伸缩*/
  display: flex;
  flex-direction: column;
}
img {
  width: auto;
  height: 100%;
  flex-grow: 1;
  object-fit: cover;
}
p {
  width: 0px;
  margin: 0 auto;
  text-align: center;
  /* height: 34px;
  line-height: 17px; */
  /* font-size: 14px; */
  text-overflow: ellipsis;
  overflow: hidden;
}
</style>
