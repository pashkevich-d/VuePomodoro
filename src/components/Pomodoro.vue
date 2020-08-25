<template>
  <v-card class="mt-8">
    <v-tabs @change="changeTimerTab" v-model="currentTimer" grow>
      <v-tab v-for="tab in tabsTitle" :key="tab.name">{{ tab.name }}</v-tab>
    </v-tabs>
    <v-card class="pa-5 d-flex flex-column align-center" color="basil" flat>
      <h1 class="time">{{ displayMinutes }}:{{ displaySeconds }}</h1>
      <div class="button-group">
        <v-btn @click="start" color="primary">
          <v-icon left small>mdi-play-circle-outline</v-icon>Start
        </v-btn>
        <v-btn @click="stop" color="error">
          <v-icon left small>mdi-stop-circle-outline</v-icon>Stop
        </v-btn>
        <v-btn @click="reset(tabsTitle[currentTimer].minutes)" :disabled="isRunning">
          <v-icon left small>mdi-restart</v-icon>Reset
        </v-btn>
      </div>
    </v-card>
    <SettingsDialog
      :dialog="dialog"
      :closeDialog="closeDialog"
      :save="save"
      :tabsTitle="tabsTitle"
    />
    <v-btn @click="dialog=true" color="secondary" dark small absolute bottom right fab>
      <v-icon>mdi-cog-outline</v-icon>
    </v-btn>
  </v-card>
</template>

<script>
import SettingsDialog from "./SettingsDialog";
export default {
  components: { SettingsDialog },
  data() {
    return {
      isRunning: false,
      currentTimer: 0,
      timerInstance: null,
      totalSeconds: 1500,
      tabsTitle: [
        { name: "Pomodoro", minutes: 25 },
        { name: "Short Break", minutes: 5 },
        { name: "Long Break", minutes: 10 },
      ],
      dialog: false,
    };
  },
  computed: {
    displayMinutes() {
      const minutes = Math.floor(this.totalSeconds / 60);
      return this.formatTime(minutes);
    },
    displaySeconds() {
      let seconds = this.totalSeconds % 60;
      return this.formatTime(seconds);
    },
  },
  methods: {
    formatTime(time) {
      if (time < 10) {
        return "0" + time;
      }
      return time.toString();
    },
    start() {
      this.stop();
      this.isRunning = true;
      this.timerInstance = setInterval(() => {
        this.totalSeconds -= 1;
        if (this.totalSeconds <= 0) {
          this.stop();
          return;
        }
      }, 1000);
    },
    stop() {
      this.isRunning = false;
      clearInterval(this.timerInstance);
    },
    reset(minutes) {
      this.stop();
      this.totalSeconds = minutes * 60;
    },
    changeTimerTab(num) {
      console.log(num);
      this.currentTimer = num;
      this.reset(this.tabsTitle[num].minutes);
    },
    closeDialog() {
      this.dialog = false;
    },
    save(updatedTimers) {
      this.tabsTitle = this.tabsTitle.map((timer, i) => {
        return { ...timer, minutes: parseInt(updatedTimers[i]) };
      });
      this.closeDialog();
    },
  },
};
</script>

<style lang="sass" scoped>
.v-card
  width: 600px
.time
  font-size: 80px
  font-weight: normal
  text-align: center
.v-btn
  margin: 0 3px
</style>
