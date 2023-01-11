<template>
  <button :class="className">{{ value }}</button>
</template>

<script>
export default {
  props: {
    value: String,
    className: String,
  }
}
</script>
