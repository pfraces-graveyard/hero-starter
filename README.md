Javascript Battle - Hero Starter Repo
=====================================

**THE YELLOW KING AND THE BLUE KING ARE AT WAR! YOUR JAVASCRIPT SKILLS ARE NEEDED TO DETERMINE THE VICTOR: CAN YOU CODE 
AN EFFECTIVE AI FOR HONOR AND GLORY?**

Usage
-----

Javascript Battle is a fun and engaging web application intended to make artificial intelligence (AI) design accessible 
to all.

Every day, your code will be pulled down from your `hero-starter` repository on Github.

Your code (along with every other user's code) will be run daily, behind the scenes, in our game engine.

[Watch](http://javascriptbattle.com/#replay) tomorrow's game and see how your hero does. Each day is going to offer a 
unique battle as each player alters which hero they decide to play with.

Don't see your hero? After you've been signed up for at least one day, you can login to see your hero's battle in 
particular.

You can login to check out your hero's personal stats, or check out the leaderboard to see how you rank up against the 
competition. 

If we can make our site better in any way or make any instructions or code more explicit, please let us know. Until 
then, may the javascripts be with you!

Improve your heroe
------------------

**We only pull down your `hero.js` and `helpers.js` files**, so any changes you make must be in those files.

Once you get acclimated to the different types of heroes and think you want to give writing your own hero a shot, try 
altering some of the code. Maybe you want your miner to wait a little longer before going to a health well? What if your
health nut was aware of where the nearest enemy was and tried to keep away? How about if the aggressor became a real 
berserker? The possibilities are endless!!! And that is exactly how we want it. Go crazy and change your hero however 
you want. **Just remember to push your changes to your github repo**.

If you are looking for even more of a challenge, go ahead and take a look at the `helpers.js` file and begin picking 
apart our helper methods. Is there anyway you could adapt our pathfinding algorithm and use a variant in your `hero.js` 
file? What other helper methods should be available to your hero that we did not include? Go ahead and make any changes 
you want to the `helpers.js` file.

Testing
-------

### Test your hero code

    npm install
    npm test
    
### Put your hero in a mini-battle

    node test_your_hero_code.js
        
This will run and print out the results of a "mini-game" of only 15 turns which takes place on a 5x5 game board against 
a single enemy hero.

The command line will output what the board looks like at each turn, and will output the moves your hero tried to make
each turn.

*   Your hero will be denoted by the code "H00", the enemy hero will be denoted by the code "H01"
*   Diamond mines will be denoted by "DXX" where the Xs are numbers
*   Health wells will be denoted by "WWW"

Remember, `test_your_hero_code.js` is there for you! Feel free to modify it however you like.

### Online simulator

We have a user-friendly testing site where you can upload your hero.js file and see immediate results in a simulated 
game. Check it out [here](http://codetester.javascriptbattle.com/).

Rules
-----

If you want to update your hero, you will benefit from knowing a little more about the rules of the game. Below, you 
will find detailed descriptions of the different aspects of the game logic.

*   Each day your code will be assigned to a team at random and will fight for that king.
*   Your hero's code will decide which way to travel depending on the state of the game. There are only 5 choices: 
    "North", "South", "East", "West", and "Stay" (anything else will be interpreted by the game as "Stay").
*   Every hero starts with the full 100 health points. If your hero's health drops below 0, it will die.

### Win Conditions

The game is decided in one of two ways:

1.  A team eliminates all of the other team's heroes or...
2.  After 1250 turns, a team collects the most diamonds

Your hero has the potential to behave in any way you program it, either by re-writing the code yourself or simply by 
replacing your hero type with another pre-defined hero type. Every change you make could alter the outcome of the game. 
If you program your hero to be a Selfish Diamond Miner, for example, then you will likely rank high in the diamond mines
captured category, but will not be helping your team, possibly causing a loss in your stats. The win conditions are 
important to keep in mind as you think about how you want to program your hero.

### Turns

Several things happen on each turn. Let's take a single turn from your hero as an example, and walk through the steps.

1.  The first question we ask on your hero's turn is whether or not the game is over. If it is, well, then that's it.
2.  The next question we ask is whether or not your hero is dead.
3.  If he or she is not, then we do a few things:

    1.  We take the direction that your hero wants to go (ie - the direction that your move function returned) and ask 
        if it is a valid coordinate. If it is, we move your hero to that tile. If not, your hero stays put.
    2.  If the tile your hero wants to move to is unoccupied (ie - another hero is not there, there is not an 
        obstruction there, etc), then your hero moves on to that tile.
    3.  If the tile your hero wants to move to is the grave of a fallen hero, your hero will rob that grave and be given
        credit.
    4.  If the tile your hero wants to move to is a diamond mine, then your hero will capture the diamond mine, but will
        not move on to that tile. Additionally, your hero will receive 20 damage because diamond mines are magic. Your 
        hero will earn 1 diamond for each mine he/she owns at the start of his/her turn.
    5.  If the tile your hero wants to move to is a health well, then your hero will receive 30 health, but will not 
        move on to that tile.
    6.  If the tile your hero wants to move to is an enemy hero, then your hero will deal 10 damage to the enemy hero, 
        but will not move on to that tile.
    7.  If the tile your hero wants to move to is a friendly hero, then the friendly hero will receive 40 health, but 
        your hero will not move on to that tile.

4.  If your hero dies after moving, then we update the hero's status to 'dead' and dig a grave where the hero once was.
5.  If your hero is still alive after moving, then your hero deals 20 damage to any enemy hero on an adjacent tile. This
    is in addition to the specific damage done by your hero earlier in the turn.
6.  After this, your hero's turn is over and we increment the game's turn and move on to the next hero.

### Statistics

After a win condition has been met, we keep track of a number of statistics. Your hero will have the following 
statistics added to his or her total in our database:

*   Wins
*   Losses
*   Deaths
*   Kills
*   Damage Dealt
*   Graves Robbed
*   Health Given
*   Health Recovered
*   Diamonds Earned
*   Mines Captured

Heroes
------

You must export the `move` function from your `hero.js` file, in order for your code to run

```js
module.exports = function move (gameData, helpers) { ... };
```

The `move` function should accept two arguments that the website will be passing in: 

*   A `gameData` object which holds all information about the current state of the battle
*   A `helpers` object, which contains useful helper functions. Check out the `helpers.js` file to see what is
    available to you

### Northerner

Walks North. Always.

```js
module.exports = function () {
  return 'North';
};
```

### Blind Man

Walks in a random direction each turn.

```js
module.exports = function () {
  var choices = ['North', 'South', 'East', 'West', 'Stay'];
  return choices[Math.floor(Math.random() * choices.length)];
};
```

### Priest

Heals nearby friendly champions.

```js
module.exports = function (gameData, helpers) {
  var myHero = gameData.activeHero;
  if (myHero.health < 60) { return helpers.findNearestHealthWell(gameData); }
  return helpers.findNearestTeamMember(gameData);
};
```

### Unwise Assassin

Attempts to kill the closest enemy hero. No matter what.

```js
module.exports = function (gameData, helpers) {
  var myHero = gameData.activeHero;
  if (myHero.health < 30) { return helpers.findNearestHealthWell(gameData); }
  return helpers.findNearestEnemy(gameData);
};
```

### Careful Assassin

Attempts to kill the closest weaker enemy hero.

```js
module.exports = function (gameData, helpers) {
  var myHero = gameData.activeHero;
  if (myHero.health < 50) { return helpers.findNearestHealthWell(gameData); }
  return helpers.findNearestWeakerEnemy(gameData);
};
```

### Safe Diamond Miner

Cares about mining diamonds and making sure he or she is alive at the end of the game to enjoy the wealth.

```js
module.exports = function (gameData, helpers) {
  var myHero = gameData.activeHero;

  // Get stats on the nearest health well
  
  var healthWellStats = helpers.findNearestObjectDirectionAndDistance(gameData.board, myHero, function(boardTile) {
    if (boardTile.type === 'HealthWell') { return true; }
  });
  
  var distanceToHealthWell = healthWellStats.distance;
  var directionToHealthWell = healthWellStats.direction;

  // Heal no matter what if low health
  if (myHero.health < 40) { return directionToHealthWell; }
  
  // Heal if you aren't full health and are close to a health well already
  if (myHero.health < 100 && distanceToHealthWell === 1) { return directionToHealthWell; }
  
  // If healthy, go capture a diamond mine!
  return helpers.findNearestNonTeamDiamondMine(gameData);
};
```

### Selfish Diamond Miner

Attempts to capture diamond mines (even those owned by teammates).

```js
module.exports = function (gameData, helpers) {
  var myHero = gameData.activeHero;
  
  // Get stats on the nearest health well
  
  var healthWellStats = helpers.findNearestObjectDirectionAndDistance(gameData.board, myHero, function(boardTile) {
    if (boardTile.type === 'HealthWell') { return true; }
  });

  var distanceToHealthWell = healthWellStats.distance;
  var directionToHealthWell = healthWellStats.direction;

  // Heal no matter what if low health
  if (myHero.health < 40) { return directionToHealthWell; }
  
  // Heal if you aren't full health and are close to a health well already
  if (myHero.health < 100 && distanceToHealthWell === 1) { return directionToHealthWell; }
  
  // If healthy, go capture a diamond mine!
  return helpers.findNearestUnownedDiamondMine(gameData);
};
```

### Coward

Finds the nearest health well and stay there.

```js
module.exports = function (gameData, helpers) {
  return helpers.findNearestHealthWell(gameData);
};
```
