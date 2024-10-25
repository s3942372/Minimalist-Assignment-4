---
title: Ma Reflection
published_at: 2024-10-23
snippet: Reflection of my first assignment.
disable_html_sanitization: true
allow_math: true
---

# Ma Reflection

## The Plan

This interactive 'art' tool was created in response to the Japanese concept of 'Ma' and minimalist design. "Ma" presents the idea that there is meaning in empty spaces, and that it is a part of a whole, working together with everything else to create something complete.

My plan for this interactive work was to combine the ideas of 'Ma' with that of the traditional art of Chinese paper cutting. Paper cutting makes use of the empty negative space (the areas that are cut out) to help form the image a person wishes to make. I figured that I could replicate this experience by making it so that the users interacting with the website are clicking and dragging their mouse around a black space, and upon releasing their mouse a shape is formed and filled in white on screen, visually resembling a piece of paper that had a hole cut into it. Like so, users would have to carefully think as they go about using this art tool, as there is no undo button and any mistakes means users will have to start all over, much like how actual paper cutting is. ‘Ma’ provokes thought and gives meaning to empty spaces, and the intention of this work is for users to give meaning to the empty spaces they form themselves.

![Ma Ice Cream](/assignment1/ma.png)

I had decided to code out the entire interactive work on my own using html, css and JavaScript as I was somewhat familiar with these coding languages after having taken both the 'Interactive Media' and the 'Creative Coding' courses in my 1st and 2nd years of study. 

## The Process

![First Try](/assignment1/ma1.png)

For my first attempt at coding this work I had set up an ‘overlay’ on top of the canvas and changed the colour to white. I then set up several functions to allow for me to ‘erase’ this ‘overlay’. By clicking and dragging the mouse around on the screen, black rectangles appear, giving the appearance that the white ‘overlay’ has been erased to reveal the black canvas beneath it.

The result was something similar to that of the 'eraser' tool on any drawing program, and my goal was to create something akin to the 'lasso' tool. 

![Second Try](/assignment1/ma2.png)

My second attempt yielded results closer to what I set out to do. Users can now draw out a shape when they click and hold their left mouse button, and upon release the shape will automatically be filled in. Each new click resets the canvas, so users had to think very carefully about what they're trying to make, and when they should release their mouse.

This attempt had quite a few limits and constraints restricting the user, but I decided to try again with the code, as I want the users to be able to draw as much as they want on one canvas, instead of it happening only once. In Paper Cutting too, people can cut as many times as they want, what really mattered was where they cut, and so I wished to keep this 'function': you can draw as much as you want on the work, it's just a matter of where you draw your lines.

![Second Try](/assignment1/ma3.png)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MA as an Interactive Art Tool</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <div id="canvas-container">
        <canvas id="canvas"></canvas>
    </div>
    <script src="script.js"></script>
</body>

</html>
```
```css
body {
    margin: 0;
    padding: 0;
    overflow: hidden;
    background: #000;
}

canvas {
    display: block;
    cursor: crosshair;
}
```
```javascript
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

ctx.fillStyle = '#000';
ctx.fillRect(0, 0, canvas.width, canvas.height);

let isDrawing = false;
let points = [];
const eraseColor = '#FFF';
const shapes = []; 
const erasedAreas = [];

canvas.addEventListener('mousedown', (e) => {
    isDrawing = true;
    points = [];
    addPoint(e);
});

canvas.addEventListener('mousemove', (e) => {
    if (isDrawing) {
        addPoint(e);
        drawCurrentShape();
    }
});

canvas.addEventListener('mouseup', () => {
    if (isDrawing) {
        isDrawing = false;
        shapes.push(points.slice());
        applyEraser(); 
        drawAllShapes(); 
    }
});

function addPoint(e) {
    const rect = canvas.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    points.push({ x, y });
}

function drawCurrentShape() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    drawAllShapes();

    ctx.strokeStyle = '#FFF';
    ctx.lineWidth = 2;
    ctx.beginPath();
    ctx.moveTo(points[0].x, points[0].y);
    for (let i = 1; i < points.length; i++) {
        ctx.lineTo(points[i].x, points[i].y);
    }
    ctx.lineTo(points[0].x, points[0].y);
    ctx.stroke();
}

function drawAllShapes() {
    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    ctx.globalCompositeOperation = 'source-atop';
    erasedAreas.forEach(area => {
        ctx.fillStyle = eraseColor;
        ctx.beginPath();
        ctx.moveTo(area[0].x, area[0].y);
        for (let i = 1; i < area.length; i++) {
            ctx.lineTo(area[i].x, area[i].y);
        }
        ctx.lineTo(area[0].x, area[0].y); 
        ctx.fill();
    });
    ctx.globalCompositeOperation = 'source-over'; 
}

function applyEraser() {
    erasedAreas.push(points.slice());
    
    ctx.globalCompositeOperation = 'destination-out';
    shapes.forEach(shape => {
        ctx.beginPath();
        ctx.moveTo(shape[0].x, shape[0].y);
        for (let i = 1; i < shape.length; i++) {
            ctx.lineTo(shape[i].x, shape[i].y);
        }
        ctx.lineTo(shape[0].x, shape[0].y);
        ctx.fill();
    });
    
    ctx.globalCompositeOperation = 'source-over';
}

window.addEventListener('resize', () => {
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;

    ctx.fillStyle = '#000';
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    drawAllShapes();
});
```

For my final attempt I was able to change the code to allow for multiple attempts at drawing, and the canvas will only refresh if you refresh the page. There is no undo button, and so users will either have to think very carefully where they're drawing, or constantly restart until they manage to make what they wanted to make.

## The Outcome

I was most satisfied with this version as it most closely reflected the act of Paper Cutting - or at least, as much as I was able to do with my limited abilities in coding.

The final interactive work was something that was rather fun to play around with once you get into it, however, as mentioned in the comment for this assignment, the depth of the concept is somewhat lacking, with the focus of interaction split into two: the clicking and dragging of the mouse, and the interplay between positive and negative spaces. I was too focused on trying to create a digitalised version of Paper Cutting, that I lost focus on what the assignment brief was actually asking of us and to draw the users focus on to a specific interaction.

Despite this failure, I do think that for the first assignment, it was a fairly okay piece of work. I had a smooth journey in making this interactive work, having come up with the idea for it immediately in class, as well as being familiar enough with the chosen coding languages to produce a working website. Though this was my least 'playful' work of the six, it was still something that offered a fun interaction, something that will set the tone for the rest of my interactive pieces.

## Video Recording

Here is a video recording of my interactive work:

<iframe id="ma" src="/assignment1/s3942372_A1_Recording.mp4" title="video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<script type="module">

    console.log (`hello world! 🚀`)

    const iframe  = document.getElementById (`ma`)
    iframe.width  = iframe.parentNode.scrollWidth
    iframe.height = iframe.width * 9 / 16

</script>