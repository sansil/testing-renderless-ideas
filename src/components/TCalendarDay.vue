<script>
export default {
  name: "tiny-calendar-day",
  inject: ["context"],
  props: {
    day: {
      type: Date,
      required: true,
    },
  },
  render: function (createElement) {
    return createElement(
      "div",
      {
        on: {
          click: this.clickHandler,
        },
      },
      this.$scopedSlots.default({})
    );
  },
  methods: {
    clickHandler() {
      if (!this.context.isLTminDate(this.day)) this.context.emitValue(this.day);
    },
  },
};
</script>


