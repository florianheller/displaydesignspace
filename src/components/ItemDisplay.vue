<template>
	<div id="itemDisplay">
		<h1 class="itemTitle">{{this.item.title}}</h1>
		<ul v-for="(author,index) in item.authors" 
			:key="index"
			id="authorList">
			<li class="author">{{author}}</li> 
		</ul>
		<figure v-if="item.imageURL">
			<a :href="imageUrlForItem(item)">
			<img 
			:src="item.imageURL"
			:key="item.name"
			id="thumbnail" >
			</a>
			<figcaption>Image source: {{imageSourceForItem(item)}}</figcaption>
		</figure>
		<table class="details" v-if="item.name">
			<tr>
				<td class="detail">Published</td>
				<td>{{item.published}}</td>
			</tr>
			<tr>
				<td class="detail">Year</td>
				<td>{{item.year}}</td>
			</tr>
			<tr>
				<td class="detail">URL</td>
				<td><a :href="item.url" target="_blank">{{item.url}}</a></td>
			</tr>
			<tr>
				<td class="detail">Placement</td>
				<td>{{item.placement.toString() | capitalize}}</td>
			</tr>
			<tr>
				<td class="detail">Type</td>
				<td>{{item.type}}</td>
			</tr>
			<tr>
				<td class="detail">Subtype</td>
				<td>{{item.subtype}}</td>
			</tr>
			<tr>
				<td class="detail">Audience</td>
				<td>{{item.audience.toString() | capitalize}}</td>
			</tr>
			<tr>
				<td class="detail">Information Density</td>
				<td>{{item.infodensity.toString() | capitalize }}</td>
			</tr>
			<tr>
				<td class="detail">Refresh Rate</td>
				<td>{{item.refreshrate.toString() | capitalize}}</td>
			</tr>
			<tr>
				<td class="detail">Persistence</td>
				<td>{{item.persistence.toString() | capitalize}}</td>
			</tr>
		</table>
	</div>
</template>

<script>
	export default {
	name: "ItemDisplay",
	data: function() {
		return {
			item: {}
		};
	},
	mounted() {
		this.$root.$on('did-select-item', (i) => { this.item = i; 
		});
	},
	methods: {
		imageSourceForItem: function(item) {
			return  (item.imageSourceTitle) ? item.imageSourceTitle : item.url
		},
		imageUrlForItem: function(item) {
			return  (item.imageSource) ? item.imageSource : item.url
		}
	}
	}; //End export
</script>


<style scoped>
	#itemDisplay {
		width: 30%;
	}
	#authorList {
		display: inline table;
		list-style-type: none;
		margin: 0.25em 1em;
		padding: 0;
	}
	#author {
		display: table-cell;
		padding: 0;
	}

	h1.itemTitle {
		font-size: 1.5rem;
		font-weight: bold;
		margin: 0;
	}

	table.details {
		margin:1em;
		text-align: left;
	}
	td.detail {
		font-weight: bold;
	}
	#thumbnail {
		display: block;
		width: 75%;
		margin: 1em auto 0 auto;
	}
	figcaption {
		font-size: 0.6rem;
		width: 75%;
		margin: 0 auto 0 auto;
	}
</style>
