<template>
    <div id="map-test" style="height: 100vh; width: 100vw;">

    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            msg: String
        },
        data() {
            return {
                randomGeoPoints: []
            };
        },
        methods: {
            loadMap() {
                function generateRandomPoints(center, radius, count) {
                    var points = [];
                    for (var i=0; i<count; i++) {
                        points.push(generateRandomPoint(center, radius));
                    }
                    return points;
                }

                function generateRandomPoint(center, radius) {
                    var x0 = center.lng;
                    var y0 = center.lat;
                    // Convert Radius from meters to degrees.
                    var rd = radius/111300;

                    var u = Math.random();
                    var v = Math.random();

                    var w = rd * Math.sqrt(u);
                    var t = 2 * Math.PI * v;
                    var x = w * Math.cos(t);
                    var y = w * Math.sin(t);

                    var xp = x/Math.cos(y0);

                    // Resulting point.
                    return {'lat': y+y0, 'lng': xp+x0};
                }


// Usage Example.
// Generates 100 points that is in a 1km radius from the given lat and lng point.
                this.randomGeoPoints = generateRandomPoints({'lat':24.23, 'lng':23.12}, 10000000, 100000);
                this.$loadScript("https://maps.googleapis.com/maps/api/js?key=AIzaSyANoGcUgMWhedKX7RKZ66wj6LYd2SEuljM")
                    .then(() => {
                        // Script is loaded, do something
                        this.$loadScript("https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/markerclusterer.js").then((resp) => {
                                this.initMap()
                        }, (error) => {

                        });
                    })
                    .catch(() => {
                        // Failed to fetch script
                    });
            },
            initMap() {
                    var map = new google.maps.Map(document.getElementById('map-test'), {
                        zoom: 3,
                        center: {lat: -28.024, lng: 140.887}
                    });

                    // Create an array of alphabetical characters used to label the markers.
                    var labels = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
                    // Add some markers to the map.
                    // Note: The code uses the JavaScript Array.prototype.map() method to
                    // create an array of markers based on a given "locations" array.
                    // The map() method here has nothing to do with the Google Maps API.
                    var markers = this.randomGeoPoints.map(function (location, i) {
                        return new google.maps.Marker({
                                                          position: location,
                                                          label: labels[i % labels.length]
                                                      });
                    });

                    // Add a marker clusterer to manage the markers.
                    var markerCluster = new MarkerClusterer(map, markers,{
        imagePath:'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m'});

            }
        },
        created(){
            this.loadMap()
        }
    };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="less">
    h3 {
        margin: 40px 0 0;
    }

    ul {
        list-style-type: none;
        padding: 0;
    }

    li {
        display: inline-block;
        margin: 0 10px;
    }

    a {
        color: #42b983;
    }
</style>
