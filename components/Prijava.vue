<template>
  <b-modal
    v-model="modalShow"
    id="modal-center"
    no-close-on-backdrop
    centered
    title="Prijava"
  >
    <form ref="form" @submit.stop.prevent="handleSubmit">
      <b-form-group
        label-for="username"
        invalid-feedback="Korisnicko ime je potrebno"
      >
        <b-form-input
          type="email"
          id="email"
          v-model="email"
          placeholder="email ili korisnicko ime"
          required
        ></b-form-input>
        <b-form-input
          type="password"
          id="text-password"
          v-model="password"
          placeholder="lozinka"
          class="mt-3"
          aria-describedby="password-help-block"
        ></b-form-input>
        <b-form-text id="password-help-block">
          Lozinka mora sadrzavati minimalno 6 znakova.<br />
          Nemaš račun, registriraj se
          <NuxtLink to="/registracija"> OVDJE </NuxtLink>
        </b-form-text>
      </b-form-group>

      <b-form-checkbox
        v-model="xchecked"
        id="zapamtiMe"
        switch
        value="rememberMe"
      >
        Zapamti me
      </b-form-checkbox>
    </form>
    <template #modal-footer>
      <b-button type="submit" size="sm" variant="success" @click="prijava">
        PRIJAVA
      </b-button>
      <b-button type="button" size="sm" variant="info" @click="empty">
        ODUSTANI
      </b-button>
    </template>
  </b-modal>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Prijava',
  data() {
    return {
      email: '',
      password: '',
      xchecked: false,
      modalShow: false,
    }
  },
  methods: {
    empty() {
      this.email = ''
      this.password = ''
      this.xchecked = ''
      this.modalShow = !this.modalShow
    },
    prijava() {
      axios
        .post(
          'http://127.0.0.1:3333/login',
          { email: this.email, password: this.password },
          {
            'Content-Type': 'application/json;charset=UTF-8',
            'Access-Control-Allow-Origin': '*',
          }
        )
        .then((res) => {
          localStorage.setItem('token', res.data.token)
          this.empty()
          window.location.reload(true)
        })
        .catch((err) => {
          console.log('AXIOS ERROR: ', err)
        })
    },
  },
}
</script>
