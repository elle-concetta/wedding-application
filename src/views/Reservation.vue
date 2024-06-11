<template>
  <section id="rsvp" class="pt-8 pb-16 bg-slate-100">
    <div class="container">
      <div class="col">
        <h2 class="h2">RSVP</h2>
        <h3>Attendance Confirmation</h3>
      </div>
      <div class="max-w-sm mx-auto alert hidden " role="alert">
        <div class="py-3 flex justify-center bg-green-200 rounded-xl text-slate-800">
          <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="currentColor"
               class="w-6 h-6 text-fuchsia-600">
            <path fill-rule="evenodd" d="M2.25 12c0-5.385 4.365-9.75 9.75-9.75s9.75 4.365 9.75 9.75-4.365 9.75-9.75 9.75S2.25 17.385 2.25 12zm13.36-1.814a.75.75 0 10-1.22-.872l-3.236 4.53L9.53 12.22a.75.75 0 00-1.06 1.06l2.25 2.25a.75.75 0 001.14-.094l3.75-5.25z" clip-rule="evenodd" />
          </svg>
          <p> <span class="font-bold pl-2">Thank you!</span> Your message has been sent.</p>
        </div>
      </div>
      <form @submit.prevent="submitForm" ref="form" name="submitTs" action="" method="POST">
        <div class="w-full lg:w-2/3 lg:mx-auto">
          <div class="w-full px-4 mb-8">
            <label for="name" class="font-bold text-slate-600">Name</label>
            <input type="text" v-model="formData.name" name="name" id="name" placeholder="Your Full Name" autofocus minlength="2" autocomplete="off" required
                   class="w-full bg-slate-200 text-slate-950 p-3 rounded-md focus:outline-none focus:ring-lime-600 focus:ring-1">
          </div>
          <div class="w-full px-4 mb-8">
            <label for="name" class="font-bold text-slate-600">Accompanying Invitees</label>
            <input type="text" v-model="formData.accInvitees" name="accInvitees" id="accInvitees" placeholder="Names of Other Invitees on Your Invitation" autocomplete="off" required
                   class="w-full bg-slate-200 text-slate-950 p-3 rounded-md focus:outline-none focus:ring-lime-600 focus:ring-1">
          </div>
          <div class="w-full px-4 mb-8">
            <label for="guests" class="font-bold text-slate-600">Number of Guests</label>
            <input type="number" v-model="formData.guests" name="guests" id="guests" placeholder="Number of Guests" required autocomplete="off"
                   class="w-full bg-slate-200 text-slate-950 p-3 rounded-md focus:outline-none focus:ring-lime-600 focus:ring-1">
          </div>
          <div class="w-full px-4 mb-8">
            <label for="status" class="font-bold text-slate-600">Status</label>
            <select v-model="formData.status" name="status" id="status" placeholder="Confirmation" required
                    class="w-full h-18 bg-slate-200 text-slate-950 p-3 rounded-md focus:outline-none focus:ring-lime-600 focus:ring-1">
              <option value="Attend">Attend</option>
              <option value="Absent">Absent</option>
              <option value="Undecided">Undecided</option>
            </select>
          </div>
          <button type="submit" id="submit" aria-label="button confirm" :disabled="loading"
                  class="btnConfirm text-base font-semibold rounded-lg outline outline-gray-800 outline-2 outline-offset-2 bg-green-600 text-gray-100 hover:text-white hover:bg-red-700 shadow-lg py-1 px-4"
                  v-bind:class="{ 'hidden': loading }">
            Send
          </button>
          <div class="w-full px-4 btnLoading" v-bind:class="{ 'hidden': !loading }">
            <button type="button" class="rounded-full outline outline-gray-800 outline-2 outline-offset-2 bg-amber-500 text-gray-100 py-1 px-4 flex" :disabled="loading">
              <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white flex flex-wrap"
                   xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                <path class="opacity-75" fill="currentColor"
                      d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z">
                </path>
              </svg>
              <span class="text-white flex flex-wrap">Processing...</span>
            </button>
          </div>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
export default {
  data() {
    return {
      formData: {
        name: '',
        accInvitees: '',
        guests: '',
        status: 'Attend'
      },
      loading: false
    };
  },
  methods: {
    submitForm() {
      this.loading = true;
      const scriptURL = 'https://script.google.com/macros/s/AKfycbzzXPl5OGmLY0IM3oUV6iWv--HklLj0HW8nL5EojJv6fW14MPXhWolm6Nje00XWzhWh/exec';
      const form = this.$refs.form;

      fetch(scriptURL, {method: 'POST', body: sendingData})
          .then(response => {
            console.log('Success! Data sent successfully', response);
            this.loading = false;
            this.showAlert();
            this.clearForm();
            this.toggleButtons();
          })
          .catch(error => {
            console.error('Error!', error.message);
            this.loading = false;
            this.toggleButtons();
          });
    },
    showAlert() {
      const alert = document.querySelector('.alert');
      alert.classList.remove('hidden');
      setTimeout(() => {
        alert.classList.add('hidden');
      }, 5000);
    },
    clearForm() {
      this.formData.name = '';
      this.formData.accInvitees = '';
      this.formData.guests = '';
      this.formData.status = 'Attend';
    },
    toggleButtons() {
      const btnLoading = document.querySelector('.btnLoading');
      const btnConfirm = document.querySelector('.btnConfirm');
      btnLoading.classList.toggle('hidden');
      btnConfirm.classList.toggle('hidden');
    }
  }
};
</script>

<style scoped>
@import url('https://use.typekit.net/fyp0lzh.css');

@font-face {
  font-family: 'Mier A';
  src: url('src/assets/mier/MierA-Regular.eot');
  src: local('Mier A Regular'), local('MierA-Regular'),
  url('src/assets/mier/MierA-Regular.eot?#iefix') format('embedded-opentype'),
  url('src/assets/mier/MierA-Regular.woff2') format('woff2'),
  url('src/assets/mier/MierA-Regular.woff') format('woff'),
  url('src/assets/mier/MierA-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: 'Mier A Demi';
  src: url('src/assets/mier/MierA-DemiBold.eot');
  src: local('Mier A DemiBold'), local('MierA-DemiBold'),
  url('src/assets/mier/MierA-DemiBold.eot?#iefix') format('embedded-opentype'),
  url('src/assets/mier/MierA-DemiBold.woff2') format('woff2'),
  url('src/assets/mier/MierA-DemiBold.woff') format('woff'),
  url('src/assets/mier/MierA-DemiBold.ttf') format('truetype');
  font-weight: 600;
  font-style: normal;
}

h2 {
  text-align: center;
  font-family: "freight-big-pro", serif !important;
  font-size: 39px;
}

h3 {
  text-align: center;
  font-family: "Mier A Demi", sans-serif !important;
  font-style: normal;
  font-size: large;
}

p {
  text-align: center;
  font-family: "Mier A", sans-serif !important;
  font-size: medium;
}

label {
  text-align: center;
  font-family: "Mier A", sans-serif !important;
  font-size: large;
}


</style>
