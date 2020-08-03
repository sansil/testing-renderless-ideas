
<script>
export default {
  inject: ["context"],
  props: ["value", "formatedDate"],
  render: function (createElement) {
    // var self = this;
    return createElement("input", {
      attrs: {
        type: "text",
      },
      domProps: {
        value: this.formatedDate,
      },
      on: {
        focus: this.focusHandler,
      },
    });
  },
  data() {
    return {
      //formatedDate: "",
    };
  },
  watch: {
    value() {
      //this.formatDate(this.value);
    },
  },
  methods: {
    // formatDate(d) {
    //   let options = { year: "numeric", month: "numeric", day: "numeric" };
    //   this.formatedDate = d.toLocaleString(["es-UY"], options);
    //   this.context.closePicker();
    //   return this.formatedDate;
    // },
    focusHandler() {
      console.log("focus");
      this.context.focusHandler();
    },
  },
};
</script>
