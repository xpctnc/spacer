<template>
  <div :class="[{ flexStart: state === 1 },'wrapper']">
    <transition name="slide">
      <img src="./assets/logo.svg" alt="logo" class="logo" v-if="state === 1">
    </transition>
    <transition name="fade">
      <HeroImage v-if="state === 0"/>
    </transition>
    <Claim v-if="state === 0"/>
    <SearchInput v-model="searchValue" @input="handleInput" :dark="state === 1"/>
    <div class="results" v-if="results && !loading && state === 1">
      <!-- eslint-disable-next-line -->
      <Item v-for="item in results" :item="item" :key="item.data[0].nasa_id" @click.native="handleModalOpen(item)"/>
    </div>
    <Modal v-if="modalOpen" :item="modalItem" @closeModal="modalOpen = 0" />
  </div>
</template>
<script>
import axios from 'axios';
import debounce from 'lodash.debounce';
import Claim from '@/components/Claim.vue';
import SearchInput from '@/components/SearchInput.vue';
import HeroImage from '@/components/HeroImage.vue';
import Item from '@/components/Item.vue';
import Modal from '@/components/Modal.vue';

const API = 'https://images-api.nasa.gov/search';

export default {
  name: 'App',
  components: {
    HeroImage,
    Claim,
    SearchInput,
    Item,
    Modal,
  },
  data() {
    return {
      modalOpen: false,
      modalItem: null,
      loading: false,
      state: 0,
      searchValue: '',
      results: [],
    };
  },
  methods: {
    // eslint-disable-next-line
        handleInput: debounce(function() {
      this.loading = true;
      axios.get(`${API}?q=${this.searchValue}&media_type=image`)
        .then((response) => {
          this.results = response.data.collection.items;
          this.loading = false;
          this.state = 1;
        })
        .catch((error) => {
          console.log(error);
        });
    }, 500),
    handleModalOpen(item) {
      this.modalOpen = true;
      this.modalItem = item;
    },
  },

};
</script>
<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Montserrat:300,400,600,800');

* {
  box-sizing: border-box;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

body {
  font-family: 'Montserrat', sans-serif;
  margin: 0;
  padding: 0;
}
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s ease;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}

.slide-enter-active, .slide-leave-active {
  transition: margin-top .5s ease;
}

.slide-enter, .slide-leave-to {
  margin-top: -50px;
}

.wrapper {
  margin: 0;
  position: relative;
  width: 100%;
  min-height: 100vh;
  padding: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;

  &.flexStart {
    justify-content: flex-start;
  }

  .logo {
    position: absolute;
    top: 30px;
  }

  div.results {
    margin-top: 50px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 20px;

    @media(min-width: 768px) {
      width: 90%;
      grid-template-columns: 1fr 1fr 1fr;
    }
  }
}
</style>
