<template>
  <div :style="style" class="item">

  </div>
</template>

<script>
export default {
  name: 'Item',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      photo: this.item.links[0].href,
      title: this.item.data[0].title,
    };
  },
  computed: {
    // eslint-disable-next-line
    style() {
      return `background-image: url('${this.photo}')`;
    },
  },
};
</script>

<style lang="scss" scoped>
  div.item {

    background-size: cover;
    background-repeat: no-repeat;
    width: 40vw;
    height: 250px;
    background-position: 50%;

    @media(min-width: 768px) {
     width: 25vw;
    }
  }
</style>
