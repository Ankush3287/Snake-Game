READ ME!!
This game is made with the help of html,css and javascript.
We need the most basic knowledge to make such games.

|
Blog Home
JavaScript snake game tutorial: Build a simple, interactive game
Jun 26, 2020 - 12 min read
Educative




editor-page-cover
The best way to learn any programming language is through hands-on projects. The Snake Game is a simple game you can make using the basics of JavaScript and HTML.

Snake is a classic video game from the late 70s. The basic goal is to navigate a snake and eat as many apples as possible without touching the walls or the snake’s body.

Today, we’ll show you step-by-step how to create this Snake Game using JavaScript and HTML. To succeed in this tutorial, you should have a basic understanding of JavaScript and HTML.

Here are the steps we’ll go through today:

Display the board and a still snake
Make the snake move automatically
Use arrow keys to change the snake’s direction
Incorporate food and score
Wrap up and resources


Master JavaScript and HTML the easy way.
Learn the basics of web development with hands-on projects.

Web Development: Unraveling HTML, CSS, and JavaScript




1. Displaying the board and a still snake
First, we need to display the game board and the snake. Start by creating the file snakegame.html. This will contain all of our code. Next, open the file in your preferred browser.

To be able to create our game, we have to make use of the HTML <canvas>, which is used to draw graphics with JavaScript.

<canvas id="gameCanvas" width="400" height="400"><canvas>
The id is what identifies the canvas; it should always be specified. The id takes the dimensions width and height as props.

Until now, the browser will not display anything since the canvas has no default background. To make our canvas visible, we can give it a border by writing some JavaScript code.

To do that, we need to insert <script> and </script> tags after the </canvas>.


Making the canvas
Now we can make the canvas, or the game board, for our snake to navigate. First, we get the canvas element using the id gameCanvas (specified earlier).

Next, we get the canvas “2d context", which means that it will be drawn into a 2D space.

We will then ​make a 400 x 400 white rectangle with a black border, which will cover the entire canvas starting from the top left corner (0, 0).

const snakeboard = document.getElementById("gameCanvas");
const snakeboard_ctx = gameCanvas.getContext("2d");

Making the snake
Now, for the snake! We need to specify the initial location of our snake on the canvas by representing the snake as an array of coordinates.

Thus, to create a horizontal snake in the middle of the canvas, at (200, 200), we list the co-ordinate of each body part of the snake.

The number of coordinates in the object will be equal to the length of the snake.

let snake = [  {x: 200, y: 200},  {x: 190, y: 200},  {x: 180, y: 200},  {x: 170, y: 200},  {x: 160, y: 200},];

The yy-coordinate for all parts is always 200. The xx-coordinate is at decrements of 10 to represent different parts of the snake’s body. The very first coordinate represents the snake’s head.

Now, to display the snake on the canvas, we can write a function to draw a rectangle for each pair of coordinates.

function drawSnakePart(snakePart) 
{  
  snakeboard_ctx.fillStyle = 'lightblue';  
  snakeboard_ctx.strokestyle = 'darkblue';
  snakeboard_ctx.fillRect(snakePart.x, snakePart.y, 10, 10);  
  snakeboard_ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
}
 
/*Function that prints the parts*/
function drawSnake() 
{  
  snake.forEach(drawSnakePart);
}

Putting step 1 together
Check out the code and click on the Output tab to see the result.

Output
HTML
12345678910111213141516171819202122232425262728293031323334353637383940414243
<!DOCTYPE html>
<html>
  <head>
  	<title>Snake Game</title>
    <link href="https://fonts.googleapis.com/css?family=Antic+Slab" rel="stylesheet">

  </head>

  <body>



Run
For now, the main function only calls the functions clearCanvas() and drawSnake(). On to the next step!




2. Making the snake move automatically
We have our canvas and our snake, but we need the snake to move so it can navigate the game space in all directions.

So, let’s learn how to make our snake move automatically on the canvas.

Horizontal movement
To make the snake move one step (10px) to the right, we can increase the xx-coordinate of every part of the snake by 10px (dx = +10).

To make the snake move to the left, we can decrease the x-coordinate of every part of the snake by 10px (dx = -10).

dx is the horizontal velocity of the snake. We need to create a function move_snake that will update the snake.

function move_snake() 
{  
  const head = {x: snake[0].x + dx, y: snake[0].y};
  snake.unshift(head);
  snake.pop();
}
In the function above, we created a new head for the snake. We then added the new head to the beginning of the snake using snake.unshift and removed the last element of the snake using snake.pop.

This way, all of the other snake parts shift into place.

Enjoying the article? Scroll down to sign up for our free, bi-monthly newsletter.


Vertical movement
To move our snake vertically, we cannot alter all the yy-coordinates by 10px as that would shift the whole snake up and down. Only the yy-coordinate of the head needs to be altered.

Decreasing it by 10px to move the snake up and increasing it by 10px to move the snake down will move the snake correctly.

To implement this, we have to update the move_snake method to also increase the y-coordinate of the head by dy (vertical velocity of the snake).

const head = {x: snake[0].x + dx, y: snake[0].y + dy};

Automatic movement
In order to move the snake, say 50px to the right, we will have to call move_snake(x) 5 times. However, calling the method 5 times will make the snake jump to the +50px position, instead of moving step-by-step towards that point.

To move the snake how we want, we can add a slight delay between each call with setTimeout. We also need to make sure to call drawSnake every time we call move_Snake, as shown below. If we don’t, we won’t be able to see the intermediate steps that show the snake moving.

setTimeout(function onTick() {  clearCanvas();  move_Snake();  drawSnake();}, 100);
setTimeout(function onTick() {  clearCanvas();  move_Snake();  drawSnake();}, 100);
...
drawSnake();
clearCanvas() is called inside setTimeout to remove all previous positions of the snake.

Although there is still a problem, nothing tells the program that it has to wait for setTimeout before moving to the next setTimeout. This means that the snake will still jump 50px forward but only after a slight delay.

To fix that, we have to wrap our code inside functions. Instead of creating an infinite number of functions that call each other, we can instead create one function (main) and call it over and over again.

function main() 
{  
   setTimeout(function onTick() 
   {    
     clearCanvas();    
     advanceSnake();  
     drawSnake();
     // Call main again
     main();
   }, 100)
}

Putting step 2 together
Check out the code and click on the Output tab to see the result.

Output
HTML
<!DOCTYPE html>
<html>
  <head>
  	<title>Snake Game</title>
    <link href="https://fonts.googleapis.com/css?family=Antic+Slab" rel="stylesheet">

  </head>

  <body>

    <canvas id="snakeboard" width="400" height="400"></canvas>

    <style>
      #snakeboard {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    </style>
  </body>

  <script>
    const board_border = 'black';
    const board_background = "white";
    const snake_col = 'lightblue';
    const snake_border = 'darkblue';
    
    let snake = [
      {x: 200, y: 200},
      {x: 190, y: 200},
      {x: 180, y: 200},
      {x: 170, y: 200},
      {x: 160, y: 200}
    ]

    // Horizontal velocity
    let dx = 10;
    // Vertical velocity
    let dy = 0;
    
    // Get the canvas element
    const snakeboard = document.getElementById("snakeboard");
    // Return a two dimensional drawing context
    const snakeboard_ctx = snakeboard.getContext("2d");
    // Start game
    main();
    
    // main function called repeatedly to keep the game running
    function main() {
        setTimeout(function onTick() {
        clear_board();
        move_snake();
        drawSnake();
        // Call main again
        main();
      }, 100)
    }
    
    // draw a border around the canvas
    function clear_board() {
      //  Select the colour to fill the drawing
      snakeboard_ctx.fillStyle = board_background;
      //  Select the colour for the border of the canvas
      snakeboard_ctx.strokestyle = board_border;
      // Draw a "filled" rectangle to cover the entire canvas
      snakeboard_ctx.fillRect(0, 0, snakeboard.width, snakeboard.height);
      // Draw a "border" around the entire canvas
      snakeboard_ctx.strokeRect(0, 0, snakeboard.width, snakeboard.height);
    }
    
    // Draw the snake on the canvas
    function drawSnake() {
      // Draw each part
      snake.forEach(drawSnakePart)
    }
    
    // Draw one snake part
    function drawSnakePart(snakePart) {

      // Set the colour of the snake part
      snakeboard_ctx.fillStyle = snake_col;
      // Set the border colour of the snake part
      snakeboard_ctx.strokestyle = snake_border;
      // Draw a "filled" rectangle to represent the snake part at the coordinates
      // the part is located
      snakeboard_ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
      // Draw a border around the snake part
      snakeboard_ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
    }

    function move_snake() {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      snake.pop();
    }
    
  </script>
</html>

Run
Now our snake can move! However, once the snake’s position moves beyond the canvas boundary, it keeps going forever. We need to fix this by incorporating the use of arrow keys to change the snake’s direction.



Keep the learning going.
Learn JavaScript and web dev without scrubbing through videos or documentation. Educative’s text-based courses are easy to skim and feature live coding environments - making learning quick and efficient.

Web Development: Unraveling HTML, CSS, and JavaScript





3. Using arrow keys to change the snake’s direction
We have a moving snake, but our next task is to make the snake change direction when one of the arrow keys is pressed.

Changing Direction
Let’s make the function change_direction. This will check if the pressed key matches one of the arrow keys. If it does, we will change the vertical and horizontal velocity. Look at the function below.

function change_direction(event) 
{  
   const LEFT_KEY = 37;
   const RIGHT_KEY = 39;
   const UP_KEY = 38;
   const DOWN_KEY = 40;
 
   const keyPressed = event.keyCode;
   const goingUp = dy === -10;
   const goingDown = dy === 10;
   const goingRight = dx === 10;  
   const goingLeft = dx === -10;
 
     if (keyPressed === LEFT_KEY && !goingRight)
     {    
          dx = -10;
          dy = 0;  
     }
 
     if (keyPressed === UP_KEY && !goingDown)
     {    
          dx = 0;
          dy = -10;
     }
 
     if (keyPressed === RIGHT_KEY && !goingLeft)
     {    
          dx = 10;
          dy = 0;
     }
 
     if (keyPressed === DOWN_KEY && !goingUp)
     {    
          dx = 0;
          dy = 10;
     }
}
We also need to check if the snake is moving in the opposite direction of the new, intended direction. This will prevent our snake from reversing, such as when you press the right arrow key when the snake is moving to the left.

To incorporate the change_direction function, we can use the addEventListener on the document to listen for when a key is pressed; then we can call change_direction with the keydown event.

document.addEventListener("keydown", change_direction)

Adding boundary condition
To prevent our snake from moving infinitely, we need to add boundary conditions. For this, let’s make the function has_game_ended, which returns true when the game has ended and false if otherwise.

There are two cases in which the game can end:

The head of the snake collides with its body.
The head of the snake collides with the canvas boundary.
These two conditions are incorporated in the code below:

function has_game_ended()
{  
  for (let i = 4; i < snake.length; i++)
  {    
    const has_collided = snake[i].x === snake[0].x && snake[i].y === snake[0].y
    if (has_collided) 
      return true
  }
  const hitLeftWall = snake[0].x < 0;  
  const hitRightWall = snake[0].x > snakeboard.width - 10;
  const hitToptWall = snake[0].y &lt; 0;
  const hitBottomWall = snake[0].y > snakeboard.height - 10;
 
  return hitLeftWall ||  hitRightWall || hitToptWall || hitBottomWall
}
First, there is a check which looks​ to see if the head has collided with any of the body parts.

If it has not, there is a further check for all of the boundary walls.


Putting step 3 together
Check out the code and click on the Output tab to see the result.

Output
HTML
<!DOCTYPE html>
<html>
  <head>
  	<title>Snake Game</title>
    <link href="https://fonts.googleapis.com/css?family=Antic+Slab" rel="stylesheet">

  </head>

  <body>

    <canvas id="snakeboard" width="400" height="400"></canvas>

    <style>
      #snakeboard {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
    </style>
  </body>

  <script>
    const board_border = 'black';
    const board_background = "white";
    const snake_col = 'lightblue';
    const snake_border = 'darkblue';
    
    let snake = [
      {x: 200, y: 200},
      {x: 190, y: 200},
      {x: 180, y: 200},
      {x: 170, y: 200},
      {x: 160, y: 200}
    ]

    // True if changing direction
    let changing_direction = false;
    // Horizontal velocity
    let dx = 10;
    // Vertical velocity
    let dy = 0;
    
    // Get the canvas element
    const snakeboard = document.getElementById("snakeboard");
    // Return a two dimensional drawing context
    const snakeboard_ctx = snakeboard.getContext("2d");
    // Start game
    main();

    document.addEventListener("keydown", change_direction);
    
    // main function called repeatedly to keep the game running
    function main() {

        if (has_game_ended()) return;

        changing_direction = false;
        setTimeout(function onTick() {
        clear_board();
        move_snake();
        drawSnake();
        // Call main again
        main();
      }, 100)
    }
    
    // draw a border around the canvas
    function clear_board() {
      //  Select the colour to fill the drawing
      snakeboard_ctx.fillStyle = board_background;
      //  Select the colour for the border of the canvas
      snakeboard_ctx.strokestyle = board_border;
      // Draw a "filled" rectangle to cover the entire canvas
      snakeboard_ctx.fillRect(0, 0, snakeboard.width, snakeboard.height);
      // Draw a "border" around the entire canvas
      snakeboard_ctx.strokeRect(0, 0, snakeboard.width, snakeboard.height);
    }
    
    // Draw the snake on the canvas
    function drawSnake() {
      // Draw each part
      snake.forEach(drawSnakePart)
    }
    
    // Draw one snake part
    function drawSnakePart(snakePart) {

      // Set the colour of the snake part
      snakeboard_ctx.fillStyle = snake_col;
      // Set the border colour of the snake part
      snakeboard_ctx.strokestyle = snake_border;
      // Draw a "filled" rectangle to represent the snake part at the coordinates
      // the part is located
      snakeboard_ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
      // Draw a border around the snake part
      snakeboard_ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
    }

    function has_game_ended() {
      for (let i = 4; i < snake.length; i++) {
        if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) return true
      }
      const hitLeftWall = snake[0].x < 0;
      const hitRightWall = snake[0].x > snakeboard.width - 10;
      const hitToptWall = snake[0].y < 0;
      const hitBottomWall = snake[0].y > snakeboard.height - 10;
      return hitLeftWall || hitRightWall || hitToptWall || hitBottomWall
    }

    function change_direction(event) {
      const LEFT_KEY = 37;
      const RIGHT_KEY = 39;
      const UP_KEY = 38;
      const DOWN_KEY = 40;
      
    // Prevent the snake from reversing
    
      if (changing_direction) return;
      changing_direction = true;
      const keyPressed = event.keyCode;
      const goingUp = dy === -10;
      const goingDown = dy === 10;
      const goingRight = dx === 10;
      const goingLeft = dx === -10;
      if (keyPressed === LEFT_KEY && !goingRight) {
        dx = -10;
        dy = 0;
      }
      if (keyPressed === UP_KEY && !goingDown) {
        dx = 0;
        dy = -10;
      }
      if (keyPressed === RIGHT_KEY && !goingLeft) {
        dx = 10;
        dy = 0;
      }
      if (keyPressed === DOWN_KEY && !goingUp) {
        dx = 0;
        dy = 10;
      }
    }

    function move_snake() {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      snake.pop();
    }
    
  </script>
</html>

Run
Perfect! The snake is now able to change direction when we press the arrow keys. The point of the game is to eat as much food a possible, so we will now learn how to incorporate food and score into the game.





4. Incorporating food and score
Now that we have a fully functional snake, it’s time to incorporate food and score into our game.

Food
For the food that our snake will eat, we want to generate a random set of coordinates. Let’s make the function random_food to randomly generate an xx-coordinate and a yy-coordinate for the food’s positions. We also have to ensure that the food is not located where the snake currently is.

If it is, then we have to generate a new food location. See the functions below:

function random_food(min, max)
{  
   return Math.round((Math.random() * (max-min) + min) / 10) * 10;
}
 
function gen_food() 
{  
   food_x = random_food(0, snakeboard.width - 10);
   food_y = random_food(0, snakeboard.height - 10);
   snake.forEach(function has_snake_eaten_food(part) {
        const has_eaten = part.x == food_x && part.y == food_y;
        if (has_eaten) gen_food();
      });
}
We also need a function to actually draw the food on the canvas and update main to incorporate the drawFood function.

function drawFood()
{
      snakeboard_ctx.fillStyle = 'lightgreen;
      snakeboard_ctx.strokestyle = 'darkgreen';
      snakeboard_ctx.fillRect(food_x, food_y, 10, 10);
      snakeboard_ctx.strokeRect(food_x, food_y, 10, 10);
}

Growing the snake
The snake will grow whenever the head of the snake is at the same position as the food. Instead of adding a body part to the snake’s body every time that happens, we can skip removing a body part in the move_snake function.

See the updated version of move_snake below:

function move_snake() {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      const has_eaten_food = snake[0].x === food_x && snake[0].y === food_y;
      if (has_eaten_food) {
        // Generate new food location
        gen_food();
      } else {
        // Remove the last part of snake body
        snake.pop();
      }
    }

Score
Incorporating a score is actually quite simple. We need to initialize a score variable and increment it every time the snake eats the food. To display the score, we will need a new div before the canvas.

We need to further update the move_snake method to incorporate the score:

function move_snake()
 {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      const has_eaten_food = snake[0].x === foodX && snake[0].y === foodY;
      if (has_eaten_Food) {
        // Increase score
        score += 10;
        // Display score on screen
        document.getElementById('score').innerHTML = score;
        // Generate new food location
        gen_food();
      } else {
        // Remove the last part of snake body
        snake.pop();
      }
}

Putting all the steps together
Check out the code and click on the Output tab to see the result.

Output
JavaScript
HTML
<!DOCTYPE html>
<html>
  <head>
  	<title>Snake Game</title>
  </head>

  <body>

    <div id="score">0</div>
    <canvas id="snakeboard" width="400" height="400"></canvas>

    <style>
      #snakeboard {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
      #score {
        text-align: center;
        font-size: 140px;
      }
    </style>
  </body>

  <script>
    const board_border = 'black';
    const board_background = "white";
    const snake_col = 'lightblue';
    const snake_border = 'darkblue';
    
    let snake = [
      {x: 200, y: 200},
      {x: 190, y: 200},
      {x: 180, y: 200},
      {x: 170, y: 200},
      {x: 160, y: 200}
    ]

    let score = 0;
    // True if changing direction
    let changing_direction = false;
    // Horizontal velocity
    let food_x;
    let food_y;
    let dx = 10;
    // Vertical velocity
    let dy = 0;
    
    
    // Get the canvas element
    const snakeboard = document.getElementById("snakeboard");
    // Return a two dimensional drawing context
    const snakeboard_ctx = snakeboard.getContext("2d");
    // Start game
    main();

    gen_food();

    document.addEventListener("keydown", change_direction);
    
    // main function called repeatedly to keep the game running
    function main() {

        if (has_game_ended()) return;

        changing_direction = false;
        setTimeout(function onTick() {
        clear_board();
        drawFood();
        move_snake();
        drawSnake();
        // Repeat
        main();
      }, 100)
    }
    
    // draw a border around the canvas
    function clear_board() {
      //  Select the colour to fill the drawing
      snakeboard_ctx.fillStyle = board_background;
      //  Select the colour for the border of the canvas
      snakeboard_ctx.strokestyle = board_border;
      // Draw a "filled" rectangle to cover the entire canvas
      snakeboard_ctx.fillRect(0, 0, snakeboard.width, snakeboard.height);
      // Draw a "border" around the entire canvas
      snakeboard_ctx.strokeRect(0, 0, snakeboard.width, snakeboard.height);
    }
    
    // Draw the snake on the canvas
    function drawSnake() {
      // Draw each part
      snake.forEach(drawSnakePart)
    }

    function drawFood() {
      snakeboard_ctx.fillStyle = 'lightgreen';
      snakeboard_ctx.strokestyle = 'darkgreen';
      snakeboard_ctx.fillRect(food_x, food_y, 10, 10);
      snakeboard_ctx.strokeRect(food_x, food_y, 10, 10);
    }
    
    // Draw one snake part
    function drawSnakePart(snakePart) {

      // Set the colour of the snake part
      snakeboard_ctx.fillStyle = snake_col;
      // Set the border colour of the snake part
      snakeboard_ctx.strokestyle = snake_border;
      // Draw a "filled" rectangle to represent the snake part at the coordinates
      // the part is located
      snakeboard_ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
      // Draw a border around the snake part
      snakeboard_ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
    }

    function has_game_ended() {
      for (let i = 4; i < snake.length; i++) {
        if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) return true
      }
      const hitLeftWall = snake[0].x < 0;
      const hitRightWall = snake[0].x > snakeboard.width - 10;
      const hitToptWall = snake[0].y < 0;
      const hitBottomWall = snake[0].y > snakeboard.height - 10;
      return hitLeftWall || hitRightWall || hitToptWall || hitBottomWall
    }

    function random_food(min, max) {
      return Math.round((Math.random() * (max-min) + min) / 10) * 10;
    }

    function gen_food() {
      // Generate a random number the food x-coordinate
      food_x = random_food(0, snakeboard.width - 10);
      // Generate a random number for the food y-coordinate
      food_y = random_food(0, snakeboard.height - 10);
      // if the new food location is where the snake currently is, generate a new food location
      snake.forEach(function has_snake_eaten_food(part) {
        const has_eaten = part.x == food_x && part.y == food_y;
        if (has_eaten) gen_food();
      });
    }

    function change_direction(event) {
      const LEFT_KEY = 37;
      const RIGHT_KEY = 39;
      const UP_KEY = 38;
      const DOWN_KEY = 40;
      
    // Prevent the snake from reversing
    
      if (changing_direction) return;
      changing_direction = true;
      const keyPressed = event.keyCode;
      const goingUp = dy === -10;
      const goingDown = dy === 10;
      const goingRight = dx === 10;
      const goingLeft = dx === -10;
      if (keyPressed === LEFT_KEY && !goingRight) {
        dx = -10;
        dy = 0;
      }
      if (keyPressed === UP_KEY && !goingDown) {
        dx = 0;
        dy = -10;
      }
      if (keyPressed === RIGHT_KEY && !goingLeft) {
        dx = 10;
        dy = 0;
      }
      if (keyPressed === DOWN_KEY && !goingUp) {
        dx = 0;
        dy = 10;
      }
    }

    function move_snake() {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      const has_eaten_food = snake[0].x === food_x && snake[0].y === food_y;
      if (has_eaten_food) {
        // Increase score
        score += 10;
        // Display score on screen
        document.getElementById('score').innerHTML = score;
        // Generate new food location
        gen_food();
      } else {
        // Remove the last part of snake body
        snake.pop();
      }
    }
    
  </script>
</html>