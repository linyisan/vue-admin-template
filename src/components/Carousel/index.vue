<template>
  <div class="vuecarousel">
    <div
      class="contain"
      :style="{width: imgWidth + 'px', height: imgHeight + 'px'}"
      @mouseenter="stop"
      @mouseleave="start"
    >
      <!--    显示区域（图片）大小-->
      <ul class="ul">
        <li
          v-for="(img, index) in imgs"
          v-show="index == showIndex"
          :key="index"
          class="items"
        >
          <img :src="img.src" alt="轮播图">
        </li>
      </ul>
      <ul
        class="dots"
        :style="{width: imgs.length * (dotWidth + 10) + 'px', height: dotWidth + 'px'}"
      >
        <!--    显示小圆点容器大小-->
        <li
          v-for="(img, index) in imgs"
          :key="index"
          :class="index == showIndex ? 'active' : ''"
          :style="{width: dotWidth + 'px', height: dotWidth + 'px'}"
          @click="showIndex = index"
        ><!--    显示小圆点大小-->
        </li>
      </ul>
      <div v-show="show" class="control">
        <span class="left" @click="previous"><</span>
        <span class="right" @click="next">></span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VueCarousel',
  props: {
    imgs: {
      type: Array,
      required: true
    },
    delay: {
      type: Number,
      default: function() {
        return 2000
      }
    },
    imgWidth: {
      default: function() {
        return 400
      }
    },
    imgHeight: {
      default: function() {
        return 302
      }
    },
    dotWidth: {
      default: function() {
        return 20
      }
    }
  },
  data() {
    return {
      showIndex: 0, // 显示第几个图片
      timer: null, // 定时器
      show: false // 前后按钮显示
    }
  },
  created() {
    this.timer = setInterval(() => {
      this.next()
    }, this.delay)
  },
  beforeDestroy() {
    clearInterval(this.timer)
  },
  methods: {
    previous() {
      if (this.showIndex <= 0) {
        this.showIndex = this.imgs.length - 1
      } else {
        this.showIndex--
      }
    },
    next() {
      if (this.showIndex >= this.imgs.length - 1) {
        this.showIndex = 0
      } else {
        this.showIndex++
      }
    },
    start() {
      this.show = false
      clearInterval(this.timer)
      this.timer = setInterval(() => {
        this.next()
      }, this.delay)
    },
    stop() {
      this.show = true
      clearInterval(this.timer)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss" scoped>
  .contain {
    position: relative;
    top: 50%;
    left: 50%;
    transition: all .8s;
    transform: translateX(-50%);
    color: #fff;
    overflow: hidden;
    cursor: pointer;
    .ul {
      height: 100%;
      list-style: none;
      .items {
        position: absolute;
        top: 0px;
        width: 100%;
        height: 100%;
        img {
          width: 100%;
          height: 100%;
        }
      }
    }
    .dots {
      position: absolute;
      left: 50%;
      bottom: 30px;
      height: 10px;
      transform: translateX(-50%);
      li {
        float: left;
        width: 10px;
        height: 10px;
        margin: 0px 5px;
        border-radius: 50%;
        transition: all .3s;
        background-color: antiquewhite;
        list-style: none;
      }
      .active {
        background-color: blue;
      }
    }
    .control {
      .left {
        position: absolute;
        top: 50%;
        left: 10px;
        padding: 5px;
        transform: translateY(-50%);
        font-size: 20px;
        cursor: pointer;
        &:hover {
          background-color: rgba($color: #000000, $alpha: 0.3);
        }
      }
      .right {
        position: absolute;
        top: 50%;
        right: 10px;
        padding: 5px;
        transform: translateY(-50%);
        font-size: 20px;
        cursor: pointer;
        &:hover {
          background-color: rgba($color: #000000, $alpha: 0.3);
        }
      }
    }
  }
</style>
