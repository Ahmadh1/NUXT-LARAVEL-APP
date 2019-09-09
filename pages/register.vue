<template>
	<div class="container mt-5">
		<div class="row">
			<div class="col-xs-8 col-sm-8 col-md-8 col-lg-8 offset-2">
				<div class="card">
					<div class="card-header">
						<h2>Register</h2>		
					</div>
					<div class="card-body">
						<form @submit.prevent="submit">
					<div class="form-group">
    					<label for="exampleInputEmail1">Name</label>
    					<input type="text" v-model.trim="form.name" class="form-control" id="exampleInputEmail1" placeholder="Enter Name" autofocus>
    					<small id="emailHelp" v-if="errors.name" class="form-text text-danger">{{ errors.name[0] }}</small>
  					</div>
					 <div class="form-group">
    					<label for="exampleInputEmail1">Email address</label>
    					<input type="email" v-model.trim="form.email" class="form-control" id="exampleInputEmail1" placeholder="Enter email">
    					<small id="emailHelp" v-if="errors.email" class="form-text text-danger">{{ errors.email[0] }}</small>
  					</div>
					<div class="form-group">
						<label for="exampleInputPassword1">Password</label>
						<input type="password" v-model.trim="form.password" class="form-control" id="exampleInputPassword1" placeholder="Password">
						<small id="emailHelp" v-if="errors.password" class="form-text text-danger">{{ errors.password[0] }}</small>
					</div>
  					<button type="submit" class="btn btn-outline-dark form-control">Register</button>
				</form>
					</div>
					<div class="card-footer">
						<p>Already have an account <nuxt-link to="/login">login</nuxt-link></p>
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
					name: '',
					 email: '',
					  password: ''
				}
			}
		},
		methods: {
			async submit() {
				// making request
				await this.$axios.$post('register', this.form)
				// logging the user
				await this.$auth.loginWith('local', {
					// with email & password
					data: {
						email: this.form.email,
						password: this.form.password
					}
				})
				// redirect the user
				this.$router.push({
	            	path: this.$route.query.redirect || '/dashboard'
	          })
			}
		}
	}
</script>