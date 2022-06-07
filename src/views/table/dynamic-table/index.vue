<template>
  <div class="div">
    <div class="StatisticalTools">
      <div>
        <img src="../../../assets/image/tongjiOne.png" alt="">
        <div class="title">区块统计</div>
      </div>
      <div>
        <img src="../../../assets/image/tongjiTow.png" alt="">
        <div class="title">存证统计</div>
      </div>
      <div>
        <img src="../../../assets/image/tongjiThree.png" alt="">
        <div class="title">合约统计</div>
      </div>
      <div>
        <img src="../../../assets/image/tongjiForme.png" alt="">
        <div class="title">用户统计</div>
      </div>
      <div>
        <img src="../../../assets/image/tongjiFire.png" alt="">
        <div class="title">节点统计</div>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.div {
    margin: 0 auto;
    padding: 9rem 8rem;
}

.StatisticalTools {
    display: flex;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    align-content: center;
    justify-content: flex-start;
    div {
        position: relative;

        img {
            width: 22rem;
            height: 10rem;
        }
        .title {
            font-size: 38px;
            font-weight: 700;
            color: #fff;
            position: absolute;
            top: calc(50% - 30px);
            left: 40px;
        }
    }
}
</style>
