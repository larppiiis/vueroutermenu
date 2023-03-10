<template>
  <div id="event-form">
    <form @submit.prevent="handleSubmit">
      <div class="mb-3">
        <label>Tapahtuman nimi </label>
        <input ref="first"
               type="text"
               :class="{ 'has-error': submitting && invalidName }"
               class="input_form"
               v-model="event.name"
               @focus="clearStatus"
               @keypress="clearStatus"
        />
      </div>
        <div id="collapsible">

          <div class="mb-3">
            <label class="form-label">Tapahtumapaikka </label>
            <input type="text"
                   :class="{ 'has-error': submitting && invalidPlace }"
                   v-model="event.place"
                   @focus="clearStatus"
                   @keypress="clearStatus"
                   class="input_form"

            />
          </div>
      </div>
      <div class="mb-3">
        <label>Tapahtuma-aika </label>
        <input
            type="date"
            format="dd/MM/yyyy"
            value-format="dd-MM-yyyy"
            :class="{ 'has-error': submitting && invalidDate }"
            v-model="event.date"
            @focus="clearStatus"
            class="input_form"
        />
      </div>

      <p v-if="error && submitting" class="error-message">❗Ole hyvä ja täytä kaikki kentät</p>
      <p v-if="success" class="success-message">✅ Tiedot lähetetty</p>
      <button type="submit" id="submitbutton" class="btn btn-primary">Lisää</button>
    </form>
  </div>
</template>
<script>
import axios from 'axios';
export default {
  name: 'AddEvent',
  data() {
    return {
      submitting: false,
      error: false,
      success: false,
      event: {
        name: '',
        date: '',
        place: '',
      },
    };
  },
  methods: {
    async handleSubmit() {
      this.clearStatus();
      this.submitting = true;
      if (this.invalidName || this.invalidDate || this.invalidPlace) {
        this.error = true;
        return;
      }
      axios.post('http://localhost:3001/events', {
        name: this.event.name,
        date: new Date(this.event.date),
        place: this.event.place
    })
    .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
      this.$refs.first.focus();
      this.event = {
        name: '',
        place: '',
        date: '',
      };
      this.error = false;
      this.success = true;
      this.submitting = false;
    },
    clearStatus() {
      this.success = false;
      this.error = false;
    },
  },
  computed: {
    invalidName() {
      return this.event.name === ''
    },
    invalidDate() {
      return this.event.date === ''
    },
    invalidPlace() {
      return this.event.place === ''
    },
  },
};
</script>
<style scoped>
.homeText {
  font-size: 35px;
  color: red;
  text-align: center;
  position: relative;
  top: 30px;
  text-shadow: 2px 2px 2px gray;
}
#event-form{
  margin-top: 20px;
}
.input_form{
  margin-left: 20px;
}
form {
  margin-bottom: 2rem;
}
[class*='-message'] {
  font-weight: 500;
}
.error-message {
  color: #d33c40;
}
.success-message {
  color: #32a95d;
}
</style>