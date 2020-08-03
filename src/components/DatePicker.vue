<script>
//let handleOutsideClick;

export default {
  name: "TinyDatePicker",
  provide() {
    return {
      ["context"]: {
        focusHandler: this.focusHandler,
        closePicker: this.closePicker,
      },
    };
  },
  render: function (createElement) {
    // var self = this;
    return createElement(
      "div",
      {
        ref: "parent",
        // attrs: {
        //   tabindex: 0,
        // },
      },
      this.$scopedSlots.default({
        showDatePicker: this.showDatePicker,
        formatedDate: this.formatedDate,
        formatDate: this.formatDate,
      }) // arreglo de hijos
      // [
      //   createElement("input", {
      //     attrs: {
      //       type: "text",
      //       class: "relative mt-12 ml-10 h-8 max-w-xl",
      //     },
      //     domProps: {
      //       value: this.formatedDate,
      //     },
      //     on: {
      //       focus: this.focusHandler,
      //     },
      //   }),

      //   this.$scopedSlots.default({
      //     showDatePicker: this.showDatePicker,
      //     formatDate: this.formatDate,
      //   }), // arreglo de hijos
      // ]
    );
  },
  data() {
    return {
      showDatePicker: false,
      formatedDate: "",
    };
  },
  mounted() {
    const listener = (e) => {
      if (e.target !== this.$el && !this.$el.contains(e.target)) {
        this.showDatePicker = false;
      }
    };
    document.addEventListener("click", listener);
    this.$once("hook:destroyed", () => {
      document.removeEventListener("click", listener);
    });
  },
  methods: {
    focusHandler() {
      this.showDatePicker = true;
    },
    closePicker() {
      this.showDatePicker = false;
    },
    formatDate(d) {
      let options = { year: "numeric", month: "numeric", day: "numeric" };
      this.formatedDate = d.toLocaleString(["es-UY"], options);
      console.log(this.formatedDate);
      this.closePicker();
    },
  },
};
</script>

