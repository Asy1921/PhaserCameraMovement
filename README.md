### Phaser Camera Movement
A simple game to show camera movement in Phaser
The Camera is the way in which all games are rendered in Phaser. They provide a view into your game world, and can be positioned, rotated, zoomed and scrolled accordingly.
A Camera consists of two elements: The viewport and the scroll values.

The viewport is the physical position and size of the Camera within your game. Cameras, by default, are created the same size as your game, but their position and size can be set to anything. This means if you wanted to create a camera that was 320x200 in size, positioned in the bottom-right corner of your game, you'd adjust the viewport to do that (using methods like setViewport and setSize).

I made a maze like structure using tiles, which is only partly visible by the camera.

The camera follows the player.

Player is always moving and changes direction whenever it touches a wall.

Player has ability to jump and double jump.


![Picture1](https://user-images.githubusercontent.com/56742160/97320209-9260ab00-1893-11eb-9358-1ca7e67d2a09.png)

#TileMap

Tile Mapping was done with the help of a tool called “Tiled”. It is a great tool for creating maps of 2D Games.

![image](https://user-images.githubusercontent.com/56742160/97320805-34809300-1894-11eb-8475-72ef2492e3b5.png)


#Camera Movement

Camera in Phaser is accessed by the 
game.camera command.

It has x and y as its variable to change the 
x and y coordinates of the camera.

![image](https://user-images.githubusercontent.com/56742160/97320851-3ea29180-1894-11eb-9fc7-50ae9091c791.png)

#Camera Follow function

Camera can be set to follow a player by using follow function.

This is done using Phaser.Camera.FOLLOW_PLATFORMER.

The camera view bounds is also given as an input.

![image](https://user-images.githubusercontent.com/56742160/97320925-537f2500-1894-11eb-8392-23b75fc54d97.png)

Basic Movement script-

![image](https://user-images.githubusercontent.com/56742160/97321126-8d502b80-1894-11eb-99c9-884e06cb4d26.png)

Basic gravity script-

![image](https://user-images.githubusercontent.com/56742160/97321201-a062fb80-1894-11eb-9d1c-7c0af10af168.png)






