cytoscape.js-edgehandles
========================

![Preview](https://raw2.github.com/cytoscape/cytoscape.js-edgehandles/master/img/preview.png)


## Description

This plugin creates handles on nodes that can be dragged to create edges between nodes.  Because the handle itself is small and shown on `mouseover`, it's not intended to work on touch devices.  To support touch devices, you can use some alternative UI (e.g. [jquery.cytoscape.js-cxtmenu](https://github.com/cytoscape/cytoscape.js-cxtmenu)) to manually start the handle drag state, and then past that point the plugin works the same.



## Dependencies

 * jQuery >=1.4
 * Cytoscape.js >=2.2.3


## Initialisation

You initialise the plugin on the same HTML DOM element container used for Cytoscape.js:

```js

$('#cy').cytoscape({
	/* ... */
});

// the default values of each option are outlined below:
$('#cy').cytoscapeEdgehandles({
	preview: true, // whether to show added edges preview before releasing selection
	handleSize: 10, // the size of the edge handle put on nodes
	handleColor: '#ff0000', // the colour of the handle and the line drawn from it
	handleLineType: 'ghost', // can be 'ghost' for real edge, 'straight' for a straight line, or 'draw' for a draw-as-you-go line
	handleLineWidth: 1, // width of handle line in pixels
	hoverDelay: 150, // time spend over a target node before it is considered a target selection
	enabled: true, // whether to start the plugin in the enabled state
	edgeType: function( sourceNode, targetNode ){
		// can return 'flat' for flat edges between nodes or 'node' for intermediate node between them
		// returning null/undefined means an edge can't be added between the two nodes
		return 'flat'; 
	},
	loopAllowed: function( node ){
		// for the specified node, return whether edges from itself to itself are allowed
		return false;
	},
	nodeParams: function( sourceNode, targetNode ){
		// for edges between the specified source and target
		// return element object to be passed to cy.add() for intermediary node
		return {};
	},
	edgeParams: function( sourceNode, targetNode ){
		// for edges between the specified source and target
		// return element object to be passed to cy.add() for edge
		return {};
	},
	start: function( sourceNode ){
		// fired when edgehandles interaction starts (drag on handle)
	},
	complete: function( sourceNode, targetNodes, addedEntities ){
		// fired when edgehandles is done and entities are added
	},
	stop: function( sourceNode ){
		// fired when edgehandles interaction is stopped (either complete with added edges or incomplete)
	}
});

```

## Classes

These classes can be used for styling the graph as it interacts with the plugin:

* `edgehandles-source` : The source node
* `edgehandles-target` : A target node
* `edgehandles-preview` : Preview elements (used with `options.preview: true`)
* `edgehandles-hover` : Added to nodes as they are hovered over as targets
* `edgehandles-ghost-edge` : The ghost handle line edge


## Events

During the course of a user's interaction with the plugin, several events are generated and triggered on the corresponding elements:

On the source node:

 * `cyedgehandles.showhandle` : when the handle is shown
 * `cyedgehandles.start` : when starting to drag on the handle
 * `cyedgehandles.stop` : when the handle is released
 * `cyedgehandles.complete` : when the handle has been released and edges are created

On the target node:

 * `cyedgehandles.addpreview` : when a preview is shown (i.e. target selected)
 * `cyedgehandles.removepreview` : when a preview is removed (i.e. target unselected)

Example binding:

```js
cy.on('cyedgehandles.start', 'node', function(e){
	var srcNode = this;

	// ...
});
```

## Plugin functions

 * `$('#cy').cytoscapeEdgehandles('enable')` : enable the plugin
 * `$('#cy').cytoscapeEdgehandles('enable')` : disable the plugin
 * `$('#cy').cytoscapeEdgehandles('option', 'preview', false)` : set individual option (e.g. `'preview'`)
 * `$('#cy').cytoscapeEdgehandles('option', { /* options */ })` : set all options
 * `$('#cy').cytoscapeEdgehandles('option', 'preview')` : get option value (e.g. `'preview'`)
 * `$('#cy').cytoscapeEdgehandles('destroy')` : destroy the plugin instance
 * `$('#cy').cytoscapeEdgehandles('start', 'some-node-id')` : start the handle drag state on node with specified id (e.g. `'some-node-id'`)
 * `$('#cy').cytoscapeEdgehandles('drawon')` : enable draw mode
 * `$('#cy').cytoscapeEdgehandles('drawoff')` : disable draw mode