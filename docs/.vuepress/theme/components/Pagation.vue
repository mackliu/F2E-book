<template>
  <div class="pagation" v-show="show">
    <div class="pagation-list">
      <span class="jump" v-show="currentPage>1" @click="goPrev" unselectable="on">&lt;</span>
      <span v-show="efont" class="jump" @click="jumpPage(1)">1</span>
      <span class="ellipsis"  v-show="efont">...</span>
      <span class="jump" v-for="num in indexs" :key="num" :class="{bgprimary:currentPage==num}" @click="jumpPage(num)">{{num}}</span>
      <span class="ellipsis"  v-show="efont&&currentPage<pages-4">...</span>
      <span v-show="efont&&currentPage<pages-4" class="jump" @click="jumpPage(pages)">{{pages}}</span>
      <span class="jump" v-show="currentPage < pages" @click="goNext">&gt;</span>
      <span class="jumppoint">跳到：</span>
      <span class="jumpinp"><input type="text" v-model="changePage"></span>
      <span class="jump gobtn" @click="jumpPage(changePage)">GO</span>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      changePage: '' // 跳转页
    }
  },
  props: {
    total: {
      type: Number,
      default: 10
    },
    perPage: {
      type: Number,
      default: 10
    },
    currentPage: {
      type: Number,
      default: 1
    }
  },
  computed: {
    pages () {
      return Math.ceil(this.total / this.perPage)
    },
    show: function () {
      return this.pages && this.pages != 1
    },
    efont: function () {
      if (this.pages <= 7) return false
      return this.currentPage > 5
    },
    indexs: function () {
      var left = 1
      var right = this.pages
      var ar = []
      if (this.pages >= 7) {
        if (this.currentPage > 5 && this.currentPage < this.pages - 4) {
          left = Number(this.currentPage) - 3
          right = Number(this.currentPage) + 3
        } else {
          if (this.currentPage <= 5) {
            left = 1
            right = 7
          } else {
            right = this.pages

            left = this.pages - 6
          }
        }
      }
      while (left <= right) {
        ar.push(left)
        left++
      }
      return ar
    }
  },
  methods: {
    goPrev () {
      let currentPage = this.currentPage
      if (this.currentPage > 1) {
        this.emit(--currentPage)
      }
    },
    goNext () {
      let currentPage = this.currentPage
      if (currentPage < this.pages) {
        this.emit(++currentPage)
      }
    },
    jumpPage: function (id) {
      if (id == '') {
        alert(`請輸入頁碼！`)
        return
      }
      if (id <= this.pages) {
        this.emit(id)
        return
      }
      alert(`請輸入小於 ${this.pages} 的頁碼！`)
    },
    emit (id) {
      this.$emit('getCurrentPage', id)
    }
  }
}
</script>

<style lang="stylus" scoped>
.pagation
  font-weight: 700;
  text-align: center;
  color: #888;
  margin: 20px auto 0;
  background: #f2f2f2;
  .pagation-list
    font-size: 0;
    background: #fff;
    line-height: 50px;
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -khtml-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    span
      font-size: 14px;
      &.jump
        border: 1px solid #ccc;
        padding: 5px 8px;
        -webkit-border-radius: 4px;
        -moz-border-radius: 4px;
        border-radius: 4px;
        cursor: pointer;
        margin-left: 5px;
      &.jumpinp
        input
          width: 55px;
          height: 26px;
          font-size: 13px;
          border: 1px solid #ccc;
          -webkit-border-radius: 4px;
          -moz-border-radius: 4px;
          border-radius: 4px;
          text-align: center;
          outline none
      &.bgprimary
        cursor: default;
        color: #fff;
        background: $accentColor !important;
        border-color: $accentColor !important;
      &.ellipsis
        padding: 0px 8px;
      &.jumppoint
        margin-left: 30px;

.reco-theme-dark
  .pagation
    background: transparent;
    .pagation-list
      background: transparent;
      .jump
        background: #333;
        color: #fff;
      .jumppoint
        color: #fff;
</style>
