<script setup lang="ts">
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faHeart } from '@fortawesome/free-solid-svg-icons';
import InfoAbout from './InfoAbout.vue';

</script>
<template>
  <section class="card-a" id="about">
      <div class="container">
        <div class="row">
          <div class="col mt-3">
            <h2 class="h2">The Wedding of Ankit & Elle</h2>
          </div>
        </div>
        <div class="row ">
          <div class="col">
            <p class="fst-italic">અંકિત અને એલે</p>
          </div>
        </div>
      </div>
    <div class="row-container-bg">
      <div class="container-bg">
        <div class="bg-groom">
          <div class="mpl-img">
            <img src="/src/img/aboutView-1.png" width="400" alt="Ankit" />
          </div>
          <div class="mpl-description">
            <h1 class="bg-name name-groom">Ankit Mahattej Amin</h1>
            <p class="name-groom">Son of Mahattej & Jolly Amin</p>
            <p class="name-groom">Brother of Mayank Amin</p>
            <p class="name-groom">Our journey took a beautiful turn on August 28, 2021. That was the day
              of our first date in downtown Raleigh, NC.</p>
          </div>
        </div>
        <div class="icon-bg">
          <FontAwesomeIcon :icon="faHeart"/>
        </div>
        <div class="bg-bride">
          <div class="mpw-img">
            <img src="/src/img/aboutView-2.png" width="400" alt="Elle" />
          </div>
          <div class="mpw-description">
            <h1 class="bg-name name-bride">Elizabeth Concetta Fassler</h1>
            <p class="name-bride">Daughter of Bernie Fassler & Amy Eppers</p>
            <p class="name-bride">Then, on March 9, 2024 in the enchanting setting of Lake Como, Italy at Villa Olmo, Ankit proposed.</p>
          </div>
        </div>
      </div>
      </div>
    </section>
  <InfoAbout></InfoAbout>
</template>
<style scoped>
@import url('https://use.typekit.net/fyp0lzh.css');
@import url('https://fonts.googleapis.com/css2?family=Noto+Serif+Gujarati:wght@100..900&display=swap');


section {
  display: flex;
  flex-direction: column;
  padding: 1rem 1rem 4rem 1rem;
  align-items: center;
  justify-content: center;
  gap: 1rem;
}

section, h2 {
  text-align: center;
  font-family: "freight-big-pro", serif !important;
  font-size: 33px;
}

.fst-italic {
  text-align: center;
  font-family: "Noto Serif Gujarati", serif;
  font-size: 29px;
  font-weight: 400;
  font-style: normal;
}

section .container {
  display: grid;
  grid-template-rows: auto;
}

section .container-bg {
  padding: 2rem;
  display: flex;
  flex-direction: column;
  gap: 2rem;
  align-items: center;
}

img.rounded-circle.img-thumbnail {
  border-radius: 100px;
  backdrop-filter: blur(1px);
}

.bg-groom {
  display: flex;
  align-items: center;
  gap: 2rem;
  justify-content: space-between;
}

.bg-bride {
  display: flex;
  flex-direction: row-reverse;
  align-items: center;
  gap: 2rem;
  justify-content: center;
}

.mpl-description, .mpw-description {
  max-width: 600px; /* Ensure a consistent width for descriptions */
}

.container-bg p {
  font-family: "Mier A", sans-serif !important;
  font-size: medium;
  font-weight: normal;
  font-style: normal;
}

.icon-bg {
  display: flex;
  justify-content: center;
  align-items: center;
}

svg.svg-inline--fa.fa-heart {
  color: #be185d;
  font-size: 4rem;
}

.bg-name {
  font-family: "freight-big-pro", serif !important;
  font-weight: 500;
  font-size: 27px;
}

.mpl-description .name-groom {
  text-align: start;
}

.mpw-description .name-bride {
  text-align: end;
}

@media (max-width: 768px) {
  section .container-bg {
    display: flex;
    flex-direction: column;
  }

  .bg-groom, .bg-bride {
    display: flex;
    flex-direction: column;
  }

  .mpl-description {
    order: -1;
  }

  .mpl-description .name-groom, .mpw-description .name-bride {
    text-align: center;
  }
}
</style>
