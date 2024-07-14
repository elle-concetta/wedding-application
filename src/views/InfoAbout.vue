<script setup>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
import { faLocationDot } from '@fortawesome/free-solid-svg-icons';
import TheCountdown from '@/components/TheCountdown.vue';

const redirectToLocation = (location) => {
  window.open(location, '_blank');
};
</script>
<template>
    <section class="card" id="info" >
        <div class="container">
            <div class="col">
                <h1> <div style="padding-bottom: 20px;">Wedding <i>Information</i></div></h1>
                <div class="description-date">
              
                    <table>
                    <tr>
                        <h3>DATES AND EVENTS </h3>                   
                        <p>Friday, November 7, 2025 | Welcome Party and Catholic Ceremony</p>                        
                        <p>Saturday, November 8, 2025 | Hindu Ceremony and Reception</p>
                    </tr>
                    <tr>
                        <h3>EVENT TIMES </h3>
                        <p>Welcome Party and Mehndi | </p>
                        <p>Catholic Ceremony | </p>
                        <p>Hindu Ceremony | </p>
                        <p>Reception | </p>
                    </tr>
                    <tr>
                        <h3>VENUE LOCATION</h3>
                        <p>Resort </p>
                        <p>Florida</p>
                    </tr>
                </table>
                </div>
            </div>
            <div class="col">
                <div class="google-maps btn">
                    <div class="btn-icon">
                        <FontAwesomeIcon :icon="faLocationDot" style="font-size: 16px;" />
                    </div>
                    <div class="btn-title">
                        <button @click="redirectToLocation('https://maps.app.goo.gl/x7m6mCvsL9WKEgxd9')" >Wedding Location</button>
                    </div>
                    
                </div>
            </div>
            <div class="col">
                <div class="description">
                  <p>Book Hotel Reservations: Hotel 1, Hotel 2, and Hotel 3</p>
                </div>
            </div>
            <div class="col">
             <figure>
                <figcaption>
                  <p>If you would like to purchase indian wear for Hindu Ceremony:</p>
                  <p>Pernia's Pop-up Shop, Kalki Fashion, Aza Fashions, Poshmark, etc.</p>
                  <p>Women: Saree, Lehenga (Lengha), or Anarkali</p>
                  <p>Men: Kurtas or Kurta Sets </p>
                  <p>If you need help, let us know <3</p>

                  <div class="">
                    <p>...</p>
                    </div>
                    <h2 class="h2">We are excited to see you at our wedding!</h2>
                </figcaption>
             </figure>
            </div>
            <div class="col">
                <TheCountdown></TheCountdown>
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


@font-face {
  font-family: 'Mier A Demi';
  src: url('@/assets/mier/MierA-DemiBold.eot');
  src: url('@/assets/mier/MierA-DemiBold.eot?#iefix') format('embedded-opentype'),
       url('@/assets/mier/MierA-DemiBold.woff2') format('woff2'),
       url('@/assets/mier/MierA-DemiBold.woff') format('woff'),
       url('@/assets/mier/MierA-DemiBold.ttf') format('truetype');
  font-weight: 600;
  font-style: normal;
}


p {
  font-family: "Mier A", sans-serif !important;
  font-weight: normal;
  font-style: normal;
  font-size: medium;
}

figcaption > p {
  font-family: "Mier A", sans-serif !important;
  font-weight: normal;
  font-style: normal;
  text-shadow: #92400e 1px 0 10px;
  font-size: medium;
}

h1 {
  text-align: center;
  font-family: "freight-big-pro", serif !important;
  font-size: 39px;
}


h2, h3 {
  font-family: "Mier A Demi", sans-serif !important;
  font-weight: normal;
  font-style: normal;
  font-size: large;
}

button {
  font-family: "Mier A", sans-serif !important;
  font-weight: normal;
  font-style: normal;
  font-size: large;
  display: flex;
  align-items: center;
  justify-content: center;
}

.btn-icon {
    display: flex;
    align-items: center;
    margin-right: 8px; /* Space between icon and text */
}

.site-title > p::before {
  content: "\2014\00A0";
}

.site-title > p {
 font-style: italic;
 color : #6c757d;
}

.col {
    gap: 0;
}

</style>
