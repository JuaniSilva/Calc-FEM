<template>
  <button>
    {{ text }}
  </button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: [String, Number],
    bg: String, 
  },
};
</script>

<style scoped>
@import "../../public/styles.css";
*,
*::after,
*::before {
  font-size: 2rem;
  color: var(--th1-grayish-blue);
}

</style>
