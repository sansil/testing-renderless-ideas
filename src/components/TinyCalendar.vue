
 


<script>
export default {
  name: "TinyCalendar",
  data() {
    return {
      today: new Date(),
      d: new Date(),
    };
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
        this.d.getFullYear(),
        this.d.getMonth(),
        1
      );
      return Array(7 * 6)
        .fill()
        .map((_, i) =>
          this.addDaysAndEvents(this.beginningOfWeek(firstDayOfMonth), i)
        );
    },
    selectedMonth() {
      return this.d.getMonth();
    },
    selectedYear() {
      return this.d.getFullYear();
    },
    selectedDay() {
      return this.d.getDay();
    },
    selectedMonthName() {
      return this.d.toLocaleString(this.locale, { month: "long" });
    },
    daysOfWeek() {
      return Array(7)
        .fill()
        .map((_, i) => this.addDaysAndEvents(this.beginningOfWeek(this.d), i));
    },
    endOfSelectedWeek() {
      return this.addDays(this.beginningOfWeek(this.d), 7);
    },
    beginningOfSelectedWeek() {
      return this.beginningOfWeek(this.d);
    },
    isLTminDate() {
      return this.d <= this.minDate;
    },
  },
  methods: {
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
      this.d = new Date(this.d.getFullYear(), this.d.getMonth() - 1, 1);
    },
    nextMonth() {
      this.d = new Date(this.d.getFullYear(), this.d.getMonth() + 1, 1);
    },
    isActiveMonth(d) {
      return d.getMonth() === this.d.getMonth();
    },
    isToday(d) {
      let date = new Date();
      return (
        d.getDate() === date.getDate() &&
        d.getMonth() === date.getMonth() &&
        d.getFullYear() === date.getFullYear()
      );
    },
    nextWeek() {
      this.d = this.endOfSelectedWeek;
    },
    prevWeek() {
      this.d = this.addDays(this.d, -7);
    },
    goToday() {
      this.d = this.today;
    },
    selectDate(d) {
      this.d = d;
    },
    isSelected(d) {
      return (
        d.getDate() === this.d.getDate() &&
        d.getMonth() === this.d.getMonth() &&
        d.getFullYear() === this.d.getFullYear()
      );
    },
  },
  props: {
    date: {
      type: Date,
      default() {
        return new Date();
      },
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
      default() {
        new Date(-8640000000000000);
      },
    },
    locale: {
      type: String,
      default: "en-US",
    },
  },
  beforeMount() {
    this.d = this.date;
  },

  render() {
    return this.$scopedSlots.default({
      days: this.days,
      prevMonth: this.prevMonth,
      nextMonth: this.nextMonth,
      nextWeek: this.nextWeek,
      prevWeek: this.prevWeek,
      selectedMonthName: this.selectedMonthName,
      daysOfWeek: this.daysOfWeek,
      isActiveMonth: this.isActiveMonth,
      daysOfMonth: this.daysOfMonth,
      selectedYear: this.selectedYear,
      isLTminDate: this.isLTminDate,
      generatedWeekdayNames: this.generatedWeekdayNames,
      beginningOfWeek: this.beginningOfSelectedWeek,
      endOfWeek: this.endOfSelectedWeek,
      isToday: this.isToday,
      isSelected: this.isSelected,
      selectDate: this.selectDate,
    });
  },
};
</script>

<style lang="sass" scoped>
</style>