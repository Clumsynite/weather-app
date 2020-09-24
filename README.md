# Weather app

A weather app built by following [this lesson](https://www.theodinproject.com/courses/javascript/lessons/weather-app) from [TheOdinProject](https://www.theodinproject.com/).

Visit the live version [here](https://clumsynite.github.io/weather-app/)

## Introduction

1. The app will request for the user's location onload.
2. If allowed, weather at that location will be displayed using OpenWeatherMap API
3. Weather reports will be retrieved either if the location permissions are given or if the user searches for a city.
4. The background image, which is a gif will change automatically according to the weather. This feature takes Giphy API into use.
5. User can change the Standard Unit for temperature by click on it.
6. There's a 5 day forecast at the bottom of the weather card. Although, for now the forecast is only for 09:00 on that day.

## Project setup

```js
npm install
```

### Compiles and hot-reloads for development

```js
npm run serve
```

### Compiles and minifies for production

```js
npm run build
```

### Lints and fixes files

```js
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## Built using

* HTML
* CSS
* Javascript
* Vue (Framework)
* OpenWeatherMap (API)
* Giphy (API)
