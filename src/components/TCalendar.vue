

<script>
//const CalendarSymbol = Symbol("Calendar");
//import vClickOutside from "v-click-outside";
//let handleOutsideClick;
let id = 0;
function generateId() {
  return `tiny-calendar-id-${++id}`;
}
export default {
  name: "Tiny-Calendar-v2",
  provide() {
    return {
      ["context"]: {
        nextMonth: this.nextMonth,
        prevMonth: this.prevMonth,
        nextWeek: this.nextWeek,
        prevWeek: this.prevWeek,
        generatedWeekdayNames: this.generatedWeekdayNames,
        days: this.days,
        emitValue: this.emitValue,
        isLTminDate: this.isLTminDate,
      },
    };
  },
  render: function (createElement) {
    return createElement(
      "div", // nombre de etiqueta
      {
        attrs: {
          id: this.id,
          // tabindex: "1",
          // tabindex: 0,
        },
        on: {
          // blur: this.onBlurCalendar,
        },
        ref: "tinycalendar",
      },
      [
        this.$scopedSlots.default({
          days: this.days,
          prevMonth: this.prevMonth,
          nextMonth: this.nextMonth,
          nextWeek: this.nextWeek,
          prevWeek: this.prevWeek,
          selectedMonthName: this.selectedMonthName,
          // daysOfWeek: this.daysOfWeek,
          isActive: this.isActive,
          daysOfMonth: this.daysOfMonth,
          selectedYear: this.selectedYear,
          isLTminDate: this.isLTminDate,
          generatedWeekdayNames: this.generatedWeekdayNames,
          beginningOfWeek: this.beginningOfSelectedWeek,
          endOfWeek: this.endOfSelectedWeek,
          isToday: this.isToday,
          isSelected: this.isSelected,
          selectDate: this.selectDate,
          mode: this.mode,
          isDisabled: this.isDisabled,
        }), // arreglo de hijos
      ]
    );
  },
  data: () => ({
    id: generateId(),
    date: new Date(),
  }),
  mounted() {
    if (this.value instanceof Date) this.date = this.value;
  },
  computed: {
    days() {
      if (this.mode === "week") return this.daysOfWeek;
      else return this.daysOfMonth;
    },
    generatedWeekdayNames() {
      let generatedWeekdays = [...this.weekdays];
      for (let i = 0; i < this.firstDayOfWeek; i++) {
        let first = generatedWeekdays.shift();
        generatedWeekdays.push(first);
      }
      return generatedWeekdays;
    },
    daysOfMonth() {
      const firstDayOfMonth = new Date(
        this.date.getFullYear(),
        this.date.getMonth(),
        1
      );
      return Array(7 * 6)
        .fill()
        .map((_, i) =>
          this.addDaysAndEvents(this.beginningOfWeek(firstDayOfMonth), i)
        );
    },
    selectedMonth() {
      return this.date.getMonth();
    },
    selectedYear() {
      return this.date.getFullYear();
    },
    selectedDay() {
      return this.date.getDay();
    },
    selectedMonthName() {
      return this.date.toLocaleString(this.locale, { month: "long" });
    },
    daysOfWeek() {
      return Array(7)
        .fill()
        .map((_, i) =>
          this.addDaysAndEvents(this.beginningOfWeek(this.date), i)
        );
    },
    endOfSelectedWeek() {
      return this.addDays(this.beginningOfWeek(this.date), 7);
    },
    beginningOfSelectedWeek() {
      return this.beginningOfWeek(this.date);
    },
    firstDateOfSelectedMonth() {
      return new Date(
        this.date.getFullYear(),
        this.date.getMonth() - 1,
        1,
        0,
        0,
        0
      );
    },
  },
  methods: {
    handelClickOutside() {
      console.log("event.target.id");
    },
    emitValue(e) {
      this.$emit("input", e);
    },
    beginningOfWeek(d) {
      return this.addDays(d, (this.firstDayOfWeek - d.getDay() - 7) % -7);
    },
    addDays(d, days) {
      let date = new Date(
        d.getFullYear(),
        d.getMonth(),
        d.getDate() + days,
        0,
        0,
        0
      );
      return date;
    },
    addDaysAndEvents(d, days) {
      let date = this.addDays(d, days);
      let dayEvents = [];
      this.events.forEach((e) => {
        let dayEvent = new Date(
          e.date.getFullYear(),
          e.date.getMonth(),
          e.date.getDate(),
          0,
          0,
          0
        );
        if (dayEvent.getTime() === date.getTime()) {
          dayEvents.push(e);
        }
      });
      return { date: date, events: dayEvents };
    },
    prevMonth() {
      this.date = new Date(
        this.date.getFullYear(),
        this.date.getMonth() - 1,
        1
      );
    },
    nextMonth() {
      this.date = new Date(
        this.date.getFullYear(),
        this.date.getMonth() + 1,
        1
      );
    },
    nextWeek() {
      this.date = this.endOfSelectedWeek;
    },
    prevWeek() {
      this.date = this.addDays(this.date, -7);
    },
    //isInActiveMonth
    isActive(d) {
      return d.getMonth() === this.date.getMonth();
    },
    isToday(d) {
      let date = new Date();
      return (
        d.getDate() === date.getDate() &&
        d.getMonth() === date.getMonth() &&
        d.getFullYear() === date.getFullYear()
      );
    },
    isLTminDate(d) {
      let midNight = new Date(
        d.getFullYear(),
        d.getMonth(),
        d.getDate() + 1,
        0,
        0,
        0
      );
      return midNight.getTime() < this.minDate.getTime();
    },
    isDisabled() {
      // let bOw = this.beginningOfSelectedWeek;
      // return this.beginningOfSelectedWeek < this.minDate;
      if (this.mode === "month") {
        let firstDay = new Date(
          this.date.getFullYear(),
          this.date.getMonth(),
          1,
          0,
          0,
          0
        );
        return firstDay < this.minDate;
      } else {
        return this.beginningOfSelectedWeek < this.minDate;
      }
    },
    goToday() {
      this.date = this.today;
    },
    isSelected(d) {
      if (this.value instanceof Date)
        return (
          d.getDate() === this.value.getDate() &&
          d.getMonth() === this.value.getMonth() &&
          d.getFullYear() === this.value.getFullYear()
        );
      else return false;
    },
  },
  props: {
    value: {
      type: [String, Date, Object],
    },
    weekdays: {
      type: Array,
      default() {
        return [
          "Sunday",
          "Monday",
          "Tuesday",
          "Wednesday",
          "Thursday",
          "Friday",
          "Saturday",
        ];
      },
    },
    firstDayOfWeek: {
      type: Number,
      default: 1, // 1: Sunday, 2: Monday, etc
    },
    events: {
      type: Array,
      default() {
        return [];
      },
    },
    mode: {
      type: String,
      default: "month",
    },
    minDate: {
      type: Date,
      default: () => {
        return new Date(-8640000000000000);
      },
    },
    multiple: {
      type: Boolean,
      default: false,
    },
    locale: {
      type: String,
      default: "en-US",
    },
  },
};
</script>

<style lang="scss" scoped>
</style>