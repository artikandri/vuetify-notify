<template>
  <v-snackbar
    :bottom="options.y === 'bottom'"
    :color="options.color"
    :left="options.x === 'left'"
    :right="options.x === 'right'"
    :timeout="options.timeout"
    :top="options.y === 'top'"
    v-model="model"
  >
    {{ text }}
    <v-btn
      text
      small
      class="text-right"
      v-show="options.closeButton.show"
      @click="close"
      icon
    >
      <v-icon>{{ options.closeButton.icon || 'mdi-close' }} </v-icon>
    </v-btn>
  </v-snackbar>
</template>
<script>
const _options = {
  x: "left",
  y: "bottom",
  timeout: 3000,
  color: "default",
  closeButton: {
    show: false,
    icon: "mdi-close",
  },
};
export default {
  data() {
    return {
      model: false,
      promise: null,
      text: {},
      options: {
        closeButton: {
          show: false,
          icon: "mdi-close",
        },
      },
    };
  },
  methods: {
    show(text, options) {
      return new Promise((resolve, reject) => {
        if (this.model) this.model = false;
        this.$nextTick(() => {
          this.promise = Object.assign({}, { resolve, reject });
          this.text = text;
          this.options = { ..._options, ...options };
          this.model = true;
        });
      });
    },
    close() {
      this.promise.resolve(true);
      this.model = false;
    },
  },
};
</script>
