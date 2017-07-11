# react-filterable-table
Extendable table with filtering, sorting, paging, and more.


[Demo](https://ianwitherow.github.io/react-filterable-table/example/index.html)

[![NPM](https://nodei.co/npm/react-filterable-table.png?compact=true)](https://npmjs.org/package/react-filterable-table)


## Install
`npm install react-filterable-table`

## Basic usage:

```
let FilterableTable = require('react-filterable-table');

// Data for the table to display; can be anything
let data = [
	{ name: "Steve", age: 27, job: "Sandwich Eater" },
	{ name: "Gary", age: 35, job: "Falafeler" },
	{ name: "Greg", age: 24, job: "Jelly Bean Juggler" },
	{ name: "Jeb", age: 39, job: "Burrito Racer" },
	{ name: "Jeff", age: 48, job: "Hot Dog Wrangler" }
];

// Fields to show in the table, and what object properties in the data they bind to
let fields = [
	{ name: 'name', displayName: "Name", inputFilterable: true, sortable: true },
	{ name: 'age', displayName: "Age", inputFilterable: true, exactFilterable: true, sortable: true },
	{ name: 'job', displayName: "Occupation", inputFilterable: true, exactFilterable: true, sortable: true }
];

<FilterableTable
	namespace="People"
	initialSort="name"
	data={data}
	fields={fields}
	noRecordsMessage="There are no people to display"
	noFilteredRecordsMessage="No people match your filters!"
/>

```

## Props

* `namespace` - `string` - The app saves settings (currently only page size) to localStorage. Namespace prevents overriding settings from other pages/apps where this is used. Use the same namespace across implementations that should share the settings. Default: 'react-filterable-table'
* `className` - `string` - Class name to apply to the component's root &lt;div&gt; element.
* `tableClassName` - `string` - Class name to apply to the component's &lt;table&gt; element.
* `trClassName` - `string` or `function` - Class name to apply to the &lt;tr&gt; elements. If a function is passed, it's called with the `record` and `index` as parameters: `function (record, index)`
* `initialSort` - `string` - The field name on which to sort on initially.
* `initialSortDir` - `bool` - The sort direction to use initially - true is ascending, false is descending. Default: `true`
* `stickySorting` - `bool` - If true, empty values will always sort to the bottom. Default: `false`
* `data` - `array` - Static data to bind to.
* `dataEndpoint` - `string` - If not using a static dataset, this can be used to fetch data with AJAX.
* `onDataReceived` - `fn` - This is called (passing the array of data) before the data is rendered. Any necessary data transformations (date parsing, etc) can be done here.
* `fields` - `array` - Array of `field`s used for building the table. These fields have their own list of props detailed below.
* `noRecordsMessage` - `string` - Message to show when there are no records.
* `noFilteredRecordsMessage` - `string` - Message to show when the user has applied filters which result in no records to show.
* `recordCountName` - `string` - Verbiage to use at the top where it says "X results". For example, "1 giraffe".
* `recordCountNamePlural` - `string` - Verbiage to use when there are more than 1 results (or 0). For example, "3 giraffes".
* `headerVisible` - `bool` - Whether or not to show the header.
* `pagersVisible` - `bool` - Whether or not to show the pagers.
* `topPagerVisible` - `bool` - Whether or not to show the top pager.
* `bottomPagerVisible` - `bool` - Whether or not to show the bottom pager.


## `field` Props

* `name` - `string` - Name of the property on the `data` object
* `displayName` - `string` - Field name as it will appear in the table header. If ommitted, `name` is used.
* `sortFieldName` - `string` - Field to use when sorting if you want to sort using a different value from what's displayed. For example, A+, A, B, C would normally sort as A, A+, B, C. You could have a separate field that maps those values to an integer, then use that field for sorting.
* `inputFilterable` - `bool` - Whether or not this field should be filtered when the user types in the Filter text box at the top.
* `exactFilterable` - `bool` - Whether or not the user can click the field's value to filter on it exactly.
* `sortable` - `bool` - Whether or not the user can sort on this field.
* `visible` - `bool` - Whether or not the field is visible.
* `thClassName` - `string` - Class name of the &lt;th&gt; element.
* `tdClassName` - `string` or `function` - Class name of the &lt;td&gt; element. If a function is passed, it's called with the same parameters as `render` (see below)
* `emptyDisplay` - `string` - Text to show when the field is empty, for example "---" or "Not Set".
* `render` - `fn` - Function called to render the field. Function is passed a `props` object which contains: `props.value` - the value of the field from the `data` object, and `props.field` - this field object ([Demo using field render functions](https://ianwitherow.github.io/react-filterable-table/example-alt/index.html)).


## Example using a `render` function

```
let renderAge = (props) => {
	/*
	 * This props object looks like this:
	 * {
	 *   value:  (value of the field in the data. In this case, it's the person's age.),
	 *   record: (the data record for the whole row, in this case it'd be: { name: "Steve", age: 27, job: "Sandwich Eater" }),
	 *   field:  (the same field object that this render function was passed into. We'll have access to any props on it, including that 'someRandomProp' one we put on there. Those can be functions, too, so we can add custom onClick handlers to our return value.)
	 * }
	 */

	// If they are over 60, use the "blind" icon, otherwise use a motorcycle
	let iconClassName = "fa fa-" + (props.value > 60 ? "blind" : "motorcycle");
	let personName = props.record.name;

	return (
		<span title={personName + "'s Age"}>
			{props.value} <span className={iconClassName}></span>
		</span>
	);
};


let data = [
	...
	{ name: "Steve", age: 27, job: "Sandwich Eater" },
	...
];

let fields = [
	...
	{ name: 'age', displayName: "Age", inputFilterable: true, exactFilterable: true, sortable: true, someRandomProp: "Tacos!", render: renderAge },
	...
]
```

## Building
To build the main library: `gulp build`

To build the example: `gulp example`
