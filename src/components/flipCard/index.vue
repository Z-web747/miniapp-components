<template>
  <view class="flip-wrap">
    <view class="card-module" id="cradMoudel">
      <view v-for="(item, index) in cardList" :key="item.id" :class="{'flip-card': item.isBack}" class="card-item" @tap="handleFlip(item)" :animation="item.animation">
        <view class="front card-item-body">请翻牌{{ index + 1 }}</view>
        <view class="back card-item-body">{{ item.text }}</view>
      </view>
    </view>
  </view>
  <button @tap="handleShuffle">洗牌</button>
</template>
<script setup lang="ts">
import { ref, onMounted } from 'vue'
import Taro,{ createAnimation } from '@tarojs/taro'
interface CardItemModel {
  animation: any;
  id: number;
  isBack: boolean;
  text: number;
}
  const cardList = ref<CardItemModel[]>([
    {
      animation: {},
      id: 1,
      isBack: false,
      text: 1
    },
    {
      animation: {},
      id: 2,
      isBack: false,
      text: 2
    },
    {
      animation: {},
      id: 3,
      isBack: false,
      text: 3
    },
    {
      animation: {},
      id: 4,
      isBack: false,
      text: 4
    },

    {
      animation: {},
      id: 5,
      isBack: false,
      text: 5
    },
    {
      animation: {},
      id: 6,
      isBack: false,
      text: 6
    },
    {
      animation: {},
      id: 7,
      isBack: false,
      text: 7
    },
    {
      animation: {},
      id: 8,
      isBack: false,
      text: 8
    },
    {
      animation: {},
      id: 9,
      isBack: false,
      text: 9
    },
  ])


  const handleFlip = (item:CardItemModel) => {
    item.isBack = true
  }

  const handleShuffle = () => {
    for (let index = 0; index < cardList?.value.length; index++) {
      shuffle(cardList.value[index], index)
    }
  }
 
  const shuffle = async (item,index) => {
    const animation = createAnimation({
      transformOrigin: "50% 50%",
      duration: 500,
      timingFunction: "ease",
      delay: 0
    })
    animation.export()
    animation.rotateY(180).step()
    animation.rotateY(0).step()
    switch (index) {
      case 0:
        animation.translateX(120).translateY(92).step()
        break;
      case 1:
        animation.translateX(0).translateY(92).step()
        break;
      case 2:
        animation.translateX(-120).translateY(92).step()
        break;
      case 3:
        animation.translateX(120).translateY(0).step()
        break;
      // case 4:
      //   animation.translateX(120).translateY(92).step()
      //   break;
      case 5:
        animation.translateX(-120).translateY(0).step()
        break; 
      case 6:
        animation.translateX(120).translateY(-92).step()
        break;
      case 7:
        animation.translateX(0).translateY(-92).step()
        break;
      case 8:
        animation.translateX(-120).translateY(-92).step()
        break; 
      default:
        break;
    }
      animation.translateX(0).translateY(0).step({
        delay: 500
      })
      item.animation = animation.export()
  }

  onMounted(() => {
      setTimeout(() => {
        Taro.createSelectorQuery().select('#cradMoudel').boundingClientRect(function(rect){
          console.log(rect);
        }).exec()
      }, 500);
  })
</script>
<style lang="scss">
  .flip-wrap{
    .card-module{
      display: flex;
      margin: 24px;
      justify-content: space-between;
      flex-wrap: wrap;
      perspective: 1000px;
      .card-item{
        position: relative;
        width: 30%;
        flex-shrink: 0;
        flex-grow: 0;
        height: 160px;
        margin-bottom: 24px;
        transition: transform 0.3s ease;
        transform-style:preserve-3d; // 让转换的子元素保留3D转换

        .card-item-body{
          position: absolute;
          top: 0;
          left: 0;
          width: 100%;
          height: 100%;
          color: #fff;
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100%;
          backface-visibility: hidden;
        }
        .front{
          width: 100%;
          z-index: 2;
          background: skyblue;
          transform: rotateY(0deg);
        }
        .back{
          width: 100%;
          display: flex;
          justify-content: center;
          align-items: center;
          z-index: 1;
          transform: rotateY(180deg);
          background: red;
        }
      }
    }
  }
  

  .flip-card{
    transform: rotateY(180deg)!important;
  }

</style>