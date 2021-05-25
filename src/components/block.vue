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
      if (this.nextPlayer == "O") {
        this.nextPlayer = "X";
      } else if ((this.nextPlayer = "X")) {
        this.nextPlayer = "O";
      }
      // console.log(this.childstats[i - 1][j - 1].player);
      // 记录玩家移动的位置
      this.history.push({
        player: this.childstats[i - 1][j - 1].player,
        locationx: i,
        locationy: j,
        id: this.childstats[i - 1][j - 1].id,
      });
      // console.log(this.history);
      // 遍历玩家移动的位置，分别获得两个玩家的行动路线
      this.player1Location = this.history
        .filter((n) => n.player == "O")
        .map((n) => parseInt(n.id));
      this.player2Location = this.history
        .filter((n) => n.player == "X")
        .map((n) => parseInt(n.id));
      // console.log(this.player1Location);
      // console.log(this.player2Location);
      // console.log(this.player1Location[1]);
      this.findWinner();
    },
    checkId(arr1, arr2) {
      for (let i = arr2.length - 1; i >= 0; i--) {
        if (!arr1.includes(arr2[i])) {
          return false;
        }
      }

      return true;
    },
    findWinner() {
      for (let i = 0; i < this.lines.length; i++) {
        if (this.checkId(this.player1Location, this.lines[i])) {
          alert("玩家1胜");
          this.game_state = 0;
          return;
        } else if (this.checkId(this.player2Location, this.lines[i])) {
          alert("玩家2胜");
          this.game_state = 0;
          return;
        }
      }
    },
  },
};
</script>

<style scoped>
.box {
  background: #fff;
  margin: 0 auto;
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