<script>
export default {
  props: {
    listLength: Number
  },
  data() {
    return {
      selectedIndex: 0
    };
  },
  render(h) {
    return h(
      "div",
      this.$scopedSlots.default({ selectedIndex: this.selectedIndex })
    );
  },
  methods: {
    /**
      38 - up
      40 - down
      13 - enter
    */
    keyHandler(event) {
      const key = event.which || event.keyCode;

      if (key === 38) {
        this.handleKeyUp(event);
      } else if (key === 40) {
        this.handleKeyDown(event);
      } else if (key === 13) {
        this.handleEnter(event);
      }
      this.$parent.$children[0].$el.scrollTop = 0; //set to top
      this.$parent.$children[0].$el.scrollTop =
        this.$el.querySelector(".selected").offsetTop -
        this.$parent.$children[0].$el.clientHeight;
    },

    handleEnter(event) {
      event.preventDefault();
      this.$emit("selected", this.selectedIndex);
    },

    handleKeyUp(event) {
      event.preventDefault();
      if (this.selectedIndex <= 0) {
        // If index is less than or equal to zero then set it to the last item index
        this.selectedIndex = this.listLength - 1;
      } else if (
        this.selectedIndex > 0 &&
        this.selectedIndex <= this.listLength - 1
      ) {
        // If index is larger than zero and smaller or equal to last index then decrement
        this.selectedIndex--;
      }
    },

    handleKeyDown(event) {
      event.preventDefault();
      // Check if index is below 0
      // This means that we did not start yet
      if (
        this.selectedIndex < 0 ||
        this.selectedIndex === this.listLength - 1
      ) {
        // Set the index to the first item
        this.selectedIndex = 0;
      } else if (
        this.selectedIndex >= 0 &&
        this.selectedIndex < this.listLength - 1
      ) {
        this.selectedIndex++;
      }
    },

    addKeyHandler() {
      window.addEventListener("keydown", this.keyHandler);
    },

    removeKeyHandler() {
      window.removeEventListener("keydown", this.keyHandler);
    }
  },

  created() {
    this.addKeyHandler();
  },

  destroyed() {
    this.removeKeyHandler();
  }
};
</script>
