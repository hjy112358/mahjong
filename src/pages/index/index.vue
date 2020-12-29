<template>
  <view class="content">
    <!-- 返回 -->
    <view class="back flex alignitem-center juscon-center">
      <img src="../../static/image/back.png" />
    </view>
    <!-- 位置 倒计时区域 -->

    <view class="positionbox" v-show="true">
      <view class="positionview" :class="'rotate'+which">
        <view class="countdown">
          <view class="countbox">
             <span>{{timer}}</span>
          </view>
         
        </view>
        <view class="topImg positionImg flex alignitem-center juscon-center" v-show="false">
          <img src="../../static/image/top.png" alt />
        </view>
        <view class="leftImg positionImg flex alignitem-center juscon-center" v-show="false">
          <img src="../../static/image/left.png" alt />
        </view>
        <view class="rightImg positionImg flex alignitem-center juscon-center">
          <img src="../../static/image/right.png" alt />
        </view>
        <view class="bottomImg positionImg flex alignitem-center juscon-center">
          <img src="../../static/image/bottom.png" alt />
        </view>
      </view>
    </view>

    <!-- 剩余 -->
    <view class="showtext showtextLeft" v-show="true">
      <p>
        剩余
        <span>{{surplus}}</span>张
      </p>
    </view>
    <!-- 局数 -->
    <view class="showtext showtextRight" v-show="true">
      <p>
        第
        <span>{{gameNum}}</span>局
      </p>
    </view>
    <!-- 百搭牌 -->
    <view class="versatile">
      <view class="versatileImg flex alignitem-center juscon-center verimg" v-if="wantOnly">
        <img :src="wantOnly.src" alt />
      </view>
    </view>
    <!-- 买庄 -->
    <view class="banerChoose flex juscon-start" v-show="false">
      <p class="needBanker">买庄</p>
      <p class="noBanker">不买</p>
    </view>
    <!-- 出的牌 -->
    <view class="userSendcard" v-for="(item, index) in 4" :key="index" v-show="true">
      <view class="flex" v-if="index==0&&cardlist[0]" :class="`sendCard${index + 1}`">
        <img
          :src="item.src"
          alt
          v-for="(item, index) in cardlist[0].sendCard"
          :key="index"
          :style="zindex(0,index)"
        />
      </view>
      <view class="flex" v-if="index==1&&cardlist[1]" :class="`sendCard${index + 1}`">
        <img :src="item.src" alt v-for="(item, index) in cardlist[1].sendCard" :key="index" />
      </view>
      <view class="flex" v-if="index==2&&cardlist[2]" :class="`sendCard${index + 1}`">
        <img :src="item.src" alt v-for="(item, index) in cardlist[2].sendCard" :key="index" />
      </view>
      <view class="flex" v-if="index==3&&cardlist[3]" :class="`sendCard${index + 1}`">
        <img
          :src="item.src"
          alt
          v-for="(item, index) in cardlist[3].sendCard"
          :key="index"
          :style="zindex(4,index)"
        />
      </view>
    </view>

    <!-- 牌及用户信息 -->
    <!-- 玩家牌 -->
    <view class="cardmsg" v-show="true">
      <view v-for="(item, index) in usersList" :key="index">
        <view class="userCard">
          <!-- 上 -->
          <view v-if="index==0&&cardlist[0]" class="flex cardlist" :class="`cardlist${index+ 1}`">
            <view class="flex cardSpace cardback" v-if="cardlist[0].get">
              <img src="../../static/image/topBack.png" />
            </view>
            <view class="flex cardSpace cardback" v-if="cardlist[0].card">
              <img
                src="../../static/image/topBack.png"
                v-for="(item, index) in cardlist[0].card"
                :key="index"
              />
            </view>
            <view class="flex cardMany cardSpace" v-if="cardlist[0].Four">
              <img
                :src="item.show?item.src:'../../static/image/topBack.png'"
                v-for="(item, index) in cardlist[0].Four"
                :key="index"
                :class="item.show?'':'cardDark'"
              />
            </view>
            <view class="flex cardMany cardSpace cardThree" v-if="cardlist[0].three">
              <img
                :src="item.show?item.src:'../../static/image/topBack.png'"
                v-for="(item, index) in cardlist[0].three"
                :key="index"
              />
            </view>

            <view class="flex cardFlower" v-if="cardlist[0].flower">
              <img :src="item.src" v-for="(item, index) in cardlist[0].flower" :key="index" />
            </view>
          </view>
          <!-- 左 -->
          <view
            v-if="index==1&&cardlist[1]"
            class="flex cardlist alignitem-end"
            :class="`cardlist${index+ 1}`"
          >
            <view class="flex cardSpace cardFlower" v-if="cardlist[1].flower">
              <img :src="item.src" v-for="(item, index) in cardlist[1].flower" :key="index" />
            </view>
            <view class="flex cardMany cardSpace alignitem-end" v-if="cardlist[1].three">
              <img
                :src="item.show?item.src:'../../static/image/card/mj_dark.png'"
                v-for="(item, index) in cardlist[1].three"
                :key="index"
                :class="item.show?'':'cardDark'"
              />
            </view>
            <view class="flex cardMany cardSpace2 alignitem-end" v-if="cardlist[1].Four">
              <img
                :src="item.show?item.src:'../../static/image/card/mj_dark.png'"
                v-for="(item, index) in cardlist[1].Four"
                :key="index"
                :class="item.show?'':'cardDark'"
              />
            </view>
            <view class="flex cardSpace alignitem-end" v-if="cardlist[1].card">
              <img
                src="../../static/image/leftBack.png"
                v-for="(item, index) in cardlist[1].card"
                :key="index"
              />
            </view>
            <view class="flex alignitem-end" v-if="cardlist[1].get">
              <img src="../../static/image/leftBack.png" />
            </view>
          </view>
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
          <!-- 右 -->
          <view
            v-if="index==3&&cardlist[3]"
            class="flex cardlist alignitem-start"
            :class="`cardlist${index+ 1}`"
          >
            <view class="flex alignitem-start cardSpace" v-if="cardlist[3].get">
              <img src="../../static/image/rightBack.png" />
            </view>
            <view class="flex cardSpace alignitem-start" v-if="cardlist[3].card">
              <img
                src="../../static/image/rightBack.png"
                v-for="(item, index) in cardlist[3].card"
                :key="index"
              />
            </view>
            <view class="flex cardMany cardSpace alignitem-start" v-if="cardlist[3].Four">
              <img
                :src="item.show?item.src:'../../static/image/card/mj_dark.png'"
                v-for="(item, index) in cardlist[3].Four"
                :key="index"
                :class="item.show?'':'cardDark'"
              />
            </view>
            <view class="flex cardMany cardSpace alignitem-start" v-if="cardlist[3].three">
              <img
                :src="item.show?item.src:'../../static/image/card/mj_dark.png'"
                v-for="(item, index) in cardlist[3].three"
                :key="index"
                :class="item.show?'':'cardDark'"
              />
            </view>
            <view class="flex cardFlower" v-if="cardlist[3].flower">
              <img :src="item.src" v-for="(item, index) in cardlist[3].flower" :key="index" />
            </view>
          </view>
        </view>
      </view>
    </view>
    <!--玩家信息-->
    <view class>
      <view v-for="(item, index) in usersList" :key="index" :class="`play${index + 1}`">
        <!-- 其他玩家信息 -->
        <view class="userbox pos-r" v-if="index!=2">
          <p class="nickname ellipsis">{{item.nickname}}</p>
          <view class="userImg flex juscon-center alignitem-center">
            <img src="../../static/image/head.png" alt />
          </view>
          <p class="userpoint ellipsis">{{item.point | moneyFilter}}</p>
          <view class="isbaner flex" v-if="bankerId==item.id">
            <img src="../../static/image/banker.png" alt />
          </view>
          <view class="flex bankermsg">
            <img src="../../static/image/needBaner.png" alt v-if="item.isbanker" />
            <img src="../../static/image/noBaner.png" alt v-else />
          </view>
        </view>

        <!-- 我 -->
        <view class="userbox" v-if="index==2">
          <p class="nickname ellipsis">{{item.nickname}}</p>
          <view class="userImg flex juscon-center alignitem-center">
            <img src="../../static/image/head.png" alt />
          </view>
          <p class="userpoint ellipsis">{{item.point | moneyFilter}}</p>
          <view class="isbaner flex" v-if="bankerId==item.id">
            <img src="../../static/image/banker.png" alt />
          </view>
          <view class="flex bankermsg">
            <img src="../../static/image/needBaner.png" alt v-if="item.isbanker" />
            <img src="../../static/image/noBaner.png" alt v-else />
          </view>
        </view>
      </view>
    </view>

    <!-- 准备 -->
    <p class="ready userbtn" v-show="false">准备({{readyTime}}s)</p>

    <!-- 结算 -->
    <view class="settle" v-show="false">
      <view class="back flex alignitem-center juscon-center">
        <img src="../../static/image/back.png" />
      </view>
      <view class="settlebox">
        <view class="flex">
          <img src="../../static/image/settletop.png" alt />
        </view>
        <view class="userSettle">
          <view
            class="flex alignitem-center pos-r settlemsg"
            v-for="(item,index) in usersList"
            :key="index"
          >
            <view class="pos-r">
              <view class="userhead flex alignitem-center juscon-center">
                <img src="../../static/image/head.png" alt />
              </view>
              <img src="../../static/image/banker.png" class="bankerhead" />
            </view>

            <span class="ellipsis">昵称</span>
            <view class="userCardlist">
              <view class="flex cardlist alignitem-center">
                <view class="flex cardMany cardSpace alignitem-center" v-if="cardlist[index].three">
                  <img
                    :src="item.imgsrc"
                    v-for="(item, index) in cardlist[index].three"
                    :key="index"
                  />
                </view>
                <view class="flex cardMany cardSpace alignitem-center" v-if="cardlist[index].Four">
                  <img
                    :src="item.imgsrc"
                    v-for="(item, index) in cardlist[index].Four"
                    :key="index"
                  />
                </view>

                <view class="flex cardSpace" v-if="cardlist[index].card">
                  <view class v-for="(item, index) in cardlist[index].card" :key="index">
                    <view class="flex verimg pos-r alignitem-center" v-if="item.iswantonly">
                      <img :src="item.imgsrc" />
                    </view>
                    <view class="flex alignitem-center" v-else>
                      <img :src="item.imgsrc" />
                    </view>
                  </view>
                </view>
                <view class="flex alignitem-center cardSpace" v-if="cardlist[index].get">
                  <img :src="cardlist[index].get[0].imgsrc" />
                </view>
                <view class="flex cardFlower alignitem-center" v-if="cardlist[index].flower">
                  <img
                    :src="item.imgsrc"
                    v-for="(item, index) in cardlist[index].flower"
                    :key="index"
                  />
                </view>
              </view>
            </view>
            <view class="winLosemsg alignitem-center flex">
              <span>+0</span>
              <!-- <span>+{{item.point | moneyFilter}}</span> -->
            </view>
            <view class="iswin flex alignitem-start">
              <img src="../../static/image/win.png" alt />
            </view>
            <view class="isneedBanker flex alignitem-start">
              <!-- <img src="../../static/image/noBaner.png" alt=""> -->
              <img src="../../static/image/needBaner.png" alt />
            </view>
          </view>

          <view class="continueBtn flex">
            <p class="userbtn sharebtn flex alignitem-center juscon-center">分享到微信</p>
            <p class="userbtn continue alignitem-center juscon-center flex">继续游戏({{continueTime}}s)</p>
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
      // 基础参数
      timer: 15, //倒计时
      surplus: 50, //剩余的牌
      gameNum: 1, //局数
      timer: 30, //倒计时时间

      // 我的信息
      userID: 1,
      token: "",
      roomId: "",
      bankerId: "3",

      // 用户信息
      usersList: [
        { nickname: "001", id: "1", point: 10000, isbanker: false },
        { nickname: "002", id: "2", point: 200, isbanker: false },
        { nickname: "003", id: "3", point: 300, isbanker: false },
        { nickname: "004", id: "4", point: 400, isbanker: true }
      ],

      // cardNum:牌面值
      // cardtype:0:万  1:条  2:筒  3:东(1)南(2)西(3)北(4)中(5)发(6)白(7) 4:春(1)夏(2)秋(3)冬(4)梅(5)兰(6)竹(7)菊(8)
      // iswantonly：true/false

      wantOnly: null,
      cardlist: [],

      //双击参数
      index: 0,
      start_time: 0,
      end_time: 0,

      //准备
      readyTime: 10,
      readyTimer: null, //准备倒计时定义

      // 继续游戏
      continueTime: 10,
      which:0
    };
  },
  created() {
    // 页面自适应
    this.remFn();
    this.getdata();
  },
  mounted() {
    this.changeUserinfo();
  },
  filters: {
    moneyFilter(value) {
      if (value >= 10000) {
        return (value / 10000).toFixed(2) + `万`;
      } else {
        return value;
      }
    }
  },
  onLoad() {},
  methods: {
    tingPai(){
      // 打出去某张牌  提示听哪张可以胡牌
      
    },
    // 改变位置
    changeUserinfo() {
      var arr = [2, 1, 0, 3];
     
      var which = null;
      for (var i = 0; i < this.usersList.length; i++) {
        if (this.usersList[i].id == this.userID) {
          which = i;
        }
      }
      this.which=which;
      var users = [];
      while (which--) {
        var tmp = arr[4 - 1];
        for (var i = 4 - 1; i >= 1; i--) {
          arr[i] = arr[i - 1];
        }
        arr[0] = tmp;
      }
      for (var i = 0; i < arr.length; i++) {
        users[i] = this.usersList[arr[i]];
      }
      this.usersList = users;
      
      this.usersList.splice();
    },
    getdata() {
      this.wantOnly = {
        cardNum: 1,
        cardtype: 1,
        iswantonly: true,
        src: "../../static/image/card/bottom/tiao1.png"
      };
      var cardlistnormal = [
        {
          card: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan1.png",
              imgsrc: "../../static/image/card/bottom/wan1.png"
            },
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan1.png",
              imgsrc: "../../static/image/card/bottom/wan1.png"
            },
            {
              cardNum: 3,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan1.png",
              imgsrc: "../../static/image/card/bottom/wan1.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan1.png",
              imgsrc: "../../static/image/card/bottom/wan1.png"
            }
          ],

          three: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/top/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/top/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/top/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            }
          ],

          Four: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/top/wan3.png",
              imgsrc: "../../static/image/card/bottom/wan3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/top/wan3.png",
              imgsrc: "../../static/image/card/bottom/wan3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/top/wan3.png",
              imgsrc: "../../static/image/card/bottom/wan3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/top/wan3.png",
              imgsrc: "../../static/image/card/bottom/wan3.png"
            }
          ],

          flower: [
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/top/f1.png",
              imgsrc: "../../static/image/card/bottom/f1.png"
            },
            {
              cardNum: 3,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/top/f2.png",
              imgsrc: "../../static/image/card/bottom/f2.png"
            }
          ],

          get: [
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/tong1.png",
              imgsrc: "../../static/image/card/bottom/tong1.png"
            }
          ],
          sendCard: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/top/tiao3.png"
            }
          ]
        },
        {
          card: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: true,
              src: "../../static/image/card/left/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 3,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            }
          ],

          three: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/left/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/left/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/left/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            }
          ],

          Four: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/left/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/left/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/left/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/left/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            }
          ],

          flower: [
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 3,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/left/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            }
          ],

          get: [
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/tiao4.png",
              imgsrc: "../../static/image/card/bottom/tiao4.png"
            }
          ],
          sendCard: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/left/tiao3.png"
            }
          ]
        },
        {
          card: [
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/bottom/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/bottom/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/bottom/wan2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 2,
              cardtype: 1,
              iswantonly: false,
              src: "../../static/image/card/bottom/tiao2.png",
              imgsrc: "../../static/image/card/bottom/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 1,
              iswantonly: true,
              src: "../../static/image/card/bottom/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 2,
              cardtype: 1,
              iswantonly: false,
              src: "../../static/image/card/bottom/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 2,
              cardtype: 3,
              iswantonly: false,
              src: "../../static/image/card/bottom/tong2.png",
              imgsrc: "../../static/image/card/bottom/tong2.png"
            },
            {
              cardNum: 1,
              cardtype: 3,
              iswantonly: false,
              src: "../../static/image/card/bottom/tong1.png",
              imgsrc: "../../static/image/card/bottom/tong1.png"
            },
            {
              cardNum: 2,
              cardtype: 3,
              iswantonly: false,
              src: "../../static/image/card/bottom/tong2.png",
              imgsrc: "../../static/image/card/bottom/tong2.png"
            },
            {
              cardNum: 2,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/bottom/zi2.png",
              imgsrc: "../../static/image/card/bottom/zi2.png"
            },
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/bottom/zi1.png",
              imgsrc: "../../static/image/card/bottom/zi1.png"
            },
            {
              cardNum: 2,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/bottom/zi2.png",
              imgsrc: "../../static/image/card/bottom/zi2.png"
            }
          ]

          // three: [
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: true,
          //     src: "../../static/image/card/bottom/wan2.png",
          //  imgsrc: "../../static/image/card/bottom/wan2.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: true,
          //     src: "../../static/image/card/bottom/wan2.png",
          //  imgsrc: "../../static/image/card/bottom/wan2.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: true,
          //     src: "../../static/image/card/bottom/wan2.png",
          //  imgsrc: "../../static/image/card/bottom/wan2.png"
          //   }
          // ],

          // Four: [
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: false,
          //     src: "../../static/image/card/bottom/wan3.png",
          //  imgsrc: "../../static/image/card/bottom/wan3.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: false,
          //     src: "../../static/image/card/bottom/wan3.png",
          //  imgsrc: "../../static/image/card/bottom/wan3.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: false,
          //     src: "../../static/image/card/bottom/wan3.png",
          //  imgsrc: "../../static/image/card/bottom/wan3.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     show: false,
          //     src: "../../static/image/card/bottom/wan3.png",
          //  imgsrc: "../../static/image/card/bottom/wan3.png"
          //   }
          // ],

          // flower: [
          //   {
          //     cardNum: 1,
          //     cardtype: 4,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/f1.png",
          //  imgsrc: "../../static/image/card/bottom/f1.png"
          //   },
          //   {
          //     cardNum: 3,
          //     cardtype: 4,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/f2.png",
          //  imgsrc: "../../static/image/card/bottom/f2.png"
          //   }
          // ],

          // get: [
          //   {
          //     cardNum: 2,
          //     cardtype: 0,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/tong1.png",
          //  imgsrc: "../../static/image/card/bottom/tong1.png"
          //   }
          // ],
          // sendCard: [
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/wan2.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/wan2.png"
          //   },
          //   {
          //     cardNum: 1,
          //     cardtype: 0,
          //     iswantonly: false,
          //     src: "../../static/image/card/bottom/tiao3.png"
          //   }
          // ]
        },
        {
          card: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: true,
              src: "../../static/image/card/right/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 3,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/tiao1.png",
              imgsrc: "../../static/image/card/bottom/tiao1.png"
            }
          ],

          three: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/right/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/right/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: true,
              src: "../../static/image/card/right/tiao2.png",
              imgsrc: "../../static/image/card/bottom/tiao2.png"
            }
          ],

          Four: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/right/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/right/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/right/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              show: false,
              src: "../../static/image/card/right/tiao3.png",
              imgsrc: "../../static/image/card/bottom/tiao3.png"
            }
          ],

          flower: [
            {
              cardNum: 1,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/right/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            },
            {
              cardNum: 3,
              cardtype: 4,
              iswantonly: false,
              src: "../../static/image/card/right/f4.png",
              imgsrc: "../../static/image/card/bottom/f4.png"
            }
          ],

          get: [
            {
              cardNum: 2,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/tiao4.png",
              imgsrc: "../../static/image/card/bottom/tiao4.png"
            }
          ],
          sendCard: [
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/wan2.png"
            },
            {
              cardNum: 1,
              cardtype: 0,
              iswantonly: false,
              src: "../../static/image/card/right/tiao3.png"
            }
          ]
        }
      ];
      this.sortCard(cardlistnormal[2]);
      this.cardlist = cardlistnormal;
    },
    sortCard(data) {},
    // 准备倒计时
    readyFn() {
      this.readyTime = this.timer;
      clearInterval(this.readyTimer);
      this.readyTimer = setInterval(() => {
        if (this.readyTime <= 1) {
          clearInterval(this.readyTimer);
          // this.exitRoomFn();
        }
        this.readyTime--;
      }, 1000);
    },
    // 退出房间
    exitRoomFn() {
      console.log("退出房间");
    },
    // 双击出牌
    fndbClick(item) {
      this.index = this.index == 0 ? 1 : this.index + 1;
      if (this.index == 1) {
        this.start_time = new Date().getTime();
        setTimeout(() => {
          this.index = this.start_time = this.end_time = 0;
        }, 200);
      } else if (this.index == 2) {
        this.end_time = new Date().getTime();
        if (this.end_time - this.start_time < 300) {
          console.log(item);
        }
        this.index = this.start_time = this.end_time = 0;
      }
    },
    //堆叠牌的z-index
    zindex(which, index) {
      var zIndex;
      if (which == 0) {
        if (index > 15) {
          zIndex = 1;
        } else if (index > 7) {
          zIndex = 2;
        } else {
          zIndex = 3;
        }
      } else {
        zIndex = 40 - index * 1;
      }

      return {
        zIndex: zIndex
      };
    }
  }
};
</script>

<style>
page {
  width: 100%;
  height: 100%;
  background: url("../../static/image/bg.png") no-repeat;
  background-size: 100% 100%;
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
}
.positionbox .positionview {
  background: url("../../static/image/position.png") no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 100%;
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
.rotate0{
    transform:rotate(90deg)
}
.rotate0 .countbox{
  transform: rotate(-90deg);
}
.rotate3{
    transform:rotate(180deg)
}
.rotate3 .countbox{
  transform: rotate(-180deg);
   
}
.rotate2{
    transform:rotate(270deg)
}
.rotate2 .countbox{
  transform: rotate(-270deg);
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
  bottom: 0.1rem;
  transform: translateX(-50%);
  width: fit-content;
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
