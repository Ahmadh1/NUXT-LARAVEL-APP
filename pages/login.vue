<template>
	<div class="container mt-5">
		<div class="row">
			<div class="col-xs-8 col-sm-8 col-md-8 col-lg-8 offset-2">
        <div class="card">
          <div class="card-header">
            <h2>Login</h2>
          </div>
          <div class="card-body">
            <form @submit.prevent="submit">
           <div class="form-group">
              <label for="exampleInputEmail1">Email address</label>
              <input type="email" v-model.trim="form.email" class="form-control" id="exampleInputEmail1" placeholder="Enter email" autofocus>
              <small id="emailHelp" v-if="errors.email" class="form-text text-danger">{{ errors.email[0] }}</small>
            </div>
          <div class="form-group">
            <label for="exampleInputPassword1">Password</label>
            <input v-model.trim="form.password" type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
            <small id="emailHelp" v-if="errors.password" class="form-text text-danger">{{ errors.password[0] }}</small>
          </div>
            <button type="submit" class="btn btn-outline-danger form-control">Login</button>
        </form>
          </div>
          <div class="card-footer">
          <p>Don't have an account? <nuxt-link to="/register" class="text-danger">Register here</nuxt-link></p>
        </div>
        </div>
			</div>
		</div>
	</div>	
</template>

<script>
    export default {
      middleware: ['guest'],
      data() {
        return {
          form: {
            email: '',
            password: ''
          }
        }
      },
      methods: {
          async submit() {
          await this.$auth.loginWith("local", {
            data: this.form
          })

          this.$router.push({
            path: this.$route.query.redirect || '/dashboard'
          })
        }
      }
    }
</script>