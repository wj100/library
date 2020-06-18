<template>
  <div class="year-picker">
    <input placeholder="请选择年份"
           :class="['year-picker__input','year-picker__input--real',showYearContent?'year-picker__input--focus':'']"
           v-model="value"
           type="text"
           @focus="inputFocus">
    <!--<a-icon type="calendar" class="year-picker__icon"/>-->
    <div class="year-picker__year-box" v-if="showYearContent">
      <div class="year-picker__input">{{value}}</div>
      <div class="year-picker__year-title">
        <span class="to-left" @click="toLeft()"><<</span>
        <strong>{{yearStart}}--{{yearEnd-1}}</strong>
        <span class="to-right" @click="toRight()">>></span>
      </div>
      <div class="year-picker__year-content">
        <span v-for="(item,index) in yearList" :key="index" :class="{ 'year-text': 1 ,'year-disable':yearDis(item)}"
              @click="chooseYear(item)">{{item}}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'YearPicker',
    props: {
      yearDisable: {
        type: String,
        default: 'no'
      },
      value: {
        type: Number,
      }
    },
    data() {
      return {
        yearLists: [],
        year: '',
        showYearContent: false,
        yearStart: 2010,
        yearEnd: 2030
      }
    },
    methods: {
      inputFocus() {
        this.showYearContent = true
      },
      inputBlur() {
        this.showYearContent = false
      },
      chooseYear(year) {
        if (year > this.yearDis) return
        this.showYearContent = false
        this.year = year + '年'
        this.$emit('input', year)
      },
      toLeft() {
        this.yearStart -= 20
        this.yearEnd -= 20
      },
      toRight() {
        this.yearStart += 20
        this.yearEnd += 20
      }
    },
    computed: {
      yearDis() {
        return function (y) {
          switch (this.yearDisable) {
            case 'before': {
              return y < new Date().getFullYear()
            }
              break;
            case 'no': {
              return false
            }
              break;
            case 'after': {
              return y > new Date().getFullYear()
            }
          }

        }
      },
      yearList() {
        let arr = []
        for (let i = this.yearStart; i < this.yearEnd; i++) {
          arr.push(i)
        }
        return arr
      }
    }
  }
</script>

<style scoped lang="less">
  @blue-border: #004c99;
  .year-picker {
    margin-left: 20px;
    display: inline-block;
    /*width: 180px;*/
    /*border: 1px solid rgba(217, 217, 217, 0.49);*/
    border-radius: 5px;

    .year-picker__icon {
      position: absolute;
      transform: translate(-26px, 10px);
      color: #d9d9d9
    }

    .year-picker__input {
      color: #595959;
      width: 174px;
      height: 32px;
      border: none;
      /*outline: none;*/
      border-radius: 5px;
      text-indent: 10px;
      /*padding-left: 10px;*/
      line-height: 32px;
    }

    .year-picker__input::placeholder {
      /*color: #d9d9d9;*/
    }

    .year-picker__input--real {
      border: 1px solid #d9d9d9;
      height: 32px;
    }

    .year-picker__input--focus {
      opacity: 0;
      /*border: 1px solid #007af4;*/
      /*width: 267px;*/
      /*height: 30px;*/
    }

    .year-picker__input--real:hover {
      border: 1px solid rgba(0, 122, 244, 0.8);
    }

    .year-picker__year-title {
      height: 40px;
      width: 270px;
      border-top: 1px solid #d4d4d4;
      border-bottom: 1px solid #d4d4d4;
      display: flex;
      justify-content: space-around;
      align-items: center;

      span {
        color: #525252;
        cursor: pointer;
      }

      span:active {
        opacity: .5;
      }
    }

    .year-picker__year-box {
      position: absolute;
      z-index: 10;
      background: #ffffff;
      border-radius: 5px;
      border: 1px solid #eeeeee;
      box-shadow: 0 0 .38rem 0 rgba(0, 0, 0, 0.1);
      transform: translateY(-32px);
    }

    .year-picker__year-content {
      padding-top: 20px;
      width: 270px;
      height: 250px;
      display: flex;
      flex-wrap: wrap;
      justify-content: space-around;

      .year-text {
        color: #525252;
        font-size: 16px;
        width: 48px;
        height: 25px;
        display: flex;
        justify-content: center;
        align-items: center;
      }

      .year-text:hover {
        cursor: pointer;
        /*border: 1px solid #333333;*/
        background: rgba(157, 219, 252, 0.41);
        border-radius: 3px;
      }

      .year-disable {
        background: #f5f5f5;
      }

      .year-disable:hover {
        cursor: no-drop;
        background: #f5f5f5;
      }

    }
  }

  .dark {
    border: 1px solid @blue-border;
    color: #8099b3;

    .year-picker__input {
      background: #003366;
      color: #8099b3;;
    }

    .year-picker__input--real {
      border: 1px solid @blue-border;
      height: 32px;
    }

    .year-picker__input::placeholder {
      color: #1c5389;
    }

    .year-picker__year-title {
      border-top: 1px solid @blue-border;
      border-bottom: 1px solid @blue-border;

      span {
        color: #8099b3;
      }
    }

    .year-text:hover {
      cursor: pointer;
      /*border: 1px solid #333333;*/
      background: rgba(157, 219, 252, 0.41);
      border-radius: 3px;
    }

    .year-picker__year-box {
      color: #8099b3;
      position: absolute;
      z-index: 10;
      background: #003366;
      border-radius: 5px;
      border: 1px solid @blue-border;
      /*box-shadow: 0 0 .38rem 0 #004c99;*/
      transform: translateY(-30px);
    }


    .year-picker__year-content {
      .year-text {
        color: #8099b3;
      }

      .year-disable {
        background: #133558;
      }

      .year-disable:hover {
        cursor: no-drop;
        background: #133558;
      }
    }
  }
</style>
