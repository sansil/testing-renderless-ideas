<script>
export default {
  name: "tiny-calendar-weekdays-names",
  inject: ["context"],
  render: function (createElement) {
    return createElement(
      "div",
      this.context.generatedWeekdayNames.map((w) => {
        return createElement("div", this.$scopedSlots.default({ wd: w }));
      })
    );
  },
};
</script>


