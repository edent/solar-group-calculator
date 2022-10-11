# Demo Solar Group Power Calculator

Last week, I attended [BrumPropHack](https://twitter.com/search?q=BrumPropHack&src=typed_query) - a hackathon in Birmingham which looked at problems with retrofitting homes to make them more energy efficient.

My demo was reasonably simple and (I thought) quite effective.  It displayed a satellite view of a street in the local area. By clicking on rooftops, virtual solar panels are added to properties. Click a button and it shows your community how much CO<sub>2</sub> they will save, and how much money they will make.

<img src="https://shkspr.mobi/blog/wp-content/uploads/2022/10/proptechhack.jpg" alt="Screenshot of the demo page. A street is covered in solar panel icons." width="1200" height="649" class="aligncenter size-full wp-image-43677" />

The demo worked (phew!) and received good feedback. For something built in a couple of hours, I'm pretty pleased with it.  The way it works is...

* Draw an interactive map using [Leaflet](https://leafletjs.com/).
* Use [ESRI's satellite images](https://www.arcgis.com/home/item.html?id=10df2279f9684e4a9f6a7f08febac2a9).
* A bit of JavaScript to detect click and draw a [cool solar panel icon](https://www.flaticon.com/free-icon/solar-energy_8598887?related_id=8598887&origin=pack).
* The EU has a great and [free API for calculating solar irradiance](https://joint-research-centre.ec.europa.eu/pvgis-photovoltaic-geographical-information-system/getting-started-pvgis/api-non-interactive-service_en). So this sends a request for each panel giving its latitude and longitude.
   * TODO! At the moment, it assumes all panels face south. A future version should let each icon be rotated to the correct degree.
   * TODO! Each icon represents 4kWp of solar. A future version should let each panel size be defined individually.
* Use [CORS Anywhere](https://github.com/Rob--W/cors-anywhere) because making JS requests is hard now.
* Do maths to work out the total installed capacity.
* Do maths to calculate the CO<sub>2</sub> value.
   * TODO! This is calculated as 0.256 Kg/kWh. I found that figure on a website. Might not be accurate for the UK.
* Do maths to calculate the cost savings.
   * TODO! It assumes 50% export at £0.25/kWh and 50% reduction in use at £0.50/kWh. That should be adjustable.
* Display a link encouraging people to sign up
   * TODO! Make the link go somewhere.
   * TODO! Display the cost to install - with suitable discounts for bulk buy.
   * TODO! Draw some pretty graphs
* Uses [Picnic CSS](https://picnicss.com/) to make it look pretty.

It is a rough and ready demo, but it does work!

## API

Taken from https://re.jrc.ec.europa.eu/pvg_tools/en/tools.html

Example link - https://re.jrc.ec.europa.eu/api/v5_2/PVcalc?lat=52.202&lon=-1.093&raddatabase=PVGIS-SARAH2&browser=1&userhorizon=&usehorizon=1&outputformat=json&js=1&select_database_grid=PVGIS-SARAH2&pvtechchoice=crystSi&peakpower=4&loss=14&mountingplace=free&angle=35&aspect=0

### Documentation

The only things we need to change are:

* `lat` is decimal latitude
* `lon` is decimal longitude
* `peakpower` is kWp (i.e. 4kWp is `4`)
* `aspect` is degrees from South (i.e South is `0`, East is `-90`, West is `90`)

```
https://re.jrc.ec.europa.eu/api/v5_2/PVcalc
	?lat=51.234
	&lon=-1.234
	&aspect=0
	&peakpower=4
	&raddatabase=PVGIS-SARAH2
	&outputformat=json
	&pvtechchoice=crystSi
	&loss=14
	&mountingplace=free
	&angle=35
```