<template>
	<v-container grid-list-xl>
		<v-layout row wrap>
			<v-flex xs10 offset-xs1>
				<h1>Book Finder</h1>
			</v-flex>	
			<v-flex xs8 offset-xs2>
				<v-text-field
					label="Search by by book title or author.."
					single-line
					outline
					clearable
					@keyup.enter="searchHandler"
				>
				</v-text-field>
			</v-flex>
			<template v-if="!loading">
				<v-flex v-for="item in book.items" :key="item.id" xs6>
					<v-card class="rounded-card">
						<v-layout>
							<v-flex xs5>
								<img :src="item.volumeInfo.imageLinks.thumbnail">
							</v-flex>
							<v-flex xs7 class="bookDetails">
								<v-card-title primary-title>
									<h2>{{ item.volumeInfo.title }}</h2></v-card-title>
									<div>
										<p><strong>By:</strong></p>
										<ul>
											<li v-for="author in item.volumeInfo.authors">{{ author }}</li>
										</ul>
										<p><strong>Published by:</strong></p>
										<p>{{ item.volumeInfo.publisher }}</p>
									</div>
									<div>
										<v-btn color="blue-gray" small depressed left v-on:click="toSite(item.volumeInfo.previewLink)">See this book</v-btn>
									</div>
							</v-flex>
						</v-layout>
					</v-card>
				</v-flex>
			</template>
		</v-layout>
	</v-container>
</template>

<script>
export default {
	data() {
		return {
			url: 'https://www.googleapis.com/books/v1/volumes?q=',
			book: {},
			loading: false,
			error: false
		}
	},
	
	methods: {
		findBook: function(title) {
			this.error = false;
			this.loading = true;
			
			fetch(this.url + title).then(res => res.json()).then(data => {
				this.book = data;
				if (this.book.error) this.error = true;
				this.loading = false;
			}).catch(err => {
				this.error = true;
				this.loading = false;
			});
		},
		
		searchHandler: function(e) {
			if (e.target.value) {
				this.findBook(e.target.value);
			}
		},
		
		toSite: function(e) {
			window.location.href = e;
		}
	}
}
</script>

<style>
$base-spacing: 12px;
$poster-width: 120px;

body {
	background: lightgray;
}

h1 {
	text-align: center;
}

h2, p, ul {
	padding: 0;
	margin: 0;
	line-height: 1.2;
}

p, li {
	margin-left: 20px;
	font-size: 0.9em;
	line-height: 1.5;
}

p + ul {
	margin-top: -$base-spacing + 2px;
}

img {
	position: relative;
	display: block;
	align-self: center;
	height: 100%;
	margin: auto;
}

li {
	display: inline-block;
}

v-btn {
	padding: 10px;
}

.bookDetails {
	padding: 0;
	margin: 0;
	position: relative;
}

.rounded-card {
	border-radius: 0.8em;
	margin: 10px;
	height: 100%;
}
</style>