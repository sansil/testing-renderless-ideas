<template>
  <div class="relative inline-block w-full max-w-xs mx-auto">
    <input
      v-model="formatedDate"
      type="text"
      class="relative mt-12 ml-10 h-8 max-w-xl"
      @click.prevent="showDatePicker=!showDatePicker"
      v-click-outside="{
        exclude: ['tinycalendar'],
        handler: 'away'
      }"
    />

    <transition
      enter-active-class="transition duration-200 ease-out transform"
      enter-class="scale-95 opacity-0"
      enter-to-class="scale-100 opacity-100"
      leave-active-class="transition duration-300 ease-in transform"
      leave-class="scale-100 opacity-100"
      leave-to-class="scale-95 opacity-0"
    >
      <t-calendar
        :weekdays="['S','M','T','W','T','F','S']"
        locale="es-UY"
        v-model="date"
        @input="formatDate"
        :mode="'month'"
        class="origin-top-right absolute right-0 z-10 rounded-md w-full"
        v-slot="{ days,selectedMonthName, selectedYear,isLTminDate,isActive,isSelected,mode,isDisabled,isToday }"
        v-show="showDatePicker"
      >
        <div class="container px-4 py-2 mx-auto">
          <div class="overflow-hidden bg-white rounded-lg shadow-lg max-w-xs">
            <!-- nav content -->
            <nav class="flex items-center justify-between px-6 py-6">
              <div>
                <span class="text-lg font-bold text-gray-800">{{selectedMonthName}}</span>
                <span class="ml-1 text-lg font-normal text-gray-600">{{selectedYear}}</span>
              </div>
              <div class="flex items-center space-x-5">
                <span class="relative z-0 inline-flex">
                  <t-c-button
                    :type="'prev'"
                    :mode="mode"
                    :class="[isDisabled()?'text-gray-300':'text-gray-500']"
                    :disabled="isDisabled()"
                    class="relative inline-flex items-center px-2 py-2 text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue"
                  >
                    <svg class="w-6 h-6" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </t-c-button>
                  <t-c-button
                    :type="'next'"
                    :mode="mode"
                    class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white text-gray-500 hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue"
                  >
                    <svg class="w-6 h-6" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </t-c-button>
                </span>
              </div>
            </nav>
            <!-- days header -->
            <weekdays-name v-slot="{wd}" class="grid grid-cols-7 py-2">
              <span
                class="flex items-center justify-center my-auto text-xs leading-5 uppercase text-gray-500 font-bold"
              >{{wd}}</span>
            </weekdays-name>
            <!-- day cell -->
            <div class="grid grid-cols-7">
              <div
                v-for="(day,index) in days"
                :key="index+'5'"
                class="relative px-2 py-1"
                :class="[isActive(day.date)?'':'bg-gray-100']"
              >
                <t-c-day
                  :day="day.date"
                  class="px-2 inline-flex items-center justify-center h-8 rounded-full w-8 text-sm leading-none text-center transition duration-300 ease-in-out cursor-pointer"
                  :class="{'bg-red-400 text-white ':isSelected(day.date),'text-gray-500':!isActive(day.date)&&!isSelected(day.date)|| isLTminDate(day.date),'text-blue-500 font-semibold': isToday(day.date)&&!isSelected(day.date)}"
                >{{day.date.getDate()}}</t-c-day>
              </div>
            </div>
          </div>
        </div>
      </t-calendar>
    </transition>
  </div>
</template>

<script>
import TCalendar from "./TCalendar";
import TCButton from "./TCalendarButton";
import WeekdaysName from "./TCalendarWeekdays";
import TCDay from "./TCalendarDay";
// This variable will hold the reference to
// document's click handler
//let handleOutsideClick;
let handleOutsideClick;
export default {
  directives: {
    "click-outside": {
      bind(el, binding, vnode) {
        // Here's the click/touchstart handler
        // (it is registered below)
        handleOutsideClick = (e) => {
          e.stopPropagation();
          // Get the handler method name and the exclude array
          // from the object used in v-closable
          const { handler, exclude } = binding.value;

          // This variable indicates if the clicked element is excluded
          let clickedOnExcludedEl = false;
          exclude.forEach((refName) => {
            // We only run this code if we haven't detected
            // any excluded element yet
            if (!clickedOnExcludedEl) {
              // Get the element using the reference name
              const excludedEl = vnode.context.$refs[refName];
              let excludedDomEl = null;
              if (excludedEl) {
                // If it's a vue component grab the element, otherwise it is the element
                excludedDomEl = excludedEl.$el ? excludedEl.$el : excludedEl;
                clickedOnExcludedEl = excludedDomEl.contains(e.target);
              }
            }
          });

          // We check to see if the clicked element is not
          // the dialog element and not excluded
          if (!el.contains(e.target) && !clickedOnExcludedEl) {
            // If the clicked element is outside the dialog
            // and not the button, then call the outside-click handler
            // from the same component this directive is used in
            vnode.context[handler]();
          }
        };
        // Register click/touchstart event listeners on the whole page
        document.addEventListener("click", handleOutsideClick);
        document.addEventListener("touchstart", handleOutsideClick);
      },
      unbind() {
        // If the element that has v-closable is removed, then
        // unbind click/touchstart listeners from the whole page
        document.removeEventListener("click", handleOutsideClick);
        document.removeEventListener("touchstart", handleOutsideClick);
      },
    },
  },
  props: ["value"],
  components: {
    TCalendar,
    TCButton,
    WeekdaysName,
    TCDay,
  },
  data() {
    return {
      date: "",
      formatedDate: "",
      showDatePicker: false,
    };
  },
  methods: {
    closePicker() {
      this.showDatePicker = false;
    },
    away() {
      this.showDatePicker = false;
    },
    onfocus() {
      console.log("focus");
    },
    onblur() {
      console.log("blur");
      this.showDatePicker = false;
    },
    onChange() {
      console.log("change");
      this.$emit("input", this.date);
    },
    formatDate() {
      this.showDatePicker = false;
      var options = { year: "numeric", month: "numeric", day: "numeric" };
      this.formatedDate = this.date.toLocaleString(["es-UY"], options);
      this.$emit("input", this.date);
    },
  },
};
</script>

<style lang="scss" scoped>
</style>