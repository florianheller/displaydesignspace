<!-- 
	A component that allows to apply a series of filters to a dataset and visualize it. 
	C2019 Florian Heller  (florian<at>uhasselt.be)
	License: MIT
-->
<template>
<div id="DataFilter">
	<div id="filters">
		<fieldset 
			v-for="(filter, index) in filters"
			v-bind:key="index"
			:id="'filter-'+index"
			v-bind:class="{ active: isFilterActive(filter) }"
		>	
		<legend>{{filter.key | capitalize }}</legend>
			<input :id="'filter-'+index+'-enable'" type="checkbox" :value="index" v-model="activeFilters">
			<ul	class="segmented-control">
				<li v-for="(value, vindex) in filter.values" :key="vindex" class="segmented-control__item">
					<input :id="index+vindex+value" v-bind:type="filter.multipleSelection ?  'checkbox' : 'radio'"  :value="value" v-model="filter.filterValues" class="segmented-control__input">
					<label class="segmented-control__label" :for="index+vindex+value">{{value | capitalize}}</label>
				</li>
			</ul>
<!-- 			<div>{{subFiltersForFilter(filter)}}</div> -->
		</fieldset>
	</div>
<!-- <span>Active filters: {{ activeFilters }}</span>
 --><ul class="userWrap">
	<li 
	v-for="(entry, index) in filteredData"
	:key="index"
	:item="entry"
	class="user"
	v-on:click="$root.$emit('did-select-item',entry);"
	>
		<h2 class="title">{{ entry.name }}</h2>
		<span class="language"><strong>{{ entry.title }}</strong></span>
	</li>
</ul> 
</div>
</template>

<script>
	// The URL with the data to be shown and filtered
	// Will be fetched asynchronously which might fail (CORB)
	// Place the file in the public directory of your Vue.js project to be served locally
	const apiURL = "./data/data.json";

	/* A filter has the following structure:
	 * filter {
		key: String, 		// The field in the dataset which will be used to filter
		values: [],  		// the possible values for this key
		filterValues: [],	// the  values that are allowed for this key.
		multipleSelection: bool, // if multiple values are allowed, these will be connected through OR
	 }
	 */

	const wearableFilters = [
		{
		'key': 'placement',
		'values': ["body", "head", "torso", "waist", "legs", "foot", "hand", "arm", "wrist"],
		'filterValues': [], 
		'multipleSelection': true,
		},
		{
		'key': 'type',
		'values': ["Accessoires", "Clothing", "Skin & Body"],
		'filterValues': [], 
		'multipleSelection': false,
		'subfilters': [] // Subfilters only work with multipleSelection: false
		},
		{
		'key': 'audience',
		'values': ["Public", "Intermediate", "Private"],
		'filterValues': [], 
		'multipleSelection': true,
		}
	]; //End WearableFilters 

	const subFilters = [
	{
		'parent': 'type',
		'parentValue': 'Accessoires', 
		'key': 'subtype', 		
		'values': ["Eyewear", "Headwear", "Chains & Necklaces", "Watches & Bracelets", "Shoes", "Bags", "Straps"],  		
		'filterValues': [],	
		'multipleSelection': false 
	},
	{
		'parent': 'type',
		'parentValue': 'Clothing', 
		'key': 'subtype', 		
		'values': ["Shirts & Tops", "Jackets & Suits", "Notions", "Pants & Skirts", "Dresses"],  		
		'filterValues': [],	
		'multipleSelection': false 
	},
	{
		'parent': 'type',
		'parentValue': 'Skin & Body', 
		'key': 'subtype', 		
		'values': ["Skin", "Hair", "Face & Neck", "Arms", "Hands & Fingers", "Legs"],  		
		'filterValues': [],	
		'multipleSelection': false 
	}];
	// Alternative check https://gist.github.com/jherax/f11d669ba286f21b7a2dcff69621eb72
	// Based on https://stackoverflow.com/questions/44590352/filter-by-multiple-keys-and-values-javascript
	const useConditions = search => a => Object.keys(search).every(
		k => a[k] === search[k] ||
		Array.isArray(search[k]) && search[k].includes(a[k]) ||
		Array.isArray(search[k]) && Array.isArray(a[k]) && search[k].some(r => a[k].includes(r)) ||
		typeof search[k] === 'object' && +search[k].min <= a[k] &&  a[k] <= +search[k].max ||
		typeof search[k] === 'function' && search[k](a[k])
		);

	export default {
		name: "DataFilter",
		data: function() {
			return {
				filters: wearableFilters,		// The list of all possible filters, will be used later when the user can dynamically create a filter set
				activeFilters: [],  // The active filters 
				data: []			// The data this component works on
			};
		},
		created() {
			fetch(apiURL)
			.then(res => res.json())
			.then(res => (this.data = res, this.$root.$emit('data-is-loaded', this.data)))
			// eslint-disable-next-line no-console
			.catch(error => console.log(error));
		},
		mounted() {
			this.$root.$emit('body-filter', wearableFilters[0]);

			this.$root.$on('body-area-selected', (a) => {
				// Get the ID of the area filter 
				let filter = this.filters.find(element => element.key == 'placement');
				let area = a;
				if (area == "left-arm" || area == "right-arm") {
					area = "arm"
				}
				if (filter) {
					filter['filterValues'].push(area);
					this.activeFilters.push(this.filters.indexOf(filter));
				}
			});

		},
		methods: {
			subFiltersForFilter(filter) {
				return subFilters.filter( f => f.parent == filter.key && filter.filterValues.length > 0 && f.parentValue == filter.filterValues)
			},
			isFilterActive: function(filter) {
				return this.activeFilters.includes(this.filters.indexOf(filter));
			},
		},
		computed: {
			filteredData: function () {
				// Collect all filterkeys, and their possible values
				let filterList = {};
				for (let index of this.activeFilters) {
					filterList[this.filters[index]['key']] = this.filters[index]['filterValues'];
				}
				return this.data.filter(useConditions(filterList));
			},
		},
	};
</script>

<style scoped>
#DataFilter {
	width:50%;
	margin-right:30%;
	margin-left:20%;
}
/* The filter part */
[type="radio"]{
  display: none;
  z-index: 5;
  position: relative;
}

fieldset {
	padding: 0px 1em;
	text-align: left;
}


.active {
	border-color: #F00 ;
}

.segmented-control {
    display: inline-table;
    width: 90%;
    margin: 0.25em 1em;
    padding: 0;
}

.segmented-control__item {
    display: table-cell;
    margin: 0;
    padding: 0;
    list-style-type: none;
}

.segmented-control__input {
    position: absolute;
    visibility: hidden;
}

.segmented-control__label {
    display: block;
    margin: 0 -1px -1px 0; /* -1px margin removes double-thickness borders between items */
    padding: 1em .25em;

    border: 1px solid #ddd;

    font: 14px/1.5 sans-serif; 
    text-align: center;  

    cursor: pointer;
}

.segmented-control__label:hover {
    background: #fafafa;
}

.segmented-control__input:checked + .segmented-control__label {
    background: #eee;
    color: #333; 
}

/* Data UL */
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
