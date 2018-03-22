# Install nodejs and npm

Follow instructions from here: http://blog.teamtreehouse.com/install-node-js-npm-mac

If everything worked as expected:

- `node` `--``version` and `npm --version` should give you an output and not error.
# Get OSM QA Tiles
- Make a folder for your project: `mkdir veena`
- Go into your project: `cd veena`
- Get the OSM data for Monaco: `wget https://s3.amazonaws.com/mapbox/osm-qa-tiles-production/latest.country/monaco.mbtiles.gz)`
- Unzip the data: `gunzip monaco.mbtiles.gz`
- Go out of the repo: `cd ..`
# Install osm-qa-tiles
- Clone osm-tag-stats: `git clone https://github.com/mapbox/osm-tag-stats.git`
- Go into the osm-tag-stats repo: `cd osm-tag-stats`
- npm install and link: `npm install && npm link`


# Get data for primary highways in Monaco
- Go to the Veena repo: `cd ../veena`
- Get data of primary highways in Monaco: `osm-tag-stats --geojson=highway_primary.geojson --mbtiles='monaco.mbtiles' --filter='../osm-tag-stats/filter/highway_primary.json'` → will give you a file called highway_primary.geojson


# Create a mapbox studio account, if you don’t have one
- https://www.mapbox.com/studio → Sign Up
- Log In. On the top right, select `Accounts` → `Access tokens` → Copy the value of the default public token there.

# Visualise this data on a map:
- Create a folder: `mkdir ../map`
- Go to folder `cd ../map`
- Create an html file: `> index.html`
- Add the following contents to index.html:
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset='utf-8' />
        <title>Monaco highways</title>
        <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
        <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.44.1/mapbox-gl.js'></script>
        <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.44.1/mapbox-gl.css' rel='stylesheet' />
        <style>
            body { margin:0; padding:0; }
            #map { position:absolute; top:0; bottom:0; width:100%; }
        </style>
    </head>
    <body>
    
    <div id='map'></div>
    <script>
    mapboxgl.accessToken = '<access_token>';
    var map = new mapboxgl.Map({
        container: 'map',
        style: 'mapbox://styles/mapbox/light-v9',
        center: [7.4189, 43.7310], //This should center on monaco
        zoom: 14
    });
    
    map.on('load', function () {
    
        map.addLayer({
            "id": "route",
            "type": "line",
            "source": {
                "type": "geojson",
                "data": <COPY THE CONTENTS OF highway_primary.geojson HERE`
            },
            "layout": {
                "line-join": "round",
                "line-cap": "round"
            },
            "paint": {
                "line-color": "#888",
                "line-width": 3
            }
        });
    });
    </script>
    
    </body>
    </html>


- Go to previous folder: `cd ../`


# Upload the file to your github account:
- After creating the repo, clone it: `git clone https://github.com/<username>/<username>.github.io`
- Go into the repo: `cd <username>.github.io`
- Copy the index.html to this directory: `cp ../map/index.html .`
- Git add the file: `git add index.html`
- Git commit the file: `git commit index.html -m``"``add index.html``"`
- Git push the file: `git push origin master`

Now, you should see the map on https://veenasankar.github.io/index.html

