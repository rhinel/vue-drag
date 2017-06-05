<template>
  <div class="drag-wrap">
    <div class="button-wrap">
      <div
        class="button"
        v-for="(i, x) in buttonList"
        key="name">
        <div
          class="showi"
          :class="{ move: i.onMove }"
          :style="{
            top: i.offsetY + 'px',
            left: i.offsetX + 'px'
          }"
          @mousedown.stop.prevent="mousedown($event, i, x, buttonList)">
          <div class="txt">
            {{ i.name }}
          </div>
          <div
            class="sub-wrap"
            v-show="i.showSub && i.sub.length">
            <div
              class="sub"
              v-for="(j, y) in i.sub">
              <div
                class="showi"
                :class="{ move: j.onMove }"
                :style="{
                  top: j.offsetY + 'px',
                  left: j.offsetX + 'px'
                }"
                @mousedown.stop.prevent="mousedown($event, j, y, i.sub)">
                <div class="txt">
                  {{ j.name }}
                </div>
              </div>
              <div
                class="blocki"
                :class="{ move: j.onMove }">
                <div></div>
              </div>
            </div>
            <i class="arrow_out"></i>
            <i class="arrow_in"></i>
          </div>
        </div>
        <div
          class="blocki"
          :class="{ move: i.onMove }">
          <div></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 移动中的对象
      thisMoving: null,
      thisMSe: null,
      thisMovingArr: null,

      buttonList: [
        {
          'eventType': 2,
          'menuType': 1,
          'name': '活动',
          'value': 'http://baidu.com',
          'current': false,
          'showSub': false,
          'sub': [],
          // 定位用的属性
          onMove: false,
          offsetX: 0,
          offsetY: 0
        },
        {
          'eventType': 2,
          'menuType': 1,
          'name': '资讯',
          'value': 'http://baidu.com',
          'current': false,
          'showSub': false,
          'sub': [],
          // 定位用的属性
          onMove: false,
          offsetX: 0,
          offsetY: 0
        },
        {
          'eventType': 2,
          'menuType': 3,
          'name': '微信',
          'value': 'http://www.baidu.com',
          'current': false,
          'showSub': false,
          'sub': [
            {
              'eventType': 2,
              'menuType': 3,
              'name': '谷歌1',
              'value': 'http://www.baidu.com',
              'current': false,
              // 定位用的属性
              onMove: false,
              offsetX: 0,
              offsetY: 0
            },
            {
              'eventType': 2,
              'menuType': 3,
              'name': '谷歌2',
              'value': 'http://www.baidu.com',
              'current': false,
              // 定位用的属性
              onMove: false,
              offsetX: 0,
              offsetY: 0
            },
            {
              'eventType': 2,
              'menuType': 3,
              'name': '谷歌3',
              'value': 'http://www.baidu.com',
              'current': false,
              // 定位用的属性
              onMove: false,
              offsetX: 0,
              offsetY: 0
            }
          ],
          // 定位用的属性
          onMove: false,
          offsetX: 0,
          offsetY: 0
        }
      ]
    }
  },
  mounted () {
    window.onmouseup = this.wrapMouseup
    window.onmousemove = this.wrapMousemove
  },
  methods: {
    mousedown (e, i, x, arr) {
      this.thisMoving = i
      this.thisMovingSe = e
      this.thisMovingArr = arr
    },
    wrapMousemove (e) {
      if (!this.thisMoving) {
        return false
      } else {
        let index = this.thisMovingArr.indexOf(this.thisMoving)

        this.thisMoving.onMove = true

        // 判断子菜单:移动方向
        if (this.thisMoving.sub) {
          this.thisMoving.offsetX = this.thisMoving.offsetX ?
            this.thisMoving.offsetX : (this.thisMovingSe.target.clientWidth * index)
          this.thisMoving.offsetY = this.thisMoving.offsetY ? this.thisMoving.offsetY : 0
        } else {
          this.thisMoving.offsetX = this.thisMoving.offsetX ? this.thisMoving.offsetX : 0
          this.thisMoving.offsetY = this.thisMoving.offsetY ?
            this.thisMoving.offsetY : (this.thisMovingSe.target.clientHeight * index)
        }

        let thisMovingCW = this.thisMovingSe.target.clientWidth
        let thisMovingCH = this.thisMovingSe.target.clientHeight

        this.thisMoving.offsetX += e.movementX / window.devicePixelRatio
        this.thisMoving.offsetY += e.movementY / window.devicePixelRatio

        // 计算位移
        let length = this.thisMovingArr.length

        // 判断子菜单:移动方向
        if (this.thisMoving.sub) {
          let minX = Math.floor((this.thisMoving.offsetX / thisMovingCW) + 0.5)
          if (
            this.thisMoving.offsetY >= (- thisMovingCH * 3 / 4) &&
            this.thisMoving.offsetY <= (thisMovingCH * 3 / 4) &&
            this.thisMoving.offsetX >= (- thisMovingCW * 3 / 4) &&
            this.thisMoving.offsetX <= thisMovingCW * (length - (1 / 4)) &&
            minX !== index &&
            minX >= 0
          ) {
            this.thisMovingArr.splice(index, 1)
            this.thisMovingArr.splice(minX, 0, this.thisMoving)
          }
        } else {
          let minY = Math.floor((this.thisMoving.offsetY / thisMovingCH) + 0.5)
          if (
            this.thisMoving.offsetX >= (- thisMovingCW * 3 / 4) &&
            this.thisMoving.offsetX <= (thisMovingCW * 3 / 4) &&
            this.thisMoving.offsetY >= (- thisMovingCH * 3 / 4) &&
            this.thisMoving.offsetY <= thisMovingCH * (length - (1 / 4)) &&
            minY !== index &&
            minY >= 0
          ) {
            this.thisMovingArr.splice(index, 1)
            this.thisMovingArr.splice(minY, 0, this.thisMoving)
          }
        }
      }
    },
    wrapMouseup (e) {
      if (!this.thisMoving) {
        return false
      } else {
        if (
          !this.thisMoving.onMove &&
          this.thisMoving.hasOwnProperty('showSub')
        ) {
          this.openSub(this.thisMoving)
        }
        this.thisMoving.onMove = false
        this.thisMoving.offsetX = 0
        this.thisMoving.offsetY = 0
        this.thisMoving = null
        this.thisMovingSe = null
        this.thisMovingArr = null
      }
    },
    openSub (item) {
      let f = item.showSub
      this.thisMovingArr.forEach((i) => {
        i.showSub = false
      })
      item.showSub = !f
    }
  }
}
</script>

<style lang="scss">
.drag-wrap {
  width: 317px;
  height: 580px;
  margin: 0 auto;
  position: relative;
  background: transparent url(https://res.wx.qq.com/mpres/htmledition/images/bg/bg_mobile_head_default2968da.png) no-repeat 0 0;
  background-position: 0 0;
  border: 1px solid #e7e7eb;
  * {
    box-sizing: border-box;
  }
}
.button-wrap {
  line-height: 50px;
  display: flex;
  border-top: 1px solid #e7e7eb;
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  .button {
    width: 33.3%;
    flex: 1;
    text-align: center;
    &:not(:last-of-type) {
      border-right: 1px solid #e7e7eb;
    }
    .showi {
      position: relative;
      .txt {
        cursor: pointer;
        user-select: none;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        background-color: #fafafa;
        &:hover {
          background-color: #eee;
        }
      }
      .sub-wrap {
        position: absolute;
        bottom: calc( 100% + 8px );
        left: -1px;
        right: -1px;
        background-color: #fff;
        border: 1px solid #e7e7eb;
        .sub {
          &:not(:last-of-type) {
            border-bottom: 1px solid #e7e7eb;
          }
          & > .showi.move {
            opacity: 0.8;
            position: absolute;
            z-index: 2;
            height: 50px;
            width: 100%;
            border: 1px solid #e7e7eb;
            & > .txt {
              background-color: #eee;
            }
          }
          & > .blocki.move {
            background-color: #fafafa;
            height: 50px;
            padding: 2px;
            & > div {
              border: 1px solid #e7e7eb;
              background-color: #fff;
              border-radius: 2px;
              height: 100%;
            }
          }
        }
      }
    }
    & > .showi.move {
      opacity: 0.8;
      position: absolute;
      z-index: 2;
      width: calc( 100% / 3 );
      height: 100%;
      border: 1px solid #e7e7eb;
      & > .txt {
        background-color: #eee;
      }
    }
    & > .blocki.move {
      background-color: #fafafa;
      height: 100%;
      padding: 2px;
      & > div {
        border: 1px solid #e7e7eb;
        background-color: #fff;
        border-radius: 2px;
        height: 100%;
      }
    }
  }
}
.arrow_out {
  position: absolute;
  left: 50%;
  margin-left: -6px;
  bottom: -6px;
  width: 0;
  height: 0;
  border-width: 6px;
  border-style: dashed;
  border-color: transparent;
  border-bottom-width: 0;
  border-top-color: #e7e7eb;
  border-top-style: solid;
}
.arrow_in {
  position: absolute;
  left: 50%;
  margin-left: -5px;
  bottom: -5px;
  width: 0;
  height: 0;
  border-width: 5px;
  border-style: dashed;
  border-color: transparent;
  border-bottom-width: 0;
  border-top-color: #fafafa;
  border-top-style: solid;
}
</style>
