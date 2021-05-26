<template>
  <!-- <div class="box" @click="changePlayer(i, j)">
    {{ childstats[i - 1][j - 1].id }}
  </div>
  <div v-for="i in 3" :key="i">
    <div class="body">
      <div v-for="j in 3" :key="j">
        <block :i="i" :j="j"></block>
      </div>
    </div>
  </div> -->
  <div>
    <div v-for="i in 3" :key="i">
      <div class="body">
        <div v-for="j in 3" :key="j">
          <div class="box" @click="changePlayer(i, j)">
            {{ childstats[i - 1][j - 1].player }}
          </div>
        </div>
      </div>
    </div>
    <button @click="goBack">返回上一步</button>
  </div>
</template>

<script>
export default {
  name: "block",
  data() {
    return {
      childstats: [
        [
          { id: "1", click_state: true, player: "" },
          { id: "2", click_state: true, player: "" },
          { id: "3", click_state: true, player: "" },
        ],
        [
          { id: "4", click_state: true, player: "" },
          { id: "5", click_state: true, player: "" },
          { id: "6", click_state: true, player: "" },
        ],
        [
          { id: "7", click_state: true, player: "" },
          { id: "8", click_state: true, player: "" },
          { id: "9", click_state: true, player: "" },
        ],
      ],
      lines: [
        [1, 2, 3],
        [4, 5, 6],
        [7, 8, 9],
        [1, 4, 7],
        [2, 5, 8],
        [3, 6, 9],
        [1, 5, 9],
        [3, 5, 7],
      ],
      nextPlayer: "O",
      history: [],
      player1Location: [],
      player2Location: [],
      game_state: 1,
    };
  },
  props: [],
  methods: {
    changePlayer(i, j) {
      if (!this.game_state) return;
      // 点击写入玩家
      if (!this.childstats[i - 1][j - 1].click_state) return;
      this.childstats[i - 1][j - 1].player = this.nextPlayer;
      this.childstats[i - 1][j - 1].click_state = false;
      // 记录玩家移动的位置
      this.history.push({
        player: this.childstats[i - 1][j - 1].player,
        locationx: i,
        locationy: j,
        id: this.childstats[i - 1][j - 1].id,
      });
      if (this.nextPlayer == "O") {
        this.nextPlayer = "X";
      } else if ((this.nextPlayer = "X")) {
        this.nextPlayer = "O";
      }
      // 遍历玩家移动的位置，分别获得两个玩家的行动路线
      this.player1Location = this.history
        .filter((n) => n.player == "O")
        .map((n) => parseInt(n.id));
      this.player2Location = this.history
        .filter((n) => n.player == "X")
        .map((n) => parseInt(n.id));
      this.findWinner();
    },
    // 比较数组1是否包含数组2
    checkId(arr1, arr2) {
      for (let i = arr2.length - 1; i >= 0; i--) {
        if (!arr1.includes(arr2[i])) {
          return false;
        }
      }

      return true;
    },
    // 判断获胜玩家函数
    findWinner() {
      for (let i = 0; i < this.lines.length; i++) {
        // 如果玩家1的位置数组中包含了胜利条件数组
        if (this.checkId(this.player1Location, this.lines[i])) {
          alert("玩家1胜");
          // 游戏结束，将游戏状态置0
          this.game_state = 0;
          return;
          // 如果玩家2的位置数组中包含了胜利条件数组
        } else if (this.checkId(this.player2Location, this.lines[i])) {
          alert("玩家2胜");
          this.game_state = 0;
          return;
        }
      }
    },
    // 玩家悔棋函数
    goBack() {
      // 判断是否是第一步
      if (!this.history[this.history.length - 1]) {
        alert("无法返回上一步！");
        return;
      }
      // 将玩家最后一步的坐标给取出来
      let i = this.history[this.history.length - 1].locationx;
      let j = this.history[this.history.length - 1].locationy;
      // 将历史记录中的最后一步移除
      this.history.pop();
      // 将最后一步的玩家删除，同时允许点击该网格
      this.childstats[i - 1][j - 1].player = "";
      this.childstats[i - 1][j - 1].click_state = true;
      // 将下一位玩家状态改为上一步
      if (this.nextPlayer == "O") {
        this.nextPlayer = "X";
      } else if ((this.nextPlayer = "X")) {
        this.nextPlayer = "O";
      }
      // 将游戏状态改为1，防止因为游戏结束问题，不能继续游戏
      this.game_state = 1;
    },
  },
};
</script>

<style scoped>
.box {
  background: #fff;
  margin: 0 aut o;
  width: 100px;
  height: 100px;
  text-align: center;
  line-height: 100px;
  float: left;
  border-left: 1px solid black;
  border-bottom: 1px solid black;
}

.body {
  width: 303px;
  height: 100px;
  margin: 0 auto;
  display: block;
  border-right: 1px solid black;
  border-top: 1px solid black;
}
</style>