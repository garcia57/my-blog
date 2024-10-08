---
layout: post
title: "API Magic with Zoho Deluge: \nAn In-Depth Guide with a real world example"
date: 2024-09-18T09:49:03Z
authors: ["Andy Garcia"]
categories: ["Zoho Deluge", "REST API"]
description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
thumbnail: "/assets/images/gen/blog/blog-21-thumbnail.webp"
image: "/assets/images/gen/blog/blog-21.webp"
comments: false

#meta_title: "API Magic with Zoho Deluge: An In-Depth Guide with a real world example"
#meta_description: This blog post will walk you through using external APIs within Zoho Deluge. You'll learn what Zoho Deluge is, how to leverage external APIs effectively, and see a real-world data science example in action. Additionally, the guide will cover best practices for storing and managing the data obtained from these APIs.
#meta_image: "/assets/images/og/og-twitter-blog-image.webp"
---

## Introducing Zoho Deluge: A Gateway to Harnassing Data Science

Picture this: You're at your desk a lead's contact details when your boss asks you to create a daily weather report on his favorite golf course. You consider using Python or C#, but it could take days—and you're not confident in those languages.

That's where a CRM's proprietary scripting language shines. Tools like Salesforce's Apex or Zoho's Deluge are designed to streamline CRM tasks, making it easy to create functions, automate workflows, and generate reports—no deep programming skills needed. Using a CRM's proprietary language is like having a specialized toolkit with high-performance tools crafted specifically for your car model. It ensures smoother, more powerful performance, maximizing coding efficiency and precise modifications compared to generic tools like Python or C#.

These proprietary languages offer many capabilities, with extensive documentation available online that I can't fully cover in a single blog post. Instead, I want to focus on fulfilling your pretend boss's request using Zoho's scripting language, Deluge, and REST APIs.

## Zoho Deluge and REST API

### Zoho Deluge:

Deluge, short for 'Data Enriched Language for the Universal Grid Environment,' is Zoho's proprietary scripting language. It offers various features, including:
- Built-in functions and wrappers tailored for Zoho applications
- No reliance on external libraries
- A design that prioritizes readability
- Fully normalized relational data models for applications
- Integrated query capabilities

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


Because Zoho Deluge is a proprietary language, there is currently no official way to write code locally from an editor like VS Code. Instead, Zoho integrates its CRM coding editors within various applications exclusively for Zoho users. Online sandbox editors [See here](https://deluge.zoho.com/tryout) are available but limited in the functions they can run. For this blog, I will assume you have access to Zoho's applications and will explain the code snippets I use.

If you want to learn more about Zoho Deluge, you can follow this link: [Click Me!](https://deluge.zoho.com/learndeluge#Welcome!)

### REST APIs

Imagine if apps were people at a networking event—APIs (Application Programming Interfaces) would be the translators making sure everyone understands each other. APIs enable different software applications to communicate, share data, and perform tasks. They have a set of rules and protocols that allows one software program to interact with another. 

REST APIs are one flavor of the API sundae. Others, like SOAP, RPC, and GraphQL, each bring their own twists to the table. But unlike the other API types, REST APIs <strong>dominate</strong> the API usage space. According to Postman, the number one tool for API management and design, [89%](https://analyzingalpha.com/api-statistics) of respondents in their 2022 survey reported their API architecture to be specifically designed around REST APIs.

REST APIs are the backbone of modern web services, powering everything from social media platforms to e-commerce sites. They facilitate data transfer between software using standard HTTP methods like ```GET```, ```POST```, ```PUT```, and ```DELETE```. For future coding, remember that REST APIs are stateless. Each client request is independent and must include all necessary information for processing.

What does that mean? Well imagine you're at a fast food counter, and each time you order, you (client) provide all the details needed (like your name, order, and payment) without relying on the cashier (server) to remember anything from your previous orders. Each order (or request) is independent and complete by itself. Here's a simple ```GET``` request example in Python:

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

For this blog walkthrough let's first define the project scope and all of the necessary pieces of data we want to handle.

### User Story
Assume your pretend boss's favorite golf course is Augusta National Golf Course. He wants Zoho Cliq (the 'Slack' of Zoho) to notify him. By inputting the command '/golftoday,' he'll receive the golf course weather data. He selects from your provided lists of common weather data points:

- Temperature: Recorded temperature (°F)
- Apparent Temp: Felt temperature by the body
- Precipitation: Chance of moisture (rain, sleet, hail)
- Rain: Likelihood of steady rain
- Showers: Likelihood of brief, intense rain
- Cloud Cover: Percentage of the sky covered by clouds
- Wind Speed: Wind speed (mph)
- Wind Direction: Measured clockwise from North (0° is North, 180° is South, 270° is West).

He suggested your command should also display both recorded and predicted hourly temperatures.

### Research
With the user story and data specified, we can proceed to research. Don't worry, this is a walkthrough blog and I've already done it for you. I'll briefly discuss my research process so you understand how the code will function further in the walkthrough:

1. We are pulling current data from external sources into Zoho Cliq, so REST APIs will be useful here
2. Open Metro has a free API that can obtain our data desired, but it requires both Longitude (-82.0200) and Latitude (33.5000)
3. Looking at a sample JSON response from the API pull there are obvious signs of various data transformation that will be needed
4. We will need to capture the transformed data into a Zoho Cliq Message Card
5. Zoho Cliq has a message builder that we can leverage to create both the list of current data points and a table of hourly temperatures

## Project Setup

First, we need the API response for the weather data. Follow along on your own device to see an example of an API response. Using [Open Metro's Documentation](https://open-meteo.com/en/docs#), we input the golf course's coordinates (-82.0200, 33.5000) from a quick Google search. We then specify our pretend boss's desired data in both 'Hourly' and 'Current' sections. For convenience, we switch the 'Settings' to the following:

- Temperature Unit: Fahrenheit °F
- Wind Speed Unit: Mph
- Precipitation Unit: Inch
- Timeformat: ISO 8601

You can click on this link for an example of what the API response looks like: [my API response](https://api.open-meteo.com/v1/forecast?latitude=33.5&longitude=-82.02&current=temperature_2m,relative_humidity_2m,apparent_temperature,precipitation,rain,showers,cloud_cover,wind_speed_10m,wind_direction_10m&hourly=temperature_2m&temperature_unit=fahrenheit&wind_speed_unit=mph&precipitation_unit=inch&forecast_days=1). 

### Root Level Keys and Nested Objects

For those who are new to JSON responses from APIs, I will quickly explain what you are seeing. If you are already familiar with root-level keys and nested objects, please feel free to skip this section.

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

Here, both ```time``` and ```temperature_2m``` are separate lists. Each index in ```time``` correlates with the same index in ```temperature_2m```, providing an hourly temperatures for the day. Some REST API responses output nested objects within nested objects. Thankfully, this weather response is simple enough without requiring any in-depth transformation 

### Design our Zoho Cliq Message Card

There are plenty of documentation detailing how to access the "Command" build-out tool from the "Bots & Tools" link in Zoho Cliq. Therefore I will not give you an in-depth explanation on setting this up. Just know that you will set up the command name as "/golftoday", the access settings as "personal", and select a chat window that no one in your company uses like a bot window. The outputs of your code will only be seen by you and they are removable within the chat window. You can also stylize the In-line message using Zoho Cliq's Message Builder by following the link [here](). I used Modern In-line and added a List (to display current weather data) and then a Table (to display today's hourly temperature) to my message. When your done styling your message copy the Deluge script and paste it in your open Zoho Cliq Command Editor window.

[Should I put an image here?]

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

## Step 2: Wind Direction

You may have noticed Open Metro's wind direction returns a numberical value (ex: 293) instead of wording like "North West". That is due to using Azimuths to represent cardinal directions. Well, knowing the wind is coming from angle 293 isn't super helpful when your trying to hit an eagle on the par-5 18th hole. So we need a dictionary of cardinal directions and the minimum angles that would be understood to reference them. In my code I use the following:

```js
windDegrees = {"North":0,"North Northeast":22.5,"Northeast":45,"East Northeast":67.5,"East":90,"East Southeast":112.5,"Southeast":135,"South Southeast":157.5,"South":180,"South Southwest":202.5,"Southwest":225,"West Southwest":247.5,"West":270,"West Northwest":292.5,"Northwest":315,"North Northwest":337.5};
```

When given an angle like 5° or 15°, it’s considered close enough to the North direction (0°) to be labeled as "North." However, when the angle reaches 21.5°, it still falls under the "North" category because it hasn't yet crossed the threshold of 22.5°, which marks the beginning of the "North Northeast" direction. Therefore, 21.5° is categorized as "North," not "North Northeast," which starts at 22.5°.

We will use a loop function to find the corresponding cardinal or intercardinal wind direction based on a given angle (degree) of wind direction. Here's my loop:

```js
//wind_dir grabs the wind_direction degree from the current_data object and converts it to a number
wind_dir = current_data.get("wind_direction_10m").toLong();
for each  entry in windDegrees
{
	next_value = entry + 22.5;
	if(wind_dir > entry && wind_dir < next_value || wind_dir == entry)
	{
		wind_dir = windDegrees.getKey(entry);
		break;
	}
}
```


<strong>Loop explained</strong>:

1. The loop goes through each entry in the windDegrees dictionary, which maps wind directions to their respective degree values.
2. For each entry, it calculates the next value (```entry``` + 22.5), establishing a range for that wind direction. For example, "North" corresponds to 0°, so its range would be 0° to 22.5°.
3. The loop checks if the given wind direction (```wind_dir```) falls within the current range or exactly matches the entry degree. For example, if ```wind_dir``` is 21.5°, it's within the range of 0° to 22.5° and hence is categorized as "North."
4. When it finds the matching range, it updates ```wind_dir``` with the corresponding key (wind direction name) from the dictionary and breaks the loop.


The same process is done for Cloud Cover using this dictionary and each ```next_value``` being the ```entry``` + 12.5

```js
cloudCover = {"Clear Sky":0,"Few Clouds":12.5,"Scattered Clouds":25,"Partly Cloudy":37.5,"Half Cloudy":50,"Mostly Cloudy":62.5,"Broken Overcast":75,"Overcast with Openings":87.5,"Overcast":100};
```

Every other ```current_data``` key value can simply be referenced with a ```.get(__)``` statement. Below is a snippet of that from my code. As you can see, I am getting the specific string value from the designated key with an attached symbol for styling. 
```js
  dataList1 = Map();
  dataList1.put("Apparent Temp",current_data.get("apparent_temperature") + " °F");
  dataList.add(dataList1);
  //
  dataList4 = Map();
  dataList4.put("Showers",current_data.get("showers") + " %");
  dataList.add(dataList4);
  //code
  dataList6 = Map();
  dataList6.put("Wind Speed",current_data.get("wind_speed_10m") + " Mph");
  dataList.add(dataList6);
  //more code

```

## Step 3: Hourly Temperature

We have now completed the current weather section but we are still left with the hourly weather portion of the project. From looking at the 2 nested lists in ```hourly_temp``` object, we will need a slightly more complex loop. 

The idea is to loop through the ```hourly_temp``` object, which contains two nested lists: one for <strong>time</strong> and one for <strong>temperature</strong>. For each time entry, we’ll convert it to a legible 12-hour format (e.g., 5 PM) and pair it with its corresponding temperature value (that we will grab using a ```count``` value as an index number). After each iteration, the ```count``` value will also be incremented by one to keep pace with the for loop. This data will then be added to ```rowsList``` list that represents rows of a table to be added in following snippets of code.

If you can, take a crack at building this loop on your own. If you don't have Cliq, you can work on your own "Hourly_temp" objected with the 2 nested list objects in the language you are most comforatble with. 

Here is my code:
```js

count = 0;
for each  index in timeList
{
	row_v = Map();
	row_v.put("Hour",index);
	dateString = timeList.get(count);
	dateTime = dateString.toDateTime("yyyy-MM-dd'T'HH:mm");
	date_hour = hour(dateTime);
	if(date_hour > 12)
	{
		new_val = date_hour - 12;
		date_hour = new_val + " PM";
	}
	else if(date_hour == 0)
	{
		date_hour = "12 AM";
	}
	else
	{
		date_hour = date_hour + " AM";
	}
	row_v.put("Hour",date_hour);
	row_v.put("Temp (°F)",tempList.get(count) + " °F");
	rowsList.add(row_v);
	count = count + 1;
}

```

<strong>Loop Explained</strong>:

1. ```count``` variable keeps track of the current index in the ```timeList``` to be used in getting specific key-values in ```tempList```.
2. The loop iterates through each entry in timeList.
3. For each iteration, my code retreives the corresponding dateString and converts it to a DateTime object.
4. This section extracts the hour from the DateTime object and formats it into a 12-hour AM/PM format. You'll notice that an additional if statement was created since ```date_hour``` = 0 is actually 12 AM. Hours greater than 12 reformatted and have the PM ending. All other hours are left alone with the AM ending.
5. I then grab the corresponding temperature value using ```count``` as an index number and add it to a map (```row_v```), which is then added to rowsList.
6. ```count``` is then incremented by 1 per loop iteration.


This loop then completes our data transformation in Zoho Deluge for the Hourly Temperature.

## Step 4: Putting it all together

With both the current and hourly tempertures complete, we then compile the two together with our Zoho Deluge Message Card syntax. The result should output a message that looks similar to this (I stylized mine for fun):

{% include framework/shortcodes/figure.html src="/assets/files/Zoho-Message-Card-Image.webp" title="The message card is scrollable, showing you the hourly temps as well" caption="Andy Garcia" alt="Photo of Zoho Cliq Command Style" link="https://figma.com" target="_blank" %}


## Conclusion


You can see the exact code I used here: [Link](https://garcia57.github.io/my-blog/assets/files/golf-today-zdeluge.txt). If you want to get even crazier with REST APIs you can configure one to trigger a Message Card to appear in softwares outside of Zoho Cliq as seen in these [documents](https://www.zoho.com/cliq/help/restapi/v2/#Message_Cards_Modern_inline). There's a lot you can do with REST APIs and I hope you take the time to read the mountains of documentation to learn what they do.

>  Mastering REST APIs is crucial in thriving in a tech-driven world.

I really hope this blog post helped you learn about proprietary scripting languages, REST APIs, and how to work with APIs in Zoho Deluge. There was a lot we didn't discuss so I hope you take time to research more about this side of programming. 

### Additional Resources

[Zoho Deluge - InvokeURL()](https://www.zoho.com/deluge/help/webhook/invokeurl-api-task.html)

[Zoho Deluge - Tutorial](https://deluge.zoho.com/learndeluge#Welcome!)

[Open Metro - Documentation](https://open-meteo.com/en/docs#)

[Zoho Cliq - Message Builder](https://cliq.zoho.com/messagebuilder#%7B%22text%22%3A%22Your%20message%20on%20the%20card%20goes%20here!%22%7D)

[Zoho Cliq - Message Builder Template 2](https://cliq.zoho.com/messagebuilder#%7B%22text%22%3A%22New%20interns%20will%20be%20joining%20these%20teams%20from%20July.%22%2C%22card%22%3A%7B%22title%22%3A%22ANNOUNCEMENT%22%2C%22thumbnail%22%3A%22https%3A%2F%2Fwww.zoho.com%2Fcliq%2Fhelp%2Frestapi%2Fimages%2Fannounce_icon.png%22%2C%22theme%22%3A%22modern-inline%22%7D%2C%22slides%22%3A%5B%7B%22type%22%3A%22table%22%2C%22title%22%3A%22Details%22%2C%22buttons%22%3A%5B%7B%22label%22%3A%22View%22%2C%22action%22%3A%7B%22type%22%3A%22open.url%22%2C%22data%22%3A%7B%22web%22%3A%22https%3A%2F%2Fimg.zohostatic.com%2Fchat%2Fdefault%2Fofficechat%2Fimages%2Fdefault%2Fsmile.png%22%7D%7D%2C%22type%22%3A%22%2B%22%7D%2C%7B%22label%22%3A%22Cancel%22%2C%22action%22%3A%7B%22type%22%3A%22open.url%22%2C%22data%22%3A%7B%22web%22%3A%22https%3A%2F%2Fimg.zohostatic.com%2Fchat%2Fdefault%2Fofficechat%2Fimages%2Fdefault%2Fsmile.png%22%7D%7D%2C%22type%22%3A%22%2B%22%7D%5D%2C%22data%22%3A%7B%22headers%22%3A%5B%22Name%22%2C%22Team%22%2C%22Reporting%20To%22%5D%2C%22rows%22%3A%5B%7B%22Name%22%3A%22Paula%20Rojas%22%2C%22Team%22%3A%22Zylker-Sales%22%2C%22Reporting%20To%22%3A%22Li%20Jung%22%7D%2C%7B%22Name%22%3A%22Quinn%20Rivers%22%2C%22Team%22%3A%22Zylker-Marketing%22%2C%22Reporting%20To%22%3A%22Patricia%20James%22%7D%5D%7D%7D%5D%7D)

