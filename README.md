<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Practice Game</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            min-height: 100vh;
            padding: 20px;
            color: #333;
        }

        .container {
            max-width: 900px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.15);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 35px;
            text-align: center;
            position: relative;
        }

        .header::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><circle cx="20" cy="20" r="2" fill="rgba(255,255,255,0.1)"/><circle cx="80" cy="30" r="1.5" fill="rgba(255,255,255,0.1)"/><circle cx="40" cy="70" r="1" fill="rgba(255,255,255,0.1)"/><circle cx="90" cy="80" r="2.5" fill="rgba(255,255,255,0.1)"/></svg>');
        }

        .header h1 {
            font-size: 2.8em;
            margin-bottom: 15px;
            position: relative;
            z-index: 1;
            text-shadow: 2px 2px 4px rgba(0,0,0,0.2);
        }

        .header p {
            font-size: 1.3em;
            opacity: 0.95;
            position: relative;
            z-index: 1;
            max-width: 600px;
            margin: 0 auto;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 15px;
            padding: 25px;
            background: #f8f9fa;
            flex-wrap: wrap;
            border-bottom: 1px solid #e9ecef;
        }

        .nav-btn {
            padding: 14px 28px;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            font-size: 17px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(71,118,230,0.35);
        }

        .nav-btn:not(.active) {
            background: white;
            color: #555;
            border: 2px solid #e9ecef;
        }

        .nav-btn:hover:not(.active) {
            background: #e9ecef;
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.12);
        }

        .game-section {
            display: none;
            padding: 35px;
            min-height: 450px;
        }

        .game-section.active {
            display: block;
            animation: fadeIn 0.6s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(25px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .word-bank {
            background: linear-gradient(135deg, #f5f7fa, #eef2f8);
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 30px;
            border: 2px solid #4776E6;
            box-shadow: 0 6px 20px rgba(71,118,230,0.18);
        }

        .word-bank h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            text-align: center;
            font-size: 1.5em;
        }

        .word-options {
            display: flex;
            flex-wrap: wrap;
            gap: 15px;
            justify-content: center;
        }

        .word-option {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 12px 22px;
            border-radius: 25px;
            font-weight: bold;
            font-size: 17px;
            box-shadow: 0 4px 12px rgba(71,118,230,0.3);
            transition: all 0.3s ease;
            cursor: default;
        }

        .word-option:hover {
            transform: translateY(-3px);
            box-shadow: 0 7px 18px rgba(71,118,230,0.4);
        }

        .question {
            background: #f8f9fa;
            padding: 28px;
            border-radius: 18px;
            margin-bottom: 25px;
            border-left: 6px solid #4776E6;
            box-shadow: 0 6px 18px rgba(0,0,0,0.06);
        }

        .question h3 {
            color: #2c3e50;
            margin-bottom: 18px;
            font-size: 1.4em;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .question h3:before {
            content: "•";
            color: #4776E6;
            font-size: 1.8em;
        }

        .question p {
            font-size: 18px;
            line-height: 1.6;
        }

        .fill-blank {
            background: #fff;
            border: 2px solid #ddd;
            border-radius: 10px;
            padding: 10px 15px;
            font-size: 17px;
            margin: 0 6px;
            min-width: 140px;
            transition: border-color 0.3s ease;
            box-shadow: 0 2px 8px rgba(0,0,0,0.08);
        }

        .fill-blank:focus {
            outline: none;
            border-color: #4776E6;
            box-shadow: 0 0 0 3px rgba(71,118,230,0.2);
        }

        .fill-blank.correct {
            border-color: #4CAF50;
            background: #e8f5e8;
        }

        .fill-blank.incorrect {
            border-color: #f44336;
            background: #ffeaea;
        }

        .options {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 18px;
            margin-top: 18px;
        }

        .option {
            padding: 18px 22px;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .option:hover {
            border-color: #4776E6;
            transform: translateY(-3px);
            box-shadow: 0 6px 18px rgba(71,118,230,0.15);
        }

        .option.selected {
            background: #4776E6;
            color: white;
            border-color: #4776E6;
        }

        .option.correct {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .option.incorrect {
            background: #f44336;
            color: white;
            border-color: #f44336;
        }

        .matching-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 35px;
            margin-top: 25px;
        }

        .match-column h4 {
            text-align: center;
            margin-bottom: 20px;
            color: #2c3e50;
            font-size: 1.3em;
            padding-bottom: 10px;
            border-bottom: 2px solid #e9ecef;
        }

        .match-item {
            padding: 18px;
            margin: 10px 0;
            border: 2px solid #ddd;
            border-radius: 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            background: white;
            text-align: center;
            font-weight: 500;
            font-size: 16px;
        }

        .match-item:hover {
            border-color: #4776E6;
            transform: translateY(-2px);
        }

        .match-item.selected {
            background: #f0f4ff;
            border-color: #4776E6;
            box-shadow: 0 4px 12px rgba(71,118,230,0.2);
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
            cursor: default;
            transform: scale(0.98);
        }

        .check-btn {
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            border: none;
            padding: 18px 35px;
            border-radius: 30px;
            cursor: pointer;
            font-size: 18px;
            font-weight: bold;
            margin: 30px auto;
            display: block;
            transition: all 0.3s ease;
            box-shadow: 0 6px 20px rgba(71,118,230,0.3);
        }

        .check-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 9px 25px rgba(71,118,230,0.4);
        }

        .check-btn:active {
            transform: translateY(1px);
        }

        .feedback {
            margin-top: 20px;
            padding: 18px;
            border-radius: 12px;
            font-weight: bold;
            text-align: center;
            animation: slideIn 0.4s ease;
            font-size: 17px;
        }

        @keyframes slideIn {
            from { transform: translateX(-25px); opacity: 0; }
            to { transform: translateX(0); opacity: 1; }
        }

        .feedback.correct {
            background: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }

        .feedback.incorrect {
            background: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }

        .score {
            position: fixed;
            top: 25px;
            right: 25px;
            background: linear-gradient(135deg, #4776E6, #8E54E9);
            color: white;
            padding: 18px 25px;
            border-radius: 30px;
            font-weight: bold;
            box-shadow: 0 6px 20px rgba(71,118,230,0.35);
            z-index: 1000;
            font-size: 18px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .score-number {
            font-size: 1.2em;
            background: white;
            color: #4776E6;
            width: 35px;
            height: 35px;
            border-radius: 50%;
            display: inline-flex;
            align-items: center;
            justify-content: center;
        }

        @media (max-width: 768px) {
            .matching-container {
                grid-template-columns: 1fr;
                gap: 25px;
            }
            
            .nav-buttons {
                flex-direction: column;
                align-items: center;
            }
            
            .nav-btn {
                width: 220px;
            }
            
            .score {
                position: static;
                margin: 20px auto;
                display: inline-flex;
                width: auto;
            }
            
            .header h1 {
                font-size: 2.2em;
            }
            
            .header p {
                font-size: 1.1em;
            }
        }
    </style>
</head>
<body>
    <div class="score">Score: <span class="score-number" id="score">0</span>/24</div>
    
    <div class="container">
        <div class="header">
            <h1>📚 Vocabulary Builder</h1>
            <p>Practice and master these useful English words and phrases!</p>
        </div>

        <div class="nav-buttons">
            <button class="nav-btn active" onclick="showSection('fill-gaps')">Fill in the Gaps</button>
            <button class="nav-btn" onclick="showSection('matching')">Match Meanings</button>
            <button class="nav-btn" onclick="showSection('multiple-choice')">Multiple Choice</button>
        </div>

        <!-- Fill in the Gaps Section -->
        <div id="fill-gaps" class="game-section active">
            <div class="word-bank">
                <h3>📝 Word Bank - Choose from these words:</h3>
                <div class="word-options">
                    <span class="word-option">un-plug</span>
                    <span class="word-option">forecast</span>
                    <span class="word-option">odd</span>
                    <span class="word-option">yield</span>
                    <span class="word-option">almost</span>
                    <span class="word-option">striking</span>
                    <span class="word-option">unprecedented</span>
                    <span class="word-option">wondering</span>
                </div>
            </div>

            <div id="questions-container">
                <!-- Questions will be dynamically inserted here -->
            </div>

            <button class="check-btn" onclick="checkFillBlanks()">Check Answers</button>
        </div>

        <!-- Matching Section with Shuffled Meanings -->
        <div id="matching" class="game-section">
            <h2 style="text-align: center; margin-bottom: 25px; color: #2c3e50;">Match the words with their meanings</h2>
            <div class="matching-container">
                <div class="match-column">
                    <h4>Words</h4>
                    <div class="match-item" data-word="un-plug" onclick="selectMatch(this)">un-plug</div>
                    <div class="match-item" data-word="forecast" onclick="selectMatch(this)">forecast</div>
                    <div class="match-item" data-word="odd" onclick="selectMatch(this)">odd</div>
                    <div class="match-item" data-word="yield" onclick="selectMatch(this)">yield</div>
                    <div class="match-item" data-word="almost" onclick="selectMatch(this)">almost</div>
                    <div class="match-item" data-word="striking" onclick="selectMatch(this)">striking</div>
                    <div class="match-item" data-word="unprecedented" onclick="selectMatch(this)">unprecedented</div>
                    <div class="match-item" data-word="wondering" onclick="selectMatch(this)">wondering</div>
                </div>
                <div class="match-column" id="meanings-column">
                    <h4>Meanings</h4>
                    <!-- Meanings will be dynamically inserted here -->
                </div>
            </div>
            <div class="feedback" id="matching-feedback" style="display: none;"></div>
        </div>

        <!-- Multiple Choice Section -->
        <div id="multiple-choice" class="game-section">
            <div class="question">
                <h3>Question 1: What does "unprecedented" mean?</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Expected and predictable</div>
                    <div class="option" onclick="selectOption(this, true)">Never done or known before</div>
                    <div class="option" onclick="selectOption(this, false)">Common and ordinary</div>
                    <div class="option" onclick="selectOption(this, false)">Recently discovered</div>
                </div>
                <div class="feedback" id="mc-feedback-1" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 2: If something is "striking", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Boring and unnoticeable</div>
                    <div class="option" onclick="selectOption(this, false)">Violent or aggressive</div>
                    <div class="option" onclick="selectOption(this, true)">Very impressive or noticeable</div>
                    <div class="option" onclick="selectOption(this, false)">Metallic in appearance</div>
                </div>
                <div class="feedback" id="mc-feedback-2" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 3: When you "un-plug" something, you:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Connect it to power</div>
                    <div class="option" onclick="selectOption(this, true)">Disconnect it from power</div>
                    <div class="option" onclick="selectOption(this, false)">Repair it</div>
                    <div class="option" onclick="selectOption(this, false)">Upgrade its components</div>
                </div>
                <div class="feedback" id="mc-feedback-3" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 4: A "forecast" is typically about:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Past events</div>
                    <div class="option" onclick="selectOption(this, true)">Future events</div>
                    <div class="option" onclick="selectOption(this, false)">Current situations</div>
                    <div class="option" onclick="selectOption(this, false)">Historical facts</div>
                </div>
                <div class="feedback" id="mc-feedback-4" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 5: If something is "odd", it is:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Completely normal</div>
                    <div class="option" onclick="selectOption(this, true)">Strange or unusual</div>
                    <div class="option" onclick="selectOption(this, false)">Numerically even</div>
                    <div class="option" onclick="selectOption(this, false)">Predictable</div>
                </div>
                <div class="feedback" id="mc-feedback-5" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 6: To "yield" means to:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Resist or oppose</div>
                    <div class="option" onclick="selectOption(this, true)">Produce or provide</div>
                    <div class="option" onclick="selectOption(this, false)">Destroy or eliminate</div>
                    <div class="option" onclick="selectOption(this, false)">Ignore or overlook</div>
                </div>
                <div class="feedback" id="mc-feedback-6" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 7: "Almost" means:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Completely and entirely</div>
                    <div class="option" onclick="selectOption(this, true)">Very nearly but not completely</div>
                    <div class="option" onclick="selectOption(this, false)">Not at all</div>
                    <div class="option" onclick="selectOption(this, false)">Exactly and precisely</div>
                </div>
                <div class="feedback" id="mc-feedback-7" style="display: none;"></div>
            </div>

            <div class="question">
                <h3>Question 8: If you are "wonderin" about something, you are:</h3>
                <div class="options">
                    <div class="option" onclick="selectOption(this, false)">Certain about it</div>
                    <div class="option" onclick="selectOption(this, true)">Thinking about it with curiosity</div>
                    <div class="option" onclick="selectOption(this, false)">Ignoring it completely</div>
                    <div class="option" onclick="selectOption(this, false)">Forgetting about it</div>
                </div>
                <div class="feedback" id="mc-feedback-8" style="display: none;"></div>
            </div>
        </div>
    </div>

    <script>
        let score = 0;
        let selectedWord = null;
        let selectedMeaning = null;
        let matchedPairs = [];
        
        // Define the questions for the fill-in-the-blanks section using the requested words
        const questions = [
            {
                id: 1,
                text: "After working on the computer all day, I decided to <input type='text' class='fill-blank' data-answer='un-plug' placeholder='answer'> and read a book instead.",
                answer: "un-plug"
            },
            {
                id: 2,
                text: "The weather <input type='text' class='fill-blank' data-answer='forecast' placeholder='answer'> predicts rain for the weekend, so we might need to cancel our picnic plans.",
                answer: "forecast"
            },
            {
                id: 3,
                text: "It's rather <input type='text' class='fill-blank' data-answer='odd' placeholder='answer'> that Sarah hasn't arrived yet; she's usually very punctual.",
                answer: "odd"
            },
            {
                id: 4,
                text: "The apple orchard is expected to <input type='text' class='fill-blank' data-answer='yield' placeholder='answer'> a bountiful harvest this year despite the early frost.",
                answer: "yield"
            },
            {
                id: 5,
                text: "We're <input type='text' class='fill-blank' data-answer='almost' placeholder='answer'> finished with the project; just one more section to complete.",
                answer: "almost"
            },
            {
                id: 6,
                text: "The contrast between the ancient temple and the modern skyscraper was truly <input type='text' class='fill-blank' data-answer='striking' placeholder='answer'>.",
                answer: "striking"
            },
            {
                id: 7,
                text: "The scientific discovery was <input type='text' class='fill-blank' data-answer='unprecedented' placeholder='answer'>; nothing like it had ever been documented before.",
                answer: "unprecedented"
            },
            {
                id: 8,
                text: "I was <input type='text' class='fill-blank' data-answer='wondering' placeholder='answer'> if you'd like to join us for dinner this weekend.",
                answer: "wondering"
            },
            {
                id: 9,
                text: "During the power outage, we had to <input type='text' class='fill-blank' data-answer='un-plug' placeholder='answer'> all our appliances to protect them from potential surges.",
                answer: "un-plug"
            },
            {
                id: 10,
                text: "Economic analysts <input type='text' class='fill-blank' data-answer='forecast' placeholder='answer'> a period of growth following the recent policy changes.",
                answer: "forecast"
            },
            {
                id: 11,
                text: "He has an <input type='text' class='fill-blank' data-answer='odd' placeholder='answer'> habit of arranging his books by color rather than by author or title.",
                answer: "odd"
            },
            {
                id: 12,
                text: "The negotiations finally began to <input type='text' class='fill-blank' data-answer='yield' placeholder='answer'> positive results after weeks of discussion.",
                answer: "yield"
            },
            {
                id: 13,
                text: "I've <input type='text' class='fill-blank' data-answer='almost' placeholder='answer'> reached my daily step goal; just 500 more steps to go.",
                answer: "almost"
            },
            {
                id: 14,
                text: "Her <input type='text' class='fill-blank' data-answer='striking' placeholder='answer'> resemblance to her grandmother was the first thing everyone noticed.",
                answer: "striking"
            },
            {
                id: 15,
                text: "The pandemic caused <input type='text' class='fill-blank' data-answer='unprecedented' placeholder='answer'> disruptions to global supply chains.",
                answer: "unprecedented"
            },
            {
                id: 16,
                text: "I've been <input type='text' class='fill-blank' data-answer='wonderin' placeholder='answer'> how you've been since you moved to the new city.",
                answer: "wondering"
            }
        ];
        
        // Define the word-meaning pairs for the matching exercise
        const wordMeanings = [
            { word: "un-plug", meaning: "To disconnect an electrical device" },
            { word: "forecast", meaning: "A prediction of future events" },
            { word: "odd", meaning: "Strange or unusual" },
            { word: "yield", meaning: "To produce or provide" },
            { word: "almost", meaning: "Very nearly but not completely" },
            { word: "striking", meaning: "Very impressive or noticeable" },
            { word: "unprecedented", meaning: "Never done or known before" },
            { word: "wondering", meaning: "Thinking about something with curiosity" }
        ];
        
        // Function to shuffle the questions array
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
            return array;
        }
        
        // Function to render the shuffled questions
        function renderQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = '';
            
            const shuffledQuestions = shuffleArray([...questions]);
            
            shuffledQuestions.forEach((question, index) => {
                const questionDiv = document.createElement('div');
                questionDiv.className = 'question';
                questionDiv.innerHTML = `
                    <h3>Question ${index + 1}:</h3>
                    <p>${question.text}</p>
                    <div class="feedback" id="feedback-${question.id}" style="display: none;"></div>
                `;
                container.appendChild(questionDiv);
            });
        }
        
        // Function to render the shuffled meanings for the matching exercise
        function renderMeanings() {
            const meaningsColumn = document.getElementById('meanings-column');
            meaningsColumn.innerHTML = '<h4>Meanings</h4>';
            
            // Shuffle the meanings
            const shuffledMeanings = shuffleArray([...wordMeanings]);
            
            // Add the shuffled meanings to the column
            shuffledMeanings.forEach(item => {
                const meaningDiv = document.createElement('div');
                meaningDiv.className = 'match-item';
                meaningDiv.dataset.meaning = item.word;
                meaningDiv.textContent = item.meaning;
                meaningDiv.onclick = function() { selectMatch(this); };
                meaningsColumn.appendChild(meaningDiv);
            });
        }
        
        // Call the function to render questions and meanings when the page loads
        window.onload = function() {
            renderQuestions();
            renderMeanings();
        };

        function showSection(sectionId) {
            // Hide all sections
            document.querySelectorAll('.game-section').forEach(section => {
                section.classList.remove('active');
            });
            
            // Remove active class from all buttons
            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected section
            document.getElementById(sectionId).classList.add('active');
            
            // Add active class to clicked button
            event.target.classList.add('active');
            
            // If it's the fill-gaps section, reshuffle the questions
            if (sectionId === 'fill-gaps') {
                renderQuestions();
            }
            
            // If it's the matching section, reshuffle the meanings
            if (sectionId === 'matching') {
                renderMeanings();
                // Reset matching state
                selectedWord = null;
                selectedMeaning = null;
                matchedPairs = [];
                document.querySelectorAll('.match-item').forEach(item => {
                    item.classList.remove('selected', 'matched');
                });
                document.getElementById('matching-feedback').style.display = 'none';
            }
        }

        function checkFillBlanks() {
            const blanks = document.querySelectorAll('.fill-blank');
            let correctCount = 0;
            
            blanks.forEach((blank) => {
                const userAnswer = blank.value.toLowerCase().trim();
                const correctAnswer = blank.dataset.answer.toLowerCase();
                
                if (userAnswer === correctAnswer) {
                    blank.classList.remove('incorrect');
                    blank.classList.add('correct');
                    correctCount++;
                } else {
                    blank.classList.remove('correct');
                    blank.classList.add('incorrect');
                }
            });
            
            // Show feedback for each question
            questions.forEach(question => {
                const feedback = document.getElementById(`feedback-${question.id}`);
                const blank = document.querySelector(`.fill-blank[data-answer="${question.answer}"]`);
                
                if (blank && blank.classList.contains('correct')) {
                    feedback.textContent = '✅ Correct!';
                    feedback.className = 'feedback correct';
                } else {
                    feedback.textContent = `❌ Incorrect. The correct answer is: "${question.answer}"`;
                    feedback.className = 'feedback incorrect';
                }
                feedback.style.display = 'block';
            });
            
            updateScore();
        }

        function selectMatch(element) {
            if (element.classList.contains('matched')) return;
            
            if (element.dataset.word) {
                // Word selected
                if (selectedWord) selectedWord.classList.remove('selected');
                selectedWord = element;
                element.classList.add('selected');
            } else {
                // Meaning selected
                if (selectedMeaning) selectedMeaning.classList.remove('selected');
                selectedMeaning = element;
                element.classList.add('selected');
            }
            
            // Check if we have both word and meaning selected
            if (selectedWord && selectedMeaning) {
                checkMatch();
            }
        }

        function checkMatch() {
            const feedback = document.getElementById('matching-feedback');
            
            if (selectedWord.dataset.word === selectedMeaning.dataset.meaning) {
                // Correct match
                selectedWord.classList.remove('selected');
                selectedWord.classList.add('matched');
                selectedMeaning.classList.remove('selected');
                selectedMeaning.classList.add('matched');
                
                matchedPairs.push(selectedWord.dataset.word);
                
                feedback.textContent = '✅ Correct match!';
                feedback.className = 'feedback correct';
                
                // Check if all matches are complete
                if (matchedPairs.length === 8) {
                    feedback.textContent = '🎉 Congratulations! You matched all terms correctly!';
                }
            } else {
                // Incorrect match
                feedback.textContent = '❌ Incorrect match. Try again.';
                feedback.className = 'feedback incorrect';
            }
            
            feedback.style.display = 'block';
            selectedWord = null;
            selectedMeaning = null;
            
            updateScore();
        }

        function selectOption(element, isCorrect) {
            // Remove selection from all options in this question
            const options = element.parentElement.querySelectorAll('.option');
            options.forEach(opt => {
                opt.classList.remove('selected');
            });
            
            // Mark the selected option
            element.classList.add('selected');
            
            const questionNumber = element.closest('.question').querySelector('h3').textContent.match(/\d+/)[0];
            const feedback = document.getElementById(`mc-feedback-${questionNumber}`);
            
            if (isCorrect) {
                element.classList.add('correct');
                feedback.textContent = '✅ Correct!';
                feedback.className = 'feedback correct';
            } else {
                element.classList.add('incorrect');
                feedback.textContent = '❌ Incorrect. Try again.';
                feedback.className = 'feedback incorrect';
                
                // Show the correct answer
                options.forEach(opt => {
                    if (opt.onclick.toString().includes('true')) {
                        opt.classList.add('correct');
                    }
                });
            }
            
            feedback.style.display = 'block';
            updateScore();
        }

        function updateScore() {
            // Calculate score for fill-in-the-blank
            let fillScore = 0;
            document.querySelectorAll('.fill-blank.correct').forEach(blank => {
                if (!blank.classList.contains('counted')) {
                    fillScore++;
                    blank.classList.add('counted');
                }
            });
            
            // Calculate score for matching (each pair is 1 point)
            const matchScore = matchedPairs.length;
            
            // Calculate score for multiple choice (each correct is 1 point)
            let mcScore = 0;
            document.querySelectorAll('.option.correct.selected').forEach(opt => {
                mcScore++;
            });
            
            // Total score
            score = fillScore + matchScore + mcScore;
            document.getElementById('score').textContent = score;
        }
    </script>
</body>
</html>
