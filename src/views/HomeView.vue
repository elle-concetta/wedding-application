<script setup>
import { RouterLink, RouterView } from 'vue-router';
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faHeart } from '@fortawesome/free-solid-svg-icons';
import {faEnvelopeOpen} from "@fortawesome/free-solid-svg-icons/faEnvelopeOpen";

</script>
<script>
export default {
  computed: {
    dynamicRoute() {
      const destination = this.$route.query.to || "Family and Friends";
      // const linkText = `Invitation to ${destination}`;
      return { path: '/', query: { to: destination } };
    },
    linkText() {
      return this.$route.query.to ? `Dear ${this.$route.query.to}` : "Dear Family and Friends";
    },
    destination() {
      return this.$route.query.to || "Family and Friends";
    }
  }
}
</script>
<template>
  <router-link :to="dynamicRoute">{{ linkText }}</router-link>
  <main>
    <div class="coverImg welcomeModel">
    </div>
    <div class="box-welcomeModel">
      <div class="description-welcomeModel">
      <div class="subtitle">The Wedding Of</div>
      <div class="title">Ankit Amin <FontAwesomeIcon :icon="faHeart"/> Elle Fassler</div>
      <div class="subtitle-tgl">November 7th-8th, 2025</div>
      <div class="text">Florida</div>
      <p class="invitation-text">{{ destination }}</p>

        <div class="button-welcomeModel">
          <RouterLink to="/landingpage"><FontAwesomeIcon :icon="faEnvelopeOpen"/> Open Invitation</RouterLink>
        </div>
      </div>
    </div>
  </main>
  <RouterView />
</template>
<style scoped>
@import url('https://use.typekit.net/fyp0lzh.css');

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


  .subtitle{
  font-family: "freight-big-pro", serif !important;
  font-weight: 300;
  font-style: italic;
  font-size: xxx-large;
  }

  .title{
  font-family: "freight-big-pro", serif !important;
  font-weight: 300;
  font-style: normal;
  font-size: xx-large;
  }

  .subtitle-tgl{
  font-family: "Mier A", sans-serif !important;
  font-style: normal;
  font-size: medium;
  }

  .text{
  font-family: "Mier A", sans-serif !important;
  font-style: normal;
  font-size: large;
  }

.invitation-text{
  font-family: "Mier A", sans-serif !important;
  font-style: normal;
  font-size: medium;
}

  main .coverImg {
    height: 100%;
    width: 100%;
    left: 0;
    top: 0;
    overflow: hidden;
    position: fixed;
    background-position: center;
    background-image: url('/src/img/front-cvr.jpg');
    background-color: rgb(241 128 128 / 80%);
    background-repeat: no-repeat;
    background-size: cover;
}

main .welcomeModel {
    display: block;
    text-align: center;
    align-content: center;
}

.text{
  margin-top: 0.75rem;
}

.box-welcomeModel p{
  margin-bottom: 0.75rem;
}

.description-welcomeModel{
  position: fixed;
  width: max-content;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: block;
    text-align: center;
    margin-bottom: 20px;
    background-color: #1a1a1a;
    color: #F3F8FF;
    border-radius: 25px 0 25px 0;
    box-shadow: 10px 6px 15px #35374B30;
    padding: 10px 20px 10px 20px;
    backdrop-filter: blur(10px);
}

.button-welcomeModel {
  background-color: whitesmoke;
  border-radius: 100px;
  cursor: pointer;
  height: 2.5rem;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 2px solid transparent;
  text-align: center;
}

.button-welcomeModel:hover {
  border: 1px solid #d48372;
}

a {
  text-decoration: none;
  font-family: "aktiv-grotesk-extended", sans-serif !important;
  color: #030712;
  background-color: transparent;
}


svg.svg-inline--fa.fa-letter {
    padding-right: 0.5rem;
}

svg.svg-inline--fa.fa-heart {
    color: #be185d;
}

</style>
