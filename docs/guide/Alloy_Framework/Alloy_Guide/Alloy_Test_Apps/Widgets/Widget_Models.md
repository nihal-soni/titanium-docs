---
title: Widget Models
weight: '30'
---

# Widget Models

Demonstrates using data models with a widget.

::: tip App Folder Location
alloy/test/apps/**widgets/models**
:::

The only difference when using models in a widget, compared to non-widget use, is that you use [Widget.createModel()](#!/api/Alloy.Widget-method-createModel) and [Widget.createCollection()](#!/api/Alloy.Widget-method-createCollection) methods to create models and collections, respectively. These methods are directly analogous to [Alloy.createModel()](#!/api/Alloy-method-createModel) and [Alloy.createCollection()](#!/api/Alloy-method-createCollection). You can also use the Model and Collection tags in widget views.

![widget_model](./widget_model.png)

The sample defines a widget named **alloy.datatable** that contains a `<TableView/>` and a `<Collection/>` element that creates a new Collection instance from the songs.js model file. Each `<TableViewRow/>` element is bound to the value of the `info` field defined by the songs collection. When a user selects a table row, the view-controller's **`handleTableClick`** method is called.

**app/widgets/alloy.datatable/views/widget.xml**

```xml
<Alloy>
    <Collection src="songs"/>
    <TableView id="table" dataCollection="songs" onClick="handleTableClick">
        <TableViewRow title="{info}"/>
    </TableView>
</Alloy>
```

The widget's view-controller populates the "songs" data collection with data using the Backbone [reset()](http://docs.appcelerator.com/backbone/0.9.2/#Collection-reset) method. The `handleTableClick()` function uses the Backbone [trigger()](http://docs.appcelerator.com/backbone/0.9.2/#Events-trigger) method to generate a "rowClick" event that's handled by the main application (see below).

**app/widgets/alloy.datatable/controllers/widget.js**

```javascript
function handleTableClick(e) {
    $.trigger('rowClick', e);
}
Widget.Collections.songs.reset([
    { info: 'Beastie Boys - Super Disco Breakin\''},
    { info: 'Pixies - Debaser'},
    { info: 'N.E.R.D. - Thrasher'},
    { info: 'Skrillex - Bangarang'},
    { info: 'Big Gigantic - Hopscotch'},
    { info: 'Infected Mushroom - Heavyweight'},
    { info: 'Nine Inch Nails - Into the Void'},
    { info: 'Ronald Jenkees - Disorganized Fun'},
    { info: 'Kazu Matsui - Stone Monkey'},
    { info: 'Apox - Stay Triumphant'},
    { info: 'Swedish House Mafia - Greyhound'},
    { info: 'G Love - Milk and Sugar'},
    { info: 'Elton John - Better Off Dead'},
    { info: 'Opiuo - King Prawn'}
]);
```

The main application view that contains a `<Widget/>` element that includes the **alloy.datatable** widget.

**app/views/index.xml**

```xml
<Alloy>
    <Window>
        <Widget src="alloy.datatable" id="dtable"/>
    </Window>
</Alloy>
```

The main view-controller uses the Backbone [on()](http://docs.appcelerator.com/backbone/0.9.2/#Events-on) function to bind a function handler to the "rowClick" event generated by the TableView.

**app/controllers/index.js**

```javascript
$.dtable.on('rowClick', function(e) {
    alert(e.row.title);
});
```