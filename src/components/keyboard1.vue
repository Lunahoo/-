<template>
  <div class="keyboard1" v-show="showKeyboard" v-clickoutside="closeModal">
    <p v-for="keys in keyList">
      <template v-for="key in keys">
        <i v-if="key === 'top'" @click.stop="clickKey" @touchend.stop="clickKey" class="iconfont icon-zhiding tab-top"></i>
        <i v-else-if="key === '123'" @click.stop="clickKey" @touchend.stop="clickKey" class="tab-num">123</i>
        <i v-else-if="key === 'del'" @click.stop="clickKey" @touchend.stop="clickKey" class="iconfont icon-delete key-delete KeyDelIconBg"></i>
        <i v-else-if="key === 'blank'" @click.stop="clickKey" @touchend.stop="clickKey" class="iconfont icon-konggejian-jianpanyong tab-blank"></i>
        <i v-else-if="key === 'symbol'" @click.stop="clickKey" @touchend.stop="clickKey" class="tab-symbol">符</i>
        <i v-else-if="key === 'point'" @click.stop="clickKey" @touchend.stop="clickKey" class="tab-point">·</i>
        <i v-else-if="key === 'enter'" @click.stop="clickKey" @touchend.stop="clickKey" class="iconfont icon-huiche tab-enter"></i>
        <i v-else @click.stop="clickKey" @touchend.stop="clickKey">{{key}}</i>
      </template>
    </p>
  </div>
</template>

<script>
  import clickoutside from '../directives/clickoutside'
  export default {
    directives: { clickoutside },
    data() {
      return {
        keyList: [],
        status: 0,//0 小写 1 大写 2 数字 3 符号
        lowercase: [
          ['1', '2', '3'],
          [ '4', '5', '6'],
          [ '7', '8', '9'],
          ['0', 'blank', 'del'],
        ],
        equip:!!navigator.userAgent.toLocaleLowerCase().match(/ipad|mobile/i)//是否是移动设备
      }
    },
    props: {
      option: {/*
       type: Object*/
      }
    },
    computed: {
      showKeyboard(){
        return this.option.show
      }
    },
    mounted() {
      this.keyList = this.lowercase
    },
    methods: {
      tabHandle({ value = '' }) {
        if(value.indexOf('tab-num') > -1){
          this.status = 2
          //数字键盘数据
        }else if(value.indexOf('key-delete') > -1){
          this.emitValue('delete')
        }else if(value.indexOf('tab-symbol') > -1){
          this.status = 3
        }
      },
      clickKey(event) {
        if(event.type === 'click' && this.equip) return
        let value = event.srcElement.innerText
        value && value !== '符' && value !== '·' && value !== '123'? this.emitValue(value) : this.tabHandle(event.srcElement.classList)
      },
      emitValue(key) {
        this.$emit('keyVal', key)
      },
      closeModal(e) {
        if (e.target !== this.option.sourceDom) {
          // this.showKeyboard = false
          this.$emit('close', false)
        }
      }
    }
  }
</script>
<style>
  .keyboard1 {
    width: 100%;
    margin: 0 auto;
    font-size: 18px;
    border-radius: 2px;
     padding-top: 0.5em;
    background-color: #e5e6e8;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
    position: fixed;
     bottom: 0;
    left: 0;
    right: 0;
    z-index: 999;
    pointer-events: auto;
  }
  .keyboard1 p {
    width: 95%;
    margin: 0 auto;
    height: 3.5rem;
    margin-bottom: 0.5em;
    display: flex;
    display: -webkit-box;
    flex-direction: row;
    flex-wrap: nowrap;
    justify-content: center;
  }
  .keyboard1 .key-delete {
    width: 7rem;
    margin: 0 1%;
    color: #827f7f;
    background: rgb(168,176,187) url(../assets/img/del.png) no-repeat 50% 50%;
    background-size: 3rem;
  }
  .keyboard1 .tab-num {
    font-size: 18px;
    background: rgb(168,176,187);
    color: #5a5959;
  }
  .keyboard1 .tab-blank {
    width: 7rem;
    font-size: 12px;
    color: #5a5959;
    line-height: 3.5rem;
  }
  .keyboard1 i {
    display: block;
    margin: 0 1%;
    height: 3.5rem;
    line-height: 3.5rem;
    font-style: normal;
    font-size: 2rem;
    border-radius: 3px;
    width: 7rem;
    background-color: #fff;
    text-align: center;
    flex-grow: 1;
    flex-shrink: 1;
    flex-basis: 0;
    -webkit-box-flex: 1;
  }
</style>
