<template>
  <div id="contact-form" class="text-sm">
    <form id="contact-form" class="text-sm" @submit.prevent="handleSubmit">
      <div class="flex flex-col">
        <label for="name" class="mb-3">_name:</label>
        <input type="text" id="name-input" v-model="formData.name" name="name" :placeholder="name"
               class="p-2 mb-5 placeholder-slate-600" required>
      </div>
      <div class="flex flex-col">
        <label for="email" class="mb-3">_email:</label>
        <input type="email" id="email-input" v-model="formData.email" name="email" :placeholder="email"
               class="p-2 mb-5 placeholder-slate-600" required>
      </div>
      <div class="flex flex-col">
        <label for="message" class="mb-3">_message:</label>
        <textarea id="message-input" v-model="formData.message" name="message" :placeholder="message"
                  class="placeholder-slate-600" required></textarea>
      </div>
      <button id="submit-button" type="submit" class="py-2 px-4">submit-message</button>
    </form>
    <p v-if="successMessage" class="mt-4 text-green-500">{{ successMessage }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'ContactForm',
  props: {
    name: {
      type: String,
      required: true
    },
    email: {
      type: String,
      required: true
    },
    message: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      formData: {
        name: this.name,
        email: this.email,
        message: this.message
      },
      successMessage: ''
    };
  },
  methods: {
    async handleSubmit() {
      const apiUrl = import.meta.env.VITE_API_URL;
      if (!apiUrl) {
        console.error("API URL is not defined");
        return;
      }
      try {
        const response = await axios.post(apiUrl, this.formData);
        if (response.status === 201) {
          this.formData.name = '';
          this.formData.email = '';
          this.formData.message = '';
          this.successMessage = 'Your message has been sent successfully!';
        }
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>

<style>

form {
  @apply font-fira_retina text-menu-text
}

input {
  background-color: #011221;
  border: 2px solid #1E2D3D;
  border-radius: 7px;

}

/* Change Autocomplete styles in Chrome*/
input:-webkit-autofill,
input:-webkit-autofill:hover,
input:-webkit-autofill:focus,
textarea:-webkit-autofill,
textarea:-webkit-autofill:hover,
textarea:-webkit-autofill:focus,
select:-webkit-autofill,
select:-webkit-autofill:hover,
select:-webkit-autofill:focus {
  -webkit-text-fill-color: rgb(190, 190, 190);
  transition: background-color 5000s ease-in-out 0s;
  border: 2px solid #607b96;
}

#message-input {
  background-color: #011221;
  border: 2px solid #1E2D3D;
  border-radius: 7px;
  resize: none;
  height: 150px;
  padding: 10px;
}

#submit-button {
  @apply font-fira_retina text-white text-sm;
  background-color: #1E2D3D;
  border-radius: 7px;
  margin-top: 20px;
  cursor: pointer;
}

#submit-button:hover {
  background-color: #263B50;
}

input:focus, #message-input:focus {
  outline: none;
  transition: none;
  border: 2px solid #607b96;
  box-shadow: #607b9669 0px 0px 0px 2px;
}

#contact-form {
  max-width: 370px;
  width: 100%;
}

@media (max-width: 1920px) {
  #contact-form {
    max-width: 320px;
    max-height: 400px;
  }

  #submit-button {
    /* width: 100%; */
    font-size: 12px;
  }

  textarea {
    font-size: 13px;
    max-height: 130px !important;
  }

  input {
    font-size: 13px;
  }
}
</style>