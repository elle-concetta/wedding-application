<script setup>
import AboutView from './AboutView.vue';
import footerView from './FooterView.vue';
import WeddingGift from './WeddingGift.vue';
import NavbarView from './NavbarView.vue'
import Carousel from './Carousel.vue';
import Gallery from './Gallery.vue';
import Reservation from './Reservation.vue'
import TheAuthComments from '@/components/TheAuthComments.vue';

</script>
<template>
  <Carousel></Carousel>
  <main>
    <AboutView></AboutView>
    <Gallery></Gallery>
    <Reservation></Reservation>
    <WeddingGift></WeddingGift>
    <TheAuthComments></TheAuthComments>
    <footerView></footerView>
  </main>
    <NavbarView></NavbarView>
</template>
