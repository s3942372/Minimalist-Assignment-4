---
title: Oulipo Reflection
published_at: 2024-10-23
snippet: Reflection of my second assignment part 1.
disable_html_sanitization: true
allow_math: true
---

# Oulipo Reflection

## The Plan

Despite this work being placed on before the other ('Hint Finction') on my website, I had actually worked on this one second, after I finished the 'Hint Fiction'. For this second half of assignment 2, I chose to focus on the 'Oulipo' (or in full French: “Ouvroir de Littéreature Potentielle”, meaning the “Workshop for Potential Literature”). It is a constrained form of writing, which I thought would fit well with the memory I chose to represent in the work - that of me being strapped down to a table in a hospital in HongKong one day when I was running a high fever. 

## The Process

During my preparation for this interactice work, I had researched on different types of 'Oulipo' techniques, before ultimately focusing on the 'cut-up' technique, where you cut up lines of a piece of work to create a new story. My original idea was for there to be words scattered across the bottom of the website, and users could click and drag them across the screen to form sentences. Forming the right sentence would trigger a change in the website, such as an image appearing.

![OG Plan](/static/assignment2/og.png)

Unfortunately however, due to my limited skills with the coding languages I have chosen, I was unable to get the physics/movement of the individual words the way I wanted it to be. The result was a clunky mess, with the words often clipping into each other when re-generated. Furthermore, placing the words in the correct sequence did not trigger any change in the environment, and so I decided to try and do something else instead.

![OG Plan](/static/assignment2/fail.png)

Taking inspiration from the grid-like model I saw on the Canvas slides during class, I decided to have my work take the form of two 4x3 grids, where users will have to click the right words in the right sequence to form a sentence describing what had happened that day in the hospital room. Triggering the right sequence of words also causes the background to change, whilst pressing the wrong word resets the entire thing.

![Oulipo Grids](/static/assignment2/oulipo.png)

Just like the previous assignment, I had decided to code out the entire interactive work on my own using html, css and JavaScript. The process for this second attempt went much smoother and faster than the first, and I was able to finish the code needed in less than a day, with the finished product being what I expected it to be.

![Oulipo End](/static/assignment2/oulipo1.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hint Fiction</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div id="story-container">
        <div class="sentence" id="sentence1">It</div>
        <div class="sentence" id="sentence2">Squirmed</div>
        <div class="sentence" id="sentence3">About</div>
        <div class="sentence" id="sentence4">Its</div>
        <div class="sentence" id="sentence5">Cage</div>
    </div>
    <script src="script.js"></script>
</body>
</html>
```
```css
body {
    margin: 0;
    overflow: hidden;
    background-color: #f0f8ff;
    font-family: Arial, sans-serif;
    position: relative;
}

#story-container {
    position: relative;
    width: 100%;
    height: 100vh;
    overflow: hidden;
    background-image: url(img/background1.png);
    background-size: cover;
    background-position: center;
    cursor: pointer;
}

.sentence {
    position: absolute;
    font-size: 24px;
    margin: 5px;
    opacity: 0;
    transition: opacity 1s ease-in-out;
    color: whitesmoke;
    text-shadow: 2px 2px 0 black, 
                 -2px -2px 0 black,  
                 2px -2px 0 black,  
                 -2px 2px 0 black;
}

#sentence1 {
    top: 10%;
    left: 10%;
}

#sentence2 {
    top: 20%;
    left: 30%;
}

#sentence3 {
    top: 40%;
    left: 50%;
}

#sentence4 {
    top: 60%;
    left: 70%;
}

#sentence5 {
    top: 80%;
    left: 90%;
}
```
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const sentences = [
        "It squirmed about its cage",
        "feasted upon the dead and",
        "slept inside a blanket of",
        "the finest silk biding time",
        "until it finally was released",
    ];

    const backgroundImages = [
        'img/cage.jpg',
        'img/feast.jpg',
        'img/slept.jpg',
        'img/blanket.webp',
        'img/boil.webp',
    ];

    const container = document.getElementById('story-container');
    let currentSentenceIndex = 0;
    const usedPositions = [];
    const spacing = 150;
    let allSentencesDisplayed = false;

    function getUniquePosition() {
        let top, left;
        let isValidPosition = false;

        while (!isValidPosition) {
            top = `${Math.random() * (container.clientHeight - spacing)}px`;
            left = `${Math.random() * (container.clientWidth - spacing)}px`;

            isValidPosition = !isOverlapping(top, left);
        }

        usedPositions.push({ top, left });
        return { top, left };
    }

    function isOverlapping(top, left) {
        return usedPositions.some(position => {
            return Math.abs(parseInt(top) - parseInt(position.top)) < spacing &&
                   Math.abs(parseInt(left) - parseInt(position.left)) < spacing;
        });
    }

    function fadeOutAllSentences() {
        const sentences = document.querySelectorAll('.sentence');
        sentences.forEach(sentence => {
            sentence.style.transition = 'opacity 0.5s';
            sentence.style.opacity = 0;
            setTimeout(() => {
                if (sentence) {
                    container.removeChild(sentence);
                }
            }, 500);
        });
    }

    function changeBackgroundImage() {
        if (backgroundImages.length > 0) {
            const imageIndex = currentSentenceIndex % backgroundImages.length;
            container.style.transition = 'background-image 0.5s';
            container.style.backgroundImage = `url('${backgroundImages[imageIndex]}')`;
        }
    }

    function fadeInSentences() {
        if (currentSentenceIndex >= sentences.length) {
            allSentencesDisplayed = true;
            return;
        }

        const sentenceElement = document.createElement('div');
        sentenceElement.classList.add('sentence');
        sentenceElement.id = `sentence${currentSentenceIndex + 6}`;
        sentenceElement.textContent = sentences[currentSentenceIndex];
        sentenceElement.style.opacity = 0;
        container.appendChild(sentenceElement);

        const { top, left } = getUniquePosition();
        sentenceElement.style.top = top;
        sentenceElement.style.left = left;

        setTimeout(() => {
            sentenceElement.style.transition = 'opacity 0.5s';
            sentenceElement.style.opacity = 1;
        }, 10); // Short delay to ensure transition is applied

        changeBackgroundImage();

        currentSentenceIndex++;
    }

    document.addEventListener('click', () => {
        if (allSentencesDisplayed) {
            fadeOutAllSentences();
            container.style.backgroundImage = '';
            currentSentenceIndex = 0;
            usedPositions.length = 0;
            allSentencesDisplayed = false;
        } else {
            fadeInSentences();
        }
    });
});
```

## The Outcome

Working on this second half of the assignment was definitely a lot easier than the first half, and though I did encounter some problems with my initial iteration of the work, I was able to overcome this fairly quickly. Part of this was due to the limited time I had, as I had been down with a cold for one week, then by an allergic reaction to my medication the next. I then struggled with the other half of the assignment, having spent time on it until I only had 2 days' time to start and finish this one. The looming deadline wass most definitely a motivating factor, as well as the sheer frustration I felt over the other interactive work.

Despite all that though, I was still quite satisfied with the finished product for this one. Of course, if I had more time I might have made more sentences the users could play around with, or drawn out the scenes instead of using image substitutes. I do think that the way it ended up being had fit quite well with the themes of limits and constraints we had, with the limited 

The final interactive work was something that was rather fun to play around with once you get into it, however, as mentioned in the comment for this assignment, the depth of the concept is somewhat lacking, with the focus of interaction split into two: the clicking and dragging of the mouse, and the interplay between positive and negative spaces. I was too focused on trying to create a digitalised version of Paper Cutting, that I lost focus on what the assignment brief was actually asking of us and to draw the users focus on to a specific interaction.

Despite this failure, I do think that for the first assignment, it was a fairly okay piece of work. I had a smooth journey in making this interactive work, having come up with the idea for it immediately in class, as well as being familiar enough with the chosen coding languages to produce a working website. Though this was my least 'playful' work of the six, it was still something that offered a fun interaction, something that will set the tone for the rest of my interactive pieces.