<template>
	<div class="conatiner">
		<div class="row">
			<div class="col-xs-8 col-sm-8 col-md-8 col-lg-8 offset-2">
				<h2 class="mt-5">Latest Topics</h2>
				<div v-for="(topic, index) in topics" :key="index">
					<div class="card mt-5 mb-5">
						<div class="card-header">
							<div class="card-title">
								<h3 class="float-left"><nuxt-link :to="{name: 'topics-id', params: {id: topic.id}}">{{ topic.title }}</nuxt-link></h3>
								<span class="float-right text-muted">{{ topic.created_at }} by {{ topic.user.name }}</span>
								<br>
								<div class="float-right" v-if="authenticated">
									<div v-if="user.id ===topic.user.id">
										<nuxt-link :to="{name: 'topics-edit', params: {id: topic.id}}">
											<button class="btn btn-outline-danger btn-sm fa fa-pencil"></button>
										</nuxt-link>
										<button class="btn btn-outline-danger btn-sm fa fa-trash-o" @click="deleteTopic(topic.id)"></button>
									</div>
								</div>
							</div>
						</div>
						<div class="card-body">
							<div v-for="(content, index) in topic.posts" :key="index" class="ml-5">
								<p>{{ content.body }}</p>
								<span class="float-right text-danger">
									{{ content.created_at }} by {{ content.user.name }}
								</span>
								<div class="btn btn-success btn-sm fa fa-thumbs-up" @click="likePost(topic.id, content)">
									<span class="badge">{{content.like_count}}</span>
								</div>
							</div>
							<hr>
						</div>
				</div>
				</div>
				<nav>
					<ul class="pagination justify-content-center">
						<li class="page-item" v-for="(key, value) in links">
							<a @click="loadMore(key)" href="#" class="page-link">{{ value }}</a>
						</li>
					</ul>
				</nav>
			</div>
		</div>
	</div>

</template>
<script>
	export default {
		data() {
			return {
				topics: [],
				links: []
			}
		},
		async asyncData({$axios}) {
			let {data, links} = await $axios.$get('/topics')
			// console.log(data)
			return {
				topics: data,
				links
			}
		},
		methods: {
			async loadMore(key) {
				let {data} = await this.$axios.$get(key)
				return this.topics = {...this.topics, ...data}
			},
			async deleteTopic(id) {
				// console.log(id)
				await this.$axios.$delete(`/topics/${id}`)
				this.$router.push('/')
			},
			async deleteTopic(id) {
		        await this.$axios.$delete(`/topics/${id}`)
		        this.$router.push('/')
		    },
		    async likePost(topicId, content) {
		    	const userFromVuex = this.$store.getters["auth/user"];
		    	if (userFromVuex) {
		          // cant like your own post
		          if (userFromVuex.id === content.user.id) {
		            alert('You cant like your own post')
		          }
		          // if user have already liked
		          if (content.users) {
		            if (content.users.some(user => user.id === userFromVuex.id)) {
		              alert('You have already liked this post')
		            } else {
		              await this.$axios.$post(`/topics/${topicId}/posts/${content.id}/likes`)
		              let {data, links} = await this.$axios.$get(`/topics`)
		              this.topics = data
		              this.links = links
		            }
		          }
		        } else {
		          alert('Please login')
		          this.$router.push('/login')
		        }
		    }
	}
	}
</script>