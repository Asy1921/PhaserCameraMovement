# *Phaser Camera Movement*
A simple game to show camera movement in Phaser
The Camera is the way in which all games are rendered in Phaser. They provide a view into your game world, and can be positioned, rotated, zoomed and scrolled accordingly.
A Camera consists of two elements: The viewport and the scroll values.

The viewport is the physical position and size of the Camera within your game. Cameras, by default, are created the same size as your game, but their position and size can be set to anything. This means if you wanted to create a camera that was 320x200 in size, positioned in the bottom-right corner of your game, you'd adjust the viewport to do that (using methods like setViewport and setSize).

I made a maze like structure using tiles, which is only partly visible by the camera.

The camera follows the player.

Player is always moving and changes direction whenever it touches a wall.

Player has ability to jump and double jump.


![Picture1](https://user-images.githubusercontent.com/56742160/97320209-9260ab00-1893-11eb-9358-1ca7e67d2a09.png)

### TileMap

Tile Mapping was done with the help of a tool called “Tiled”. It is a great tool for creating maps of 2D Games.

![image](https://user-images.githubusercontent.com/56742160/97320805-34809300-1894-11eb-8475-72ef2492e3b5.png)


### Camera Movement

Camera in Phaser is accessed by the 
game.camera command.

It has x and y as its variable to change the 
x and y coordinates of the camera.

![image](https://user-images.githubusercontent.com/56742160/97320851-3ea29180-1894-11eb-9fc7-50ae9091c791.png)

### Camera Follow function

Camera can be set to follow a player by using follow function.

This is done using Phaser.Camera.FOLLOW_PLATFORMER.

The camera view bounds is also given as an input.

![image](https://user-images.githubusercontent.com/56742160/97320925-537f2500-1894-11eb-8392-23b75fc54d97.png)

![image](https://user-images.githubusercontent.com/56742160/97356139-d4064b80-18bd-11eb-9e92-7f0144b33468.png)


### Basic Movement script-

![image](https://user-images.githubusercontent.com/56742160/97321126-8d502b80-1894-11eb-99c9-884e06cb4d26.png)

### Basic gravity script-

![image](https://user-images.githubusercontent.com/56742160/97321201-a062fb80-1894-11eb-9d1c-7c0af10af168.png)

### Flowchart for Camera Movement

![image](https://user-images.githubusercontent.com/56742160/97356173-e3859480-18bd-11eb-91fe-032389e0d9cc.png)

## *Complete Code*
javascript
var game;
var gameOptions = {

    // width of the game, in pixels
    gameWidth: 640,

    // height of the game, in pixels
    gameHeight: 480,

    // background color
    bgColor: 0x444444,

    // player gravity
    playerGravity: 900,

    // player friction when on wall
    playerGrip: 100,

    // player horizontal speed
    playerSpeed: 200,

    // player jump force
    playerJump: 400,

    // player double jump force
    playerDoubleJump: 300
}
window.onload = function() {
    game = new Phaser.Game(gameOptions.gameWidth, gameOptions.gameHeight);
    game.state.add("PreloadGame", preloadGame);
    game.state.add("PlayGame", playGame);
    game.state.start("PreloadGame");
}
var preloadGame = function(game){}
preloadGame.prototype = {
    preload: function(){
        game.stage.backgroundColor = gameOptions.bgColor;
        game.scale.scaleMode = Phaser.ScaleManager.SHOW_ALL;
        game.scale.pageAlignHorizontally = true;
        game.scale.pageAlignVertically = true;
        game.stage.disableVisibilityChange = true;

        // loading level tilemap
        game.load.tilemap("level", 'level.json', null, Phaser.Tilemap.TILED_JSON);
        game.load.image("tile", "tile.png");
        game.load.image("hero", "hero.png");
    },
    create: function(){
        game.state.start("PlayGame");
    }
}
var playGame = function(game){}
playGame.prototype = {
    create: function(){

        // starting ARCADE physics
        game.physics.startSystem(Phaser.Physics.ARCADE);

        // creatin of "level" tilemap
        this.map = game.add.tilemap("level");

        // adding tiles (actually one tile) to tilemap
        this.map.addTilesetImage("tileset01", "tile");

        // tile 1 (the black tile) has the collision enabled
        this.map.setCollision(1);

        // which layer should we render? That's right, "layer01"
        this.layer = this.map.createLayer("layer01");

        // adding the hero sprite
        this.hero = game.add.sprite(300, 376, "hero");

        // setting hero anchor point
        this.hero.anchor.set(0.5);

        // enabling ARCADE physics for the  hero
        game.physics.enable(this.hero, Phaser.Physics.ARCADE);

        // setting hero gravity
        this.hero.body.gravity.y = gameOptions.playerGravity;

        // setting hero horizontal speed
        this.hero.body.velocity.x = gameOptions.playerSpeed;

        // the hero can jump
        this.canJump = true;

        // the hern cannot double jump
        this.canDoubleJump = false;

        // the hero is not on the wall
        this.onWall = false;

        // waiting for player input
        game.input.onDown.add(this.handleJump, this);

        // set workd bounds to allow camera to follow the player
        game.world.setBounds(0, 0, 1920, 1440);

        // making the camera follow the player
        game.camera.follow(this.hero, Phaser.Camera.FOLLOW_PLATFORMER, 0.1, 0.1);
    },
    handleJump: function(){
        // the hero can jump when:
        // canJump is true AND the hero is on the ground (blocked.down)
        // OR
        // the hero is on the wall
        if((this.canJump && this.hero.body.blocked.down) || this.onWall){

            // applying jump force
            this.hero.body.velocity.y = -gameOptions.playerJump;

            // is the hero on a wall?
            if(this.onWall){

                // change the horizontal velocity too. This way the hero will jump off the wall
                this.setPlayerXVelocity(true);
            }

            // hero can't jump anymore
            this.canJump = false;

            // hero is not on the wall anymore
            this.onWall = false;

            // the hero can now double jump
            this.canDoubleJump = true;
        }
        else{

            // cam the hero make the doubple jump?
            if(this.canDoubleJump){

                // the hero can't double jump anymore
                this.canDoubleJump = false;

                // applying double jump force
                this.hero.body.velocity.y = -gameOptions.playerDoubleJump;
            }
        }
    },
    update: function(){

        // set some default gravity values. Look at the function for more information
        this.setDefaultValues();

        // handling collision between the hero and the tiles
        game.physics.arcade.collide(this.hero, this.layer, function(hero, layer){

            // some temporary variables to determine if the player is blocked only once
            var blockedDown = this.hero.body.blocked.down;
            var blockedLeft = this.hero.body.blocked.left
            var blockedRight = this.hero.body.blocked.right;

            // if the hero hits something, no double jump is allowed
            this.canDoubleJump = false;

            // hero on the ground
            if(blockedDown){

                // hero can jump
                this.canJump = true;
            }

            // hero on the ground and touching a wall on the right
            if(blockedRight){

                // horizontal flipping hero sprite
                this.hero.scale.x = -1;
            }

            // hero on the ground and touching a wall on the right
            if(blockedLeft){

                // default orientation of hero sprite
                this.hero.scale.x = 1;
            }

            // hero NOT on the ground and touching a wall on the right
            if((blockedRight || blockedLeft) && !blockedDown){

                // hero on a wall
                this.onWall = true;

                // remove gravity
                this.hero.body.gravity.y = 0;

                // setting new y velocity
                this.hero.body.velocity.y = gameOptions.playerGrip;
            }

            // adjusting hero speed according to the direction it's moving
            this.setPlayerXVelocity(!this.onWall || blockedDown);
        }, null, this);
    },

    // default values to be set at the beginning of each update cycle,
    // which may be changed according to what happens into "collide" callback function
    // (if called)
    setDefaultValues: function(){
        this.hero.body.gravity.y = gameOptions.playerGravity;
        this.onWall = false;
        this.setPlayerXVelocity(true);
    },

    // sets player velocity according to the direction it's facing, unless "defaultDirection"
    // is false, in this case multiplies the velocity by -1
    setPlayerXVelocity(defaultDirection){
        this.hero.body.velocity.x = gameOptions.playerSpeed * this.hero.scale.x * (defaultDirection ? 1 : -1);
    }
}







