<template>
  <div id="app" class="bg-red-500 antialiased">
    <!-- <t-calendar :level="1" id="sansi">Hola mundo!</t-calendar> -->
    <div class="w-full flex justify-center">
      <t-c-date-picker v-model="datePick"></t-c-date-picker>
    </div>
    {{datePick}}
    <date-picker
      v-slot="{showDatePicker, formatedDate,formatDate}"
      class="relative inline-block w-full max-w-xs mx-auto"
    >
      <t-dinput class="relative mt-12 ml-10 h-8 max-w-xl" :formatedDate="formatedDate"></t-dinput>
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
          :mode="'month'"
          @input="formatDate"
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
    </date-picker>
    <div>sansilll</div>
    <t-calendar
      :weekdays="['S','M','T','W','T','F','S']"
      locale="es-UY"
      v-model="date"
      :mode="mode"
      v-slot="{ days,selectedMonthName, selectedYear,isLTminDate,isActive,isSelected,mode,isDisabled }"
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
                :class="{'bg-red-400 text-white ':isSelected(day.date),'text-gray-500':!isActive(day.date)&&!isSelected(day.date)|| isLTminDate(day.date)}"
              >{{day.date.getDate()}}</t-c-day>
            </div>
          </div>
        </div>
      </div>
    </t-calendar>

    <tiny-calendar
      :date="new Date()"
      :mode="mode"
      :weekdays="['S','M','T','W','T','F','S']"
      locale="es-UY"
    >
      <div
        slot-scope="{ days,prevMonth,nextMonth,nextWeek,prevWeek,selectedMonthName,isActiveMonth,daysOfMonth,selectedYear,daysOfWeek,isLTminDate,generatedWeekdayNames,isSelected,selectDate}"
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
                <!-- <span class="relative z-0 inline-flex rounded-md shadow-sm">
                  <button
                    type="button"
                    @click="mode='week'"
                    class="relative inline-flex items-center px-4 py-2 text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-700 border-gray-300 rounded-l-md hover:text-gray-500 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-700"
                  >Semana</button>
                  <button
                    type="button"
                    @click="mode='month'"
                    class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-700 border-gray-300 rounded-r-md hover:text-gray-500 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-700"
                  >Mes</button>
                </span>-->

                <span class="relative z-0 inline-flex shadow-sm">
                  <button
                    type="button"
                    @click="mode=='week'?prevWeek():prevMonth()"
                    class="relative inline-flex items-center px-2 py-2 text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border border-gray-300 rounded-l-md hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500"
                    :class="[isLTminDate?'text-gray-300':'text-gray-500']"
                    :disabled="isLTminDate"
                    aria-label="Previous"
                  >
                    <svg class="w-4 h-4" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                  <button
                    type="button"
                    @click="mode=='week'?nextWeek():nextMonth()"
                    class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-500 border-gray-300 rounded-r-md hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500"
                    aria-label="Next"
                  >
                    <svg class="w-4 h-4" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </span>
              </div>
            </nav>

            <!-- days header -->
            <div class="grid grid-cols-7 py-2 bg-primary-600">
              <div
                v-for="(w,index) in generatedWeekdayNames"
                :key="index"
                class="flex items-center justify-center"
              >
                <span class="text-sm leading-5 uppercase text-gray-500 font-bold">{{w}}</span>
              </div>
            </div>

            <!-- days cell -->
            <div class="grid grid-cols-7">
              <div
                v-for="(day,index) in days"
                :key="index+'5'"
                class="relative px-2 py-1"
                :class="[isActiveMonth(day.date)?'':'bg-gray-100',{'h-64':mode==='week'}]"
              >
                <div class>
                  <div
                    @click="selectDate(day.date)"
                    class="inline-flex items-center justify-center w-8 h-8 text-sm leading-none text-center transition duration-100 ease-in-out rounded-full cursor-pointer"
                    :class="{'bg-red-400 text-white':isSelected(day.date),'text-gray-500':!isActiveMonth(day.date)&&!isSelected(day.date)}"
                  >{{day.date.getDate()}}</div>
                  <!-- <div class="flex-grow mt-1 overflow-x-auto overflow-y-auto">
                    <router-link
                      v-for="(e,index) in day.events"
                      :key="index"
                      class="block p-1 mt-0 mb-1 text-xs leading-tight rounded-sm"
                      :class="e.customData.class"
                      :to="'/dashboard/evento/'+ e.customData._id"
                    >{{ e.customData.title }}</router-link>
                  </div>-->
                  <!-- <div>{{day.events}}</div> -->
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </tiny-calendar>

    <tiny-calendar
      :minDate="new Date()"
      :date="new Date()"
      :mode="mode"
      :weekdays="['Dom.','Lun.','Mar.','Mie.','Jue.','Vie.','Sab.']"
      locale="es-UY"
    >
      <div
        slot-scope="{ days,prevMonth,nextMonth,nextWeek,prevWeek,selectedMonthName,isActiveMonth,daysOfMonth,selectedYear,daysOfWeek,isLTminDate,generatedWeekdayNames,isToday}"
      >
        <div class="container px-4 py-2 mx-auto">
          <div class="overflow-hidden bg-white rounded-lg shadow">
            <!-- nav content -->
            <nav class="flex items-center justify-between px-6 py-6">
              <div>
                <span class="text-2xl font-bold text-gray-800">{{selectedMonthName}}</span>
                <span class="ml-1 text-2xl font-normal text-gray-600">{{selectedYear}}</span>
              </div>
              <div class="flex items-center space-x-5">
                <span class="relative z-0 inline-flex rounded-md shadow-sm">
                  <button
                    type="button"
                    @click="mode='week'"
                    class="relative inline-flex items-center px-4 py-2 text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-700 border-gray-300 rounded-l-md hover:text-gray-500 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-700"
                  >Semana</button>
                  <button
                    type="button"
                    @click="mode='month'"
                    class="relative inline-flex items-center px-4 py-2 -ml-px text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-700 border-gray-300 rounded-r-md hover:text-gray-500 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-700"
                  >Mes</button>
                </span>

                <span class="relative z-0 inline-flex shadow-sm">
                  <button
                    :disabled="isLTminDate"
                    type="button"
                    @click="mode=='week'?prevWeek():prevMonth()"
                    class="relative inline-flex items-center px-2 py-2 text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border border-gray-300 rounded-l-md hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500"
                    :class="[isLTminDate?'text-gray-300':'text-gray-500']"
                    aria-label="Previous"
                  >
                    <svg class="w-5 h-5" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M12.707 5.293a1 1 0 010 1.414L9.414 10l3.293 3.293a1 1 0 01-1.414 1.414l-4-4a1 1 0 010-1.414l4-4a1 1 0 011.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                  <button
                    type="button"
                    @click="mode=='week'?nextWeek():nextMonth()"
                    class="relative inline-flex items-center px-2 py-2 -ml-px text-sm font-medium leading-5 transition duration-150 ease-in-out bg-white border text-gray-500 border-gray-300 rounded-r-md hover:text-gray-400 focus:z-10 focus:outline-none focus:border-blue-300 focus:shadow-outline-blue active:bg-gray-100 active:text-gray-500"
                    aria-label="Next"
                  >
                    <svg class="w-5 h-5" viewBox="0 0 20 20" fill="currentColor">
                      <path
                        fill-rule="evenodd"
                        d="M7.293 14.707a1 1 0 010-1.414L10.586 10 7.293 6.707a1 1 0 011.414-1.414l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414 0z"
                        clip-rule="evenodd"
                      />
                    </svg>
                  </button>
                </span>
              </div>
            </nav>

            <!-- days header -->
            <div class="grid grid-cols-7 py-2 px-2 bg-primary-600">
              <div
                v-for="w in generatedWeekdayNames"
                :key="w"
                class="flex items-center justify-center"
              >
                <span class="my-auto text-xs leading-5 uppercase text-secondary-200">{{w}}</span>
              </div>
            </div>

            <!-- days cell -->
            <div class="grid grid-cols-7 border-t border-l">
              <div
                v-for="(day,index) in days"
                :key="index+'3'"
                class="relative px-2 pt-2 border-b border-r border-gray-300 h-24"
                :class="[isActiveMonth(day.date)?'':'bg-gray-100',{'h-64':mode==='week'}]"
              >
                <div class="z-10 flex flex-col w-full h-full overflow-hidden">
                  <div
                    class="inline-flex items-center justify-center w-6 h-6 text-sm leading-none text-center transition duration-100 ease-in-out rounded-full"
                    :class="{'bg-red-400 text-white':isToday(day.date),'text-gray-400':!isActiveMonth(day.date)&&!isToday(day.date)}"
                  >{{day.date.getDate()}}</div>
                  <div class="flex-grow mt-1 overflow-x-auto overflow-y-auto">
                    <router-link
                      v-for="(e,index) in day.events"
                      :key="index"
                      class="block p-1 mt-0 mb-1 text-xs leading-tight rounded-sm"
                      :class="e.customData.class"
                      :to="'/dashboard/evento/'+ e.customData._id"
                    >{{ e.customData.title }}</router-link>
                  </div>
                  <!-- <div>{{day.events}}</div> -->
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </tiny-calendar>
  </div>
</template>

<script>
import TinyCalendar from "./components/TinyCalendar.vue";
import TCalendar from "./components/TCalendar";
import TCButton from "./components/TCalendarButton";
import WeekdaysName from "./components/TCalendarWeekdays";
import TCDay from "./components/TCalendarDay";
import TCDatePicker from "./components/TCDatePicker";
import DatePicker from "./components/DatePicker";
import TDinput from "./components/TDinput";
export default {
  name: "App",
  components: {
    TinyCalendar,
    TCalendar,
    TCButton,
    WeekdaysName,
    TCDay,
    TCDatePicker,
    DatePicker,
    TDinput,
  },
  data() {
    return {
      mode: "month",
      date: new Date(),
      datePick: "",
      vcinput: "",
    };
  },
  methods: {
    onInput(e) {
      console.log(e);
    },
    test() {
      console.log("test", this.$refs["tinycalendar"]);
    },
  },
  mounted() {
    console.log(this.$refs.tinycalendar);
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>
