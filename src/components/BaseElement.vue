<template>
  <div ref="element" class="element" draggable="false" :style="style">
    this is a basic element
    <connector-input
      v-for="input in inputs"
      :key="input"
      :id="input"
      :element="name"
      @startconnecting="onStartConnecting"
    />
    <connector-output
      v-for="output in outputs"
      :key="output"
      :id="output"
      :element="name"
      @cancelconnecting="onStopConnecting"
      @stopconnecting="onCancelConnecting"
    />
  </div>
</template>

<script>
import PlainDraggable from "@phanmn/plain-draggable";
import ConnectorInput from "./ConnectorInput";
import ConnectorOutput from "./ConnectorOutput";

export default {
  components: {
    ConnectorInput,
    ConnectorOutput,
  },
  props: {
    name: {
      type: String,
      required: true,
    },
    top: {
      type: Number,
      default: 0,
    },
    left: {
      type: Number,
      default: 0,
    },
    inputs: {
      type: Array,
      default: () => [],
    },
    outputs: {
      type: Array,
      default: () => [],
    },
  },
  computed: {
    style() {
      return {
        top: this.top + "px",
        left: this.left + "px",
      };
    },
  },
  mounted() {
    this.draggable = new PlainDraggable(this.$refs.element, {
      leftTop: true,
    });
    this.draggable.onDrag = this.onDrag.bind(this);
  },
  methods: {
    onDrag(event) {
      this.$emit("drag", { left: event.left, top: event.top });
    },
    onStartConnecting(event) {
      this.$emit("startconnecting", event);
    },
    onStopConnecting(event) {
      this.$emit("stopconnecting", event);
    },
    onCancelConnecting(event) {
      this.$emit("stopconnecting", event);
    },
  },
};
</script>

<style>
.element {
  position: absolute;
  border: 1px dotted black;
  height: 40px;
  width: 120px;
}
</style>
