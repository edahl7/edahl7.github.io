<html>
    <head>
      <title>A Leaflet map!</title>
      <link rel="stylesheet" href="https://unpkg.com/leaflet@1.2.0/dist/leaflet.css" />
      <link rel="stylesheet" href="leaflet-slider.css"/>
      <script src="https://unpkg.com/leaflet@1.2.0/dist/leaflet.js"></script>
      <script src="shp.js"></script>
      <script src="leaflet.shpfile.js"></script>
      <script src="catiline.js"></script>
      <script src ="leaflet-slider.js"></script>
      
      

      <style>
      #mapId{ width: 900px; height: 500px; }
      </style>
    </head>
    <body>        
        <div id="mapId"></div>
        <script>
            //Options for the shapefile being added, so anything that would be
            //done normally with L.geoJSON. is contained within here. So popups,
            //style or any options with the ohio map.

            function getFeature(value){
                var semesters = ["AU__00", "SP__01", "AU__01","SP__02", "AU__02", "SP__03","AU__03", "SP__04", "AU__04", "SP__05",
                             "AU__05", "SP__06", "AU__06","SP__07", "AU__07", "SP__08","AU__08", "SP__09", "AU__09", "SP__10",
                             "AU__10", "SP__11", "AU__11","SP__12", "AU__12", "SP__13","AU__13", "SP__14", "AU__14", "SP__15",
                             "AU__15", "SP__16", "AU__16","SP__17", "AU__17"];
                var semester = semesters[value]
                return semester;
            }
            //Sets color based of range, can be changed to anything
             function getColor(d) {
                
                return d > 1000 ? '#800026' :
                    d > 500 ? '#BD0026' :
                    d > 200 ? '#E31A1C' :
                    d > 100 ? '#FC4E2A' :
                    d > 50 ? '#FD8D3C' :
                    d > 20 ? '#FEB24C' :
                    d > 10 ? '#FED976' :
                    '#FFEDA0';
                }
                
           
            
        
            
           
            var map = L.map('mapId').setView([40,-83], 7);
            //var ohioMap = new L.Shapefile('ohio.zip',options);
            var slider = L.control.slider(function(value) {
                var options = {
                //Setting up popups
                onEachFeature:function(feature,layer){
                    if(feature.properties) {
                        layer.bindPopup(Object.keys(feature.properties).map(function(k) {
                            if(k === '__color__'){
                                return;
                            }
                            return k + ": " + feature.properties[k];
                        }).join("<br />"), {
                            maxHeight: 200
                        });              
                    }
                },

                //Style is set here, getColor function is called to find color
                style: function(feature) {
                    return {
                        opacity: 1,
                        fillOpacity: 1,
                        radius: 6,
                        weight: 1,
                        color: "black",
                        fillcolor: getColor(feature.properties[getFeature(value)])
                    }
                }
            }

            var newOhioMap = new L.Shapefile('ohio.zip',options).addTo(map);
            
            }, {
    		max: 34,
    		value: 1,
            step:1,
            showValue: false,
            size: '800px',
            collapsed: false,
            position: 'bottomleft',
    		id: 'slider'
		});



            //ohioMap.addTo(map);
            slider.addTo(map);
            map.touchZoom.disable();
            map.doubleClickZoom.disable();
            map.scrollWheelZoom.disable();
            map.boxZoom.disable();
            map.dragging.disable();

    </script>
    </body>
</html>

