<template>
    <input
      id="search"
      name="search"
      :class="{ dark: dark }"
      :value="value"
      @input="handleChange"
    />
</template>
<script>
// line 5
// className: props
export default {
  name: 'SearchInput',
  props: {
    value: {
      required: true,
      type: String,
    },
    dark: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    handleChange(e) {
      this.$emit('input', e.target.value);
    },
  },
};
</script>
<style lang="scss" scoped>

input {
  margin-top: 50px;
  display: flex;
  width: 250px;
  color: white;
  text-align: center;
  font-size: 18px;
  font-weight: 300;
  height: 30px;
  border: 0;
  background: none;
  border-bottom: 1px solid white;
  transition: box-shadow .3s ease-out;

  @media (min-width: 1024px) {
    font-weight: 400;
  }
}

input:focus {
  outline: none;
  box-shadow: 0 10px 20px -8px rgba(255,255,255, .5);
}

.dark {
  color: #1e3d4a;
  border-bottom-color: #1e3d4a;

  &:focus {
    box-shadow: 0 10px 20px -8px rgba(#1e3d4a, .2);
  }
}

</style>
