Google Map API
===
By Weng Fei Fung

![Last Commit](https://img.shields.io/github/last-commit/Siphon880gh/wengs-guide-on-google-map-api/main)
<a target="_blank" href="https://github.com/Siphon880gh" rel="nofollow"><img src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" alt="Github" data-canonical-src="https://img.shields.io/badge/GitHub--blue?style=social&logo=GitHub" style="max-width:10ch;"></a>
<a target="_blank" href="https://www.linkedin.com/in/weng-fung/" rel="nofollow"><img src="https://img.shields.io/badge/LinkedIn-blue?style=flat&logo=linkedin&labelColor=blue" alt="Linked-In" data-canonical-src="https://img.shields.io/badge/LinkedIn-blue?style=flat&amp;logo=linkedin&amp;labelColor=blue" style="max-width:10ch;"></a>
<a target="_blank" href="https://www.youtube.com/@WayneTeachesCode/" rel="nofollow"><img src="https://img.shields.io/badge/Youtube-red?style=flat&logo=youtube&labelColor=red" alt="Youtube" data-canonical-src="https://img.shields.io/badge/Youtube-red?style=flat&amp;logo=youtube&amp;labelColor=red" style="max-width:10ch;"></a>

## Explanation

Code snippet and instructions on how to implement Google Map API. The embed is the easiest style where your options are in the URL. But the Google Map Javascript API broke down the textfield you typically expect to search places and affect the map into a library Places and its feature Autocomplete. In addition, you would have to bind the map to the autocomplete (which was binded to your text field) AND detect for a place_changed and then implement in its callback to center and zoom the map.

Embed.html is the Embed API. The js.html is the Google Map Javascript API.

Please note that I removed my access key so you have to replace them at YOUR_ACCESS_KEY, so I don't get billed.

Here is my quick start guide:

## Quick Start and Technical Walkthrough

Two ways to do it. Embed is easiest. Then there's Static and Dynamic API's (which are default style and dashboard controlled styles via Map ID's), and Javascript API's. Look at URL which has address/place url query and access key url param

You may have to enable them at your Project at console.cloud.google.com (All SDKs are connected to google cloud because they have all services in the cloud servers). Go to APIs -> Then enable the correct map API.

Then there is dynamic and static Map API's. Under Map Management at Console, you can add a specific Map ID which will go under Dynamic Maps. Map ID is for saving specific styles (You style a Map ID at Console -> Project -> Map Styles)

The API's are sometimes referred as SKU (For example, Static Maps SKU... that's for your stock keeping unit and accounting purposes). Just imagine the word SKU as interchangeable with API.

Get access to API documentations from clicking LEARN at top right. Then a learning/hints side panel appears on the right -> API & References -> Map Static API / Maps Javascript API / etc. Go for Overview (that acts like a Quick Start), then at bottom of Overview go for "Recommended for you"

You must create an access key, even for embed map, because they track uses and charge you. As of fall 2022, you get free $200 credit every month (if starting out, there's a trial vs $200 used up) and if your rate passes that then you get charged. You click Credentials on the left -> Then create an API key. By default, an API key tracks and allows access to all API services; you should in practice Edit API and restrict its scope to your specific API.


### Method 1 - Embed API

Look at URL which has address/place url query and access key url param
```
<!-- 
    https://developers.google.com/maps/get-started
-->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

    <script>
        window.link = "https://www.google.com/maps/embed/v1/place?key=AIzaSyCCnP33_rNflEnRSLnih3sF7WoyEBlP0Uc&q="
    </script>
</head>

<body>
    <iframe id="map" width="600" height="450" style="border:0" loading="lazy" allowfullscreen referrerpolicy="no-referrer-when-downgrade" src="https://www.google.com/maps/embed/v1/place?key=AIzaSyCCnP33_rNflEnRSLnih3sF7WoyEBlP0Uc
      &q=CTRL+Pasadena">
  </iframe>
    <form action="javascript:void(0)">
        <label for="change-place">Type address:</label>
        <input id="change-place" type="text">
        <button type="submit" onclick="document.getElementById('map').src=window.link+document.getElementById('change-place').value">Search</button>

    </form>
</body>

</html>
```

### Method 2 - Javascript API:
For Javascript API:
https://developers.google.com/maps/documentation/javascript/adding-a-google-map#maps_add_map-javascript

Make sure to click all tabs JS/CSS/HTML. Click TypeScript if you use typescript. Then for initial settings:https://developers.google.com/maps/documentation/javascript/url-params

Search and autocomplete a text field that searches cities etc? Use Places API which is a library that you addon to the Google Map. So two things:
Its code needs to be inside initMap function of Google Map! 
You only need one script tag that has a &library that includes this addon library Places
Then you will use the Autocomplete from Places:https://developers.google.com/maps/documentation/javascript/place-autocomplete
"Autocomplete is a feature of the Places library in the Maps JavaScript API. You can use autocomplete to give your applications the type-ahead-search behavior of the Google Maps search field. "


But for the textfield that's attached to Autocomplete to affect the map, you need to bind it (which is in the documentation):autocomplete.bindTo("bounds", map);
But that binding only allows for the map to be changed

You have to detect when the place is changed from Autocomplete. So Autocomplete has the event place_changed. You'll want to set center and set zoom level (in case you're at a default continent level that barely nudges around the state if it's already centered at the state). This was NOT in the documentation but is in the Demo the documentation link to
autocomplete.addListener("place_changed", () => {
                const place = autocomplete.getPlace();
                map.setCenter(place.geometry.location);
                map.setZoom(17);
            });


So therefore the code is:

```
<!-- 
    https://developers.google.com/maps/get-started
-->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>

</head>

<body>
    <div id="map"></div>
    <br/>
    <form action="javascript:void(0)">
        <label for="pac-input">Search</label>
        <input type="text" id="pac-input">
        <!-- <button id="more">Search</button> -->
    </form>

    <!-- <script src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCCnP33_rNflEnRSLnih3sF7WoyEBlP0Uc&callback=initMap&v=weekly" defer></script> -->
    <script async src="https://maps.googleapis.com/maps/api/js?key=AIzaSyCCnP33_rNflEnRSLnih3sF7WoyEBlP0Uc&libraries=places&callback=initMap">
    </script>

    <script>
        // Initialize and add the map
        function initMap() {
            // The location of Uluru
            const uluru = {
                lat: -25.344,
                lng: 131.031
            };
            // The map, centered at Uluru
            const map = new google.maps.Map(document.getElementById("map"), {
                zoom: 4,
                center: uluru,
            });
            // The marker, positioned at Uluru
            const marker = new google.maps.Marker({
                position: uluru,
                map: map,
            });

            // PLACES
            // const center = {
            //     lat: 50.064192,
            //     lng: -130.605469
            // };
            // // Create a bounding box with sides ~10km away from the center point
            // const defaultBounds = {
            //     north: center.lat + 0.1,
            //     south: center.lat - 0.1,
            //     east: center.lng + 0.1,
            //     west: center.lng - 0.1,
            // };
            const input = document.getElementById("pac-input");
            const options = {
                // bounds: defaultBounds,
                componentRestrictions: {
                    country: "us"
                },
                fields: ["address_components", "geometry", "icon", "name"],
                strictBounds: false,
                // types: ["establishment"],
            };
            const autocomplete = new google.maps.places.Autocomplete(input, options);

            // Binding to allow the map to be changed
            autocomplete.bindTo("bounds", map);

            autocomplete.addListener("place_changed", () => {
                const place = autocomplete.getPlace();
                map.setCenter(place.geometry.location);
                map.setZoom(17);
            });


        }

        window.initMap = initMap;
    </script>
    <style>
        /* Set the size of the div element that contains the map */
        
        #map {
            height: 400px;
            /* The height is 400 pixels */
            width: 100%;
            /* The width is the width of the web page */
        }
    </style>

    <script>
    </script>

</body>

</html>
```
