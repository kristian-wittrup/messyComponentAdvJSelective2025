<template>
  <div>
    <h1>Contact Us</h1>
    <form @submit.prevent="submitForm">
      <div>
        <label>Name:</label>
        <input type="text" v-model="name">
      </div>
      <div>
        <label>Email:</label>
        <input type="email" v-model="email">
      </div>
      <div>
        <label>Message:</label>
        <textarea v-model="message"></textarea>
      </div>
      <button type="submit">Send</button>
    </form>

    <div v-if="errorMessage" style="color:red">
      {{ errorMessage }}
    </div>

    <div v-if="successMessage" style="color:green">
      {{ successMessage }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'ContactForm',
  data() {
    return {
      name: '',
      email: '',
      message: '',
      errorMessage: '',
      successMessage: ''
    }
  },
  methods: {
    submitForm() {
      this.errorMessage = ''
      this.successMessage = ''

      // name validation
      if (this.name === '' || this.name.length < 3) {
        this.errorMessage = 'Name must be at least 3 characters long.'
        alert('Name must be at least 3 characters long.')
        return
      }

      // email validation
      if (this.email === '' || !this.email.includes('@') || !this.email.includes('.')) {
        this.errorMessage = 'Please enter a valid email address.'
        alert('Please enter a valid email address.')
        return
      }

      // message validation
      if (this.message === '' || this.message.length < 10) {
        this.errorMessage = 'Message must be at least 10 characters long.'
        alert('Message must be at least 10 characters long.')
        return
      }

      // fake submit
      fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        body: JSON.stringify({
          name: this.name,
          email: this.email,
          message: this.message
        }),
        headers: {
          'Content-type': 'application/json; charset=UTF-8',
        }
      })
        .then(res => res.json())
        .then(data => {
          console.log('Form submitted:', data)
          this.successMessage = 'Your message has been sent!'
          this.name = ''
          this.email = ''
          this.message = ''
        })
        .catch(err => {
          console.error('Error submitting form:', err)
          this.errorMessage = 'Something went wrong. Please try again.'
        })
    }
  }
}
</script>