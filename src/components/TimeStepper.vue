<template>
  <div class="time-stepper">
    <button
      class="time-stepper__button time-stepper__button--minus"
      @click="subtract"
    ></button>

    <TimeSelect
      :min-hour="minHour"
      :max-hour="maxHour"
      :minute-interval="minuteInterval"
      :hour="hour"
      :minute="minute"
      :fade-mode="fadeMode"
      @update:hour="updateHour"
      @update:minute="updateMinute"
    ></TimeSelect>
    <button
      class="time-stepper__button time-stepper__button--plus"
      @click="add"
    ></button>
  </div>
</template>

<script>
import moment from "moment";
import TimeSelect from "./TimeSelect";

// @vue/component
export default {
  components: {
    TimeSelect
  },

  props: {
    minHour: {
      type: Number,
      default: 8
    },

    maxHour: {
      type: Number,
      default: 22
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

    isReadOnly: {
      type: Boolean,
      default: false
    }
  },

  data: () => ({
    fadeMode: "fade-up"
  }),

  methods: {
    updateHour(hour) {
      this.$emit("update:hour", hour);
    },

    updateMinute(minute) {
      this.$emit("update:minute", minute);
    },

    subtract() {
      this.fadeMode = "fade-down";
      if (this.hour === this.minHour) return;
      this.updateHour(this.hour - 1);
    },

    add() {
      this.fadeMode = "fade-up";
      if (this.hour === this.maxHour) return;
      this.updateHour(this.hour + 1);
    }
  }
};
</script>

<style lang="scss" scoped>
$color-pink-basic: #c6007e;
$color-snow-basic: #f2f2f2;

.time-stepper {
  position: relative;
  display: flex;
  align-items: center;
  width: 150px;
  height: 62px;
  font-size: 16px;
  font-weight: 600;
  border: 2px solid #919191;
  border-radius: 3px;

  &__input {
    display: flex;
    align-items: center;
    flex: 1;
    justify-content: center;
    width: 100%;
    height: 100%;
    font-size: 16px;
    font-weight: 600;
    border: none;

    &--hour {
      text-align: right;
    }

    &--minute {
      text-align: left;
    }
  }

  &__button {
    position: relative;
    z-index: 0;
    width: 40px;
    height: 58px;
    cursor: pointer;
    background: none;
    border: none;
    outline: none;
    transition: all 200ms;

    &:hover {
      background-color: $color-pink-basic;
    }

    &:active {
      background-color: lighten($color-pink-basic, 5%);
    }

    &--minus {
      border-radius: 1px 0 0 1px;

      &::after {
        position: absolute;
        top: 50%;
        right: 9px;
        left: 9px;
        z-index: 9;
        display: block;
        height: 2px;
        margin-top: -1px;
        background-color: $color-pink-basic;
        content: " ";
      }

      &:hover::after {
        background-color: #fff;
      }

      &:active::after {
        background-color: #fff;
      }
    }

    &--plus {
      border-radius: 0 1px 1px 0;

      &::after {
        position: absolute;
        top: 50%;
        right: 9px;
        left: 9px;
        z-index: 9;
        display: block;
        height: 2px;
        margin-top: -1px;
        background-color: $color-pink-basic;
        content: " ";
      }

      &:hover::after {
        background-color: #fff;
      }

      &:hover::before {
        background-color: #fff;
      }

      &:active::after {
        background-color: #fff;
      }

      &::before {
        position: absolute;
        top: 19px;
        bottom: 19px;
        left: 50%;
        z-index: 9;
        display: block;
        width: 2px;
        margin-left: -1px;
        background-color: $color-pink-basic;
        content: " ";
      }

      &:active::before {
        background-color: #fff;
      }
    }
  }
}
</style>
