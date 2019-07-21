<template>
  <div class="time-select">
    <div
      class="time-select__display"
      @click.stop="toggleDropdown"
      type="text"
      readonly
    >
      <transition :name="fadeMode" mode="out-in">
        <span :key="displayHour">{{ displayHour }}</span>
      </transition>
      <span>:</span>
      <transition :name="fadeMode" mode="out-in">
        <span :key="displayMinute">{{ displayMinute }}</span>
      </transition>
    </div>

    <div
      class="time-select__overlay"
      v-if="showDropdown"
      @click.stop="toggleDropdown"
    ></div>

    <transition name="fold">
      <div class="time-select__dropdown" v-if="showDropdown">
        <div class="time-select__select-list">
          <ul class="time-select__hours">
            <li class="time-select__hint">{{ hourType }}</li>
            <li
              v-for="(hr, index) in hours"
              :key="index"
              @click.stop="selectHour(hr);"
            >
              {{ formatTime(hr) }}
            </li>
          </ul>
          <ul class="time-select__minutes">
            <li class="time-select__hint">{{ minuteType }}</li>
            <li
              v-for="(min, index) in minutes"
              :key="index"
              @click.stop="selectMinute(min);"
            >
              {{ formatTime(min) }}
            </li>
          </ul>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
export default {
  name: "TimeSelect",

  props: {
    minHour: {
      type: Number,
      default: 0
    },

    maxHour: {
      type: Number,
      default: 23
    },

    minuteInterval: {
      type: Number,
      default: 15
    },

    hour: {
      type: Number,
      required: true
    },

    minute: {
      type: Number,
      required: true
    },

    // 'fade-up', 'fade-down'
    fadeMode: {
      type: String,
      default: "fade-up"
    }
  },

  data: () => ({
    showDropdown: false,
    hourType: "HH",
    minuteType: "mm",
    hideClearButton: false
  }),

  computed: {
    displayHour() {
      return this.formatTime(this.hour);
    },

    displayMinute() {
      return this.formatTime(this.minute);
    },

    hours() {
      const hours = [];
      for (let i = this.minHour; i <= this.maxHour; i++) {
        hours.push(i);
      }
      return hours;
    },

    minutes() {
      const minutes = [];
      for (let i = 0; i <= 59; i += this.minuteInterval) {
        minutes.push(i);
      }
      return minutes;
    }
  },

  methods: {
    toggleDropdown() {
      this.showDropdown = !this.showDropdown;
    },

    selectHour(hour) {
      this.$emit("update:hour", hour);
    },

    selectMinute(minute) {
      this.$emit("update:minute", minute);
    },

    formatTime(time) {
      const numStr = time.toString();
      return numStr.length > 1 ? numStr : numStr.padStart(2, "0");
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.time-select {
  display: flex;
  font-size: 16px;
  flex: 1;
  height: 100%;
  font-family: sans-serif;
  justify-content: center;

  &__display {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 70px;
    height: 100%;
    font-size: 16px;
    text-align: center;
    overflow: hidden;
  }

  &__overlay {
    z-index: 1;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
  }

  &__dropdown {
    position: absolute;
    z-index: 2;
    top: 62px;
    left: 0;
    background: #fff;
    box-shadow: 0 1px 6px rgba(0, 0, 0, 0.15);
    width: 100%;
    max-height: 150px;
    font-weight: normal;

    .time-select__hours,
    .time-select__minutes {
      padding: 25px 0 0 0;
      margin: 0;
      list-style: none;

      flex: 1;
      display: flex;
      flex-direction: column;
      overflow-x: hidden;
      overflow-y: auto;

      li {
        text-align: center;
        padding: 5px 0;
        color: #161616;
      }

      li:not(.time-select__hint):hover {
        background: rgba(0, 0, 0, 0.08);
        color: #161616;
        cursor: pointer;
      }
    }
  }

  &__select-list {
    display: flex;
    width: 100%;
    max-height: 140px;
    overflow: hidden;
  }

  &__minutes {
    border-left: 1px solid #fff;
  }

  &__hint {
    position: absolute;
    top: 0;
    width: 50%;
    color: #a5a5a5;
    cursor: default;
    font-size: 0.8em;
    background-color: #f9f9f9;
  }
}

// FADE UP
.fade-up-enter-active,
.fade-up-leave-active {
  transition: all 200ms cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transition: all 200ms;
}
.fade-up-enter {
  transform: translateY(-20px);
  opacity: 0;
}

.fade-up-enter-to {
  opacity: 1;
}

.fade-up-leave-to {
  transform: translateY(20px);
  opacity: 0;
}

// FADE DOWN

.fade-down-enter-active,
.fade-down-leave-active {
  transition: all 200ms cubic-bezier(0.68, -0.55, 0.265, 1.55);
  transition: all 200ms;
}
.fade-down-enter {
  transform: translateY(20px);
  opacity: 0;
}

.fade-down-enter-to {
  opacity: 1;
}

.fade-down-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}

// FOLD

.fold-enter-active,
.fold-leave-active {
  transition: all 200ms;
  transform-origin: top;
}
.fold-enter {
  transform: rotateX(-90deg);
  opacity: 0;
}

.fold-enter-to {
  opacity: 0.5;
  transform: rotateX(0deg);
}

.fold-leave-to {
  transform: rotateX(-90deg);
  opacity: 0;
}
</style>
