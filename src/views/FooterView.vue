<script setup>
import SocialItem from '@/components/SocialItem.vue';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faHeart } from '@fortawesome/free-solid-svg-icons';
import { ref , onMounted } from 'vue';

const currentYear  = ref('');

onMounted(() => {
  getCurrentYear();
});

function getCurrentYear(){
  const year = new Date().getFullYear();
  currentYear.value = year.toString();
}
</script>
<template>
  <section class="card">
    <div class="container">
      <div class="col">
        <div class="item-text">
        <h2> &copy; {{ currentYear }} Created with <FontAwesomeIcon :icon="faHeart" class="faHeartPulse"/> by <a href="https://github.com/elle-concetta" target="_blank" rel="noopener noreferrer">Elle</a></h2>
        <SocialItem></SocialItem>
      </div>
      </div>
    </div>
  </section>
</template>
<style scoped>

@font-face {
  font-family: 'Mier A';
  src: url('@/assets/mier/MierA-Regular.eot');
  src: url('@/assets/mier/MierA-Regular.eot?#iefix') format('embedded-opentype'),
       url('@/assets/mier/MierA-Regular.woff2') format('woff2'),
       url('@/assets/mier/MierA-Regular.woff') format('woff'),
       url('@/assets/mier/MierA-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}


h2 {
  font-family: "Mier A", sans-serif !important;
  font-weight: normal;
  font-style: normal;
}

  .item-text{
    display: flex;
    align-items: center;
    flex-direction:column-reverse;
    margin-bottom: 5rem;
  }

  a {
    cursor: pointer;
    text-decoration: none;
    color: #5639af;
    font-weight: 700;
  }

  .faHeartPulse{
    color: #be185d;
  }
</style>
