<script setup lang="ts">
import { onMounted } from "vue";
import { initFlowbite } from "flowbite";

onMounted(() => {
  initFlowbite();
});
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome';
import { faGift, faFaceSmile } from '@fortawesome/free-solid-svg-icons';
</script>
<template>
  <section class="card" id="gifts">
    <div class="container">
      <div class="col">
        <h2 class="h2">Wedding <i>Registry</i></h2>
        <p>
          Our Amazon Wedding Registry below <FontAwesomeIcon :icon="faFaceSmile" class="icon-smile"/>
        </p>
      </div>
      <div class="col">
        <img
            src="/src/img/floating-qr.png" width="350" alt="qr-code" />
      </div>
      <div class="col">
        <button
    onclick="window.location.href='https://www.amazon.com/wedding/share/ankit-and-elle';"
    data-modal-target="popup-modal"
    data-modal-toggle="popup-modal"
    class="block rounded-lg outline outline-gray-800 outline-2 outline-offset-2 text-white bg-fuchsia-800 hover:bg-fuchsia-800 focus:ring-4 focus:outline-none focus:ring-fuchsia-300 font-medium text-sm px-4 py-2 text-center dark:bg-fuchsia-600 dark:hover:bg-fuchsia-700 dark:focus:ring-fuchsia-800"
    type="button"
  >
    <FontAwesomeIcon :icon="faGift" /> Registry
  </button>
      </div>
      </div>
  </section>
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


h2 {
  text-align: center;
  font-family: "freight-big-pro", serif !important;
  font-size: 39px;
}

p {
  font-family: "Mier A", sans-serif !important;
  font-weight: normal;
  font-style: normal;
  font-size:large;
}

.copyable-text {
  user-select: auto;
}

.icon-smile {
  font-size: 1rem;
  color: #eab308;
border-radius: 100px;
  border: 1px solid #5639af;
}
</style>
