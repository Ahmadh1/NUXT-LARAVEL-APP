<template>
	<div class="container">
		<div class="row">
			<div class="col-xs-10 col-sm-10 col-md-10 col-lg-10 offset-1">
				<div class="card border-dark mt-5">
				  <div class="card-body text-dark">
				  	<h3>{{ topic.title }}</h3>
				  	<p class="text-muted">{{ topic.created_at }} by {{ topic.user.name }}</p>
				  	<!-- internal card -->
				  	<div v-for="(content, index) in topic.posts" :key="index" class="ml-5 mb-2">
				  		<div class="card">
				  			<div class="card-header text-muted">
				  				<p class="pull-left">By {{ content.user.name }}</p>
				  				<p class="pull-right">{{ content.created_at }}</p>
				  			</div>
				  			<div class="card-body">
				  				<p class="card-text">{{ content.body }}</p>
				  				<div class="float-right" v-if="authenticated">
									<div v-if="user.id ===content.user.id">
										<nuxt-link :to="{name: 'topics-posts-edit', params: {id: $route.params.id, body: content.id}}">
											<button class="btn btn-outline-danger btn-sm fa fa-pencil"></button>
										</nuxt-link>
										<button class="btn btn-outline-danger btn-sm fa fa-trash-o" @click="deletePost(content.id)"></button>
									</div>
								</div>
				  			</div>
				  		</div>
				  	</div>
				  </div>
				  <div class="card-footer">
				  	<nuxt-link to="/topics">Back to All Posts</nuxt-link>
				  </div>
				</div>
			</div>
		</div>
		<div class="row" v-if="authenticated">
			<div class="col-xs-10 col-sm-10 col-md-10 col-lg-10 offset-1">
				<div class="card border-primary mt-5 mb-5">
				  <div class="card-header">Contribute in this topic</div>
				  <div class="card-body text-primary">
				    <h5 class="card-title">Enter text</h5>
				    <form @submit.prevent="create">
				    	<div class="form-group">
				    		<textarea placeholder="Write something..." class="form-control" rows="3" v-model="body"></textarea>
				    		<small class="form-text text-danger" v-if="errors.body">{{ errors.body[0] }}</small>
				    	</div>
				    	<div class="form-group">
				    		<button class="btn btn-success">Post</button>
				    	</div>
				    </form>
				  </div>
				</div>
			</div>
		</div>
	</div>
</template>

<script type="text/javascript">
	export default {
		data() {
			return {
				topic: '',
				body: ''
			}
		}, 
		async asyncData({$axios, params}) {
			const {data} = await $axios.$get(`/topics/${params.id}`)
			return {
				topic: data
			}
		},
		methods: {
			async create() {
				var success = await this.$axios.$post(`/topics/${this.$route.params.id}/posts`, { body: this.body })
					  .catch(error => {
					    // handle error
					  })
						if (success) {
							this.$router.push('/topics')
						} else { 
							// show errors 
						}
			},
			async deletePost(id) {
				await this.$axios.$delete(`/topics/${this.$route.params.id}/posts/${id}`)
				this.$router.push('/')
			}
		}
	}
</script>