
SVG World Map JS
================

🗺 A JavaScript library to easily integrate one or more SVG world maps with all nations (countries) and second-level political subdivisions (countries, provinces, states).  

![](https://raphaellepuschitz.github.io/SVG-World-Map/demo/svg-world-map.png)  

Use this map and library as boilerplate for a **Strategy Game**, for **Data Visualization** of scientific research and other data, or as **Interactive Map** for your article, paper, website or app. 

This is just a small library (with a big world map) for fast and simple data projection. If you need more advanced features for data visualization and SVGs, have a look at [d3js](https://github.com/d3/d3).  

> ***Attention:*** This library is under development and currently in early beta phase. Use it carefully! 

This package constists of 3 parts (which could also be used separately):

* A detailed **[SVG world map](#part-1-the-map)** with **239 nations and countries** and **3000 second-level provinces**, ready for editing with your preferred graphics editor
* A **[List of all world countries](#part-2-the-list)** with additional information, ready for use with the SVG map
* A **[JavaScript SVG library](#part-3-the-library)** developed for the map and optimized for **quick SVG path access**, customizable with **options** and a **callback-API**, including a addon module for **time controls** and **visual data animation**

To unleash the full power of *SVG World Map JS* you should of course use all 3 combined ;-)


Demos
-----

* **[Basics](https://raphaellepuschitz.github.io/SVG-World-Map/demo/basics.html)**
* **[Custom options](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-options.html)**
* **[Custom callbacks](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-callbacks.html)**
* **[Custom data](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-data.html)**
* **[Groups, callbacks & zoom](https://raphaellepuschitz.github.io/SVG-World-Map/demo/groups-callbacks-zoom.html)** <sup>ᴢ</sup>
* **[Wikipedia table data projection](https://raphaellepuschitz.github.io/SVG-World-Map/demo/wikipedia-data.html)** <sup>ᴢ</sup>
* **[Time animation and controls](https://raphaellepuschitz.github.io/SVG-World-Map/demo/time-animation.html)** <sup>ᴛ ᴢ</sup>
* **[COVID-19 Corona virus world map](https://raphaellepuschitz.github.io/SVG-World-Map/demo/corona-world-map.html)** <sup>ᴛ ᴢ ᴄ ᴀ</sup> 
<!-- * Strategy game -->

<sup>ᴛ</sup> with the **Time Controls** addon module, 
<sup>ᴢ</sup> with [svg-pan-zoom.js](https://github.com/ariutta/svg-pan-zoom), 
<sup>ᴄ</sup> with [chart.js](https://www.chartjs.org), 
<sup>ᴀ</sup> data from the [Coronavirus Tracker API](https://github.com/ExpDev07/coronavirus-tracker-api)


Part 1: The Map
---------------

Download current version: [world-states-provinces.svg](./src/world-states-provinces.svg)  

The map is based on the creative commons [Blank Map World Secondary Political Divisions.svg](https://commons.wikimedia.org/wiki/File:Blank_Map_World_Secondary_Political_Divisions.svg) from [Wikimedia Commons](https://commons.wikimedia.org).  
It was strongly modified to serve the purpose of this JavaScript library, so all world nations are **grouped**, **sorted** and **named** by their official [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) country codes.  
The country paths in the SVG are not the work of the library author. See the version history and authorship of the original file [here](https://commons.wikimedia.org/wiki/File:Blank_Map_World_Secondary_Political_Divisions.svg).  
Because of all the detailed subregions the map has a lot of vectors and is rather large for an image (~3,8 MB).  

**Make sure to use the SVG only, if:**

* You really need a fully detailed world map with all nations, provinces and states. If not, there's a ton of smaller SVGs at [Wikimedia Commons: SVG blank maps of the world](https://commons.wikimedia.org/wiki/Category:SVG_blank_maps_of_the_world) and elsewhere. 
* Your users have a fast internet connection, or:
* You add a preloader or caching layer for faster delivery (a simple loading bar will also work in some cases). 

> The political subdivisions (countries, provinces, states) are mostly not named correctly, like `<path id="path12345" ...>`. This issue will be addressed in future versions. 


Part 2: The List
----------------

There are 2 versions of the country list: 

* A [CSV file](./src/countrydata.csv) for easy editing with any office software
* A [JSON file](./src/countrydata.json) for easy integration with frontend or backend systems

The list includes 250 countries and microstates from Andorra to Zimbabwe with this additional information:  

* The ISO country **code**, e.g. "AT"
* The country **name**, e.g. "Austria"
* The official **longname**, e.g. "The Republic of Austria"
* The countries **sovereignty**, e.g. "UN"
* The world **region**, e.g. "EU"
* (For some countries also the **provinces**, e.g. "Vienna")

> Note: Most political subdivisions (countries, provinces, states) are currently not included in the country list. They will be added in future versions. 


Part 3: The Library
-------------------

### Requirements

Although this project only uses frontend technologies, most browsers nowadays don't accept the `file://` protocol for SVGs or JSONs, so SVG World Map needs to run on a (local) server. If you are new to HTML / SVG / JavaScript integration, please see the Pitfalls section first.  


### How to use

| **&raquo; Demo:** [Basics](https://raphaellepuschitz.github.io/SVG-World-Map/demo/basics.html) |
| --- |

First, add [world-states-provinces.svg](./src/world-states-provinces.svg) and [svg-world-map.js](./src/svg-world-map.js) to your HTML document, then initialize the library:

```html
<object id="svg-world-map" data="world-states-provinces.svg" type="image/svg+xml"></object>
```

```html
<script src="svg-world-map.js"></script>
```

```js
window.onload = function() { svgWorldMap('svg-world-map'); }; // Quick and dirty basic implementation
```

The better way of calling the library is of course to load the SVG via `addEventListener()` and bind the world map to a variable: 

```js
var mySVG = document.getElementById('svg-world-map'); 
mySVG.addEventListener("load", function() {
    var myWorldMap = svgWorldMap(mySVG); 
})
```

So far for the basic setup.

> Note that the HTML, the SVG and the JS must all be loaded before the library is initialized. There's multiple ways of doing this, like `window.onload()`, `mySVG.addEventListener("load", ...)`, `$(document).ready()` (in JQuery), etc. 


### Custom options 

| **&raquo; Demo:** [Custom options](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-options.html) |
| --- |

* All default options can be overruled by passing an object of custom options
* All `color`, `fill` and `stroke` arguments take hex and rgb(a) values or 'none' as input
* If `showInfoBox` is set to `true`, the library will add a `<div id="map-infobox"></div>` plus basic CSS to your page
* `mapClick`, `mapOver`, `mapOut`, `mapTable` and `mapDate` are callback function names, see the API section for details
* `timeControls` will activate the **Time Controls** addon module, see below for more information

```js
// Default options
var options = {

    // Basic options
    showOcean: true, // Show or hide ocean layer
    showLabels: true, // Show country labels
    showMicroLabels: false, // Show microstate labels
    showMicroStates: true, // Show microstates on map
    showInfoBox: false, // Show info box

    // Color options
    oceanColor: '#D8EBFF', 
    worldColor: '#FFFFFF', 
    labelFill: { out: '#666666',  over: '#CCCCCC',  click: '#000000' }, 
    countryStroke: { out: '#FFFFFF',  over: '#FFFFFF',  click: '#333333' }, 
    countryStrokeWidth: { out: '0.5',  over: '1',  click: '1' }, 
    provinceFill: { out: '#B9B9B9',  over: '#FFFFFF',  click: '#666666' }, 
    provinceStroke: { out: '#FFFFFF',  over: '#FFFFFF',  click: '#666666' }, 
    provinceStrokeWidth: { out: '0.1',  over: '0.5',  click: '0.5' }, 

    // Group options
    groupCountries: true, // Enable or disable country grouping
    groupBy: [ "region" ], // Sort countryData by this value(s) and return to countryGroups

    // Callback functions from the map to the outside, can have custom names
    mapOut: "mapOut", 
    mapOver: "mapOver", 
    mapClick: "mapClick", 
    mapTable: "mapTable", // (Custom) callback function for HTML data parsing
    mapDate: "mapDate", // (Custom) callback function for time control date return

    // Time control addon module
    timeControls: false, // Set to 'true' for time controls
    timePause: true, // Set to 'false' for time animation autostart
    timeLoop: false //  Set to 'true' for time animation loop
};
```

The custom options are passed as second (optional) parameter to the map at startup. You can either hand them over as an object similar to the one above (with `myCustomOptions` instead of `options`): 

```js
var myWorldMap = svgWorldMap(mySVG, myCustomOptions); 
```

Or as inline paramater: 

```js
var myWorldMap = svgWorldMap(mySVG, { showOcean: false, groupCountries: false, mapClick: "customMapClick" }); 
```


### Map object return values

After initialization, the `svgWorldMap()` function will give you an object in return.  
If the map is called like `var myWorldMap = svgWorldMap(mySVG)`, then the return data of `myWorldMap` looks something like this: 

```js
myWorldMap: { 
    countries: { AD: '<g id="AD">', AE: '<g id="AE">', ... }, 
    countryData: { AD: { name: "Andorra", longname: ... }, ... }, 
    countryGroups: { region: { AF: {...}, AN: {...}, ... }, ... }, 
    countryLabels: { AD: '<text id="AD-label">', AE: '<text id="AE-label">', ... }, 
    out: function(id) { ... }, // Calling home functions from outside into the map 
    over: function(id) { ... }, 
    click: function(id) { ... }, 
    update: function(data) { ... }, 
    reset: function(data) { ... }, 
    labels: function(data) { ... }, 
    table: function(data) { ... }, 
    date: function(data) { ... } 
};
```

Let's break this down in detail, as each return object can be very useful:


#### Country list

This sub object `svgWorldMap.countries` includes a list of all top level countries (the main groups in the SVG), where key is the country code and value is the `<g id="AD">` group element (which includes all sub provinces).  

There are also some special informations added **directly to the `<g>` and all child `<path>`** elements, which can come in very handy for quick element and information access. Let's have a look at Canada: 

```js
svgWorldMap.countries['CA']: <g id="CA">: {
    ...
    border: '<path id="ca" fill="none" stroke="#FFFFFF" stroke-width="0.5" d="...">', // Border path (= nation path stroke)
    country: '<g id="CA">'; // The group element of the main country
    name: 'Canada'; 
    region: 'NA'; // North America
    provinces: [ '<path id="CA-AB" fill="#B9B9B9" ...>', '<path id="CA-BC" fill="#B9B9B9" ...>', ... ]; // Array with all sub-province paths
    ...
}
```

> The political subdivisions (countries, provinces, states) are mostly not named correctly, like `<path id="path12345" ...>`. This issue will be addressed in future versions. 


#### Country data 

The `svgWorldMap.countryData` is a copy of the (optional) `countryData` parameter at startup (see next section) or the fallback data from the library. If you don't modify the basic country data, this object inculdes the following information: 

```js
svgWorldMap.countryData['CA']: {
    name: "Canada",
    region: "NA", // North America
}
```

If you load the basic cutom data from [countrydata.json](./src/countrydata.json) (see below), the data provides a little more (and is also added to each country in `svgWorldMap.countries`): 

```js
svgWorldMap.countryData['CA']: {
    name: "Canada",
    longname: "Canada",
    region: "NA", // North America
    sovereignty: "UN" // United Nations member state
    population: 37958039,
    provinces: { // Note: Most countries currently don't have province detail data 
        "CA-AB": { name: "Alberta", capital: "Edmonton", population: 4413146 },
        "CA-BC": { name: "British Columbia", capital: "Victoria", population: 5110917 }, ...
    }
}
```


#### Country groups

| **&raquo; Demo:** [Groups, callbacks & zoom](https://raphaellepuschitz.github.io/SVG-World-Map/demo/groups-callbacks-zoom.html) |
| --- |

Country groups are an essential feature of the SVG World Map library and can help you access a whole bunch of countries simultaneously. Countries are grouped by the information of the `svgWorldMap.countryData` explained before. By default, the countries are grouped by their respective world region, so `svgWorldMap.countryGroups` contains the data as follows: 

```js
svgWorldMap.countryGroups.region: {
    AF: {                       // Africa 
        AO: '<g id="AO">',      // Angola
        BF: '<g id="BF">',      // Burkina Faso
        BI: '<g id="BI">',      // Burundi
        ...
        ZA: '<g id="ZA">',      // South Africa
        ZM: '<g id="ZM">',      // Zambia
        ZW: '<g id="ZW">'       // Zimbabwe
    }, 
    AN: { '<g id="AQ">', ... }, // Antarctica
    AS: { '<g id="AE">', ... }, // Asia
    ​​EU: { '<g id="AD">', ... }, // Europe
    ​​NA: { '<g id="AG">', ... }, // North America
    ​​OC: { '<g id="AS">', ... }​​, // Australia & Oceania
    SA: { '<g id="AR">', ... }​​  // South America
}
```

Groups can be deactivated by setting the `options.groupCountries` value to `false` (default is `true`):

```js
var myWorldMap = svgWorldMap(mySVG, { groupCountries: false }); 
```

If you want to add a country group, you have to add the category key from country data to the `options.groupBy`. Let's say we also want a `sovereignty` group, then the options would have to look like: 

```js
var myCustomOptions = {
    ...
    groupCountries: true, 
    groupBy: [ "region", "sovereignty" ], // Sort countryData by this value(s) and return to countryGroups
};
var myWorldMap = svgWorldMap(mySVG, myCustomOptions); 
```


#### Country labels

The labels are a group in the SVG map and are sorted like the country list. They are basically controlled via the `labels()` API function (see there). Each one has a attribute called `microstate` which can be `true` or `false`: 

```js
svgWorldMap.countryLabels['AD']: <text id="AD-label">: {
    ...
    microstate: true,
    ...
}
```

#### Country `over()`, `out()`, `click()`, `update()`, `reset()`, `labels()`, `table()` and `date()`

All these functions are part of the **API**, please see below for further information.


### Changing the basic country data

There's two kinds of data for countries: 

* The **initial country data** is passed as a JSON object in the same format as the [countrydata.json](./src/countrydata.json) 
* There's an **update map API-call**. For further information about manipulating the country color at runtime, see the sections below. 

> If you want to add custom data from a modified country list in CSV format, please covert it to a **keyed JSON file**. A good CSV to JSON converter can be found [here](https://www.convertcsv.com/csv-to-json.htm) (make sure to select "CSV to keyed JSON"). CSV import will be added in future versions. 


#### JSON format

| **&raquo; Demo:** [Custom data](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-data.html) |
| --- |

To add or change the country information on startup, you can simply pass your JSON data to the `svgWorldMap()` as third (optional) parameter (the second one is options).  

```js
var myWorldMap = svgWorldMap(mySVG, {}, countryData); 
```

The library will then do the logic: For example, you could upload your own list with the country population in the year 1000 or change all the countries names with planet names from *Star Trek*. In that case, you would have to change the country data from this...

```js
var countryData = {
    AD: {
        name: "Andorra",
        longname: "The Principality of Andorra",
        sovereignty: "UN",
        region: "EU"
    }, ... 
}
```

...to this:

```js
var countryData = {
    AD: {
        name: "Andoria",
        longname: "Andorian Empire",
        sovereignty: "UFP", // United Federation of Planets
        region: "AQ" // Alpha Quadrant
    }, ... 
}
```

> **Note:** You can change all country information ***except*** the country code, which is the identifier for the corresponding map path.  


### Callback and home APIs

| **&raquo; Demo:** [Custom callbacks](https://raphaellepuschitz.github.io/SVG-World-Map/demo/custom-callbacks.html) |
| --- |

| **&raquo; Demo:** [Wikipedia table data projection](https://raphaellepuschitz.github.io/SVG-World-Map/demo/wikipedia-data.html) |
| --- |

#### Calling back from the map

As seen in the options setup, there are five callback functions for over, out, click, table and date, which can also have custom names:

```js
var options = {
    // Callback functions from the map to the outside, can have custom names
    mapOut: "myCustomOut", 
    mapOver: "myCustomOver", 
    mapClick: "myCustomClick", 
    mapTable: "myCustomTable", 
    mapDate: "myCustomDate", 
};
```

With the callback of these functions you can catch the hovered or clicked country, get the return JSON from a parsed HTML table or receive the current date of the **Time Control** module (see below). Let's say you named your functions `"myCustomClick()"`, `"myCustomOver()"`, `"myCustomOut()"`, `"myCustomTable()"` and `"myCustomDate()"`, then the code would look something like this: 

```js
function myCustomClick(country) {
    var countryId = country.id; // Id of the clicked path on the map
    ...
}

function myCustomOver(country) {
    var countryId = country.id; // Id of the hovered over path 
    ...
}

function myCustomOut(country) {
    var countryId = country.id; // Id of the hovered out path 
    ...
}

function myCustomTable(object) {
    var tableData = object; // JSON of the parsed HTML table (e.g. from a Wikipedia country list)
    ...
}

function myCustomDate(date) {
    var currentDay = date; // Callback for the current date (e.g. day, month, year) from the Time Control module 
    ...
}
```


#### Calling home to the map

There are 8 calling home functions, 3 for country `over()`, `out()` and `click()`, then `update()` and `reset()` for (un)coloring countries, a `label()` control function, the HTML `table()` parser and `date()` (the last one is just a routing helper for mapDate).  

* **`over()`**, **`out()`** and **`click()`** will trigger the attribute changes for `fill`, `stroke` and `stroke-width` defined in `options`. They only need the country id parameter:  
 
    ```js
    myWorldMap.out('AT');
    myWorldMap.over('AT');
    myWorldMap.click('AT');
    ```

    Or as inline HTML: 

    ```html
    <li id="AT" onmouseover="myWorldMap.over('AT')" onmouseout="myWorldMap.out('AT')" onclick="myWorldMap.click('AT')">Austria</li>
    ```

* **`update()`** changes the fill color of one or more countries at once. The input object uses the same format as `svgWorldMap.countries`. Each country can have an individual color, which will stay the same on mouseover. The data is passed via country id and a color value:  

    ```js
    myWorldMap.update({ DE: '#00FF00', AT: '#00FF00', CH: '#00FF00' });
    ```

* **`reset()`** is the reverse function for `update()` and will reset all country colors to their inital state: 

    ```html
    <button onclick="myWorldMap.reset()">Reset map</button>
    ```

* **`label()`** toggles the visibility of the country labels on and off. The input parameter can be `"all"` or `"micro"` (for microstates): 

    ```html
    <button onclick="myWorldMap.labels('all')">Show labels</button>
    ```

* **`table()`** accepts a HTML string and will try to DOM parse it and find a valid `<table>` with '*iso*', '*country*', '*state*', '*name*', '*nation*', etc. in the `<th>` table headers. If such a table is found, the data inside will be scraped, sorted and returned as JSON object, ordered by the country (ISO) key. As the data parsing runs asynchronously, you have to catch the JSON via the `mapTable()` callback function mentioned above. 

    | **&raquo; Demo:** [Wikipedia table data projection](https://raphaellepuschitz.github.io/SVG-World-Map/demo/wikipedia-data.html) |
    | --- |

    Fire the calling home function with any HTML string (no URL or DOM object):

    ```js
    myWorldMap.table(htmlstring);
    ```

    Then catch the callback with the returned JSON (function can have a custom name, defined in options):

    ```js
    function mapTable(object) {
        var tableData = object; // JSON of the parsed HTML table (e.g. from a Wikipedia country list)
        ...
    }
    ```


### Time Control addon module

| **&raquo; Demo:** [Time animation and controls](https://raphaellepuschitz.github.io/SVG-World-Map/demo/time-animation.html) |
| --- |

| **&raquo; Demo:** [COVID-19 Corona virus world map](https://raphaellepuschitz.github.io/SVG-World-Map/demo/corona-world-map.html) |
| --- |

The SVG World Map library includes a nice addon for **animated data visualization**: A [JavaScript module](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) for an easy integration of time series data on the map with changing colors for each country or province. 

Before activating the Time Control module, make sure to have [svg-world-map-time-controls.mjs](./src/svg-world-map-time-controls.mjs) and the webfont folder `/src/font/...` included in your project.  

* Set the options parameter `timeControls` to `true` to load and acivate the module
* Use `timePause` and `timeLoop` for further adjustments
* `mapDate` is the custom callback function (see above)

```js
var myTimeOptions = {
    timeControls: true, // Set to 'true' for time controls
    timePause: true, // Set to 'false' for time animation autostart
    timeLoop: false //  Set to 'true' for time animation loop
    mapDate: "myCustomDate", // (Custom) callback function for time control date return
};

var myWorldMap = svgWorldMap(mySVG, myTimeOptions); 
```

This will asynchronously load `/src/svg-world-map-time-controls.mjs`, **inject the calling document** and:  

* Load the **flaticon webfont** for the control icons
* Add (prepend) some **CSS** before the closing `</head>` tag 
* Add **HTML** inside a new `<div id="map-controls">` element before the closing `</body>` 

Please override the CSS as you like and change or hide the added HTML elements.  

```html
<html>
    <head>
        ...
        <link rel="stylesheet" href="../src/font/flaticon.css">
        <style>
            #map-controls { position: absolute; bottom: 0; left: 0; right: 0; width: auto; height: 40px; padding: 0 10px; background-color: rgba(0, 0, 0, .75); }
            #map-control-buttons, #map-slider-container, #map-speed-controls, #map-date { float: left; }
            #map-control-buttons { width: 20%; }
            ...
        </style>
    </head>
    <body>
        ...
        <div id="map-controls">
            <div id="map-control-buttons">
                <button id="map-control-start" onclick="clickControl()"><i class="flaticon-previous"></i></button>
                <button id="map-control-back" onclick="clickControl()"><i class="flaticon-rewind"></i></button>
                <button id="map-control-play-pause" onclick="clickPlayPause()"><i class="flaticon-play"></i></button>
                <button id="map-control-forward" onclick="clickControl()"><i class="flaticon-fast-forward"></i></button>
                <button id="map-control-end" onclick="clickControl()"><i class="flaticon-skip"></i></button>
            </div>
            <div id="map-slider-container">
                <input id="map-slider" type="range" min="0" max="10">
            </div>
            <div id="map-speed-controls">
                <button id="map-control-slower" onclick="clickSpeed()"><i class="flaticon-minus"></i></button>
                <button id="map-control-faster" onclick="clickSpeed()"><i class="flaticon-plus"></i></button>
            </div>
            <div id="map-date"></div>
        </div>
    </body>
</html>
```

The activated module without a *time series dataset* (see below) will load the controls (play, pause, forward, faster, ...) and add the ticks per day (or month, year, ...) logic to the map. The control buttons and the current date will be shown, but the time slider will not be visible (as there is *no* end date).  

To catch the callback when the map goes to the next date, just use the callback function mentioned before:

```js
function myCustomDate(date) {
    var currentDay = date; // Callback for the current date (e.g. day, month, year) from the Time Control module 
    ... // Do something with the new date
}
```


#### Country data time animation 

To animate the color of countries from one date to the next, you have to pass an array with the **dates** and the **country colors** (as sub objects) to the `svgWorldMap()` main function as fourth parameter (after SVG, options and country data). 

The inner data for each date is similar to the data passed to the `update()` function or as the `svgWorldMap.countries` return object.  

Let's say you want to highlight the Baltic states *Estonia*, *Latvia* und *Lithuania* after each other, then the code would look something like this: 

```js
var myTimeData = [
    { 'Date 1': { EE: '#1E37EE', LV: '#FFFFFF', LT: '#FFFFFF' } },
    { 'Date 2': { EE: '#FFFFFF', LV: '#9C1733', LT: '#FFFFFF' } },
    { 'Date 3': { EE: '#FFFFFF', LV: '#FFFFFF', LT: '#FBB934' } }
];

var myWorldMap = svgWorldMap(mySVG, myTimeOptions, false, myTimeData); // countryData can be false, but not empty
```


Further Development & Changelog
-------------------------------


### TODOs

* Add game controls
* Add strategy game demo
* Add capitals to countrydata
* Add bubbles / circles to the map
* Add minified JS and CSS files
* Optimize drag and click
* Optimize zoom integration
* Modify the library for use with other SVG maps (RPG gamers, I'm talking to you!)
* Name all provinces in the SVG correctly (This may take a while... Help appreciated!)
* Name all provinces in the JSON and CSV correctly (This may take a while... Help appreciated!)


### Done

* 0.1.5
  * Added Wikipedia (and other) HTML table import
  * Added Wikipedia table import demo
  * Added mouse over info box
  * Added map reset function
* 0.1.4
  * Added library addon module for time controls and animation
  * Added time data visualization demo
  * Tested node live server support
* 0.1.3
  * Improved Corona virus map demo
  * Tested basic mobile support
* 0.1.2
  * Added Corona virus data visualization demo
  * Improved provinces handling
* 0.1.1
  * Improved country over, out, click
  * Added country labels (names) to the map
  * Added microstate handling
* 0.1.0
  * Cleanup SVG
  * Added population to country data
* 0.0.9
  * Fixed path bugs
  * Added basic demos
  * Added further options
  * Improved callback API


Pitfalls, Known Issues & Bugs
-----------------------------


### Pitfalls

* A lot of problems with SVGs include loading issues, make sure the SVG is fully loaded before you initialize the library
* SVGs can not be called via `file://`, so make sure you have a (local) server for this library (although it's completely frontend)
* Most browsers don't accept a mix of `http://` and `https://`, this can also affect SVG or JSON loading


### Known Issues & Bugs

* Currently several small bugs, mainly SVG path related
* Slow or old computers or bad internet conncetion will show nothing but the map
* If you find a bug, be nice to it (and also let me know of it) ;-) 
