<template>
  <text class="uni-badge" v-if="text" :class="setClass" @click="onClick()">{{text}}</text>
</template>

<script>
export default {
  props: {
    type: {
      type: String,
      default: "default"
    },
    inverted: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      default: ""
    },
    size: {
      type: String,
      default: "normal"
    }
  },
  computed: {
    setClass() {
      let classList = ["uni-badge-" + this.type, "uni-badge-size-" + this.size];
      if (this.inverted === true) {
        classList.push("uni-badge-inverted");
      }
      return classList.join(" ");
    }
  },
  methods: {
    onClick() {
      this.$emit("click");
    }
  }
};
</script>

<style>
.uni-badge,
.uni-badge-default {
  font-family: "Helvetica Neue", Helvetica, sans-serif;
  box-sizing: border-box;
  font-size: 12px;
  line-height: 1;
  display: inline-block;
  padding: 3px 6px;
  color: #333;
  border-radius: 100px;
}

.uni-badge.uni-badge-size-small,
.uni-badge-default.uni-badge-size-small {
  height: 10px;
  line-height: 11px;
  padding: 0px 3px;
  font-size: 8px !important;
}

.uni-badge.uni-badge-inverted {
  padding: 0 5px 0 0;
  color: #929292;
  background-color: transparent;
}

.uni-badge-primary {
  color: #fff;
  background-color: #007aff;
}

.uni-badge-blue.uni-badge-inverted,
.uni-badge-primary.uni-badge-inverted {
  color: #007aff;
  background-color: transparent;
}

.uni-badge-green,
.uni-badge-success {
  color: #fff;
  background-color: #4cd964;
}

.uni-badge-green.uni-badge-inverted,
.uni-badge-success.uni-badge-inverted {
  color: #4cd964;
  background-color: transparent;
}

.uni-badge-warning,
.uni-badge-yellow {
  color: #fff;
  background-color: #f0ad4e;
}

.uni-badge-warning.uni-badge-inverted,
.uni-badge-yellow.uni-badge-inverted {
  color: #f0ad4e;
  background-color: transparent;
}

.uni-badge-danger,
.uni-badge-red {
  color: #fff;
  background-color: #d81e06;
}

.uni-badge-danger.uni-badge-inverted,
.uni-badge-red.uni-badge-inverted {
  color: #d81e06;
  background-color: transparent;
}

.uni-badge-purple,
.uni-badge-royal {
  color: #fff;
  background-color: #8a6de9;
}

.uni-badge-purple.uni-badge-inverted,
.uni-badge-royal.uni-badge-inverted {
  color: #8a6de9;
  background-color: transparent;
}
</style>
