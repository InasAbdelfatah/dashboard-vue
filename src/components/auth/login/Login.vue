<template>
  <div class="login">
    <h2>{{ $t('auth.welcome') }}</h2>
    <form method="post" action="" @submit.prevent="login()" name="login">
      <div class="form-group">
        <div class="input-group">
          <input type="text" id="email" required="required" v-model="form.email"/>
          <label class="control-label" for="email">
            {{ $t('auth.email') }}
          </label>
          <i class="bar"/>
        </div>
      </div>
      <div class="form-group">
        <div class="input-group">
          <input type="password" id="password" required="required" v-model="form.password"/>
          <label class="control-label" for="password">
            {{ $t('auth.password') }}
          </label>
          <i class="bar"/>
        </div>
      </div>
      <div class="d-flex align--center justify--space-between">
        <button class="btn btn-primary" type="submit">
          {{ $t('auth.login') }}
        </button>
        <!-- <router-link class='link flex-center pl-2 text-center' :to="{name: 'signup'}">
          {{ $t('auth.createAccount') }}
        </router-link> -->
      </div>
    </form>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'login',
  data () {
    return {
      email: '',
      password: '',
      form: {
        'email':'',
        'password':''
      }
    }
  },
  methods: {
    login(){
        axios.post('http://test.pos.local/auth/login' , this.form)
        .then(response => {
        console.log(response.data.response);
        
      })
      .catch(error => {
        console.log(error)
        //this.errored = true
      })
      .finally(() => this.loading = false)

      }
  }
}
</script>

<style lang="scss">
.login {

  @include media-breakpoint-down(md) {
    width: 100%;
    padding-right: 2rem;
    padding-left: 2rem;
    .down-container {
      display: none;
    }
  }

  h2 {
    text-align: center;
  }
  width: 21.375rem;

  .down-container {
    margin-top: 3.125rem;
  }
}
</style>
