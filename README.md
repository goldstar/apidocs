![Goldstar icon](https://res.cloudinary.com/goldstar/image/upload/e_colorize,co_rgb:fec200,w_300/v1542129455/Goldstar_HorizontalLockup_Black.png)
# Goldstar Developer API


At Goldstar, we love live entertainment and we want to make it easy for everyone else to go out more and love it as much as we do.  The Goldstar Developer API makes it easy to build applications to help users find, discover and fall in love with live entertainment. The Goldstar API typically offers more than 3,000 unique events and 15,000 shows at any time across all 26 Goldstar territories. 


The API allows you to display event listings directly on your website or in your mobile app.  After finding a great concert, theater performance, comedy show or sporting event on your site or in your mobile app, the user is sent to Goldstar to select dates, times and seating sections before  completing their ticket purchase.  

If you’re interested in giving your users the opportunity to purchase Goldstar tickets directly in your shopping cart, visit <https://goldstar.com/company/distribution-partners> or [email us](partnerships@goldstar.com) to learn more about our transactional APIs.  

## Overview

The Goldstar Developer API is available in XML and JSON formats and allows you to tap into all of Goldstar’s events, or a subset of events based on the Territory or Category you’re interested in.  API calls return events in order of popularity with the most popular events listed first.

Goldstar’s feeds are not intended to be used in real time or by your individual users. We recommend that you request the feeds no more than once every 15 minutes.


Occasionally, we’re able to customize our API to fit the specific needs of our partners. If this is something you’d be interested in, [email us](partnerships@goldstar.com) and let us know what you’re looking for.


## Authentication

All requests to the Goldstar Developer API require a valid API key. If you have not yet obtained an API key, you can request one by [emailing us](partnerships@goldstar.com).  To authenticate your request, you’ll need to add your API key as a Bearer token to the `Authentication` HEADER of your request. Here is an example:

```
curl -i 'https://www.goldstar.com/api/territories.json' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

In addition, you’ll need an Affiliate ID in order to generate unique tracking links and get credit for the traffic you send to us. If you haven’t yet received an Affiliate ID, apply at <https://affiliates.goldstar.com/signup>.  


## Concepts

* **Categories**: Genres of events, like Music or Sports,  Goldstar offers.

* **Territories**: Geographic areas Goldstar has established that roughly map to U.S. Census Bureau Designated Market Areas (DMA).

* **Listings**: Event listings consist of a set of data fields like event title, description, dates, times, location, etc.


## Categories

Categories are genres of events Goldstar offers.  

The Category reflects  content (e.g. "Theatre"), context (e.g. "Good for Groups"), or a seasonal promotion (e.g. "Summer Concerts").  The list of potential Categories changes regularly with new Categories being added and others being deactivated. 


To obtain Goldstar’s current list of Categories, use the Categories Call:

JSON:
```
curl -i 'https://www.goldstar.com/api/categories.json' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

XML:
```
curl -i 'https://www.goldstar.com/api/categories.xml' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

#### Response Document:

| Property     | Type          | Description  |
| ------------ | ------------- | ------------ | 
| id		   | integer       | The Category's ID|
| name | string  | The name of the Category |

Once you have information about Goldstar’s current Categories, you can use the Category ID to request events in a particular Category.  Continue reading for additional information.

<br>
## Territories

“Territories” are geographic areas that Goldstar has established. They roughly map to [Designated Market Areas](https://www.nielsen.com/intl-campaigns/us/dma-maps.html).  This list changes infrequently.

To obtain Goldstar’s current list of Territories, use the Territories Call:

JSON:
```
curl -i 'https://www.goldstar.com/api/territories.json' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

XML:
```
curl -i 'https://www.goldstar.com/api/territories.xml' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

The response will include each Territory’s ID, name, slug, initials and time zone.

#### Response Document:

| Property     | Type          | Description  |
| ------------ | ------------- | ------------ | 
| id		   | integer       | The Territory's ID|
| name | string | The name of the Territory|
| slug | string | The URL slug used for the Territory|
| initials | string |The Territory’s initials |
| timezone | string |The Territory’s time zone |


Once you have information about Goldstar’s current Territories, you can use the Territory ID to request events in a particular Territory.


## Listings

The primary feed partners take advantage of Goldstar’s Listings Call.  By accessing the feed, you will be given a list of events available for your use.


Basic Listings Call: 

JSON:
```
curl -i 'https://www.goldstar.com/api/listings.json' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

XML:
```
curl -i 'https://www.goldstar.com/api/listings.xml' \
  -H "Authorization: Bearer token=<api_key goes here>"
```

#### Response Document:

| Property     | Type          | Description  |
| ------------ | ------------- | ------------ | 
| id | integer | The Event ID is our internal ID for the event object. You can use these IDs or map them to your own IDs| 
| type | string | Defines the object as an event
| title_as_text | string | The event’s title (text format)
| title_as_html | string | The event’s title (HTML format)
| headline_as_text | string | A headline that explains more about the event (text format)
| headline_as_html | string | A headline that explains more about the event (HTML format)
| summary_as_text | string | A description of the event (text format)
| summary_as_html | string | A description of the event (HTML format)
| upcoming_dates | array of objects | A list of all upcoming shows for the event
| upcoming_dates[].id | integer | The show ID
| upcoming_dates[].type | string | Defines the object as an event date
| upcoming_dates[].date | string | The show date, formatted as YYYY-MM-DD
| upcoming_dates[].date_and_time | string | The show’s date and time. Ex. “Saturday November 12, 2016 / 7:30 pm” 
| upcoming_dates[].time_note | string | This may be the show’s time, or it may include a note such as  “All Day” or “Every thirty minutes between Noon and 2pm” or even something like “Various Times,” which means that each specific offer has a time (like spa reservations). If you require  a specific time; in most cases, it is safe to to simply parse out the first time in the string and use that.  The end time is unknown to us for most of our events.
| our_price_range | string | Our price range for the event
| full_price_range | string | The full price range for the event
| image | string | Image URLs in the feed will link to a full-size image (largest we have). You may download this and resize, crop or thumbnail as necessary.  You may also add parameters to the image's URLs to have us resize or crop it using our Images API, which is discussed below.
| link | string | The event link to use, which will include your specific affiliate ID.  The URLs for each event are specific to your API key and may have tracking parameters. Please do not remove these.
| catetory_list | Array of objects | The list of Categories associated with the event
| category_list[].id | integer | The Category ID
| category_list[].name | string | The Category name
| category_list[].type | string | Defines the object as a Category
| venue | object | The location for the event.  Because our venue records change occasionally, we recommend that | you update each venue every time that you access the feed to keep your data up to date.
| venue[].id | integer | The venue ID
| venue[].name | string | The venue name
| venue[].address | object | The  venue’s address
| venue[].address[].id | integer | The address ID
| venue[].address[].street_address | string | The first line of the venue’s street address
| venue[].address[].extended_address | string | The second line of the venue’s street address.  This is often blank
| venue[].address[].locality | string | The city in which the venue is located
| venue[].address[].region | string | The state in which the venue is located
| venue[].address[].postal_code | integer | The venue’s five-digit Zip Code
| venue[].address[].country_name | string | The country in which the venue is located
| venue[].link | string | A link to the venue’s page on Goldstar
| venue[].image | string | The image associated with the venue
| venue[].geocode_latitude | number/string | The venue’s latitude (decimal degrees)
| venue[].geocode_longitude | number/string | The venue’s longitude (decimal degrees)
| venue[].phone | string | The venue’s phone number, using dashes
| venue[].capacity | integer | The venue’s capacity
| badges | string | This property has been deprecated  
| promotional_codes | string | If the event is included in one of Goldstar’s promotions, it will be indicated here. 
| featured | boolean | Indicates whether the event is featured
| sold_out | boolean | Indicates whether the event is sold out
| rating | decimal  | The event’s rating (value ranges from 0 to 5 in tenths)
| artists | array of objects | The artist(s) associated with the event. This is usually used with concert and comedy events, and is often blank
| artists[].id | integer | The artist’s ID
| artists[].name | string | The artist’s name
| artists[].type | string | Defines the object as an artist


The feed contains every event (that meets the filters provided) in its current state.  Events will change, go away, and possibly come back over time as show dates are added and expire.  By tracking the Event ID, you can update the content on your site as the new dates or prices are added to existing events.  *We strongly recommend that you update each event every time you access the feed to keep your data up to date.  If an event no longer appears in the feed, that means it's no longer available on Goldstar.*

The feed returns events in order of popularity, with the most popular events listed first.


#### Parameters:


You can tailor the results of the feed by passing parameters into the URL.  This will allow you to obtain events for a particular Territory, Category, date range, etc.  To create the parameter string, each parameter key and value should be encoded using the equals character (“=”) to create a key/value pair.  All key/value pairs should be concatenated together with the ampersand character (“&”).

| Property     | Type          | Description  |
| ------------ | ------------- | ------------ | 
| territory_id | integer | To limit the results to a specific Territory. Territory IDs can be found using the Territory call
| postal_code | integer | Adding postal_code is an alternative to territory_id. This will look up the postal_code's territory_id and scope the results based on the Territory.
| category_id | integer | Specifying category_id limits the results to a specific Category or Categories. You can get multiple Categories in one call.  Category IDs can be found using the Category call
| not_category_id | integer | This filters the results to exclude a specific Category
| from_date & to_date | strings |  This scopes your results to events with shows on the specified date or in the range.  Use the format YYYY-MM-DD. from_date can be used on its own, while to_date must be used in conjunction with from_date.  <br> <br> *For example,* to find all events with performances on January 1, 2017, add &from_date=2017-01-01&to_date=2017-01-10.  Add &from_date=2016-11-15 to find events with performances after November 11, 2016.  To find events with performances before a specific date, use &from_date=[current date]&to_date=[date specified]
| limit | integer | This reduces the number of results returned.  Since the results are sorted by popularity, this will give you the x most popular listings
| page & per_page | integers | These parameters can be used together to paginate the feed.  Use per_page to specify the number of events per page and page to specify the page


#### Example Request:

The top Concert event in Los Angeles:

```
curl -i 'https://www.goldstar.com/api/listings.xml?territory_id=1&category_id=66&limit=1' \
  -H "Authorization: Bearer token=<api_key goes here>"
```


#### Example Response:
	
	<listings>
		<event id="122129">
			<title_as_text>James Blake</title_as_text>
			<title_as_html>James Blake</title_as_html>
			<headline_as_text>James Blake: Grammy-Nominated Electro-Soul Star</headline_as_text>
			<headline_as_html>James Blake: Grammy-Nominated Electro-Soul Star</headline_as_html>
			<summary_as_text>
				Since dropping his breakthrough singles "The Wilhelm Scream" and his cover of Feist's 
				"Limit to Your Love" back in 2011, soul-stirring British crooner James Blake has 
				become one of the most sought-after artists in the electronic world. He's 
				collaborated with the likes of Beyoncé, Chance the Rapper and Drake and his moody, 
				haunting ballads have earned him a Mercury Prize, as well as a Grammy nod. With his 
				signature lilting vocals, heartfelt lyrics, layered synths and spare yet driving 
				beats, he's racked up one international hit after another, including "CMYK," "Life 
				Round Here" and "Retrograde." Now Blake heads to Santa Barbara's Arlington Theatre in 
				support of his critically acclaimed 2016 album "The Colour in Anything", which 
				features contributions from Frank Ocean, Rick Rubin and Justin Vernon of Bon Iver. 
				The theater's plush seating and elaborate al fresco décor make for the perfect 
				setting to take in intimate tracks like "Modern Soul" and "I Need a Forest Fire."
			</summary_as_text>
			<summary_as_html>
				Since dropping his breakthrough singles "The Wilhelm Scream" and his cover of Feist's 
				"Limit to Your Love" back in 2011, soul-stirring British crooner James Blake has 
				become one of the most sought-after artists in the electronic world. He's 
				collaborated with the likes of Beyoncé, Chance the Rapper and Drake and his moody, 
				haunting ballads have earned him a Mercury Prize, as well as a Grammy nod. With his 
				signature lilting vocals, heartfelt lyrics, layered synths and spare yet driving 
				beats, he's racked up one international hit after another, including "CMYK," "Life 
				Round Here" and "Retrograde." Now Blake heads to Santa Barbara's Arlington Theatre in 
				support of his critically acclaimed 2016 album <em>The Colour in Anything</em>, which 
				features contributions from Frank Ocean, Rick Rubin and Justin Vernon of Bon Iver. 
				The theater's plush seating and elaborate al fresco décor make for the perfect 
				setting to take in intimate tracks like "Modern Soul" and "I Need a Forest Fire."
			</summary_as_html>
			<upcoming_dates>
				<event_date id="1210172">
					<date>2016-10-18</date>
					<date_and_time>Tuesday October 18, 2016 / 8:00pm</date_and_time>
					<time_note>8:00pm</time_note>
				</event_date>
			</upcoming_dates>
			<our_price_range>$22.50 - $32.50</our_price_range>
			<full_price_range>$45</full_price_range>
			<image>
				https://i.gse.io/gse_media/116/9/1475805653-James_Blake_tickets.jpg?p=1
			</image>
			<link>
				https://tracking.goldstar.com/aff_c?aff_id=<your_id>&aff_sub4=james-
				blake&aff_sub=122129&offer_id=24
			</link>
			<category_list>
				<category id="66">
					<name>Concerts</name>
				</category>
				<category id="25">
					<name>Added This Week</name>
				</category>
			</category_list>
			<venue>
				<name>Arlington Theatre</name>
				<address>
					<street_address>1317 State Street</street_address>
					<extended_address/>
					<locality>Santa Barbara</locality>
					<region>CA</region>
					<postal_code>93101</postal_code>
					<country_name>United States</country_name>
				</address>
				<link>
					https://www.goldstar.com/venues/santa-barbara-ca/arlington-theatre
				</link>
				<image>
					https://i.gse.io/gse_media/images/000/006/661/1380813206-2191267110057532647 
					HjVepl_fs.jpg?p=1
				</image>
				<geocode_latitude>34.424597</geocode_latitude>
				<geocode_longitude>-119.706475</geocode_longitude>
				<phone>805-963-4408</phone>
				<capacity>2000</capacity>
			</venue>
			<badges/>
			<promotional_codes/>
			<featured>false</featured>
			<sold_out>false</sold_out>
			<rating>0</rating>
			<artists>
				<artist id="1908">
				<name>James Blake</name>
				</artist>
			</artists>
		</event>
	</listings>


## Images

Our Listings feed includes the full size image for an event. To obtain different image sizes, you can use our Images API. This is the only API that does not require you to include your API key.


There are two ways to determine the base URL for an event image. First, you can use the listings API to find the image URL. In the example above, the image URL for the second event is `https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg?p=1`.  Alternatively, you can find the event on <https://www.goldstar.com>, right-click on the event image, and select “Copy image address.”


#### Parameters

* `h=<pixels>` - To specify the maximum height (in pixels) for the image 
* `w=<pixels>` - To specify the maximum width (in pixels) for the image
* `c=1` - To crop the image. Use with the h & w options to get an exact size.
* `q=<quality>` - To adjust the jpeg compression (1-100)
* `p=1` - To use a progressive (interlaced) format if available


#### Example Requests

Full-size image:

	https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg


Fixed width image (height varies):

	https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg?w=200


Fixed height image (width varies):

	https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg?h=200


Max size (width or height will vary):

	https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg?h=200&w=100


Cropped image:
	
	https://i.gse.io/gse_media/116/8/1474568117-kygo_tickets.jpg?h=200&w=100&c=1


## Support

If you have any questions or need any help with the Goldstar Developer API, or [email us](partnerships@goldstar.com) and we’ll get back to you as soon as possible.
