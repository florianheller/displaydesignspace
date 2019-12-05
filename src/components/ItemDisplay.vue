<template>
	<div id="itemDisplay">
		<h1 class="itemTitle">{{this.item.title}}</h1>
		<ul v-for="(author,index) in item.authors" 
			:key="index"
			id="authorList">
			<li class="author">{{author}}</li> 
		</ul>
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
				<td><a :href="item.url">{{item.url}}</a></td>
			</tr>
			<tr>
				<td class="detail">Placement</td>
				<td>{{item.placement | capitalize}}</td>
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
				<td>{{item.audience}}</td>
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
</style>
