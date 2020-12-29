<template>
  <view class="content">
    <!-- 牌及用户信息 -->
    <!-- 玩家牌 -->

    <view class="cardmsg">
      <view v-for="(item, index) in usersList" :key="index">
        <view class="userCard">
          <!-- 我 -->
          <view
            v-if="index==2&&cardlist[2]"
            class="flex cardlist alignitem-end"
            :class="`cardlist${index+ 1}`"
          >
            <view class="flex cardSpace cardFlower" v-if="cardlist[2].flower">
              <img :src="item.src" v-for="(item, index) in cardlist[2].flower" :key="index" />
            </view>
            <view class="flex cardMany cardSpace" v-if="cardlist[2].three">
              <img :src="item.src" v-for="(item, index) in cardlist[2].three" :key="index" />
            </view>
            <view class="flex cardMany cardSpace" v-if="cardlist[2].Four">
              <img
                :src="index%4==3&&!item.show?item.src:'../../static/image/topBack.png'"
                v-for="(item, index) in cardlist[2].Four"
                :key="index"
              />
            </view>

            <view class="flex cardSpace" v-if="cardlist[2].card">
              <view class v-for="(item, index) in cardlist[2].card" :key="index">
                <view class="flex verimg pos-r" v-if="item.iswantonly">
                  <img :src="item.src" />
                </view>
                <view class="flex" v-else>
                  <img :src="item.src" @tap="fndbClick(item)" />
                </view>
              </view>
            </view>
            <view class="flex" v-if="cardlist[2].get">
              <img :src="cardlist[2].get[0].src" @tap="fndbClick(item)" />
            </view>
          </view>
        </view>
      </view>
    </view>
  </view>
</template>

<script>
import { requestUrl } from "../../util/common.js";
export default {
  data() {
    return {
      // cardNum:牌面值
      // cardtype:0:万  1:条  2:筒  3:东(1)南(2)西(3)北(4)中(5)发(6)白(7) 4:春(1)夏(2)秋(3)冬(4)梅(5)兰(6)竹(7)菊(8)
      // iswantonly：true/false
      cardlist: [{}, {}, {}, {}],

      wantOnly: {
        cardNum: 1,
        cardtype: 1,
        iswantonly: true,
        src: "../../static/image/card/bottom/tiao1.png"
      },
      // 用户信息
      usersList: [
        { nickname: "001", id: "1", point: 10000, isbanker: false },
        { nickname: "002", id: "2", point: 200, isbanker: false },
        { nickname: "003", id: "3", point: 300, isbanker: false },
        { nickname: "004", id: "4", point: 400, isbanker: true }
      ]
    };
  },
  created() {
    // 页面自适应
    this.remFn();
    this.sortCard();
    this.hupai();
  },

  onLoad() {},
  methods: {
    sortCard() {
      var cardslist = {
        card: [
          {cardNum: 2,cardtype: 0,iswantonly: false,src: "../../static/image/card/bottom/wan2.png",num:2},
          {cardNum: 1,cardtype: 1,iswantonly: false,src: "../../static/image/card/bottom/tiao2.png" ,num:11},
          {cardNum: 6, cardtype: 2,iswantonly: false,src: "../../static/image/card/bottom/tong6.png",num:26},
          {cardNum: 8, cardtype: 2, iswantonly: false,src: "../../static/image/card/bottom/tong8.png",num:28},
          {cardNum: 2,cardtype: 3, iswantonly: false,src: "../../static/image/card/bottom/zi2.png",num:34},
          {cardNum: 4,cardtype: 3,iswantonly: false,src: "../../static/image/card/bottom/zi4.png",num:38},
          {cardNum: 4,cardtype: 3,iswantonly: false,src: "../../static/image/card/bottom/zi4.png",num:38},
          {cardNum: 7,cardtype: 2,iswantonly: false,src: "../../static/image/card/bottom/tong7.png",num:27},
          {cardNum: 2,cardtype: 3,iswantonly: false, src: "../../static/image/card/bottom/zi2.png" ,num:34},
          {cardNum: 2, cardtype: 1,iswantonly: false,src: "../../static/image/card/bottom/tiao2.png",num:12},
          {cardNum: 3,cardtype: 0,iswantonly: false,src: "../../static/image/card/bottom/wan3.png",num:3},
          {cardNum: 2,cardtype: 3,iswantonly: false,src: "../../static/image/card/bottom/zi2.png",num:34},
          {cardNum: 4,cardtype: 0,iswantonly: false,src: "../../static/image/card/bottom/wan4.png",num:4},
          {cardNum: 4, cardtype: 4,iswantonly: false,src: "../../static/image/card/bottom/f4.png",num:44},
          {cardNum: 3, cardtype: 1,iswantonly: true,src: "../../static/image/card/bottom/tiao3.png",num:13}
        ]
      };
      var carlist = cardslist.card;
      // 排序
      var min;
      for (var i = 0; i < carlist.length - 1; i++) {
        for (var j = i + 1; j < carlist.length; j++) {
          if (carlist[i].cardtype > carlist[j].cardtype) {
            min = carlist[j];
            carlist[j] = carlist[i];
            carlist[i] = min;
          }
        }
      }
      for (var i = 0; i < carlist.length - 1; i++) {
        for (var j = i + 1; j < carlist.length; j++) {
          if (
            carlist[i].cardtype == carlist[j].cardtype &&
            carlist[i].cardNum > carlist[j].cardNum
          ) {
            min = carlist[j];
            carlist[j] = carlist[i];
            carlist[i] = min;
          }
        }
      }
      // 拿到牌之后判断自己有没有花牌 有的话就立即补花
      // 轮到自己的顺序后，首先看自己手上有没有可以杠的牌，摸到的牌询问是否要碰还是杠
      //

      this.cardlist[2].card = carlist;
      
    },
    // 胡牌  1.有花  百搭可有可无  2.无花  无白搭
    hupai() {
      // 有花 有百搭
      // 有花 没有百搭
      // 无花 无百搭
      // console.log(this.cardlist[2].card);
      var cards = this.cardlist[2].card;
      var baida = []; //百搭
      var hua = []; //花牌
       var cardsarr= [];
      for (var i = 0; i < cards.length; i++) {
        if(cards[i].cardtype!=4&&!cards[i].iswantonly){
            cardsarr.push(cards[i].num)
        }
        
        if (cards[i].iswantonly) {
          baida.push(cards[i]);
        } else if (cards[i].cardtype == 4) {
          hua.push(cards[i]);
        } else {
          continue;
        }
      }
      cards.sort(function (a,b) {
        return a-b;
      });
      console.log(cardsarr)
      var can=this.canHuLaizi(cardsarr,baida.length);
      console.log(can)
      // console.log(baida);
      // console.log(hua);
      
      if (baida.length >= 1 && hua.length >= 1&&can) {
        console.log('有花又百搭')
      } else if (hua.length >= 1 && baida.length == 0&&can) {
        console.log('有花五百搭')
      } else if (hua.length == 0 && baida.length == 0&&can) {
        console.log('无花无百搭')
      }
     
     
   
    },
    isShunKe(cards, laiziCount) {
      if (cards.length === 0) {
        return true;
      } // 若第一张是顺子中的一张
      for (var first = cards[0] - 2; first <= cards[0]; first++) {
        if (first % 10 > 7 || (laiziCount === 0 && first < cards[0])) {
          // 顺子第一张牌不会大于7点、无赖子情况下顺子第一张只能用手上的牌
          continue;
        }
        var shunCount = 0;
        for (var i = 0; i < 3; i++) {
          if (cards.indexOf(first + i) >= 0) {
            shunCount++;
          }
        }
        if (shunCount === 3 || shunCount + laiziCount >= 3) {
          // 找到包含第一张牌的顺子
          var puCards = cards.slice();
          var puLaizi = laiziCount;
          for (var i = 0; i < 3; i++) {
            var deletePos = puCards.indexOf(first + i);
            if (deletePos >= 0) {
              puCards.splice(deletePos, 1);
            } else {
              puLaizi--;
            }
          }
          if (this.isShunKe(puCards, puLaizi)) {
            // 剩下的牌成扑
            return true;
          }
        }
      } // 若第一张是刻子中的一张
      var keziCount = 1;
      var keziCard = cards[0];
      if (cards[1] === keziCard) {
        keziCount++;
      }
      if (cards[2] === keziCard) {
        keziCount++;
      }
      if (keziCount === 3 || keziCount + laiziCount >= 3) {
        var puCards = cards.slice();
        var puLaizi = laiziCount;
        for (var i = 0; i < 3; i++) {
          var deletePos = puCards.indexOf(keziCard);
          if (deletePos >= 0) {
            puCards.splice(deletePos, 1);
          } else {
            puLaizi--;
          }
        }
        if (this.isShunKe(puCards, puLaizi)) {
          return true;
        }
      }
      return false;
    },
    canHuLaizi(cards, laiziCount) {  
      console.log(cards.length)
      if ((cards.length + laiziCount + 1) % 3 != 0) {  
          // 若牌张数不是2、5、8、11、14则不能胡  
          console.log(1)
          return false;  
      }  
    // 排序方便胡牌判断  
      cards.sort(function(a, b) {  
          return a - b;  
      })  
      // 依次删除一对牌做将，其余牌全部成扑则可胡  
      for (var i = 0; i < cards.length; i++) {  
          if (i > 0 && cards[i] == cards[i - 1]){  
              // 和上一次是同样的牌，避免重复计算  
              continue;   
          }  
          if ((i + 1 < cards.length && cards[i] == cards[i + 1]) || laiziCount > 0) {  
              // 找到对子、或是用一张癞子拼出的对子  
              var puCards = cards.slice();  
              var puLaizi = laiziCount;  
              puCards.splice(i, 1);  
              if (puCards[i] == cards[i]) {  
                  puCards.splice(i, 1);  
              }  
              else {  
                  puLaizi--;  
              }  
              // 删去对子判断剩下的牌是否成扑  
              if (this.isShunKe(puCards, puLaizi)) {  
                console.log(2)
                  return true;  
              }  
          }  
      }  
      if (laiziCount >= 2 && this.isShunKe(cards, laiziCount - 2)) {  
          // 两个癞子做将牌特殊判定处理  
          console.log(3)
          return true;  
      }  
      console.log(4)
      return false;  
    }  

  }
};
</script>

<style>
page {
  width: 100%;
  height: 100%;
  /* background: url("../../static/image/bg.png") no-repeat; */
  /* background-size: 100% 100%; */
}
/* 返回 */
.back {
  position: absolute;
  left: 0.16rem;
  top: 0.16rem;
  width: 0.25rem;
  height: 0.25rem;
}

/* 中间位置 */
.positionbox {
  width: 0.7rem;
  height: 0.69rem;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: url("../../static/image/position.png") no-repeat;
  background-size: 100% 100%;
}
.countdown {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 0.14rem;
  color: #fff;
}
.positionImg {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.positionImg.topImg,
.positionImg.bottomImg {
  width: 0.56rem;
  height: 0.18rem;
}
.positionImg.leftImg,
.positionImg.rightImg {
  width: 0.18rem;
  height: 0.56rem;
}
.positionImg.bottomImg {
  margin-top: 0.22rem;
}
.positionImg.topImg {
  margin-top: -0.22rem;
}
.positionImg.leftImg {
  margin-left: -0.22rem;
}
.positionImg.rightImg {
  margin-left: 0.22rem;
}
.showtext {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 0.76rem;
  height: 0.18rem;
  text-align: center;
  font-size: 0.1rem;
  color: #7ec991;
  border: 1px solid #7ec991;
  border-radius: 0.1rem;
}
.showtext span {
  color: #ffff4e;
}
.showtext.showtextLeft {
  margin-left: -0.8rem;
}
.showtext.showtextRight {
  margin-left: 0.8rem;
}
/* 出牌展示区域 */

.userSendcard img {
  width: 0.23rem;
  height: 0.33rem;
  margin-top: -0.1rem;
}

.sendCard1 {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateY(-50%);
  margin-top: -0.7rem;
  flex-wrap: wrap-reverse;
  flex-direction: row-reverse;
  width: 1.84rem;
  margin-left: -0.92rem;
}
.sendCard1 img {
  width: 0.26rem;
  height: 0.36rem;
  margin-left: -0.05rem;
}
.sendCard2 {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateX(-50%);
  margin-left: -1.35rem;
  flex-wrap: wrap-reverse;
  flex-direction: column;
  height: 1.84rem;
  margin-top: -0.8rem;
}

.sendCard3 {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateY(-50%);
  margin-top: 0.84rem;
  flex-wrap: wrap;
  flex-direction: row;
  width: 1.84rem;
  margin-left: -0.92rem;
}
.sendCard4 {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translateX(-50%);
  margin-left: 1.35rem;
  flex-wrap: wrap;
  flex-direction: column-reverse;
  height: 1.84rem;
  margin-top: -0.8rem;
}

/* 用户信息 */
.userbox {
  width: 0.43rem;
}
.userbox .userImg {
  width: 0.36rem;
  height: 0.35rem;
  background: #f2e5bf;
  border: 0.04rem solid #d1a05b;
  border-radius: 0.03rem;
  overflow: hidden;
}
.userbox .nickname {
  color: #fff;
  font-size: 0.1rem;
  text-align: center;
}
.userbox .userpoint {
  background: #000;
  color: #fff;
  font-size: 0.1rem;
  text-align: center;
}
.play1 {
  position: absolute;
  right: 1.39rem;
  top: 0.05rem;
}
.play2 {
  position: absolute;
  left: 0.18rem;
  top: 50%;
  transform: translateY(-50%);
}
.play3 {
  position: absolute;
  left: 0.18rem;
  bottom: 0.37rem;
}
.play4 {
  position: absolute;
  right: 0.18rem;
  top: 50%;
  transform: translateY(-50%);
}
/* 百搭牌 */
.versatile {
  position: absolute;
  top: 0.33rem;
  right: 0.18rem;
}
.versatileImg {
  width: 0.32rem;
  height: 0.45rem;
  position: relative;
}
.verimg:after {
  content: "";
  background: url("../../static/image/verAfter.png") no-repeat;
  width: 0.14rem;
  height: 0.16rem;
  position: absolute;
  background-size: 100% 100%;
  right: 0rem;
  top: 0.02rem;
}

/* 玩家手上牌显示 */

.cardlist1 {
  position: fixed;
  top: 0.1rem;
  left: 50%;
  transform: translateX(-50%);
}

.cardlist1 .verimg,
.cardlist1 .cardThree img {
  width: 0.26rem;
  height: 0.36rem;
  margin-left: -0.05rem;
}
.cardlist img {
  width: 0.22rem;
  height: 0.3rem;
}
.cardlist2 {
  position: fixed;
  left: 0.8rem;
  top: 50%;
  transform: translateY(-50%);
  flex-direction: column;
}
.cardlist2 view,
.cardlist4 view {
  flex-direction: column;
}
.cardlist2 img,
.cardlist4 img {
  width: 0.14rem;
  height: 0.4rem;
  margin-top: -0.2rem;
}
.cardlist3 {
  position: fixed;
  left: 50%;
  bottom: 50%;
  transform: translate(-50%, -50%);
  width: fit-content;
}
.test1 .cardlist3 {
  position: fixed;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
}
.cardlist3 img {
  width: 0.4rem;
  height: 0.56rem;
}
.cardlist4 {
  position: fixed;
  right: 0.8rem;
  top: 50%;
  transform: translateY(-50%);
  flex-direction: column;
}
/*碰杠牌 */

.cardlist3 .cardMany img {
  width: 0.3rem;
  height: 0.4rem;
}
.cardlist2 .cardMany img,
.cardlist4 .cardMany img {
  width: 0.22rem;
  height: 0.3rem;
  margin-top: -0.07rem;
}

.cardSpace {
  margin-right: 0.05rem;
}
.cardlist2 .cardSpace,
.cardlist4 .cardSpace {
  margin-right: 0;
  margin-bottom: 0.1rem;
}
.cardlist1 .cardFlower {
  position: absolute;
  top: 0.45rem;
  left: 0;
}
.cardlist3 .cardFlower {
  position: absolute;
  top: -0.35rem;
  right: 0;
}
.cardlist2 .cardFlower {
  position: absolute;

  bottom: -0.07rem;
  right: -0.3rem;
}
.cardlist4 .cardFlower {
  position: absolute;
  top: -0.1rem;
  left: -0.3rem;
}
.cardlist2 .cardFlower img,
.cardlist4 .cardFlower img {
  margin-top: -0.07rem;
}
.cardFlower img {
  width: 0.22rem;
  height: 0.32rem;
}
.cardlist1 .cardback img {
  width: 0.3rem;
  height: 0.4rem;
}
.cardSpace2 {
  margin-bottom: 0.2rem;
}

/* 准备 */
.userbtn {
  background: url("../../static/image/btn/btn.png") no-repeat;
  font-size: 0.14rem;
  color: #fff;
  background-size: 100% 100%;
  display: inline-block;
}
.ready {
  padding: 0.1rem 0.1rem;
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* 买庄 */
.banerChoose {
  position: absolute;
  bottom: 30%;
  left: 50%;
  transform: translateX(-50%);
}
.needBanker {
  background: url("../../static/image/btn/btn.png") no-repeat;
  font-size: 0.14rem;
  color: #fff;
  background-size: 100% 100%;
  display: inline-block;
  padding: 0.1rem 0.2rem;
  margin-right: 0.1rem;
}

.noBanker {
  background: url("../../static/image/btn/btn_green.png") no-repeat;
  font-size: 0.14rem;
  color: #fff;
  background-size: 100% 100%;
  display: inline-block;
  padding: 0.1rem 0.2rem;
}

/* 庄家图标 */
.isbaner {
  position: absolute;
  top: 0.15rem;
  left: -0.08rem;
  width: 0.2rem;
  height: 0.2rem;
}
.bankermsg {
  height: 0.2rem;
}
/* 结算 */
.settle {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.8);
}
.settlebox {
  margin: 0 0.49rem;
}
.userSettle .settlemsg {
  height: 0.6rem;
  background: #efe4d0;
  border-radius: 0.01rem;
  margin-bottom: 0.03rem;
}
.userSettle .userhead {
  width: 0.34rem;
  height: 0.34rem;
  border: 0.03rem solid #cecacb;
  border-radius: 0.05rem;
  overflow: hidden;
  background: #f2e5bf;
  margin-left: 0.15rem;
}
.userSettle .bankerhead {
  position: absolute;
  width: 0.18rem;
  height: 0.2rem;
  top: -0.05rem;
  left: 0.1rem;
}
.userSettle span {
  font-size: 0.14rem;
  color: #7d553e;
  font-weight: bold;
  margin-left: 0.1rem;
  margin-right: 0.1rem;
}
.userCardlist img {
  width: 0.24rem;
  height: 0.34rem;
}
.userCardlist .cardFlower {
  margin-top: 0.2rem;
}
.userCardlist .cardFlower img {
  width: 0.2rem;
  height: 0.3rem;
}
.winLosemsg span {
  font-size: 0.24rem;
  background-image: -webkit-linear-gradient(top, #f6f39d, #b46500);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}
.winLosemsg {
  position: absolute;
  top: 50%;
  right: 0;
  transform: translateY(-50%);
}
.userSettle .iswin,
.userSettle .isneedBanker {
  width: 0.16rem;
  height: 0.19rem;
  margin-top: -0.4rem;
  position: absolute;
  left: 4.2rem;
}
.userSettle .isneedBanker {
  left: 4.42rem;
  width: 0.44rem;
  height: 0.2rem;
}
.continueBtn {
  position: absolute;
  bottom: 0.3rem;
  left: 50%;
  transform: translateX(-50%);
}
.continueBtn .continue {
  background: url("../../static/image/btn/btn1.png") no-repeat;
  background-size: 100% 100%;
  width: 1.2rem;
  height: 0.45rem;
}
.continueBtn .sharebtn {
  background: url("../../static/image/btn/btn2.png") no-repeat;
  background-size: 100% 100%;
  width: 1.2rem;
  height: 0.45rem;
  margin-right: 0.8rem;
}
/* 公用 */
img {
  max-width: 100%;
}
.clearfix:after {
  content: " ";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}

.clearfix {
  *zoom: 1;
}

.hidden {
  display: none !important;
}

.fl {
  float: left;
}

.fr {
  float: right;
}

/* flex */
.flex {
  display: -webkit-flex;
  display: flex;
}

.juscon-start {
  justify-content: flex-start;
}

.juscon-end {
  justify-content: flex-end;
}

.juscon-center {
  justify-content: center;
}

.juscon-between {
  justify-content: space-between;
}

.juscon-around {
  justify-content: space-around;
}

.alignitem-start {
  align-items: start;
}

.alignitem-center {
  align-items: center;
}

.alignitem-end {
  align-items: flex-end;
}

.ellipsis {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  max-width: 0.5rem;
}

.pos-r {
  position: relative;
}
</style>
