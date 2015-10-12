# Pokemon

Create interactive visualizations for the Pokemon data. Complete the script
so that when a user clicks on each menu button, there will be a different
bar chart shown in the Viz block.

You will be reusing the code you wrote for generating SVG bar charts a couple weeks
ago. The main challenge is to figure out how to connect your visualization code
with the front-end code (event handlers ... etc).

## Menu

<button id="viz-horizontal">Attack (Horizontal Bars)</button>
<button id="viz-vertical">Attack (Vertical Bars)</button>
<button id="viz-attack-defense">Attack vs. Defense</button>
<button id="viz-speed-defense">Speed vs. Defense</button>
<button id="viz-horizontal-sorted">Attack (sorted from low to high)</button>
<button id="viz-horizontal-sorted-desc">Attack (sorted from high to low)</button>
<button id="viz-attack-speed">Attack (width) vs. Speed (color)</button>

## Viz

<div class="myviz" style="width:100%; height:500px; border: 1px black solid;">
Data is not loaded yet
</div>

{% script %}
// pokemonData is a global variable
pokemonData = 'not loaded yet'

$.get('/data/pokemon-small.json')
 .success(function(data){
     console.log('data loaded', data)
     // TODO: show in the myviz that the data is loaded
     pokemonData = data
     $('.myviz').html('number of records load:' + data.length)      
 })


function vizAsHorizontalBars(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d['Attack']
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}



// for each of the TODOs below, you will write a "callback function" similar
// to vizAsHorizontalBars and a $('something').click to add this callback
// function to respond to the click event of a particular button

// TODO: add code to visualize the attack points as a series
// of vertical bars (without labels)

function vizAsVerticalBars(){
    // define a template string
    var tplString = '<g transform="translate(${d.x} ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return i * 20
    }

    function computeWidth(d, i) {
        return 20
    }

    function computeHeight(d, i) {
        return d['Attack']
    }

    function computeY(d, i) {
        return 0
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    height: computeHeight(d,i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}



// TODO: add code visualize the attack points vs. defense
// points as side-by-side horizontal bar charts (with labels)


function vizAttackDefense(){
    var tplString = '<g transform="translate(${d.attx} ${d.atty})"> \
                    <rect   \
                         height="${d.attheight}"    \
                         width="${d.attwidth}"    \
                         style="fill:${d.attcolor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.attlabel} \
                    </text> \
                    </g>'
    tplString += '<g transform="translate(${d.defx} ${d.defy})"> \
                    <rect   \
                         height="${d.defheight}"    \
                         width="${d.defwidth}"    \
                         style="fill:${d.defcolor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.deflabel} \
                    </text> \
                    </g>'


    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i, x) {
        if (x) {
            return 0
        } else {
            return 150
        }
    }

    function computeY(d, i, x) {
        return i * 20
    }

    function computeWidth(d, i, x) {
        if (x) {
            return d['Attack']
        } else {
            return d['Defense']
        }
    }

    function computeHeight(d, i, x) {
        return 20
    }

    function computeColor(d, i, x) {
        if (x){
            return 'red' 
        } else {
            return 'blue'
        }
    }

    function computeLabel(d, i, x) {
        return d['Name']
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    attx: computeX(d, i, true),
                    atty: computeY(d, i, true),
                    attwidth: computeWidth(d, i, true),
                    attheight: computeHeight(d, i, true),
                    attcolor: computeColor(d, i, true),
                    attlabel: computeLabel(d, i, true),
                    defx: computeX(d, i, false),
                    defy: computeY(d, i, false),
                    defwidth: computeWidth(d, i, false),
                    defheight: computeHeight(d, i, false),
                    defcolor: computeColor(d, i, false),
                    deflabel: computeLabel(d, i, false)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}





// TODO: add code visualize the speed points vs. defense
// points as side-by-side horizontal bar charts (with labels)


function vizSpeedDefense(){
    var tplString = '<g transform="translate(${d.speedx} ${d.speedy})"> \
                    <rect   \
                         height="${d.speedheight}"    \
                         width="${d.speedwidth}"    \
                         style="fill:${d.speedcolor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.speedlabel} \
                    </text> \
                    </g>'
    tplString += '<g transform="translate(${d.defx} ${d.defy})"> \
                    <rect   \
                         height="${d.defheight}"    \
                         width="${d.defwidth}"    \
                         style="fill:${d.defcolor};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.deflabel} \
                    </text> \
                    </g>'


    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i, x) {
        if (x) {
            return 0;
        } else {
            return 150;
        }
    }

    function computeY(d, i, x) {
        return i * 20
    }

    function computeWidth(d, i, x) {
        if (x) {
            return d['Speed'];
        } else {
            return d['Defense'];
        }
    }

    function computeHeight(d, i, x) {
        return 20
    }

    function computeColor(d, i, x) {
        if (x){
            return 'green' 
        } else {
            return 'blue'
        }
    }

    function computeLabel(d, i, x) {
        return d['Name']
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    speedx: computeX(d, i, true),
                    speedy: computeY(d, i, true),
                    speedwidth: computeWidth(d, i, true),
                    speedheight: computeHeight(d, i, true),
                    speedcolor: computeColor(d, i, true),
                    speedlabel: computeLabel(d, i, true),
                    defx: computeX(d, i, false),
                    defy: computeY(d, i, false),
                    defwidth: computeWidth(d, i, false),
                    defheight: computeHeight(d, i, false),
                    defcolor: computeColor(d, i, false),
                    deflabel: computeLabel(d, i, false)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}







// TODO: add code to visualize the attack points in ascending order as a
// series of horizontal bar charts (with labels)

function vizHorizontalSort(){

    // TODO: modify this function to visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    <text transform="translate(0 15)"> \
                        ${d.label} \
                    </text> \            
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d['Attack']
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i){
        return d['Name']
    }

    var viz = _.map(pokemonData, function(d, i){
                    return {
                        x: computeX(d, i),
                        y: computeY(d, i),
                        width: computeWidth(d, i),
                        color: computeColor(d, i),
                        label: computeLabel(d, i)
                    }
                })

    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}



// TODO: add code to visualize the attack points in descending order as a
// series of horizontal bar charts (with labels)

// TODO: add code to visualize the attack points as a series of horizontal bar
// charts (with labels), and using the brightness of red to represent defense
// points




$('button#viz-horizontal').click(vizAsHorizontalBars)
$('button#viz-vertical').click(vizAsVerticalBars)
$('button#viz-attack-defense').click(vizAttackDefense)
$('button#viz-speed-defense').click(vizSpeedDefense)
$('button#viz-horizontal-sorted').click(vizHorizontalSort)







{% endscript %}
