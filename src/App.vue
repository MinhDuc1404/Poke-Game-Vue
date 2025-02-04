<template>
  <main-screen v-if="StatusPlay === 'None'" @OnPlayMatch="MatchOptions($event)" />
  <match-screen v-if="StatusPlay === 'Play'" :Card="Setting.ArrayContext" @OnFinishMatch="SwitchResult" />
  <result-screen v-if="StatusPlay === 'Finish'" :timer="Setting.TimeFinish" @onStartAgain="StartAgain" />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import MatchScreen from "./components/MatchScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { SortRamdomArray } from './utils/SortRandom.js'
export default {
  name: "App",
  components: {
    MainScreen,
    MatchScreen,
    ResultScreen,
  },
  data() {
    return {
      Setting:
      {
        totalcell: 0,
        ArrayContext: [],
        TimeStart: null,
        TimeFinish: null,
      },
      StatusPlay: "None",
    };
  },
  methods: {
    MatchOptions(config) {
      this.StatusPlay = "Play";
      this.Setting.TimeStart = new Date().getTime();
      const FirstArray = Array.from({ length: config.totalcell / 2 }, (_, i) => i + 1);
      const SecondArray = [...FirstArray];
      this.Setting.ArrayContext = [...FirstArray, ...SecondArray];

      this.Setting.ArrayContext = SortRamdomArray(SortRamdomArray(SortRamdomArray(this.Setting.ArrayContext)))
    },
    SwitchResult() {
      this.StatusPlay = "Finish";
      this.Setting.TimeFinish = new Date().getTime() - this.Setting.TimeStart;
    },
    StartAgain() {
      this.StatusPlay = "None";
    }
  },
};
</script>
