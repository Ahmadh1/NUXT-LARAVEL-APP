<template>
	<div class="container">
		<div class="row">
			<div class="col-xs-8 col-sm-8 col-md-8 col-lg-8 offset-2">
				<h2>{{ user.name }}'s Dashboard</h2>
				<hr>
				<div class="card">
					<div class="card-header">
						<h4 class="card-title">Add new Topic</h4>
					</div>
					<div class="card-body">
						<form @submit.prevent="create">
				           <div class="form-group">
				              <label for="exampleInputEmail1">Title</label>
				              <input type="text" v-model="form.title" class="form-control" id="exampleInputEmail1" placeholder="Title..." autofocus>
				              <small id="emailHelp" v-if="errors.title" class="form-text text-danger">{{ errors.title[0] }}</small>
				           </div>
				          	<div class="form-group">
					            <label for="exampleInputPassword1">Body</label>
					            <textarea v-model="form.body" rows="5" placeholder="Body of the Topic" class="form-control"></textarea>
					            <small id="emailHelp" v-if="errors.body" class="form-text text-danger">{{ errors.body[0] }}</small>
				          	</div>
				            <button type="submit" class="btn btn-outline-danger form-control">Add</button>
				        </form>
					</div>
					<div class="card-footer"></div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		middleware: ['auth'],
		data() {
			return {
				form: {
					title: '',
					body: ''
				}
			}
		},
		methods: {
			async create() {
				// console.log('submitted')
				await this.$axios.$post('/topics', this.form)
				this.$router.push('/topics')
			}
		}
	}
</script>