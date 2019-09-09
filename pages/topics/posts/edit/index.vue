<template>
	<div class="container">
		<div class="row">
			<div class="col-xs-8 col-sm-8 col-md-8 col-lg-8 offset-2 mt-5">
				<h3 class="mb-5">Update Post</h3>
				<hr>
				<form @submit.prevent="update">
					<div class="form-group">
						<textarea v-model="post.body" rows="5" class="form-control"></textarea>
						<small class="form-text text-danger" v-if="errors.body">{{ errors.body[0] }}</small>
					</div>
					<div class="form-group">
						<button type="submit" class="btn btn-outline-dark form-control">Update</button>
					</div>
				</form>
				<nuxt-link to="/topics" class="btn btn-outline-dark">back to topics</nuxt-link>
			</div>
		</div>
	</div>
</template>
<script type="text/javascript">
	export default {
		data() {
			return {
				post: {
					body: ''
				}
			}
		},
		async asyncData({$axios, params}) {
			const {data} = await $axios.$get(`/topics/${params.id}/posts/${params.body}`)
			return {
				post: data
			}
		},
		methods: {
			async update() {
				await this.$axios.patch(`/topics/${this.$route.params.id}/posts/${this.$route.params.body}`, {
					body: this.post.body
				})
					// redirect
					this.$router.push('/topics')		
					
				
			},

		}
	}
</script>