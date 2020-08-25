<template>
  <v-dialog persistent v-model="dialog" max-width="600px">
    <v-card>
      <v-card-title>
        <span class="headline">Settings</span>
      </v-card-title>
      <v-card-text>
        <v-text-field
          type="number"
          v-for="(timer,i) in updatedTimers"
          :key="i"
          v-model="updatedTimers[i]"
          label="Pomodoro"
        ></v-text-field>
      </v-card-text>

      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="closeDialog">Close</v-btn>
        <v-btn color="blue darken-1" text @click="save(updatedTimers)">Save</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    dialog: {
      type: Boolean,
      required: true,
    },
    closeDialog: {
      type: Function,
      required: true,
    },
    save: {
      type: Function,
      required: true,
    },
    tabsTitle: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      updatedTimers: [],
    };
  },
  mounted() {
    this.updatedTimers = this.tabsTitle.map((timer) => {
      return timer.minutes;
    });
  },
};
</script>

<style>
</style>template>