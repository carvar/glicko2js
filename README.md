# Glicko 2 javascript implementation

The Glicko-2 rating system is a method for assessing a player's strength in games of skill, such as chess and go.
The algorithm is explained by its author, Mark E. Glickman, on http://glicko.net/glicko.html.

## Calculation principle

Each player begins with a rating, a rating deviation (accuracy of the rating) and a volatility (speed of rating evolution). These values will evolve according to the outcomes of matches with other players.

## Usage

You init a ranking and create players with initial ratings, rating deviations and volatilities.

``` javascript
var glicko2 = require('glicko2');
var settings = {
  tau : 0.5, // "Reasonable choices are between 0.3 and 1.2, though the system should be tested to decide which value results in greatest predictive accuracy."
  rating : 1500, //default rating
  rd : 200, //Default rating deviation (small number = good confidence on the rating accuracy)
  vol : 0.06 //Default volatility (expected fluctation on the player rating)
};
var ranking = new glicko2.Ranking(settings);

// Create players
var Ryan = ranking.makePlayer();
var Bob = ranking.makePlayer(1400, 30, 0.06);
var John = ranking.makePlayer(1550, 100, 0.06);
var Mary = ranking.makePlayer(1700, 300, 0.06);
```

You enter the results

``` javascript
ranking.startPeriod();
ranking.addResult(Ryan, Bob, 1); //Ryan won over Bob
ranking.addResult(Ryan, John, 0); //Ryan lost against John
ranking.addResult(Ryan, Mary, 0.5); //A draw between Ryan and Mary
ranking.stopPeriod();
```

You get the new rankings

``` javascript
console.log("Ryan new rating: " + Ryan.getRating());
console.log("Ryan new rating deviation: " + Ryan.getRd());
console.log("Ryan new volatility: " + Ryan.getVol());
```

## Install

glicko2.js is available as a npm module.

Install globally with:

``` shell
$ npm install -g glicko2
```