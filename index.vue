<template>
  <div class="spec-preview">
    <!-- //图片地址  传参 -->
    <img :src="skuImageList || sup" />
    <!--   //定义鼠标事件 -->
    <div class="event" @mousemove="handler"></div>
    <!--  //定义放大后的图样式 -->
    <div class="big">
      <!--   //放大后的图的原始图片  -->
      <img :src="skuImageList || sup" ref="big" />
    </div>
    <!-- 遮罩层 -->
    <div class="mask" ref="mask"></div>
  </div>
</template>

<script>
export default {
  name: 'Zoom',
  // 使用props传参
  props:{
    sup:{
    type:String,
    },
    skuImageList:{
      type:String,
    },
  },
  data() {
    return {
      currentIndex: 0
    };
  },
  computed: {
    ImageList() {
      return this.skuImageList[this.currentIndex] || {};
    }
  },
  mounted() {
    //全局事件总线获取兄弟组件传递过来的索引值
    /* this.$bus.$on('getIndex', index => {
      this.currentIndex = index;
    }); */
  },
  methods: {
    //这里是放大方法的主要处理逻辑
    handler(event) {
      let mask = this.$refs.mask;
      let big = this.$refs.big;
      let left = event.offsetX - mask.offsetWidth / 2;
      let top = event.offsetY - mask.offsetHeight / 2;
      //约束范围
      if (left <= 0) left = 0;
      if (left >= mask.offsetWidth) left = mask.offsetWidth;
      if (top <= 0) top = 0;
      if (top >= mask.offsetHeight) top = mask.offsetHeight;
      mask.style.left = left + 'px';
      mask.style.top = top + 'px';
      big.style.left = -2 * left + 'px';
      big.style.top = -2 * top + 'px';
    }
  }
};
</script>

<style lang="scss" scoped>
// img原大小框
.spec-preview {
  position: relative;
  width: 500px;
  height: 500px;
  border: 1px solid #ccc;

  // 原大小
  img {
    width: 100%;
    height: 100%;
  }

  .event {
    width: 100%;
    height: 100%;
    position: absolute;
    top: 0;
    left: 0;
    z-index: 998;
  }

  .mask {
    width: 50%;
    height: 50%;
    background-color: rgba(171, 250, 171, 0.3);
    position: absolute;
    left: 0;
    top: 0;
    display: none;
  }
  // 放大的照片大小
  .big {
    width: 100%;
    height: 87%;
    position: absolute;
    top: -1px;
    left: 100%;
    border: 1px solid #aaa;
    overflow: hidden;
    z-index: 998;
    display: none;
    background: white;
  margin-left: 20px;
    img {
      width: 200%;
      max-width: 200%;
      height: 200%;
      position: absolute;
      left: 0;
      top: 0;
    }
  }

  .event:hover ~ .mask,
  .event:hover ~ .big {
    display: block;
  }
}
</style>
