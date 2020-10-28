<template>
  <b-container class="pomodoro">
    <h3 class="text-center">POMODORO</h3>
    <b-row class="mt-4">
      <b-col sm="8" md="4" offset-sm="2" offset-md="4">
        <b-alert
          v-if="isBreakTime"
          :show="breakAlert"
          variant="success"
          class="text-center"
        >
          Mulai istirahat ?
        </b-alert>

        <b-alert
          v-if="!isBreakTime && breakTimesNumber"
          :show="focusAlert"
          variant="primary"
          class="text-center"
        >
          siap untuk kembali fokus ?
        </b-alert>
      </b-col>

      <b-col sm="8" md="4" offset-sm="2" offset-md="4">
        <b-card no-body>
          <b-tabs pills fill card>
            <b-tab title="Start" active>
              <b-card-text>
                <h2 class="m-0 text-center my-4" id="displayTimer">
                  {{ displayMenit }}:{{ displayDetik }}
                </h2>
                <b-row no-gutters class="pomodoro-button">
                  <b-col class="pr-1">
                    <b-btn
                      variant="success"
                      class="btn-block"
                      size="sm"
                      v-if="!isRunning"
                      @click="start"
                      title="play"
                    >
                      <span class="material-icons">
                        play_arrow
                      </span>
                    </b-btn>
                    <b-btn
                      variant="danger"
                      class="btn-block"
                      size="sm"
                      v-if="isRunning"
                      @click="pause"
                      title="pause"
                    >
                      <span class="material-icons">
                        stop_circle
                      </span>
                    </b-btn>
                  </b-col>
                  <b-col class="pl-1">
                    <b-btn
                      variant="primary"
                      class="btn-block"
                      size="sm"
                      title="reset"
                      @click="reset"
                    >
                      <span class="material-icons">
                        sync
                      </span>
                    </b-btn>
                  </b-col>
                </b-row>
              </b-card-text>
            </b-tab>

            <!-- settings tab -->
            <b-tab title="Settings">
              <b-card-text>
                <b-form-group label="Waktu" label-for="waktu-pomodoro">
                  <b-form-input
                    id="waktu-pomodoro"
                    v-model="waktuPomodoro"
                    type="number"
                    required
                    min="1"
                  ></b-form-input>
                  <small>satuan menit</small>
                </b-form-group>

                <b-form-group label="Short break" label-for="short-break">
                  <b-form-input
                    id="short-break"
                    v-model="shortBreak"
                    type="number"
                    required
                    min="1"
                  ></b-form-input>
                  <small>satuan menit</small>
                </b-form-group>

                <b-form-group label="Long break" label-for="long-break">
                  <b-form-input
                    id="long-break"
                    v-model="LongBreak"
                    type="number"
                    required
                    min="1"
                  ></b-form-input>
                  <small>satuan menit</small>
                </b-form-group>
              </b-card-text>
            </b-tab>
          </b-tabs>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  data() {
    return {
      waktuPomodoro: 25,
      totalDetik: 25 * 60,
      shortBreak: 5,
      LongBreak: 30,
      isRunning: false,
      handle: null,
      breakTimesNumber: 0,
      isBreakTime: false,
      breakAlert: false,
      focusAlert: false,
    };
  },
  methods: {
    pause() {
      clearInterval(this.handle);
      this.isRunning = false;
    },
    reset() {
      clearInterval(this.handle);
      this.breakTimesNumber = 0;
      this.isBreakTime = false;
      this.focusAlert = false;
      this.breakAlert = false;
      this.isRunning = false;
      this.totalDetik = this.waktuPomodoro * 60;
    },
    start() {
      if (this.isRunning) return;

      this.focusAlert = false;
      this.breakAlert = false;
      this.handle = setInterval(() => {
        this.totalDetik--;
      }, 1000);
      this.isRunning = true;
      // console.log("pomodoro di mulai");
    },
  },
  computed: {
    displayMenit() {
      let menit = Math.floor(this.totalDetik / 60);
      if (menit < 10) return "0" + menit;
      return menit;
    },
    displayDetik() {
      let detik = this.totalDetik % 60;
      if (detik < 10) return "0" + detik;
      return detik;
    },
  },
  watch: {
    waktuPomodoro(newVal) {
      this.totalDetik = newVal * 60;
    },
    totalDetik(newVal) {
      // jika totaldetik kurang dari 1
      if (newVal < 1 && this.isRunning) {
        // jika this.isBreakTime bernilai true rubah jadi false dan jalankan pomodoro
        if (this.isBreakTime) {
          this.focusAlert = true;
          this.isBreakTime = false;
          this.totalDetik = this.waktuPomodoro * 60;
          this.pause();
        } else {
          // jika this.isBreakTimes bernilai false jalankan breaktime
          // jika breakTimesNumber bernilai 2 jalankan longBreak
          if (this.breakTimesNumber == 2) {
            this.breakTimesNumber = 0;
            this.totalDetik = this.LongBreak * 60;
          } else {
            this.breakTimesNumber += 1;
            this.totalDetik = this.shortBreak * 60;
          }
          this.breakAlert = true;
          this.isBreakTime = true;
          this.pause();
        }
      }
    },
  },
};
</script>

<style>
h3 {
  opacity: 0.8;
}

label {
  font-size: 15px;
}

#displayTimer {
  font-size: 70px !important;
  opacity: 0.85;
}

.card-header {
  background-color: #fff !important;
  border-bottom: none !important;
}

.tab-pane {
  height: 280px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
