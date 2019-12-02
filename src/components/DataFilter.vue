<template>
<div id="DataFilter">
<div id="filters" 
	v-for="(filter, index) in filters"
	v-bind:key="index"
	>
	<input :id="'filter'+index+'enable'" type="checkbox" name="radioBtn" :value="index" v-model="activeFilters">
	<fieldset v-for="(value, index) in filter.values" :key="index" >
	<input :id="value" type="radio" name="radioBtn" :value="value" v-model="filter.filterValues">
	<label class="labels" :for="value">{{value}}</label>
	</fieldset>
<span>Checked names: {{ filter.filterValues }}</span>
</div>
<span>Active filters: {{ activeFilters }}</span>
<ul class="userWrap">
	<li 
	v-for="(entry, index) in filteredData"
	:key="index"
	:item="entry"
	class="user"
	>
		<h2 class="title">{{ entry.name }}</h2>
		<span class="language">Primary Language: <strong>{{ entry.mainLanguage }}</strong></span>
	</li>
</ul> 
</div>
</template>

<script>
	/* A filter has the following structure:
	 * filter {
		key: String, 		// The field in the dataset which will be used to filter
		values: [],  		// the possible values for this key
		filterValues: [],	// the  values that are allowed for this key.
		mutipleSelection: bool, // if multiple values are allowed, these will be connected through OR
	 }
	 */
	// Alternative check https://gist.github.com/jherax/f11d669ba286f21b7a2dcff69621eb72
	const useConditions = search => a => Object.keys(search).every(
		k => a[k] === search[k] ||
		Array.isArray(search[k]) && search[k].includes(a[k]) ||
		typeof search[k] === 'object' && +search[k].min <= a[k] &&  a[k] <= +search[k].max ||
		typeof search[k] === 'function' && search[k](a[k])
		);

	export default {
		name: "DataFilter",
		data: function() {
			return {
				filters: [{
					'key': 'mainLanguage',
					'values': ["JavaScript", "Python", "PHP", "Java"],
					'filterValues': [], 
					'mutipleSelection': false
					},
					{
					'key': 'isActive',
					'values': [true, false],
					'filterValues': [], 
					'mutipleSelection': true
					}],		// The list of all possible filters, will be used later when the user can dynamically create a filter set
				activeFilters: [],  // The active filters 
				data: []			// The data this component works on
			};
		},
		created() {
			//var apiURL = "https://next.json-generator.com/api/json/get/4JCnNiTCr";
			var apiURL = "http://www2.heller-web.net/IDDS/data/data.json";
			fetch(apiURL)
			.then(res => res.json())
			.then(res => (this.data = res))
			// eslint-disable-next-line no-console
			.catch(error => console.log(error));
		},
		computed: {
			filteredData: function () {
				// Collect all filterkeys, and their possible values
				let filterList = {};
				for (let index of this.activeFilters) {
					filterList[this.filters[index]['key']] = this.filters[index]['filterValues'];
				}
				return this.data.filter(useConditions(filterList));
			}
		},
	};
</script>

<style scoped>
#DataFilter {
    position:absolute;
    left:440px;
    top: 60px;
}
button {
  background: #74b6cc;
  border: none;
  color: #fff;
  padding: 10px;
  margin: 5px;
}
button.active {
  background: #0089ba;
}
fieldset {
	display:inline;
}
.userWrap {
  list-style-type: none;
  padding: 2%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  flex-direction: row;
}
.user {
  padding: 10px;
  margin: 1% 0;
  border: 1px solid #ddd;
  border-radius: 3px;
  width: 30%;
  text-align: left;
}
h2.title {
  font-size: 1.3rem;
  font-weight: bold;
  margin: 0;
}
.language {
  display: block;
  font-size: 0.9rem;
}
</style>
