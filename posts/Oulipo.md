---
title: Oulipo Reflection
published_at: 2024-10-23
snippet: Reflection of my second assignment part 2.
disable_html_sanitization: true
allow_math: true
---

# Oulipo Reflection

## The Plan

For this second half of assignment 2, I chose to focus on the 'Oulipo' (or in full French: “Ouvroir de Littéreature Potentielle”, meaning the “Workshop for Potential Literature”). It is a constrained form of writing, which I thought would fit well with the memory I chose to represent in the work - that of me being strapped down to a table in a hospital in HongKong one day when I was running a high fever. 

## The Process

During my preparation for this interactive work, I had researched on different types of 'Oulipo' techniques, before ultimately focusing on the 'cut-up' technique, where you cut up lines of a piece of work to create a new story. My original idea was for there to be words scattered across the bottom of the website, and users could click and drag them across the screen to form sentences. Forming the right sentence would trigger a change in the website, such as an image appearing.

![OG Plan](/assignment2/og.png)

Unfortunately however, due to my limited skills with the coding languages I have chosen, I was unable to get the physics/movement of the individual words the way I wanted it to be. The result was a clunky mess, with the words often clipping into each other when re-generated. Furthermore, placing the words in the correct sequence did not trigger any change in the environment, and so I decided to try and do something else instead.

![OG Plan](/assignment2/fail.png)

Taking inspiration from the grid-like model I saw on the Canvas slides during class, I decided to have my work take the form of two 4x3 grids, where users will have to click the right words in the right sequence to form a sentence describing what had happened that day in the hospital room. Triggering the right sequence of words also causes the background to change, whilst pressing the wrong word resets the entire thing.

![Oulipo Grids](/assignment2/oulipo.png)

Just like the previous assignment, I had decided to code out the entire interactive work on my own using html, css and JavaScript. The process for this second attempt went much smoother and faster than the first, and I was able to finish the code needed in less than a day, with the finished product being what I expected it to be. I chose for there to be 3 different sentences the users have to piece together, each describing a different perspective/thing that was happening in the hospital room at the time.

![Oulipo End](/assignment2/oulipo1.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Oulipo</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="tables-container">
        <table id="table1">
            <tbody>
                <tr>
                    <td data-target="1-1">Some</td>
                    <td data-target="1-2">A</td>
                    <td data-target="1-3">The</td>
                </tr>
                <tr>
                    <td data-target="2-1">On</td>
                    <td data-target="2-2">Prepares</td>
                    <td data-target="2-3">Strap</td>
                </tr>
                <tr>
                    <td data-target="3-1">Hospital</td>
                    <td data-target="3-2">Restraints</td>
                    <td data-target="3-3">Instruments</td>
                </tr>
                <tr>
                    <td data-target="4-1">Torture</td>
                    <td data-target="4-2">Cries</td>
                    <td data-target="4-3">Themselves</td>
                </tr>
            </tbody>
        </table>

        <table id="table2">
            <tbody>
                <tr>
                    <td data-target="1-1">Nurses</td>
                    <td data-target="1-2">Child</td>
                    <td data-target="1-3">Doctor</td>
                </tr>
                <tr>
                    <td data-target="2-1">The</td>
                    <td data-target="2-2">His</td>
                    <td data-target="2-3">The</td>
                </tr>
                <tr>
                    <td data-target="3-1">Of</td>
                    <td data-target="3-2">For</td>
                    <td data-target="3-3">Bed</td>
                </tr>
                <tr>
                    <td data-target="4-1">Steadily</td>
                    <td data-target="4-2">Out</td>
                    <td data-target="4-3">On</td>
                </tr>
            </tbody>
        </table>
    </div>
    <div id="background-image"></div>
    <script src="script.js"></script>
</body>
</html>
```
```css
body {
    margin: 0;
    font-family: Arial, sans-serif;
    height: 100vh;
    overflow: hidden;
    display: flex;
    align-items: center;
    justify-content: center;
}

.tables-container {
    display: flex;
    justify-content: space-around;
    align-items: center;
}

table {
    border-collapse: collapse;
    width: 200px;
    margin: 10px;
}

td {
    border: 1px solid #ddd;
    padding: 20px;
    text-align: center;
    cursor: pointer;
    transition: background-color 0.3s;
}

td:hover {
    background-color: #f0f0f0;
}

td.clicked {
    background-color: #5ab2ff;
}

#background-image {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-size: cover;
    background-position: center;
    z-index: -1;
    transition: background-image 1s ease-in-out;
}
```
```javascript
document.addEventListener('DOMContentLoaded', () => {
    const sequences = [
        ['1-1', '1-1', '2-3', '2-1', '3-2', '3-2', '4-1', '4-3'],
        ['1-2', '1-3', '2-2', '2-2', '3-3', '3-1', '4-1', '4-1'],
        ['1-3', '1-2', '2-1', '2-3', '3-1', '3-3', '4-2', '4-2']
    ];

    const backgroundImages = [
        'url(img/bg1.jpg)',
        'url(img/bg2.jpg)',
        'url(img/bg3.jpg)',
    ];

    let currentSequenceIndex = 0;
    let currentStep = 0;

    function handleCellClick(event) {
        const cell = event.target;
        const target = cell.getAttribute('data-target');

        if (target === sequences[currentSequenceIndex][currentStep]) {
            cell.classList.add('clicked');
            currentStep++;

            if (currentStep >= sequences[currentSequenceIndex].length) {
                // Apply corresponding background image
                const backgroundImage = backgroundImages[currentSequenceIndex];
                document.getElementById('background-image').style.backgroundImage = backgroundImage;
                
                // Reset for next interactions
                currentStep = 0;
                document.querySelectorAll('.clicked').forEach(clickedCell => {
                    clickedCell.classList.remove('clicked');
                });

                currentSequenceIndex = (currentSequenceIndex + 1) % sequences.length;
            }
        } else {
            currentStep = 0;
            document.querySelectorAll('.clicked').forEach(clickedCell => {
                clickedCell.classList.remove('clicked');
            });
        }
    }

    document.querySelectorAll('td').forEach(cell => {
        cell.addEventListener('click', handleCellClick);
    });
});
```

## The Outcome

Working on this second half of the assignment was definitely a lot easier than the first half, and though I did encounter some problems with my initial iteration of the work, I was able to overcome this fairly quickly. Part of this was due to the limited time I had, as I had been down with a cold for one week, then by an allergic reaction to my medication the next. I then struggled with the other half of the assignment, having spent time on it until I only had 2 days' time to start and finish this one. The looming deadline was most definitely a motivating factor, as well as the sheer frustration I felt over the other interactive work.

Despite all that though, I was still quite satisfied with the finished product for this one. Of course, if I had more time I might have made more sentences the users could play around with, or drawn out the scenes instead of using image substitutes. I do think that the way it ended up being had fit quite well with the themes of limits and constraints we had, and the fact that there are no instructions - so the users have to guess and figure out themselves what they're meant to do - parallels how I felt all those years ago, strapped down to a table, not knowing what was happening. As users piece together the sentences they slowly come to understand the story(?) that is being told, and the image appearing in the background once a sentence is complete further drives it home.

The final interactive work was something that can be frustrating to newcomers, as they do have to try and memorise the sequence of when and what to click. This was an intentional part of the design, as the limits placed on the users was meant to mimic the restraints placed on me all those years ago. This one was also definitely a more cohesive design than that of the 'Hint Fiction' and 'Ma' interactive works, delivering aspects of both the 'Oulipo' and that of my memory clearly.

The design of this one is more playful than that of the previous two works, with a puzzle-solving-esque element making up the main interaction for the users. Both my 'Oulipo' and my 'Hint Fiction' works also lead me to want to explore more narrative-based interactions, games with plot and storylines, which dsoes end up influencing my last three works for this course.


