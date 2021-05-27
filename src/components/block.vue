<template>
  <div>
    <div class="body">
      <div
        class="box"
        v-for="item in childstats"
        :key="item"
        @click="changePlayer(item)"
      >
        {{ item.player }}
      </div>
    </div>

    <!-- <button @click="goBack">返回上一步</button> -->
    <button
      v-for="(item, index) of history"
      :key="index"
      @click="goBack(index)"
    >
      返回第{{ index }}步
    </button>
  </div>
</template>

<script>
export default {
  name: "block",
  data() {
    return {
      childstats: [
        { id: "1", player: "" },
        { id: "2", player: "" },
        { id: "3", player: "" },
        { id: "4", player: "" },
        { id: "5", player: "" },
        { id: "6", player: "" },
        { id: "7", player: "" },
        { id: "8", player: "" },
        { id: "9", player: "" },
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
    changePlayer(item) {
      if (!this.game_state) return;
      // 点击写入玩家
      // 如果玩家不为空
      if (item.player) return;
      item.player = this.nextPlayer;
      // 记录玩家移动的位置
      this.history.push({
        player: item.player,
        id: item.id,
      });
      this.nextPlayer = this.nextPlayer == "O" ? "X" : "O";
      // 遍历玩家移动的位置，分别获得两个玩家的行动路线
      this.player1Location = this.history
        .filter((n) => n.player == "O")
        .map((n) => parseInt(n.id));
      this.player2Location = this.history
        .filter((n) => n.player == "X")
        .map((n) => parseInt(n.id));
      this.findWinner();
      console.log(this.childstats.player);
    },
    // 比较数组1是否包含数组2
    checkId(arr1, arr2) {
      return arr2.every((v) => arr1.includes(v));
    },
    // 判断获胜玩家函数
    findWinner() {
      this.lines.some((v) => {
        // 如果玩家1的位置数组中包含了胜利条件数组;
        if (this.checkId(this.player1Location, v)) {
          alert("玩家1胜");
          // 游戏结束，将游戏状态置0
          this.game_state = 0;
          return;
          // 如果玩家2的位置数组中包含了胜利条件数组
        } else if (this.checkId(this.player2Location, v)) {
          alert("玩家2胜");
          this.game_state = 0;
          return;
        }
      });
    },
    goBack(index) {
      this.game_state = 1;
      this.clearOut();
      // slice返回一个数组，第一个参数为开始位置，第二个为结束位置
      // this.history2 = this.history.slice(index);
      this.history = this.history.slice(0, index);
      // 如果index为0那就初始化棋盘，将下一个玩家置为O
      if (!index) {
        this.nextPlayer = "O";
        return;
      }
      this.history.forEach((v) => {
        this.childstats[Number(v.id) - 1].player = v.player;
      });
      this.nextPlayer = this.history[index - 1].player == "O" ? "X" : "O";
    },
    clearOut() {
      this.childstats.forEach((v) => (v.player = ""));
    },
  },
};
</script>

<style scoped>
.box {
  width: 100px;
  height: 100px;
  line-height: 100px;
  background: #fff;
  border-left: 1px solid black;
  border-bottom: 1px solid black;
  text-align: center;
  font-size: 40px;
}

.body {
  display: flex;
  width: 303px;
  flex-wrap: wrap;
  border-right: 1px solid black;
  border-top: 1px solid black;
  margin: auto;
}
button {
  margin-top: 10px;
}
</style>