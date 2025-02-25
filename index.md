---
layout: post
title: Take A Byte
search_exclude: true
hide: true
---

<div style="text-align: center; margin: 0;">
    <h1 style="font-size: 48px; font-weight: bold; color: #4CAF50; text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2); background: -webkit-linear-gradient(45deg, #4CAF50, #2196F3); -webkit-background-clip: text; -webkit-text-fill-color: transparent; letter-spacing: 2px;">Take a Byte</h1>
</div>

<!--menu: nav/home.html-->

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Website Search</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
        }
        .search-container {
            position: fixed;
            top: 5px;
            right: 5px;
            width: 300px;
            transition: all 0.3s ease;
            z-index: 99999;
        }
        .search-container * {
            z-index: 99999;
        }
        .search-container input[type="text"] {
            width: calc(100% - 20px);
            padding: 15px 45px 15px 20px;
            font-size: 16px;
            border: 2px solid #e0e0e0;
            border-radius: 25px;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(5px);
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
            position: relative;
        }
        .search-container input[type="text"]:focus {
            width: calc(100% + 50px);
            border-color: #4CAF50;
            box-shadow: 0 6px 20px rgba(76, 175, 80, 0.2);
            outline: none;
        }
        .search-container button {
            position: absolute;
            right: 5px;
            top: 50%;
            transform: translateY(-50%);
            background: none;
            border: none;
            padding: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        .search-container button:hover {
            transform: translateY(-50%) scale(1.1);
        }
        .search-icon {
            width: 20px;
            height: 20px;
            border: 2px solid #4CAF50;
            border-radius: 50%;
            position: relative;
        }
        .search-icon::after {
            content: '';
            position: absolute;
            right: -7px;
            bottom: -7px;
            width: 10px;
            height: 2px;
            background: #4CAF50;
            transform: rotate(45deg);
        }
        .results {
            position: absolute;
            top: 100%;
            left: 0;
            right: 0;
            margin-top: 10px;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 15px;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            max-height: 300px;
            overflow-y: auto;
            opacity: 0;
            transform: translateY(-10px);
            transition: all 0.3s ease;
            z-index: 1000;
        }
        .results.show {
            opacity: 1;
            transform: translateY(0);
        }
        .results a {
            display: block;
            padding: 12px 20px;
            color: #333;
            text-decoration: none;
            border-bottom: 1px solid #eee;
            transition: all 0.2s ease;
        }
        .results a:hover {
            background: rgba(76, 175, 80, 0.1);
            padding-left: 25px;
        }
        .results a:last-child {
            border-bottom: none;
        }
        .aboutButton {
            display: inline-block;
            padding: 10px 20px;
            font-size: 18px;
            color: white !important;
            background-color: #4CAF50;
            text-decoration: none;
            border-radius: 8px;
            transition: background 0.3s ease, transform 0.2s ease;
            margin-top: -80px;
        }
        .aboutButton:hover {
            background-color: #45a049;
            transform: scale(1.05);
            color: white !important;
        }
        /* Add specific style for the link */
        h3 a.aboutButton {
            color: white !important;
        }
        #search-results {
            position: fixed;
            z-index: 99999;
            background: white;
            width: 300px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Search for content...">
        <button onclick="performSearch()">Search</button>
        <div class="results" id="results">
            <!-- Search results will appear here -->
        </div>
    </div>
    
<script>
        // List of all pages and their content
        const pages = [
            { url: '/flocker_frontend_period4/navigation/about', title: 'About', keywords: ['about', 'team', 'company', 'fridge'] },
            { url: '/flocker_frontend_period4/navigation/cuisine/thai', title: 'Thai Cuisine', keywords: ['thai', 'food', 'asian', 'cuisine'] },
            { url: '/flocker_frontend_period4/navigation/cuisine/indian', title: 'Indian Cuisine', keywords: ['indian', 'food', 'curry', 'cuisine'] },
            { url: '/flocker_frontend_period4/navigation/cuisine/italian', title: 'Italian Cuisine', keywords: ['italian', 'pasta', 'pizza', 'cuisine'] },
            { url: '/flocker_frontend_period4/navigation/buttons/posting', title: 'Post Recipe', keywords: ['post', 'recipe', 'share', 'create'] },
            { url: '/flocker_frontend_period4/navigation/feedback', title: 'Feedback', keywords: ['feedback', 'comments', 'suggestions'] },
            { url: '/flocker_frontend_period4/natcountrygen', title: 'NatCountryGen', keywords: ['generator', 'country', 'national'] }
            // Add more pages as needed
        ];

        function performSearch() {
            const query = document.getElementById('searchInput').value.trim().toLowerCase();
            const resultsContainer = document.getElementById('results');
            resultsContainer.innerHTML = '';

            if (!query) {
                resultsContainer.classList.remove('show');
                return;
            }

            const matchingPages = pages.filter(page => 
                page.keywords.some(keyword => keyword.includes(query)) ||
                page.title.toLowerCase().includes(query)
            );

            if (matchingPages.length === 0) {
                resultsContainer.innerHTML = '<div style="padding: 12px 20px; color: #666;">No results found</div>';
            } else {
                matchingPages.forEach(page => {
                    const link = document.createElement('a');
                    link.href = page.url;
                    link.textContent = page.title;
                    resultsContainer.appendChild(link);
                });
            }

            resultsContainer.classList.add('show');
        }

        // Add event listener for real-time search
        document.getElementById('searchInput').addEventListener('input', performSearch);

        // Close results when clicking outside
        document.addEventListener('click', (e) => {
            const resultsContainer = document.getElementById('results');
            const searchContainer = document.querySelector('.search-container');
            if (!searchContainer.contains(e.target)) {
                resultsContainer.classList.remove('show');
            }
        });
    </script>

    <h3></h3>
    <h3>
      <a class="FridgeButton" href="{{site.baseurl}}/about">
        <div class="mini-fridge">
          <div class="mini-handle"></div>
          <div class="mini-display">4°C</div>
          <div class="mini-shelf"></div>
          <div class="mini-shelf"></div>
        </div>
      </a>
    </h3>

    <style>
      .FridgeButton {
        display: inline-block;
        width: 250px;    /* Reduced from 300px to 250px */
        height: 500px;    /* Reduced from 600px to 500px */
        text-decoration: none;
        position: absolute;
        right: 20px;
        top: 150px;
        transition: all 0.3s ease;
        transform-style: preserve-3d;
        perspective: 1000px;
        z-index: 9999;
        margin: 0;
        padding: 0;
        background: none;
      }

      .mini-fridge {
        width: 100%;
        height: 100%;
        background: linear-gradient(145deg, #e8e8e8, #d4d4d4);
        border-radius: 15px;
        position: relative;
        box-shadow: 
          0 8px 15px rgba(0,0,0,0.2),
          inset 0 0 0 2px rgba(255,255,255,0.1);
        overflow: hidden;
        transition: all 0.3s ease;
        transform-origin: left center;  /* Changed from right to left */
      }

      .FridgeButton:hover .mini-fridge {
        transform: rotateY(15deg);    /* Changed from -15deg to 15deg */
        box-shadow: 
          -20px 8px 15px rgba(0,0,0,0.2),  /* Changed shadow direction */
          inset 0 0 0 2px rgba(255,255,255,0.2);
      }

      /* Adjust inner shadow direction */
      .mini-fridge::before {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: linear-gradient(
          to left,          /* Changed from right to left */
          rgba(0,0,0,0.2) 0%,
          transparent 20%
        );
        opacity: 0;
        transition: opacity 0.3s ease;
        pointer-events: none;
      }

      .FridgeButton:hover .mini-fridge::before {
        opacity: 1;
      }

      .mini-handle {
        position: absolute;
        right: 15px;          /* Adjusted from 12px */
        top: 50%;
        transform: translateY(-50%);
        width: 12px;          /* Increased from 10px */
        height: 60%;
        background: linear-gradient(90deg, #888, #999);
        border-radius: 4px;
        box-shadow: 
          inset -1px 0 3px rgba(0,0,0,0.3),
          1px 0 2px rgba(255,255,255,0.5);
      }

      .mini-display {
        position: absolute;
        top: 20px;            /* Adjusted from 15px */
        left: 50%;
        transform: translateX(-50%);
        background: #000;
        color: #00ff00;
        padding: 6px 12px;    /* Increased from 4px 10px */
        border-radius: 4px;
        font-family: 'Digital', monospace;
        font-size: 16px;      /* Increased from 14px */
        box-shadow: 
          inset 0 0 3px rgba(0,255,0,0.5),
          0 0 5px rgba(0,0,0,0.2);
      }

      .mini-shelf {
        position: absolute;
        left: 10%;
        width: 80%;
        height: 1px;
        background: rgba(0,0,0,0.1);
        box-shadow: 0 1px 2px rgba(255,255,255,0.5);
      }

      .mini-shelf:nth-child(3) { top: 40%; }
      .mini-shelf:nth-child(4) { top: 70%; }

      .mini-fridge::after {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        background: 
          repeating-linear-gradient(
            45deg,
            rgba(255,255,255,0.05) 0px,
            rgba(255,255,255,0.05) 1px,
            transparent 1px,
            transparent 3px
          );
        pointer-events: none;
      }
    </style>
</body>


</html>

<style>
    .nav-links {
        display: grid;
        grid-template-columns: repeat(2, auto); /* Two columns */
        grid-template-rows: repeat(2, auto); /* Two rows */
        gap: 10px;
        justify-content: center;
        margin: 20px 0;
    }

    .nav-links h3 {
        margin: 0;
    }

    .nav-links a {
        font-size: 14px;  /* Increased from 12px */
        padding: 8px 16px;  /* Increased from 6px 12px */
        background-color: #4CAF50;
        color: white;
        text-decoration: none;
        border-radius: 4px;
        display: inline-block;
    }

    .nav-links a:hover {
        background-color: #45a049;
    }
</style>

<div style="display: flex; justify-content: center; align-items: center; flex-direction: column;">
    <div class="nav-links" style="display: flex; justify-content: center; gap: 20px; margin-left: 50px;">
        <h3><a href="{{site.baseurl}}/navigation/about">About page</a></h3>
        <h3><a href="{{site.baseurl}}/natcountrygen">Regional Dishes Generator</a></h3>
        <h3><a href="{{site.baseurl}}/navigation/buttons/posting">User Posts</a></h3>
        <h3><a href="{{site.baseurl}}/navigation/feedback">User Experience</a></h3>
    </div>

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Random Cuisine Spinner</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f4f4f9;
        }
        .wheel-container {
            position: relative;
            width: 400px;    /* Increased from 300px */
            height: 400px;   /* Increased from 300px */
            border-radius: 50%;
            border: 5px solid #000;
            overflow: hidden;
            margin-top: 50px;
        }
        .wheel {
            width: 100%;
            height: 100%;
            position: absolute;
            transform-origin: center;
            transition: transform 5s cubic-bezier(0.33, 1, 0.68, 1);
        }
        .slice {
            position: absolute;
            width: 50%;
            height: 50%;
            background-color: #eee;
            border: 2px solid #ccc;
            box-sizing: border-box;
            clip-path: polygon(0% 0%, 100% 0%, 50% 100%);
            transform-origin: 100% 100%;
            display: flex; 
            justify-content: center;  
            align-items: center; 
            color: white;  
            font-size: 16px;   /* Increased from 12px to match larger wheel */
            text-align: center; 
            padding: 5px;  
        }
        .slice:nth-child(1) { background: #f94144; transform: rotate(0deg); }
        .slice:nth-child(2) { background: #f3722c; transform: rotate(60deg); }
        .slice:nth-child(3) { background: #f8961e; transform: rotate(120deg); }
        .slice:nth-child(4) { background: #f9c74f; transform: rotate(180deg); }
        .slice:nth-child(5) { background: #90be6d; transform: rotate(240deg); }
        .slice:nth-child(6) { background: #43aa8b; transform: rotate(300deg); }
        .pointer {
            position: absolute;
            top: -15px;
            left: 50%;
            width: 0;
            height: 0;
            border-left: 15px solid transparent;
            border-right: 15px solid transparent;
            border-bottom: 30px solid #000;
            transform: translateX(-50%);
            z-index: 1;
        }
        button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
            background-color: #4caf50;
            color: white;
            border: none;
            border-radius: 5px;
        }
        button:disabled {
            background-color: #aaa;
            cursor: not-allowed;
        }
        #result {
            margin-top: 20px;
            font-size: 20px;
            font-weight: bold;
            color: #333;
        }
        #spinButton {
          transform: translateX(50%)
        }
    </style>
</head>
<body>
    <div class="wheel-container">
        <div class="pointer"></div>
        <div class="wheel" id="wheel">
            <div class="slice">Italian food</div>
            <div class="slice">Chinese food</div>
            <div class="slice">Indian food</div>
            <div class="slice">Japanese food</div>
            <div class="slice">Mexican food</div>
            <div class="slice">Thai food</div>
        </div>
    </div>
    <button id="spinButton" style="transform: none; margin-top: 20px; padding: 15px 40px; font-size: 18px; min-width: 200px;" onclick="spinWheel()">Spin the Wheel</button>
    <div id="result"></div>

<script>
        let currentRotation = 0;
        const spinButton = document.getElementById('spinButton');
        const resultDiv = document.getElementById('result');

        // Add single wheel sound effect
        const spinSound = new Audio('https://cdn.freesound.org/previews/242/242501_4414128-lq.mp3');  // Click sound for spin

        function spinWheel() {
            spinButton.disabled = true;
            
            // Play spin sound
            spinSound.volume = 0.3;
            spinSound.play();

            const wheel = document.getElementById('wheel');
            const randomRotations = Math.floor(Math.random() * 4) + 5;
            const randomSlice = Math.floor(Math.random() * 360);
            const totalRotation = randomRotations * 360 + randomSlice;

            currentRotation += totalRotation;
            wheel.style.transform = `rotate(${currentRotation}deg)`;

            setTimeout(() => {
                spinButton.disabled = false;
                const normalizedRotation = currentRotation % 360;
                const sliceIndex = (6 - Math.floor(normalizedRotation / 60)) % 6;
                const slices = document.querySelectorAll('.slice');
                const selectedCuisine = slices[sliceIndex].textContent;
                resultDiv.textContent = `The Spinner Chose: ${selectedCuisine}`;
                createDynamicButton(selectedCuisine);
            }, 5000);
        }

        // Function to create a button and set its link based on the selected cuisine
        function createDynamicButton(selectedCuisine) {
            // Clear any existing button
            const existingButton = document.querySelector(".dynamic-link");
            if (existingButton) existingButton.remove();

            // Get the link associated with the selected cuisine
            const link = cuisinePages[selectedCuisine];

            if (link) {
                // Create a button element
                const button = document.createElement("button");
                button.textContent = `Go to ${selectedCuisine.charAt(0).toUpperCase() + selectedCuisine.slice(1)} Cuisine`;
                button.classList.add("dynamic-link");

                // Add a click event to redirect to the page
                button.addEventListener("click", () => {
                    window.location.href = link;
                });

                // Append the button to the document body (or another container)
                document.body.appendChild(button);
            }
        }

        // Object mapping variable values to URLs
        const cuisinePages = {
            "Italian food": "{{site.baseurl}}/navigation/cuisine/italian",
            "Chinese food": "{{site.baseurl}}/navigation/cuisine/chinese",
            "Indian food": "{{site.baseurl}}/navigation/cuisine/indian",
            "Japanese food": "{{site.baseurl}}/navigation/cuisine/japanese",
            "Mexican food": "{{site.baseurl}}/navigation/cuisine/mexican",
            "Thai food": "{{site.baseurl}}/navigation/cuisine/thai"
        };
    </script>
</body>







<head>
  <title>Recipe Posting System</title>
  <style>
        /* Wrapper to isolate the container */
        .recipe-wrapper {
        position: absolute;
        top: 55%; /* Reduced from 60% to move it higher */
        left: 20px;
        transform: translateY(-50%);
        padding: 15px;
        max-width: 300px;  /* Reduced from 400px to 300px */
        background-color: #f4f4f9;
        border: 1px solid #ddd;
        border-radius: 10px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        font-size: 12px;
        }
        body {
        margin: 0;
        padding: 0;
        background-color: #fdfdfd;
        }
        .container {
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }
        form label {
        display: block; /* Labels on their own line */
        margin-bottom: 5px; /* Small spacing below the label */
        font-weight: bold;
        color: #333;
        }
        .recipe-wrapper h1 {
            font-size: 18px;
            margin-bottom: 10px;
        }
        .recipe-wrapper input,
        .recipe-wrapper textarea {
            width: 90%;
            padding: 5px;
            margin-bottom: 8px;
        }
        .recipe-wrapper .container {
            padding: 10px;
        }
        .discover-more-button {
          display: inline-block;
          margin-top: 10px;
          padding: 10px 15px;
          background-color: #007BFF;
          color: white;
          text-decoration: none;
          border-radius: 5px;
          font-size: 1rem;
      }
      .discover-more-button:hover {
          background-color: #0056b3;
      }
    </style>
</head>

<body>
  <section class="recipe-wrapper">
    <div class="container">
        <h1>Recipe Posting System</h1>
        <form id="recipeForm">
        <label for="name">Your Name:</label>
        <input type="text" id="name" placeholder="Enter your name" required>
        <label for="dish">Dish Name:</label>
        <input type="text" id="dish" placeholder="Enter the dish name" required>
        <label for="cuisine">Cuisine:</label>
        <input type="text" id="cuisine" placeholder="Enter the cuisine type" required>
        <label for="link">Recipe Link:</label>
        <input type="url" id="link" placeholder="Enter the recipe URL" required>
        <label for="comments">Comments:</label>
        <textarea id="comments" placeholder="Enter your comments" required></textarea>
        <button type="submit">Post Recipe</button>
        <button type="button" onclick="window.location.href='/flocker_frontend_period4/navigation/buttons/posting';">Go to Posts</button>
        </form>
        <div id="postsContainer">
        </div>
    </div>


  <script>
      var pythonURI;
      if (location.hostname === "localhost") {
        pythonURI = "http://localhost:8887";
      } else if (location.hostname === "127.0.0.1") {
        pythonURI = "http://127.0.0.1:8887";
      } else {
        pythonURI = "https://takeabyte.stu.nighthawkcodingsociety.com";
      }

      document.getElementById("recipeForm").addEventListener("submit", async function (e) {
        e.preventDefault();

        const name = document.getElementById("name").value;
        const dish = document.getElementById("dish").value;
        const cuisine = document.getElementById("cuisine").value;
        const link = document.getElementById("link").value;
        const comments = document.getElementById("comments").value;

        const post = { name, dish, cuisine, link, comments };

        try {
          const response = await fetch(pythonURI + "/api/posting/create", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: JSON.stringify(post)
          });

          if (response.ok) {
            document.querySelector(".container").innerHTML = `
              <p>Your recipe has been posted!</p>
              <p><a href="/flocker_frontend_period4/navigation/buttons/posting" class="discover-more-button">Discover More</a></p>
            `;
          } else {
            alert("Error posting recipe.");
          }
        } catch (error) {
          console.error("Error:", error);
        }
      });
    </script>
