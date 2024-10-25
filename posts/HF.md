---
title: Hint Fiction
published_at: 2024-10-23
snippet: Reflection of my second assignment part 1.
disable_html_sanitization: true
allow_math: true
---

# Hint Fiction Reflection

## The Plan

For the first half of assignment 2, I chose to focus on the topic of 'Hint Fiction' (a form of extremely short story that is told in 25 words or less). When it comes to 'Hint Fiction', the more constraints, the greater the creation - and in this case - the less words, the more expansive the story. Because of this, what a hint fiction story is about is often up to interpretation. I wanted to try and utilise this aspect for the memory I chose: that of raising silkworms in class during 3rd grade, only for them to be taken and made into silk the minute they turned into cocoons.

## The Process

I decided to write my own 'Hint Fiction' story based on the memory, and kept the words ambiguous enough that users wouldn't immediately link it to silkworms. This was fairly easy to do, and within 5 minutes I had my story: "It squirmed about its cage, feasted upon the dead and slept inside a blanket of the finest silk, biding time until it finally was released."

![Reasoning](/assignment2/hintfiction.png)

My initial plan with this interactive work was a more game-like website with different stages representing the different stages of the silkworms' lives as we raised them. Stage one would have users clicking leaves and watching a silkworm move around on screen and 'eating' them. Stage two would occur right after the last leaf is eaten, where users can click on a stick and the silkworm would move over to it and become a cocoon. Stage three would have the users click on or drag the cocoon around. After clicking enough/dragging too far the cocoon would be animated to drop down the screen and into a pot of boiling water that appears. Stage four would have users click on the ladle in the pot to stir it around, and after enough stirs the end is triggered, with the ladle being lifted from the pot with strands of silk clinging to it, and a congratulatory message appears on screen.

![OG Plan](/assignment2/og1.png)

Unfortunately however, due to my limited skills with the coding languages I have chosen, I was unable to code past the stage where the silkworm becomes a cocoon, much to my frustration. No matter what I tried and how much of the code I've changed, the transition into the pot just wouldn't happen. I even got desperate enough to try and coonsult ChatGPT on how to fix my code, only for ChatGPT to fail to fix it too, and not only fail to fix it, but break everything else as well, resulting in me having to spend more time reworking everything back to how it was.

![OG Look](/assignment2/hintfiction1.png)

I was then running on less than four days to not only finish my 'Hint Fiction' work, but also to start and finish my 'Oulipo' interactive work as well. Things were not looking up with the initial idea, so I had to pivot. Instead of the game(?) I had originally planned, I decided to instead double down on the storytelling element, breaking the 'Hint Fiction' I wrote into 5 lines and coding it so that each time the user clicks on screen, one line appears accompanied by a change of image in the background. A far cry from the original idea, but at this point I was just desperate to finish and move on.

![Terrible Interactive Novel](/assignment2/hintfiction2.png)

Just like the previous attempt, I had decided to code out the entire interactive work on my own using html, css and JavaScript. The process for this second attempt went much smoother and faster than the first, and I was able to finish the code needed in less than a day, with the finished product being something I was most definitely not satisfied with, but had to make do with anyways. I added a screenshot with the instruction to click the screen and coded it so that it appears first when you load up the website before deciding that the work was as complete as I could get it before moving on to start my 'Oulipo' work.

![Honestly My Greatest Shame](/assignment2/hintfiction3.png)

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

Working on this half of the assignment was definitely a lot more frustrating than any other work I've done for this course this semester, especially for my initial idea for 'Hint Fiction'. Never have I felt a greater urge to bash my head against a wall and end my mysery, and I was very miserable. My second idea was alot easier to implement, and I was able to get it up and running in less than a day, a matter which greatly frustrates me, especially since I had struggled so much with the previous idea. It didin't help that I was still recovering from a cold and an allergic reaction to my medication, so all in all it was a miserable time for me, which I do think was reflected in the amount of (or lack thereof) care I had put into this interactive work.

This work, if I were to be generous, could be described as a short, interactive novel. I was definitely not satisfied with how it turned out, and, having now complete all of the required works for assignment 3, I can say that had I known 'Scratch' existed during this time, I most definitely would've - and should've - used that to code out my initial idea instead.

The initial shock factor I had planned to use in the first idea - that of the silkworm you raised into becoming a cocoon and expected to become a beautiful little silkmoth, only for it to be dumped into a pot of water, killed and had its cocoon turned into silk - couldn't be implemented at all in this second version, as the story and the time it took for the interaction to play out is too short for any shock value. The game-like elements had to all be stripped off as well, only leaving behind the storytelling elements in the final product.

Although this work did show elements of 'Hint Fiction' with its sequential structure, it still wasn't a good end product (or at the very least, one I was satisfied with) and it had a much less cohesive design than the rest of my interactive works. Both the final and initial ideas did push me further into wanting to explore more narrative-based games and interactions though, as I have always liked games and storytelling (I would, after all, like to one day work on the narratives in games), which can be seen later on in my last three works for this course.


