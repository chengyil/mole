<template>
  <v-app>
    <v-row sm="12">
      <v-col sm="8" offset-sm="2">
        <v-toolbar color="primary" dark>
          <v-toolbar-title>Whack the creep!</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-subheader>Score : {{ score }}</v-subheader>
          <v-btn v-on:click="play" v-if="state !== 'started'">Play</v-btn>
          <v-btn v-on:click="stop" v-if="state === 'started'">Stop</v-btn>
        </v-toolbar>
        <v-content>
          <Slots v-bind:slots="slots" v-on:scored="scored" v-on:losed="losed" />
          <v-dialog v-bind:value="state === 'won'" max-width="290">
            <v-card>
              <v-card-title class="headline"
                >Good job whacking that creep!</v-card-title
              >
            </v-card>
          </v-dialog>
          <v-dialog v-bind:value="state === 'lose'" max-width="290">
            <v-card>
              <v-card-title class="headline">Oh nooooooo...</v-card-title>
            </v-card>
          </v-dialog>
          <v-dialog v-bind:value="init" max-width="400">
            <v-card>
              <v-card-title class="title"
                >A simple Whack a Mole game.</v-card-title
              >
              <v-card-text class="display">
                It is silly but I hope it you enjoy whacking the creep or at
                least find it funny.
              </v-card-text>
              <v-card-text class="display"
                >Press Play to start the game! Score 10 points to
                win!</v-card-text
              >
              <v-card-text class="display"
                >You lose if you take too long to hit the creep!</v-card-text
              >
            </v-card>
          </v-dialog>
        </v-content>
      </v-col>
    </v-row>
  </v-app>
</template>

<script>
import Slots from "@/components/Slots";

const SIZE = 9;

export default {
  name: "App",

  components: {
    Slots,
  },

  methods: {
    play() {
      this.score = 0;
      this.state = "started";
      this.slots = [];
      for (let index = 0; index < SIZE; index++) {
        this.slots.push({ id: index, target: false });
      }
      this.clearTimer();
      this.timer = setInterval(() => {
        this.pickTarget();
      }, 500);
    },
    stop() {
      this.clearTimer();
      this.state = "new";
    },
    losed() {
      this.clearTimer();
      if (this.state === "started") {
        this.state = "lose";
      }
    },
    scored(index) {
      this.slots[index].target = false;
      this.score += 1;
      if (this.score >= 10) {
        this.clearTimer();
        this.state = "won";
      }
    },
    clearTimer() {
      if (this.timer !== null) {
        clearInterval(this.timer);
      }
    },
    pickTarget() {
      let notTargets = this.slots.filter((slot) => !slot.target);
      if (notTargets.length > 0) {
        let target = Math.floor(Math.random() * notTargets.length);
        notTargets[target].target = true;
      }
    },
  },
  created() {
    for (let index = 0; index < SIZE; index++) {
      this.slots.push({ id: index, target: false });
    }
  },
  computed: {
    started() {
      return this.state === "started";
    },
    newGame() {
      return this.state === "new";
    },
    won() {
      return this.state === "won";
    },
    won() {
      return this.state === "lose";
    },
  },
  data() {
    return {
      state: "new",
      slots: [],
      timer: null,
      score: 0,
      init: true,
    };
  },
};
</script>
