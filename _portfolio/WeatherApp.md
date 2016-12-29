---
layout: post
title: WeatherApp
thumbnail-path: "img/WeatherApp.png"
short-description: WeatherApp is made for FreeCodeCamp
---

<p data-height="300" data-theme-id="27032" data-slug-hash="PbMwYq" data-default-tab="result" data-user="theXodus" data-embed-version="2" data-pen-title="Weather with Weather Underground" data-preview="true" class="codepen">See the Pen <a href="http://codepen.io/theXodus/pen/PbMwYq/">Weather with Weather Underground</a> by Jared Price (<a href="http://codepen.io/theXodus">@theXodus</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## Tech Used
* Jquery
* Javascript
* Tachyons

This weather app is part of the intermediate projects of FreeCodeCamp Frontend track. It started out really rough with the API they suggested using. So I decided to go with [WeatherUnderground](http://wunderground.com) API, which has far more features and better icons.

## User Stories
* User Story: I can see the weather in my current location
* User Story: I can see a different icon or background image (e.g. snowy mountain, hot desert) depending on the weather.
* User Story: I can push a button to toggle between Fahrenheit and Celsius.

## Problems
One of the main hangups on this project was getting data from the API, but that was more a problem with the API and Codepen. Instead of using the HTML5 Geolocation I found it much easier to get it from the IP address. The two main issues I'll talk about in this case study are cycling through the API object and switching between Imperial/Metric.

## API Object
data -> forecast -> simpleforecast -> forecastday
This is obvious if you're familiar with objects and this return back an array.
{% gist c83dd5168a34f2b5cf7300b4b9aae5cf %}
In this Gist you can see there are two different kinds of objects. The simpleforecast object holds the date and the txt_forecast does not. I select the first item in the array to set todays weather and then use a for loop to go set up the rest of the week.
{% gist 1d49e1010ee50f482a4e12dd9f319eb8 %}
I tried using the jQuery.each method but it was clunky and I found it harder to read than the plain Javascript code.

## Imperial/Metric
I decided to attach a data attribute to the button and have it be equal to whatever is being shown on the screen.
{% gist c76f189f73fea14c4eb8819ed0492144 %}
I decided to put the logic for this inside of the getLocation function since I didn't want to rewire everything. In $document.ready there's an if statement that sets the text and data of the button and then gets the location.
{% gist efa85ff5d99ad505113e56244f42c2d3 %}
In the getLocation function it checks the role of the button and then calls setWeather with the appropriate argument.
{% gist 8499a18670b21e966d327f5b1671d908 %}
This allows for us to check which data we want to assign to the variables we call from the api.

That wraps up what I learned from this really fun project. My understanding of APIs has really deepened and I'm sure it will increase even more in the next two challenges. 
