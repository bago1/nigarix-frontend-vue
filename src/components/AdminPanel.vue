<template>

  <div class="container">
    <form>
      <div class="form-group">
        <label for="fullname">fullname</label>
        <input type="text" class="form-control" id="fullname" v-model="form.fullname" placeholder="Full Name">
      </div>
      <div class="form-group">
        <label>Email address</label>
        <input type="text" class="form-control" aria-describedby="emailHelp" v-model="form.emailAddress"
               placeholder="Enter email">
      </div>

      <div class="form-group">
        <label for="subject">subject</label>
        <input type="text" class="form-control" id="subject" aria-describedby="emailHelp" v-model="form.subject"
               placeholder="Enter Subject">
      </div>
      <div class="form-group">
        <label for="message">message</label>
        <input type="text" class="form-control" id="message" v-model="form.message">
      </div>

      <button type="button" class="btn btn-primary" @click="submitForm">Send Mail</button>
    </form>
    <br><br>

    <div class="file">
      <form @submit="submitImage()" enctype="multipart/form-data">
        <div class="fields">
          <input type="file" ref="file" @change="onSelect()">
        </div>
        <div class="fields">
          <button class="btn btn-primary">Change Profile Photo</button>
        </div>
        <div class="message">
          <h5>{{ message }}</h5>
        </div>
      </form>
    </div>

    <div>
      <h1>Welcome our site {{ name }}</h1>

      <form>
        <!--  <button type="button" class="btn btn-primary" @click="getphoto">getphoto2</button>-->
      </form>

890065
            <img v-bind:src="'data:image/png;base64,'+sekil"/>

    </div>

  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      name: 'asd',
      posts: [],
      form: {
        fullname: "",
        emailAddress: "",
        subject: "",
        message: "",
      },

      file: "",
      message: "",
      sekil: ""

    }
  },
  mounted() {
    this.getphoto()
  },
  methods: {
    getphoto() {
      return axios.get('http://localhost:8080/getavatar')
          .then(response => this.sekil = response.data)
          .catch(e => {
            console.log(e)
          })
    },
    onSelect() {
      const file = this.$refs.file.files[0];
      this.file = file;
    },
    async submitImage() {
      const formData = new FormData();
      formData.append('file', this.file)
      try {
        await axios.post('http://localhost:8080/updateavatar', formData)
        this.message = "uploaded"
      } catch (err) {
        console.log(err);
        this.message = "SMTH WENT WRONG";
      }
    },
    submitForm() {
      axios.post('http://localhost:8080/email', this.form)
          .then(response => {
            console.log(response)
          }).catch(e => {
        console.log(e)
      })
    },

    created() {
      axios.get(`http://jsonplaceholder.typicode.com/posts`, {
        responseType: 'arraybuffer'
      })
          .then(response => Buffer.from(response.data, 'binary').toString('base64'))
          .catch(e => {
            this.errors.push(e)
          })
    }
  }
}
</script>