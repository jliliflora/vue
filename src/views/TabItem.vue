<template>
  <button @click="$emit('input', id)" :class="[active, 'tab']">
    {{ label }}
  </button>
</template>

<script>
export default {
  props: {
    id: Number,
    label: String,
    value: Number,
  },
  computed: {
    active() {
      return this.value === this.id ? "active" : false;
    },
  },
};
</script>

<style lang="scss" scoped>
.tab {
  border-radius: 2px 2px 0 0;
  background: #ffffff;
  color: #311d0a;
  line-height: 24px;
}
.tab:hover {
  background: #eeeeee;
}
.active {
  background: #f7c9c9;
}
</style>
