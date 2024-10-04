---
layout: post
title: "API Magic with Zoho Deluge: A Hands-On Guide for Data Science Enthusiasts"
date: 2024-09-18T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Zoho Deluge", "REST API"]
description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
thumbnail: "/assets/images/gen/blog/blog-21-thumbnail.webp"
image: "/assets/images/gen/blog/blog-21.webp"
comments: false

meta_title: "API Magic with Zoho Deluge: A Hands-On Guide for Data Science Enthusiasts"
meta_description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
meta_image: "/assets/images/og/og-twitter-blog-image.webp"
---

## Introduction to Zoho Deluge and Its Relevance for Data Science

Picture this: You're at your desk working on a lead when your boss asks you to create a daily weather report on his favorite golf course. You consider using Python or C#, but it could take days—and you're not confident in those languages.

That's where a CRM's proprietarys scripting language shines. Tools like Salesforce's Apex or Zoho's Deluge are built to steamline CRM tasks by making it easy to create functions, automate workflows, and generate reports—no deep programming skills required. Using a CRM's proprietary programming language is like having a specialized toolkit with high-performance tools crafted for your car model. It guarantees smoother, more powerful rides, maximizing coding efficiency and precision modifications compared to generic tools like Python or C#.

There's a lot you can do with these proprietary languages and there a designated web pages full of documentation that I cannot fully encapuslate in a single blog post. Instead, I want to focus on fulfilling your pretend boss' request using Zoho's scripting language, Deluge, and REST APIs.

## Zoho Deluge and REST API

### Zoho Deluge:

Deluge, short for "Data Enriched Language for the Universal Grid Environment", is Zohos proprietarys scripting language. It comes with a variety of features such as:
- built-in functions and wrappers that are specifically tailored for Zoho applications
- no reliance on external libraries
- designed to be easy to read
- applications built with the language have fully normalized relational data models
- the language is query-integrated

Here are some examples of the language:

```js
// Fetch the contact record
contact = zoho.crm.getRecordById("Contacts", contact_id);

// Update the phone number
contact.get("Phone") = "123-456-7890";

// Save the updated record
zoho.crm.updateRecord("Contacts", contact_id, contact);
```

<!--https://www.zoho.com/deluge/help/deluge-editor.html#:~:text=Deluge%20editor%20allows%20users%20to%20dry%20run%20their%20scripts%20using-->

Because Zoho Deluge is a proprietary language, there is currently no official method to write code locally from an editor (such as VS Code). Instead, Zoho integrates their CRM coding editors within its various applications for Zoho users exclusively. There are online sandbox editors [See here](https://deluge.zoho.com/tryout), but those are limited in the type of functions they are allowed to run. Nevertheless, I will continue this blog assuming you have access to any of Zoho's applications and will demonstrate my code snippets used.

If you want to learn more about Zoho Deluge, you can follow this link: [Click Me!](https://deluge.zoho.com/learndeluge#Welcome!)

### REST APIs

If you've ever delved into the world of programming or software design, chances are you know about REST APIs. If not, then I highly recommend you take some time to do a deep dive into learning all aobut it. REST APIs are the backbone of modern web services, powering everything from social media platforms to e-commerce sites. They are extremely common and learning about how they transfer data between different softewares using their standard HTTP methods like ```GET```, ```POST```, ```PUT```, and ```DELETE``` will do wonders for your future interactions with software. For the sake of brevity-and mataining focus on the article topic-, know that REST APIs are stateless, meaning that requests from a client to a server are independent and must contain all the information needed to process the request.

Still confused? Well imagine you're at a fast food counter, and each time you order, you (client) provide all the details needed (like your name, order, and payment) without relying on the cashier (server) to remember anything from your previous orders. Each order (or request) is independent and complete by itself. Here's a simple ```GET``` request example in Python:


```python
import requests

# Define the API endpoint
url = "https://api.example.com/data"

# Send a GET request to the API
response = requests.get(url)

# Check if the request was successful
if response.status_code == 200:
    # Parse the JSON response
    data = response.json()
    # Print the data
    print(data)
else:
    print(f"Failed to retrieve data: {response.status_code}")
```

Another important aspect of REST APIs is their nature of returning data in a structured format like JSON or XML.

## Begin with the User Story and Research

For this blog walkthrough lets first define the project scope and all of the necessary pieces of data we want to handle.

### User Story
Let's assume that your pretend boss' favorite golf course is Augusta National Golf Course and he wants the notification on Zoho Cliq (the "Slack" of Zoho). He wants to be able to input a Zoho Cliq command "/golftoday" and receive weather data on the golf course. He selects the following data points from your provided lists of common weather data: 

- Temperature: Actual recorded temperature (in Farenheit)
- Apparent Temp: Temperature as it is felt by the human body
- Precipitation: Overall probability of moisture from the sky whether it’s rain, sleet, or hail.
- Rain: Probability of rain hitting the course. Steady precipitation that covers a large area.
- Showers: Probability of showers hitting the course. Brief, often intense, moments of precipitation.
- Cloud Cover: Percentage of the sky obscured by clouds when observed from the course.
- Wind Speed: Speed of the wind in miles per hour
- Wind Direction: The angle measured clockwise from the north direction on a compass (Azimuths). For example, 0° is North, 180° is South, and 270° is West.

He also commented that it would be cool if your command could also display the recorded and predicted hourly temperatures of the day. 

### Research

With the user story obtained and data specified, we can proceed into reasearching the topic. Don't worry, this is a walkthrough blog and I've already done the research for you. I will just briefly discuss my research process so you understand how our code will function further in the walkthrough. 

1. We are pulling current data from external sources into Zoho Cliq, so REST APIs will be useful here
2. Open Metro has a free API that can obtain our data desired, but it requires both Longitude (-82.0200) and Latitude (33.5000)
3. Looking at a sample JSON response from the API pull there are obvious signs of various data transformation that will be needed
4. We will need to capture the transformed data into a Zoho Cliq Message Card
5. Zoho Cliq has a message builder that we can leverage to create both the list of current data points and a table of hourly temperatures

## Project Setup

From our research we know the first thing we need is the API response for the weather response. I would highly recommend you follow along with this process to see what an API response looks like. From [Open Metro's Documentation](https://open-meteo.com/en/docs#) we input our Longitude (-82.0200) and Latitude (33.5000) coordinate. We then specify our pretend boss' desired data values in both the 'Hourly' and 'Current' sections. For convenience, we switch the 'Settings' to the following:
- Temperature Unit: Fahrenheit °F
- Wind Speed Unit: Mph
- Precipitation Unit: Inch
- Timeformat: ISO 8601

You can click on this link for an example of what the API response looks like: [my API response](https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1). 

### Root Level Keys and Nested Objects

For those who are new to JSON responses from API, I will quickly explain what your are seeing. If you are already familiar with this root level keys and nested objects, please feel free to skip this section.

<strong>Root Level Keys:</strong> These are the top-most keys in a JSON response. They contain direct values or nested objects. They act like the main categories of your data. In your JSON response, these would be those root level keys:

```JSON
{
    "latitude": 33.505344,
    "longitude": -82.02521,
    "generationtime_ms": 0.10502338409423828,
    "utc_offset_seconds": 0,
    "timezone": "GMT",
    "timezone_abbreviation": "GMT",
    "elevation": 89,
    ...
}
```

<strong>Nested Objects:</strong> These are objects within root-level keys. They help organize related data efficiently, grouping complex data into sub-categories. In your JSON response, these would be the nested objects:

```JSON
{
    "current_units": {
        "time": "iso8601",
        "interval": "seconds",
        "temperature_2m": "°F",
        ...
    },
    "current": {
        "time": "2024-10-04T17:00",
        "interval": 900,
        "temperature_2m": 79.7,
        ...
    },
    ...
}
```

Some nested objects can contain multiple lists instead of key-value pairs. This is visible in your ```hourly``` nested object within that JSON response:

```JSON
"hourly": {
        "time": [
            "2024-10-04T00:00",
            "2024-10-04T01:00",
            "2024-10-04T02:00",
            "2024-10-04T03:00",
            ...
        ],
        "temperature_2m": [
            78.1,
            76.4,
            73.7,
            72.4,
            ...
        ]
    }
```

Here, both ```time``` and ```temperature_2m``` are seperate lists. Why? Well, each index in the ```time``` list correlates directly with the same index in the ```temperature_2m list```, providing an hour-by-hour record of temperatures for the current day. I have seen some REST API responses output nested objects within nested objects. Thankfully, the weather response here is simple enough to work with without any in-depth transformation.


### Design our Zoho Cliq Message Card

There are plenty of documentation detailing how to access the "Command" build-out tool from the "Bots & Tools" link in Zoho Cliq. Therefore I will not give you an in-depth explanation on setting this up. Just know that you will set up the command name as "/golftoday", the access settings as "personal", and select a chat window that no one in your company uses like a bot window. The outputs of your code will only be seen by you and they are removable within the chat window. You can also stylize the In-line message using Zoho Cliq's Message Builder by following the link [here](). I used Modern In-line and added a List (to display current weather data) and then a Table (to display today's hourly temperature) to my message. When your done styling your message copy the Deluge script and paste it in your open Zoho Cliq Command Editor window.

[Image HEREEEE]

## Step 1: invokeURL

With both our API Url createed and our Zoho Cliq Message Post code pasted we can finally begin building code to connect the two ends. 

To begin, we will pull the data using Zoho's invokeURL() function. This function invokes the URL provided to obtain a response. Here is an example of the invokeURL command I use in my code:

```js
 header_data  = Map();
 param_data = Map();

 response = invokeUrl
 [
 url:  "https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1"
 type: GET
 parameters: param_data.toString()
 headers: header_data
 ];
```

In my invokeUrl function you can see that I am getting (```GET```) data from the url and inputting it into my ```response``` object. You will also notice that I did not specify the header and parameter data. That is due to the simplicity of Open Metro's API. Because there is no requirement for header or parameter data, you can see what data your receiving by pasting the url into your browsers url.

For ease in future coding reading, I also create objects that take sections of the reponse for use in data transformation and data translation. That way, when I'm working on the Current Weather side of this project, I reference the ```current_data``` object. And when I'm working on the hourly weather portion, I reference the ```hourly_temp``` object. 

```js
current_data = response.get("current");
hourly_temp = response.get("hourly");
```

### Is requesting data via API always this easy?

Sadly, no. Other APIs have requriements for certain parameters in order to recieve a response. You can fulfill this in Zoho Deluge by adding those mandated values into seperate maps and putting it into the the header/parameter map respectively.

Let's say Open Metro required a ```date_range``` from a ```start_date``` and ```end_date``` to pull weather on those days:
```js
 date_range = Map();
 date_range.put("start_date","10-01-2024");
 date_range.put("start_date","10-01-2024");
 
 header_data  = Map();
 param_data = Map();
 param_data.put("date_range", data_range);

 response = invokeUrl
 [
 url:  "https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1"
 type: GET
 parameters: param_data.toString()
 headers: header_data
 ];
```

You would repeat this process for any other parameters desired.

### Step 2: Wind Direction

You may have noticed Open Metro's wind direction returns a numberical value (ex: 293) instead of wording like "North West". That is due to using Azimuths to represent cardinal directions. Well, knowing the wind is coming from angle 293 isn't super helpful when your trying to hit an eagle on the par-5 18th hole. So we need a dictionary of cardinal directions and the minimum angles that would be understood to reference them. In my cod I use the following:

```js
windDegrees = {"North":0,"NorthNorthEast":22.5,"NorthEast":45,"EastNorthEast":67.5,"East":90,"EastSouthEast":112.5,"SouthEast":135,"SouthSouthEast":157.5,"South":180,"SouthSouthwest":202.5,"Southwest":225,"WestSouthwest":247.5,"West":270,"WestNorthwest":292.5,"Northwest":315,"NorthNorthwest":337.5};
```









<!--Testing Copy blocks-->
##
<tab><tab>code/text here

##
    fdslfdsf


### Extracting and Transforming pulled Data






```js
$ curl "https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1"
```
<!--
https://www.zoho.com/cliq/help/platform/cliq-objects/message-object.html#:~:text=The%20card%20property%20in%20a%20message%20object%20can%20be%20used
-->

<!--
https://www.zoho.com/cliq/help/restapi/v2/#Message_Cards_Modern_inline
-->

<!--
https://cliq.zoho.com/messagebuilder#%7B%22text%22%3A%22New%20interns%20will%20be%20joining%20these%20teams%20from%20July.%22%2C%22card%22%3A%7B%22title%22%3A%22ANNOUNCEMENT%22%2C%22thumbnail%22%3A%22https%3A%2F%2Fwww.zoho.com%2Fcliq%2Fhelp%2Frestapi%2Fimages%2Fannounce_icon.png%22%2C%22theme%22%3A%22modern-inline%22%7D%2C%22slides%22%3A%5B%7B%22type%22%3A%22table%22%2C%22title%22%3A%22Details%22%2C%22buttons%22%3A%5B%7B%22label%22%3A%22View%22%2C%22action%22%3A%7B%22type%22%3A%22open.url%22%2C%22data%22%3A%7B%22web%22%3A%22https%3A%2F%2Fimg.zohostatic.com%2Fchat%2Fdefault%2Fofficechat%2Fimages%2Fdefault%2Fsmile.png%22%7D%7D%2C%22type%22%3A%22%2B%22%7D%2C%7B%22label%22%3A%22Cancel%22%2C%22action%22%3A%7B%22type%22%3A%22open.url%22%2C%22data%22%3A%7B%22web%22%3A%22https%3A%2F%2Fimg.zohostatic.com%2Fchat%2Fdefault%2Fofficechat%2Fimages%2Fdefault%2Fsmile.png%22%7D%7D%2C%22type%22%3A%22%2B%22%7D%5D%2C%22data%22%3A%7B%22headers%22%3A%5B%22Name%22%2C%22Team%22%2C%22Reporting%20To%22%5D%2C%22rows%22%3A%5B%7B%22Name%22%3A%22Paula%20Rojas%22%2C%22Team%22%3A%22Zylker-Sales%22%2C%22Reporting%20To%22%3A%22Li%20Jung%22%7D%2C%7B%22Name%22%3A%22Quinn%20Rivers%22%2C%22Team%22%3A%22Zylker-Marketing%22%2C%22Reporting%20To%22%3A%22Patricia%20James%22%7D%5D%7D%7D%5D%7D
-->


Code:
```js

// Here we create 2 Map objects that will be used to transform numberical data from the REST API response into understandable language
// Wind Degrees Map transforms degrees to cardinal directions
windDegrees = {"North":0,"NorthNorthEast":22.5,"NorthEast":45,"EastNorthEast":67.5, "East":90,"EastSouthEast":112.5, "SouthEast":135,"SouthSouthEast":157.5,"South":180,"SouthSouthwest":202.5,"Southwest":225, "WestSouthwest":247.5,"West":270, "WestNorthwest":292.5,"Northwest":315, "NorthNorthwest":337.5};
// CloudCover transforms the "cloud coverage percentage" mathematical process into common words 
cloudCover = {"Clear Sky":0,"Few Clouds":12.5,"Scattered Clouds":25,"Partly Cloudy":37.5, "Half Cloudy":50,"Mostly Cloudy":62.5,"Broken Overcast":75,"Overcast with Openings":87.5,"Overcast":100};
//-----------------------------
//
// Step 1: Pull the full Data from Metrosfofesj via REST API into the "response" object
 raw_data = Map();
 header_data  = Map();
 response = invokeUrl
 [
 url:  "https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1"
 type: GET
 parameters: raw_data.toString()
 headers: header_data
 ];
 //
 // Step 2: We grab the two sections of data that we will be using in our Cliq Command. 
 current_data = response.get("current");
 hourly_temp = response.get("hourly"); 
 //
 // Step 3: Converting Wind Degrees to Cardinal directions. 
 // Here we optain the wind direction value and then find the term the number would refer to using our windDegree Map
 wind_dir = current_data.get("wind_direction_10m").toLong();
 
 for each entry in windDegrees{
	 next_value = entry + 22.5;
	 if(wind_dir > entry &&   wind_dir < next_value || wind_dir == entry){
		 wind_dir = windDegrees.getKey(entry);
		 break;
	 }
 }

 // Step 4: Converting Cloud Cover percentage to common wording
 // We follow the same process as in Step 3 but with cloudCover Map
 cloud_cov = current_data.get("cloud_cover").toLong();
 
for each  thing in cloudCover {
	next_value = thing + 12.5;
	if(cloud_cov > thing && cloud_cov < next_value || cloud_cov == thing) {
		cloud_cov = cloudCover.getKey(thing);
		break;
	}
}
 
// Step 5: Creating the Card Message in Zoho Cliq:
// Here we use the generated template found on the Zoho Cliq site
golf_msg = map();
bot = map();
bot.put("name","Weather Report");
golf_msg.put("bot",bot);
card = map();
card.put("title","Augusta National Golf Club");
card.put("thumbnail","https://img.freepik.com/premium-vector/golf-icon-logo-vector-design-template_827767-1928.jpg?w=900");
card.put("theme","modern-inline");
golf_msg.put("card",card);
slidesList = list();
slidesList0 = map();
slidesList0.put("type","label");
slidesList0.put("title","Current Weather");
buttonsList = list();
buttonsList0 = map();
buttonsList0.put("label","Google Maps");
buttonsList0.put("type","+");
action = map();
action.put("type","open.url");
data = map();
data.put("web","https://maps.app.goo.gl/BgT7d8oJEToZc1NB6");
action.put("data",data);
buttonsList0.put("action",action);
buttonsList.add(buttonsList0);
slidesList0.put("buttons",buttonsList);
// Here we put in the values we optained from the current temp
dataList = list();
dataList0 = map();
dataList0.put("Temperature", current_data.get("temperature_2m")+" °F");
dataList.add(dataList0);
dataList1 = map();
dataList1.put("Apparent Temp",current_data.get("apparent_temperature")+" °F");
dataList.add(dataList1);
dataList2 = map();
dataList2.put("Precipitation",current_data.get("precipitation")+" %");
dataList.add(dataList2);
dataList3 = map();
dataList3.put("Rain",current_data.get("rain")+" %");
dataList.add(dataList3);
dataList4 = map();
dataList4.put("Showers",current_data.get("showers")+" %");
dataList.add(dataList4);
dataList5 = map();
dataList5.put("Cloud Cover",cloud_cov);
dataList.add(dataList5);
dataList6 = map();
dataList6.put("Wind Speed",current_data.get("wind_speed_10m")+" Mph");
dataList.add(dataList6);
dataList7 = map();
dataList7.put("Wind Direction",wind_dir);
dataList.add(dataList7);
slidesList0.put("data",dataList);
slidesList.add(slidesList0);
// Step 6: Creating the Hourly Table as part of the message
// Column 1: Here we will create a table that takes in the date-time in iso8601 format and covert it to a 12hr format
// Column 2: While we are formating the time value, we will also input the temperature value in the same row for colummn 2
// Below is the general creation of the title and table
slidesList1 = map();
slidesList1.put("type","table");
slidesList1.put("title","Hourly Temperature");
data = map();
headersList = list();
headersList.add("Hour");
headersList.add("Temp (°F)");
data.put("headers",headersList);
rowsList = list();
timeList = hourly_temp.get("time");
tempList = hourly_temp.get("temperature_2m");
// We create a running count to designate what temperature value to grab as we loop through the dateTime MAP
count = 0;
// During each loop we grab that index's DateTime value and convert it from string to Date-Time Data type
// We will then grab the hour of that time and then transform to be used in 12 hour format with its relevant AM/PM
// After each loop, we append the Hour and Temp into a Map ("aka the next row of the table") and add it to our rowsList. 
for each index in timeList
{
	row_v = Map();
	row_v.put("Hour", index);
	dateString = timeList.get(count);
	dateTime = dateString.toDateTime("yyyy-MM-dd'T'HH:mm");

	testy = hour(dateTime);
	if (testy > 12){
		new_val = testy - 12;
		testy = new_val + " PM";
	} else if (testy == 0){
		testy = "12 AM";
	} else {
		testy = testy + " AM";
	}
	
	row_v.put("Hour",testy);
    row_v.put("Temp (°F)", tempList.get(count) + " °F");
    rowsList.add(row_v);
	count = count + 1;
}
// After we grab all of our rows into our "rowsList" we then add them to our slidesList1 object, which is the second part of our Cliq command.
data.put("rows",rowsList);
slidesList1.put("data",data);
slidesList.add(slidesList1);
golf_msg.put("slides",slidesList);
golf_msg.put("text","This is the current and hourly weather at the golf course");
return golf_msg;

```









### invoke 




<!--
lati: 33.5000 
long: -82.0200
-->

<!--
https://open-meteo.com/
-->

<!--
$ curl "https://api.open-meteo.com/v1/forecast?latitude=52.52&longitude=13.41&current=temperature_2m,wind_speed_10m&hourly=temperature_2m,relative_humidity_2m,wind_speed_10m"
-->





## Setting Up Zoho Deluge for API Integration

## How to Integrate External APIs with Zoho Deluge

## Storing and Managing API Data in Zoho

## Implementing a Real-World Use Case: A Step-by-Step Example

## Tips for Optimizing API Integration in Zoho Deluge



## Conclusion and Additional Resources

[Zoho Deluge - InvokeURL()](https://www.zoho.com/deluge/help/webhook/invokeurl-api-task.html)

[Zoho Deluge - Tutorial](https://deluge.zoho.com/learndeluge#Welcome!)

[Open Metro - Documentation](https://open-meteo.com/en/docs#)

[Zoho Cliq - Message Builder](https://cliq.zoho.com/messagebuilder#%7B%22text%22%3A%22Your%20message%20on%20the%20card%20goes%20here!%22%7D)

##



John Gruber created the [Markdown](#) language in 2004 in collaboration with Aaron Swartz on the syntax, with the goal of enabling people "to write using an easy-to-read and easy-to-write plain text format". Its key design goal is readability. That the language be readable as-is.

> "Markdown is a lightweight markup language with plain-text-formatting syntax"

To this end, its main inspiration is the existing conventions for marking up plain text in email, though it also draws from earlier markup languages, notably setext, Textile, and reStructuredText.

## Markdown Flavours

From 2012, a group of people including Jeff Atwood and John MacFarlane launched what Atwood characterized as a standardization effort. A community website now aims to "document various tools and resources available to document authors and developers, as well as implementors of the various markdown implementations".

![image]({{ site.baseurl }}/assets/images/gen/content/content-4.webp){:class="custom-image-class"}
{% include framework/shortcodes/image.html src="/assets/images/gen/content/content-3.webp" %}

### GitHub Flavored Markdown (GFM)

In 2017, GitHub released a formal specification of their GitHub Flavored Markdown (GFM) that is based on CommonMark. It follows the CommonMark specification exactly except for tables, strikethrough, autolinks and task lists, which the GitHub spec has added as extensions. GitHub also changed the parser used on their sites accordingly, which required that some documents be changed. For instance, GFM now requires that the hash symbol that creates a heading be separated from the heading text by a space character.he user to create their own.

{% include framework/shortcodes/figure.html src="/assets/images/gen/content/content-2.webp" title="There are many popular text editors for Markdown" caption="VSCode Editor" alt="Photo of designing a website in Figma" link="https://figma.com" target="_blank" %}

### Markdown Extra

Markdown Extra is a lightweight markup language based on Markdown implemented in PHP (originally), [Python](#) and [Ruby](#). It adds features not available with plain Markdown syntax. Markdown Extra is supported in some content management systems such as, for example, Drupal.

### MDX

At the same time, a number of ambiguities in the informal specification had attracted attention.These issues spurred the creation of tools such as Babelmark to compare the output of various implementations, and an effort by some developers of Markdown parsers for standardisation. However, Gruber has argued that complete standardization would be a mistake:

```js
$(window).scroll(function () {
  // this will work when your window scrolled.
  var scroll = $(window).scrollTop();
  if (scroll > 100) {
    $(".header").addClass("header-scrolled");
  } else {
    $(".header").removeClass("header-scrolled");
  }
});
```

Gruber avoided using curly braces in Markdown to unofficially reserve them for implementation-specific extensions. Markdown Extra adds the following features to Markdown:

- markdown markup inside HTML blocks
- elements with id/class attribute
- fenced code blocks that span multiple lines of code
- tables
- definition lists
- footnotes
- abbreviations
