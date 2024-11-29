<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Interactive Cherry Blossom Flashcards</title>
    <link href="https://fonts.googleapis.com/css2?family=Great+Vibes&family=Playfair+Display:wght@400;700&display=swap" rel="stylesheet">
    <style>
        body, html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
            font-family: 'Arial', sans-serif;
        }

        /* Video Container */
        .video-container {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: -1;
            overflow: hidden;
        }

        video {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        /* Button container */
        .button-container {
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1;
        }

        .button-container button {
            padding: 15px 30px;
            font-size: 18px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }

        .button-container button:hover {
            background-color: #0056b3;
        }

        /* Section 2 (Interactive Flashcards) */
        #interactive-section {
            display: none;
            width: 100%;
            height: 100%;
            position: relative;
            flex-direction: column;
            overflow: hidden;
        }

        #top-margin-img {
            width: 100%;
            height: 30%; /* Increased height for the top margin */
            background: url('https://www.dropbox.com/scl/fi/qfbxmi3xa1iqw3mmf4b2k/20241129_095927.jpg?rlkey=dsmozxial1sunswjkm2wq7rw6&st=1xgi72ci&dl=1') no-repeat center center;
            background-size: cover;
        }

        #video-container {
            position: relative;
            flex: 1;
            display: flex;
            justify-content: center;
            align-items: center;
            overflow: hidden;
        }

        #video-bg {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 100vw;
            height: auto;
            transform: translate(-50%, -50%);
            z-index: -1;
            pointer-events: none;
        }

        .text-box {
            position: absolute;
            padding: 20px 30px;
            font-size: 1.5rem;
            background: url('https://www.dropbox.com/scl/fi/jvitnqnd2z3rh34hvdk4i/output_image.jpeg?rlkey=t2edgs3x69jlmvwglkw8hwfhf&st=sgxilviq&dl=1') no-repeat center center;
            background-size: cover;
            border: 2px solid rgba(255, 105, 180, 0.6);
            border-radius: 15px;
            text-align: center;
            color: #b76e79;
            text-shadow: 0 0 5px rgba(183, 110, 121, 0.8), 
                         0 0 10px rgba(183, 110, 121, 0.6), 
                         0 0 15px rgba(183, 110, 121, 0.4);
            max-width: 300px;
            height: 400px;
            display: flex;
            flex-direction: column;
            justify-content: center;
            transform-origin: center;
            transition: transform 0.8s, opacity 0.8s;
            cursor: pointer;
            perspective: 1000px;
            z-index: 1;
        }

        .text-box-left {
            top: 50%;
            left: 20%;
            transform: translate(-50%, -50%);
            box-shadow: -10px 10px 20px rgba(0, 0, 0, 0.3);
            font-family: 'Playfair Display', serif;
        }

        .text-box-right {
            top: 50%;
            right: 20%;
            transform: translate(50%, -50%);
            box-shadow: -10px 10px 20px rgba(0, 0, 0, 0.3);
            font-family: 'Great Vibes', cursive;
            font-size: 1.8rem;
        }

        .flipped {
            transform: translateY(-50%) rotateY(180deg);
        }

        #bottom-margin-img {
            width: 100%;
            height: 30%;
            background: url('https://www.dropbox.com/scl/fi/x26nfhpzkflj21q7834cd/20241129_095945.jpg?rlkey=6s5q7yj5awykib0jdssec7wl7&st=xqo4wuuw&dl=1') no-repeat center center;
            background-size: cover;
        }
    </style>
</head>
<body>

    <!-- Video Container (Initial) -->
    <div class="video-container">
        <video id="bgVideo" autoplay muted preload="auto">
            <source src="https://www.dropbox.com/scl/fi/uz0ujia9p7ug0tupzy58j/istockphoto-481914794-640_adpp_is-vmake.mp4?rlkey=8lsag7ry0lkv17pjh5vsmhhvr&st=uzmqqxy3&dl=1" type="video/mp4">
            Your browser does not support the video tag.
        </video>
    </div>

    <!-- Button Container -->
    <div class="button-container" id="buttonContainer">
        <button onclick="goToInteractiveSection()">Click Me</button>
    </div>

    <!-- Interactive Section (Flashcards) -->
    <div id="interactive-section">
        <!-- Top Margin Image -->
        <div id="top-margin-img"></div>

        <!-- Centered Video Background -->
        <div id="video-container">
            <video id="video-bg" autoplay loop muted>
                <source src="https://www.dropbox.com/scl/fi/upom995rz6wi7rrdrvg96/Register-Login.mp4?rlkey=w9osf5ailuvw8jr6eh3bo2b2c&st=mn36enws&dl=1" type="video/mp4">
                Your browser does not support the video tag.
            </video>

            <!-- Left Text Box -->
            <div class="text-box text-box-left" id="left-box">
                <p>Bottom Left Text</p>
            </div>

            <!-- Right Text Box -->
            <div class="text-box text-box-right" id="right-box">
                <p>Siddiqua, your beauty blooms like sakura petals, delicate yet captivating, a fleeting moment of eternal grace.</p>
            </div>
        </div>

        <!-- Bottom Margin Image -->
        <div id="bottom-margin-img"></div>
    </div>

    <script>
        const bgVideo = document.getElementById('bgVideo');
        const buttonContainer = document.getElementById('buttonContainer');
        const interactiveSection = document.getElementById('interactive-section');

        // Ensure the video starts playing as soon as it's ready
        bgVideo.oncanplay = () => {
            bgVideo.play();
        };

        // Show the button after the video ends
        bgVideo.onended = () => {
            buttonContainer.style.display = 'block';
        };

        // Redirect to the interactive section
        function goToInteractiveSection() {
            buttonContainer.style.display = 'none';
            interactiveSection.style.display = 'flex'; // Show interactive section
        }

        // Function to add flip effect
        function flipBox(box) {
            // Add the 'flipped' class to trigger the flip effect
            box.classList.add('flipped');
            
            // After flip animation, remove the class to allow re-triggering the flip effect
            setTimeout(() => {
                box.classList.remove('flipped');
            }, 800); // Match the flip duration (0.8s)
        }

        // Add click event listener for the left box
        document.getElementById('left-box').addEventListener('click', () => {
            flipBox(document.getElementById('left-box'));
        });

        
        // Add click event listener for the right box
        document.getElementById('right-box').addEventListener('click', () => {
            flipBox(document.getElementById('right-box'));
        });
    </script>

</body>
</html>
