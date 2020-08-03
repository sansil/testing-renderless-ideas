

<script>
export default {
  name: "Tiny-Calendar-buttons",
  inject: ["context"],
  props: {
    type: {
      type: String,
      default() {
        return "next";
      },
    },
    mode: {
      type: String,
      default: "month",
    },
  },
  render: function (createElement) {
    return createElement(
      "button",
      {
        on: {
          click: this.clickHandler,
        },
      },
      this.$scopedSlots.default({}) // arreglo de hijos
    );
  },
  methods: {
    clickHandler() {
      if (this.mode === "month") {
        if (this.type === "prev") {
          this.context.prevMonth();
          return;
        }
        this.context.nextMonth();
      } else {
        if (this.type === "prev") {
          this.context.prevWeek();
          return;
        }
        this.context.nextWeek();
      }
    },
  },
};
</script>

<style lang="scss" scoped>
</style>