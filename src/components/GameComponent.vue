<template lang="pug">
.container
  h1 Tic Tac Toe
  form
    p
      label Player X :
        input(
          type="text",
          v-model="playerx",
          placeholder="Player X Name",
          :disabled="ready"
        )
    p
      label Player O :
        input(
          type="text",
          v-model="playero",
          placeholder="Player O Name",
          :disabled="ready"
        )
    button(:disabled="!playerx || !playero", @click.prevent="showGame") Start Game
  hr

  .game(v-show="ready")
    .names
      span {{ playerx }}
      | : {{ countx }} |
      span 
      |
      | {{ playero }}
      | : {{ counto }}

    table.tb(border="1")
      .stop
      tr
        td(data-index1="0", data-index2="0", @click="storageData")
        td(data-index1="0", data-index2="1", @click="storageData")
        td(data-index1="0", data-index2="2", @click="storageData")

      tr
        td(data-index1="1", data-index2="0", @click="storageData")
        td(data-index1="1", data-index2="1", @click="storageData")
        td(data-index1="1", data-index2="2", @click="storageData")

      tr
        td(data-index1="2", data-index2="0", @click="storageData")
        td(data-index1="2", data-index2="1", @click="storageData")
        td(data-index1="2", data-index2="2", @click="storageData")
    button(@click="playAgain") Play Again
    button(@click="popupOption") Reset

.popup-reset
  .reset-option
    p Do You Want To Change Names?
    button(@click="withChange") Yes
    button(@click="withoutChange") No
</template>

<script>
export default {
  name: "GameComponent",
  data() {
    return {
      playerx: "",
      playero: "",
      board: [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0],
      ],
      countx: 0,
      counto: 0,
      state: false,
      ready: false,
      totalRowOne: "",
      totalRowTwo: "",
      totalRowThree: "",
      totalColOne: "",
      totalColTwo: "",
      totalColThree: "",
      totalD: "",
      totalDrevers: "",
      winner: "",
    };
  },
  methods: {
    showGame: function () {
      this.ready = true;
      document.querySelector(".game").classList.add("show");
    },
    popupOption: function () {
      document.querySelector(".popup-reset").classList.add("open");
    },
    withoutChange: function () {
      document.querySelector(".popup-reset").classList.remove("open");
      this.countx = 0;
      this.counto = 0;
      this.board = [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0],
      ];
      document.querySelectorAll("td").innerText = "";
      this.winner = "";
      document.querySelectorAll("td").forEach((el) => {
        el.innerText = "";
      });
      document.querySelector(".stop").classList.remove("overlay");
    },
    withChange: function () {
      document.querySelector(".game").classList.remove("show");
      document.querySelector(".popup-reset").classList.remove("open");
      this.playerx = null;
      this.playero = "";
      this.ready = false;
      this.countx = 0;
      this.counto = 0;
      this.board = [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0],
      ];
      this.winner = "";
      document.querySelectorAll("td").forEach((el) => {
        el.innerText = "";
      });
      document.querySelector(".stop").classList.remove("overlay");
    },
    playAgain: function () {
      this.board = [
        [0, 0, 0],
        [0, 0, 0],
        [0, 0, 0],
      ];
      this.winner = "";
      document.querySelectorAll("td").forEach((el) => {
        el.innerText = "";
      });
      document.querySelector(".stop").classList.remove("overlay");
    },
    storageData: function (e) {
      if (this.board[e.target.dataset.index1][e.target.dataset.index2] === 0) {
        if (this.state === false) {
          e.target.innerText = "X";
          this.board[e.target.dataset.index1][e.target.dataset.index2] = "x";
          this.state = true;
        } else {
          e.target.innerText = "O";
          this.board[e.target.dataset.index1][e.target.dataset.index2] = "o";
          this.state = false;
        }
      }
      for (let i = 0; i < this.board.length; i++) {
        this.totalRowOne += this.board[0][i];
        this.totalRowTwo += this.board[1][i];
        this.totalRowThree += this.board[2][i];
        this.totalColOne += this.board[i][0];
        this.totalColTwo += this.board[i][1];
        this.totalColThree += this.board[i][2];
        this.totalD += this.board[i][i];
      }
      for (let i = 0; i < this.board.reverse().length; i++) {
        this.totalDrevers += this.board[i][i];
      }
      console.log(this.totalRowOne.substr(this.totalRowOne.length - 3));
      if (
        this.totalRowOne.substr(this.totalRowOne.length - 3) === "xxx" ||
        this.totalRowTwo.substr(this.totalRowTwo.length - 3) === "xxx" ||
        this.totalRowThree.substr(this.totalRowThree.length - 3) === "xxx" ||
        this.totalColOne.substr(this.totalColOne.length - 3) === "xxx" ||
        this.totalColTwo.substr(this.totalColTwo.length - 3) === "xxx" ||
        this.totalColThree.substr(this.totalColThree.length - 3) === "xxx" ||
        this.totalD.substr(this.totalD.length - 3) === "xxx" ||
        this.totalDrevers.substr(this.totalDrevers.length - 3) === "xxx"
      ) {
        this.winner = this.playerx;
        alert(this.winner);
        this.countx++;
      } else if (
        this.totalRowOne.substr(this.totalRowOne.length - 3) === "ooo" ||
        this.totalRowTwo.substr(this.totalRowTwo.length - 3) === "ooo" ||
        this.totalRowThree.substr(this.totalRowThree.length - 3) === "ooo" ||
        this.totalColOne.substr(this.totalColOne.length - 3) === "ooo" ||
        this.totalColTwo.substr(this.totalColTwo.length - 3) === "ooo" ||
        this.totalColThree.substr(this.totalColThree.length - 3) === "ooo" ||
        this.totalD.substr(this.totalD.length - 3) === "ooo" ||
        this.totalDrevers.substr(this.totalDrevers.length - 3) === "ooo"
      ) {
        this.winner = this.playero;
        this.counto++;
        alert(this.winner);
      }
      if (this.winner !== "") {
        document.querySelector(".stop").classList.add("overlay");
      }
    },
  },
};
</script>