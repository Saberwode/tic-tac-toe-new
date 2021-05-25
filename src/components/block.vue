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
      nextPlayer: "O",
    };
  },
  props: [],
  methods: {
    changePlayer(i, j) {
      // console.log("nextPlayer");
      // console.log(this.nextPlayer);
      if (!this.childstats[i - 1][j - 1].click_state) return;
      this.childstats[i - 1][j - 1].player = this.nextPlayer;
      this.childstats[i - 1][j - 1].click_state = false;
      if (this.nextPlayer == "O") {
        this.nextPlayer = "X";
      } else if ((this.nextPlayer = "X")) {
        this.nextPlayer = "O";
      }
      console.log("当前点击的player");
      console.log(this.childstats[i - 1][j - 1].player);
      // console.log("转换后的player");
      // console.log(this.nextPlayer);
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