<template>
  <div class="bingo">
    <div><h1>Bingo (No.1~75)</h1></div>
    <div class="box">
      <div class="currentNum" v-if="show">{{ currentNum }}</div>
      <div class="message" v-if="notShow">{{ message }}</div>
    </div>
    <div class="button">
      <button class="startstop" type="button" v-on:click="onclick" v-if="show">
        {{ button }}</button
      >&nbsp;&nbsp;<button class="reset" type="button" v-on:click="reset">
        Reset
      </button>
    </div>
    <ul class="number">
      <li
        class="selectedNum"
        v-for="(item, i) of selectedNum"
        v-bind:key="item"
      >
        {{ selectedNum[i] }}
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";

@Component
export default class Bingo extends Vue {
  //最小値
  private min = 1;
  //最大値
  private max = 75;
  //表示用
  private currentNum = 0;
  //最小値から最大値を入れる数値型の配列を用意
  private bingo: number[] = [];
  //出た数字を入れる配列
  private selectedNum: number[] = [];
  //ボタンの名前
  private button = "Start";
  //タイマー初期化
  private timer = 0;
  //リセットボタンの確認
  private confirmReset = true;
  //数字スロット用
  private show = true;
  //終了時メッセージ用
  private notShow = false;
  //終了時メッセージ
  private message = "Complete";

  /**
   *   min~maxの配列を作成
   */
  created(): void {
    for (let i = this.min; i <= this.max; i++) {
      this.bingo.push(i);
    }
  }
  /**
   * 配列の中から数字をランダムに選び、配列から削除
   */
  onclick(): void {
    if (this.button === "Start" && this.bingo.length === 1) {
      this.show = false;
      this.notShow = true;
    } else if (this.button === "Start") {
      //タイマースタート
      this.timer = setInterval(() => {
        //数字をランダムに選ぶ
        this.currentNum =
          this.bingo[Math.floor(Math.random() * this.bingo.length)];
      }, 25);
      //表示された数字を既出の配列に入れ、最初の配列から削除
      this.bingo = this.bingo.filter((item) => item !== this.currentNum);
      //ボタン表示の切り替え
      this.button = "Stop";
    } else {
      //タイマー停止
      clearInterval(this.timer);
      //配列にpush
      this.selectedNum.push(this.currentNum);
      //ボタン表示の切り替え
      this.button = "Start";
    }
  }
  /**
   * リセットする
   */
  reset(): void {
    //リセット確認用
    this.confirmReset = window.confirm("本当にリセットしますか？");
    if (this.confirmReset === true) {
      //タイマー停止
      clearInterval(this.timer);
      //配列を空にする
      this.selectedNum = [];
      this.bingo = [];
      //初期表示を0にする
      this.currentNum = 0;
      //ボタン表示の切り替え
      this.button = "Start";
      //新たに配列を作る
      for (let i = this.min; i <= this.max; i++) {
        this.bingo.push(i);
      }
      this.show = true;
      this.notShow = false;
    } else {
      return;
    }
  }
}
</script>
<style scoped>
.box {
  width: 70%;
  margin: 0 auto;
  font-size: 15vw;
  border: solid;
}
.selectedNum {
  font-size: 150%;
}
.button {
  margin: 10px;
}
button {
  margin: 0 20px;
  font-size: 200%;
  width: 100px;
  color: white;
  border-radius: 10px;
}
.startstop {
  background-color: #2c7cff;
}
.button .startstop:hover {
  background-color: #0000cd;
}

.reset {
  background-color: #999999;
}
.button .reset:hover {
  background-color: #555555;
}
ul {
  list-style: none;
}

.selectedNum {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0 auto;
}
.number {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  margin: 0 auto;
  list-style: none;
  width: 80%;
  padding: 0;
}
.number li {
  background-color: midnightblue;
  color: white;
  border-radius: 10px;
  height: 45px;
  margin: 4px;
  width: 45px;
}
</style>
