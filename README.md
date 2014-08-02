&lt;sortable-table&gt;
================

Polymer Web Component that generates a sortable &lt;table&gt; from JSON.

Maintained by [Steven Skelton](https://github.com/stevenrskelton)

[Additional Documentation on Table Sorting](http://stevenskelton.ca/sortable-table-with-polymer-web-components/)

[Additional Documentation on Templates](http://stevenskelton.ca/advanced-uses-polymer-templates/)

## Live Examples

> [Data Formats](http://files.stevenskelton.ca/sortable-table/examples/data-formats.html)

> [DOM Elements](http://files.stevenskelton.ca/sortable-table/examples/dom-elements.html)

> [Auto-Generated Columns](http://files.stevenskelton.ca/sortable-table/examples/autogenerated-columns.html)

> [Row Templates](http://files.stevenskelton.ca/sortable-table/examples/row-templates.html)

> [Row Editor](http://files.stevenskelton.ca/sortable-table/examples/row-editor.html)

> [Selected Rows, Multi-Select](http://files.stevenskelton.ca/sortable-table/examples/selected-rows.html)

> [Dynamically Changing Columns and Templates](http://files.stevenskelton.ca/sortable-table/examples/dynamic-columns.html)

> [Paging, Top-N Rows](http://files.stevenskelton.ca/sortable-table/examples/paging.html)

> [Themes](http://files.stevenskelton.ca/sortable-table/examples/themes.html)

## Usage

1. Add the library using the Javascript package manager [Bower](http://bower.io/):

	```bower install --save sortable-table```

2. Import Web Components' polyfill:

	```html
	<script src="bower_components/platform/platform.js"></script>
	```

3. Import Custom Element:

	```html
	<link rel="import" href="bower_components/sortable-table/sortable-table.html">
	```

4. Start using it!

	Start simple and use DOM to configure the grid:

	```html
	<sortable-table>
		<sortable-column>fruit</sortable-column>
		<sortable-column>alice</sortable-column>
		<sortable-column>bill</sortable-column>
		<sortable-column>casey</sortable-column>
		[
			[ "apple", 4, 10, 2 ],
			[ "banana", 0, 4, 0 ],
			[ "grape", 2, 3, 5 ],
			[ "pear", 4, 2, 8 ],
			[ "strawberry", 0, 14, 0 ]
		]
	</sortable-table>
	```

	Or use Javascript attributes:

	```html
	<sortable-table columns='["fruit","alice","bill","casey"]' data='[
			[ "apple", 4, 10, 2 ],
			[ "banana", 0, 4, 0 ],
			[ "grape", 2, 3, 5 ],
			[ "pear", 4, 2, 8 ],
			[ "strawberry", 0, 14, 0 ] ]'>
	</sortable-table>
	```

	Or take advanced control with custom templates and 2-way data binding:

	```html
	<sortable-table data="{{data}}" columns="{{columns}}">
		<!-- add templates here -->
	</sortable-table>
	```

## Options

Attribute				| Options		| Default									| Description
---						| ---			| ---										| ---
`data`	 				| *array*		| `[]`										| Data rows
`columns`				| *array*		| `[]`										| Columns to display, with options. If `[]`, columns will be computed from `data`.  _See_ [Columns](#columns)
`sortColumn`			| *string*		| `null`									| Current sorted `column.name`
`sortDescending`		| *boolean*		| `false`									| Current sorted column sort direction
`checkbox`				| *boolean*		| `false`									| Renders a checkbox column as first column, facilitating row selection.
`disableColumnMove`		| *boolean*		| `false`									| Disables columns from being drag-and-dropped into different positions.  Drag-and-drop will be automatically disabled if entire row templates (`rowTemplate` or `rowEditorTemplate`) are used. CSS styling using _:nth-of-type_ will likely break unless this is set to true.
`rowSelection`			| *boolean*		| `false`									| Enable user interactive row selection
`multiSelect`			| *boolean*		| `false`									| Multiple rows can be selected
`selected`				| *object*		| `null`									| Element of `data` (if `!multiSelect`)
`selected`				| *array*		| `[]`										| Elements of `data` (if `multiSelect`)
`selectedRowStyle`		| *string*		| `background-color:` `rgba(0,96,200,0.2);`	| CSS style to apply to `selected` row
`pageSize`				| *int*			| `-1`										| Maximum number of records to display, `-1` is all records.
`page`					| *int*			| `1`										| Number of pages to skip, `pageSize * (page-1)` records skipped.
`rowTemplate`			| *string*		| `null`									| _See_ [Table § rowTemplate](#table--rowtemplate)
`rowEditorTemplate`		| *string*		| `null`									| _See_ [Table § rowEditorTemplate](#table--roweditortemplate)
`cellTemplate`   		| *string*		| `null`									| _See_ [Table § cellTemplate](#table--celltemplate)
`headerTemplate`		| *string*		| `null`									| _See_ [Table § headerTemplate](#table--headertemplate)
`theme`					| *string*		| `null`									| _See_ [Themes](#themes)

### Data

Input format for `data` rows is an array of objects, where data for each column is a property of the row object.

### Columns

Attribute  				| Options		| Default									| Description
---						| ---			| ---										| ---
`name`	  				| *string*		| _required_								| Name of row property
`title`	  				| *string*	   	| `name`									| Text to display in column header
`formula`				| *function*	| `null`									| Single parameter `row`, return will override any value for property in `data`, as well as be used for sorting
`cellTemplate`   		| *string*		| `null`									| _See_ [Column § cellTemplate](#column--celltemplate)
`headerTemplate`		| *string*		| `null`									| _See_ [Column § headerTemplate](#column--headertemplate)
`footerTemplate`		| *string*		| `null`									| _See_ [Column § footerTemplate](#column--footertemplate)

### Templates

All templates must be nested inside the `<sortable-table>` tag to be accessible to the polymer element.

Any filter used (eg: `sum` in a following example) must be a member of `PolymerExpressions.prototype`.
See the [Polymer Filters](#polymer-filters) section for more details.

As always, only a very limited subset of Javascript is allowed within `{{ }}` expressions.
See the [Polymer documentation](http://www.polymer-project.org/docs/polymer/expressions.html) on Expression syntax.

#### Table Scoped Templates

These are passed as attributes to the `sortable-table` element, and act across all rows and columns in the table - unless overwritten by a corresponding [Column Scoped Template](#column-scoped-templates).

##### Table § rowTemplate

Renderer for contents of `<tr></tr>` row.

Template Variable		|	Description
---						|	---
`{{record}}`			|	JSON object
`{{record.selected}}`	|	Boolean indicating this row is contained in `selected`
`{{record.edit}}`		|	Boolean indicating this row is in edit mode
`{{record.fields}}`		|	JSON map with keys for each `column.name`.  Values are JSON objects containing computed `value` for the cell, the `row` and `column`
`{{record.row}}`		|	Row in `data`

Example of a `rowTemplate` that prints out column values directly from the raw `row` of the `data` array.
This is useful for rows that need to recalculate when values change:

```html
<template id="myRowTemplate_1">
	<td><input type="text" value="{{record.row.number}}"></td>
	<td>{{record.row.price}}</td>
	<td>{{record.row.number * record.row.price}}</td>
</template>
```

Example of a `rowTemplate` that prints out column values based on internally calculated field values.
The `field` names (ie: alice, bill, casey) are the names of the table columns, this is useful where `column` formulas are applied:

```html
<template id="myRowTemplate_2">
	<td>{{record.fields.alice.value}}</td>
	<td>{{record.fields.bill.value}}</td>
	<td>{{record.fields.casey.value}}</td>
</template>
```

Example of a `rowTemplate` that uses a template (and a filter `toKeyValueArray` that turns an object into an array):

```html
<template id="myRowTemplate_3">
	<template repeat="{{kv in record.fields | toKeyValueArray}}" bind>
		<td>{{kv.value.value}}</td>
	</template>
</template>
```

##### Table § rowEditorTemplate

Row template to use for a row in its user editing state.  Only 1 row can be in the editing state at a time.
Renderer for contents of `<tr></tr>` row when in edit mode (_double clicked_).
Similiar to [Table § rowTemplate](#table--rowtemplate).

##### Table § cellTemplate

Renderer for entire `<td></td>` cell. Will be overwritten if `columns` specifies a `cellTemplate`.

Template Variable		|	Description
---						|	---
`{{column}}`			|	Current column from `columns`
`{{value}}`				|	Computed value for cell
`{{row}}`				|	Current row from `data`

Example of a `cellTemplate` that displays an image beside the column value:

```html
<template id="myCellTemplate">
	<td>
		<img src="{{row.img}}" alt="{{row.title}}"/>{{value}}
	</td>
</template>
```
__Note:__  Normally `row[column.name] == value`, but `value` can be manually set by specifying a `formula`. This is useful if `value` won't sort correctly but you need access to the original value.

##### Table § headerTemplate

Renderer for entire `<th></th>` cell. Access to `{{column}}`.  Will be overwritten if specified in `columns`.

Example of a `headerTemplate` using images to indicate sorting:

```html
<template id="myHeaderTemplate">
	<th>
		{{!(column.title) ? column.name : column.title}}
		<img hidden?="{{!(sortColumn==column.name && sortDescending)}}" alt="up" />
		<img hidden?="{{!(sortColumn==column.name && !sortDescending)}}" alt="down" />
	</th>
</template>
```

##### Table § footerTemplate

Renderer for an additional row which spans all columns at the bottom of the table.  This is useful for paging buttons.
There is a built in template called `defaultPaging` that can be used, or a different one can be specified.
This is independent to [Column § footerTemplate](#column--footertemplate) as they serve different purposes and can be used concurrently.

Template Variable		|	Description
---						|	---
`{{page}}`				|	Current `page` of data
`{{pageSize}}`			|	Number of records per page
`{{previousPage}}`		|	Function to move to the previous page (only if one exists)
`{{previousPage}}`		|	Function to move to the next page (only if one exists)
`{{data}}`				|	`data`

Example of a `footerTemplate` that allows the user to traverse between pages.

```html
<template id="defaultPaging">
	<div style="text-align:center">
		<div on-click="{{previousPage}}" style="
			float:left;
			cursor:pointer;
			color:{{page==1 ? '#666':'#fff'}}
		">
			Prev
		</div>
		<div on-click="{{nextPage}}" style="
				float:right;
				cursor:pointer;
				color:{{page * pageSize > data.length ? '#666':'#fff'}}
		">
			Next
		</div>
		Page {{page}} of {{
			(data.length - (data.length % pageSize) + pageSize*1) / pageSize
		}}
	</div>
</template>
```

#### Column Scoped Templates

These are defined within the `columns` attribute at a per-column scope.

##### Column § cellTemplate

Renderer for entire `<td></td>` cell.
Overrides table's global cell template for a specific column.
See [Table § cellTemplate](#table--celltemplate).


##### Column § headerTemplate

Renderer for entire `<th></th>` cell. Access to `{{column}}`.
Overrides table's global header template for a specific column.
See [Table § cellTemplate](#table--celltemplate).

##### Column § footerTemplate

Renderer for entire `<td></td>` cell. If no columns specify a `footerTemplate` the additional footer row will be omitted.
If some but not all columns specify a template, the columns without a template specified will render an empty cell.

Template Variable		|	Description
---						|	---
`{{column}}`			|	Current column from `columns`
`{{values}}`			|	Array of all computed values for cells in the current column
`{{rowValues}}`			|	Array of all values from `data` for cells in the current column.  Since a column may use a function to compute its value, there may not be any bound values: so this array may be empty.

Two convinience polymer filters are included, `sum` and `average` which compute the numerical sum and average.  Custom filters can also be used, see [Polymer Filters](#polymer-filters).

Example of a `footerTemplate` that computes the sum of a column using a filter named `sum`:

```html
<template id="sumTemplate">
	<td class="sortable-table-header" style="text-align:right">
		{{values | sum}}
	</td>
</template>
```

## Themes

The component can be themed using CSS by following Polymer's [Shadom DOM piercing syntax](http://www.polymer-project.org/docs/polymer/styling.html#sdcss)
A few full themes are built-in and can be enabled using the `theme` attribute.

Theme					|	Description
---						|	---
						|	Default theme.
extjs					|	Theme modeled after Senca's ExtJs Grid

## Polymer Filters

Referencing the [polymer documentation](http://www.polymer-project.org/docs/polymer/expressions.html#filters), filters can be used in expressions to transform data:
```
{{ expression | filterName([optional parameters]}}
```
To use a filter in one of your _sortable-table_ templates, it must be registered into the _PolymerExpressions_ scope, which is accessible after polymer has loaded:
```javascript
window.addEventListener('polymer-ready', function(){
	PolymerExpressions.prototype.myFilter = function(){ ... }
});
```

If a filter is being used in a `rowTemplate` or `rowEditorTemplate` and references more than one field, it might be useful to pass the `row` as an argument:
```{{ record.row | myFilter }}```
however, this won't automatically observe the individual fields of the `row`.  To tell polymer to watch individual fields, they must all be sent as optional parameters:
```{{ record.row | myFilter(record.row.myField1, record.row.myField2) }}```

The added benefit is that the function can be reused in `rowTemplate`s and as a `column.formula`:
```javascript
function myFilter(row){
	return [your logic here];
}
...
PolymerExpressions.prototype.myFilter = myFilter
...
//in the columns array
{ name:'field1', formula: myFilter }
```

## Todo

- better support for internal row field change observers
- better CSS theming
- integration with IndexedDB
- maybe: max and fixed table sizing / scrolling
- maybe: cell selection
- maybe: figure out how to sort by selected (click on header of checkbox column?)
- maybe: column grouping
- maybe: reload data if individual row fields change

## Bugs

- __Internet Explorer is completely broken until it supports templates__
- header templates not working correctly in Native Support browsers (Chromium)
- header templates suffer slow performance with polyfil (Firefox)

## History

For detailed changelog, check [Releases](https://github.com/stevenrskelton/sortable-table/releases).

## License
[MIT License](http://opensource.org/licenses/MIT) © Steven Skelton