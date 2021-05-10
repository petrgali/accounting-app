<template>
  <button :class="this.$props.meta.class" @click="click">
    {{ this.$props.meta.title }}
  </button>
</template>

<script>
export default {
  props: ["meta"],
};
</script>
