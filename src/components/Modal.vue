<template>
  <div class="outerWrapper">
    <div class="innerWrapper">
      <div class="photo">
        <img :src="photo" alt="">
      </div>
      <div class="description">
        <h2 class="title">{{ title }}</h2>
        <p class="desc">{{ description }}</p>
      </div>
    </div>
    <div class="close" @click="$emit('closeModal')"></div>
  </div>
</template>

<script>
export default {
  name: 'Modal',
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      photo: null,
      title: null,
      description: null,
    };
  },
  mounted() {
    this.photo = this.item.links[0].href;
    this.title = this.item.data[0].title;
    this.description = this.item.data[0].description.substring(0, 200);
  },
};
</script>

<style lang="scss" scoped>
.outerWrapper {
  background-color: #f6f6f6;
  max-width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;

  @media(min-width: 1024px) {
    max-width: 70%;
    height: 60%;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    margin: auto;
    box-shadow: 0 30px 30px -10px rgba(0, 0, 0, .3);
  }
}

.innerWrapper {
  display: flex;
  height: 100%;
  padding: 50px;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  @media(min-width: 1024px) {
    flex-direction: row;

    .photo {
      min-width: 50%;
      margin-right: 20px;
    }
  }

  .photo {
    width: 50%;
    height: auto;
    background-color: black;

    img {
      width: 100%;
    }
  }

  .description {
    color: #333;
  }

}

.close {
  position: absolute;
  right: 0;
  top: 0;
  width: 30px;
  height: 30px;
  padding: 30px;

  &::before, &::after {
    content: '';
    top: 30px;
    right: 20px;
    cursor: pointer;
    width: 20px;
    height: 2px;
    background-color: black;
    display: block;
    position: absolute;
  }

  &::before {
    transform: rotate(45deg);
  }

  &::after {
    transform: rotate(-45deg);
  }
}
</style>
