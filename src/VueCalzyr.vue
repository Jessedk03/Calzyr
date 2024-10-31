<template>
  <div :class="['calzyr-container', darkMode ? 'dark' : 'light']">

    <!--    Calendar -->
    <div :class="['calzyr-title', darkMode ? 'dark' : 'light']">

      <!--    Showing the options button if the showOptions attribute is being added to the VueCal tag-->
      <button v-if="showOption" class="calzyr-button calzyr-settings" @click="toggleOptions">
        <Gear :color="darkMode ? '#cccccc' : '#4d4d4d'"/>
      </button>

      <button class="previous-month" @click="previousMonth">
        <ArrowLeft :color="darkMode ? '#cccccc' : '#4d4d4d'"/>
      </button>
      <p class="calzyr-title-date"> {{ currentMonth }} {{ currentYear }} </p>
      <button class="next-month" @click="nextMonth">
        <ArrowRight :color="darkMode ? '#cccccc' : '#4d4d4d'"/>
      </button>
    </div>

    <!--    Calendar content-->
    <div class="calzyr-content">
      <!--      Showing the days of the week if the showDaysOfWeek attribute is added to the VueCal tag -->
      <div v-if="showDayOfWeek" v-for="day in weekDays" :key="day" class="calzyr-weekday">
        {{ day }}
      </div>

      <div v-for="(day, index) in daysToDisplay" :key="index" class="calzyr-day">
        <span :class="{ today: highlightToday && isToday(day)}">
          {{ day !== null ? day : '' }}
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import ArrowRight from "@/assets/ArrowRight.vue";
import ArrowLeft from "@/assets/ArrowLeft.vue";
import Gear from "@/assets/Gear.vue";

const date = new Date();

export default {
  name: "Calzyr",
  components: {
    ArrowRight,
    ArrowLeft,
    Gear
  },
  props: {
    // Sets the darkMode to true by adding that attribute to the VueCal tag
    darkMode: {type: Boolean, default: false},
    // Show options
    showOption: {type: Boolean, default: false},
    // Shows week numbers (week 30, 31...)
    showWeekNumber: {type: Boolean, default: false},
    // Show day of week (M, T...)
    showDayOfWeek: {type: Boolean, default: false},
    // Highlights today
    highlightToday: {type: Boolean, default: false}
  },
  data() {
    const months = [
      "January", "February", "March", "April", "May", "June", "July", "August",
      "September", "October", "November", "December"
    ];
    return {
      currentMonth: months[date.getMonth()],
      currentMonthIndex: date.getMonth() + 1,
      currentYear: date.getFullYear(),
      showOptionsPopup: false,
      todayDate: date.getDate(),
      todayMonth: date.getMonth() + 1,
      todayYear: date.getFullYear(),
    };
  },
  computed: {
    daysInMonth() {
      return Array.from(
          {length: new Date(this.currentYear, this.currentMonthIndex, 0).getDate()},
          (_, i) => i + 1
      );
    },
    weekDays() {
      return ["M", "T", "W", "T", "F", "S", "S"];
    },
    calendarWeeks() {
      const weeks = [];
      let weekNumber = this.getWeekNumber(new Date(this.currentYear, this.currentMonthIndex - 1, 1));
      let week = {weekNumber, days: Array(this.firstDayOfMonth).fill(null)};

      this.daysInMonth.forEach((day, index) => {
        if (week.days.length === 7) {
          weeks.push(week);
          weekNumber++;
          week = {weekNumber, days: []};
        }
        week.days.push(day);
      });

      if (week.days.length) {
        while (week.days.length < 7) {
          week.days.push(null)
        }
        weeks.push(week);
      }
      return weeks;
    },
    firstDayOfMonth() {
      const day = new Date(this.currentYear, this.currentMonthIndex - 1, 1).getDay();
      return day === 0 ? 6 : day - 1;
    },
    daysToDisplay() {
      const emptyDays = Array(this.firstDayOfMonth).fill(null);
      return [...emptyDays, ...this.daysInMonth];
    }
  },
  methods: {
    toggleOptions() {
    //   TODO: Make an options popup.
    },
    isToday(day) {
      return (
          day === this.todayDate &&
          this.currentMonthIndex === this.todayMonth &&
          this.currentYear === this.todayYear
      );
    },
    getWeekNumber(date) {
      const firstDayOfYear = new Date(date.getFullYear(), 0, 1);
      const days = Math.floor((date - firstDayOfYear) / (24 * 60 * 60 * 1000));
      return Math.ceil((days + firstDayOfYear.getDay() + 1) / 7);
    },
    previousMonth() {
      if (this.currentMonthIndex === 1) {
        this.currentMonthIndex = 12;
        this.currentYear--;
      } else {
        this.currentMonthIndex--;
      }
      this.updateCurrentMonth();
    },
    nextMonth() {
      if (this.currentMonthIndex === 12) {
        this.currentMonthIndex = 1;
        this.currentYear++;
      } else {
        this.currentMonthIndex++;
      }
      this.updateCurrentMonth();
    },
    updateCurrentMonth() {
      const months = [
        "January", "February", "March", "April", "May", "June", "July", "August",
        "September", "October", "November", "December"
      ];
      this.currentMonth = months[this.currentMonthIndex - 1];
    }
  }
}
</script>

<!-- Calendar style -->
<style src="./style.css"></style>