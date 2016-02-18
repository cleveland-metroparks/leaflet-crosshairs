# leaflet-crosshairs
No no no, read _me_.

See the hosted example here:
http://cleveland-metroparks.github.io/leaflet-crosshairs

The problem space is this: Your web development team is good with forms. They build forms like diurnal animals sleep -- daily: day in, day out, day in, day out. So, they want to build a form for a mobile web app _and_ it just happens to use HTML5 to grab the geolocation from field deployed phones, tablets, etc.. Great! Geo problem solved -- when we collect our form data, we'll get geo-data _for free_.

Not so fast, form-building Valentine -- what's the quality of those data? How accurate is that phone GPS? Is it good _enough_. The answer probably is _yes, most of the time_.

But, most of the time isn't good enough, Faye. What I want is an embedded map where I can move the crosshairs to the actual location inside the form. Simple. Hence, questions like this: http://gis.stackexchange.com/questions/90225/how-to-add-a-floating-crosshairs-icon-above-leaflet-map and pages like this: https://www.mapbox.com/blog/help-search-MH370/

So, we build a small page that has the following features:
* A cross hairs that is stationary
* A map that moves
* When the map moves, our lat/lon values update in our form

Main code as follows (careful -- careless use of jquery follows):

```Javascript
        <script>
            // Initiate map
            var map = L.map('map');

            // load map
            L.tileLayer('https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token=pk.blablabla', {
                maxZoom: 20,
                id: 'mapbox.satellite'
            }).addTo(map);

            // Now a function to populate our form with latitude and longitude values
            function onMapMove(e) {
                // txtLatitude.val(map.getCenter());
                var locale = map.getCenter();
                $('#txtLatitude').val(locale.lat);
                $('#txtLongitude').val(locale.lng);
            }
            
            // Boilerplate...
            function onLocationError(e) {
                alert(e.message);
            }

            // When the map moves we run our function up above
            map.on('move', onMapMove);

            // Boilerplate
            map.on('locationerror', onLocationError);
            
            // When we load the map, we should zoom to our current position using device geolocation
            map.locate({ setView: true, maxZoom: 20 });
        </script>
```

## Appearance:
![](https://raw.githubusercontent.com/cleveland-metroparks/leaflet-crosshairs/gh-pages/IMG_2752.PNG)

## Things to fix:
* Alignment of crosshairs so they are properly centered
* Better looking crosshairs
* Rounding for those coordinate values
