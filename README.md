<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MY LET REVIEWER AID</title>
    <style>
        :root {
            --bg: #020617; --card: #1e293b; --text: #f8fafc;
            --accent: #38bdf8; --correct: #22c55e; --rational: #f472b6; --timer: #ef4444;
        }
        body {
            font-family: 'Arial Black', sans-serif;
            background-color: var(--bg); color: var(--text);
            margin: 0; display: flex; justify-content: center; align-items: center; height: 100vh; overflow: hidden;
        }
        #app { width: 95%; max-width: 1400px; text-align: center; }
        
        /* Timer Bar */
        .timer-container { width: 100%; background: #334155; height: 15px; border-radius: 10px; margin-bottom: 10px; overflow: hidden; }
        #timer-bar { width: 100%; height: 100%; background: #22c55e; transition: width 1s linear; }
        #timer-text { font-size: 2.5rem; color: var(--timer); font-weight: bold; margin-bottom: 5px; }

        .category { font-size: 2.2rem; color: var(--accent); text-transform: uppercase; margin-bottom: 5px; }

        .q-box {
            background: var(--card); padding: 40px; border-radius: 30px;
            box-shadow: 0 25px 50px rgba(0,0,0,0.5); min-height: 520px;
            display: flex; flex-direction: column; justify-content: space-between;
        }
        /* Extra large text for back of room visibility */
        .q-text { font-size: 3.5rem; font-weight: 900; line-height: 1.1; margin-bottom: 30px; color: #fff; }
        
        .options { display: grid; grid-template-columns: 1fr 1fr; gap: 20px; }
        .opt {
            font-size: 2.2rem; padding: 25px; background: #334155;
            border-radius: 15px; text-align: left; border: 4px solid transparent;
        }
        .correct { background: var(--correct) !important; color: white; border-color: white; }
        
        .rat-box {
            display: none; background: #000; color: var(--rational);
            font-size: 1.8rem; padding: 20px; border-radius: 15px;
            margin-top: 25px; border-left: 12px solid var(--rational); text-align: left;
        }

        .nav { margin-top: 25px; display: flex; justify-content: center; gap: 15px; }
        button {
            font-size: 1.8rem; padding: 15px 35px; border-radius: 12px;
            border: none; cursor: pointer; font-weight: bold; transition: 0.2s;
        }
        .btn-nav { background: var(--accent); color: #000; }
        .btn-reveal { background: #eab308; color: #000; }
        .btn-rat { background: var(--rational); color: white; }
    </style>
</head>
<body>

<div id="app">
    <div id="timer-text">30s</div>
    <div class="timer-container"><div id="timer-bar"></div></div>
    
    <div class="category" id="cat">LET REVIEWER</div>
    <div id="count" style="font-size: 1.2rem; opacity: 0.6; margin-bottom: 10px;">Question 1</div>
    
    <div class="q-box">
        <div>
            <div class="q-text" id="q-text">Loading...</div>
            <div class="options" id="options"></div>
        </div>
        <div class="rat-box" id="rat-box"><strong>RATIONALIZATION:</strong> <span id="rat-text"></span></div>
    </div>

    <div class="nav">
        <button class="btn-nav" onclick="move(-1)">PREV</button>
        <button class="btn-reveal" onclick="reveal()">ANSWER</button>
        <button class="btn-rat" onclick="showRat()">EXPLAIN</button>
        <button class="btn-nav" onclick="move(1)">NEXT</button>
    </div>
</div>

<script>
    const quizData = [ 
// --- GENERAL EDUCATION ---
{ 
    cat: "Gen Ed", 
    q: "Who is the 'Father of the Philippine Constitution'?",
    a: ["A. Jose Laurel", "B. Claro M. Recto", "C. Sergio Osmena", "D. Manuel Quezon"], 
    c: 1, 
    rat: "Claro M. Recto presided over the 1934 Constitutional Convention and is the primary author of the 1935 Constitution."
        },
{ 
    cat: "Gen Ed", 
    q: "Which Filipino president is known for the 'Filipino First Policy'?", 
    a: ["A. Diosdado Macapagal", "B. Carlos P. Garcia", "C. Ramon Magsaysay", "D. Ferdinand Marcos"], 
    c: 1, 
    rat: "Carlos P. Garcia's policy prioritized Filipino-owned businesses over foreign investors to boost the local economy."
},
{ 
    cat: "Gen Ed", 
    q: "If a circle has a radius of 7cm, what is its approximate area? (Use π = 3.14)", 
    a: ["A. 43.96 cm²", "B. 153.86 cm²", "C. 49 cm²", "D. 14.14 cm²"], 
    c: 1, 
    rat: "Area = πr². So, 3.14 × 7 × 7 = 153.86 cm²."
},
{ 
    cat: "Gen Ed", 
    q: "Which of the following is an example of a physical change?", 
    a: ["A. Burning wood", "B. Rusting of iron", "C. Melting ice", "D. Souring of milk"], 
    c: 2, 
    rat: "Melting ice is a physical change because only the state of matter changes, not the chemical identity."
},
{ 
    cat: "Gen Ed", 
    q: "Identify the figure of speech: 'The stars winked at me from the night sky.'", 
    a: ["A. Simile", "B. Metaphor", "C. Personification", "D. Oxymoron"], 
    c: 2, 
    rat: "Personification gives human qualities (winking) to non-human objects (stars)."
},
{ 
    cat: "Gen Ed", 
    q: "What is the process by which plants make their own food?", 
    a: ["A. Respiration", "B. Photosynthesis", "C. Fermentation", "D. Digestion"], 
    c: 1, 
    rat: "Photosynthesis uses sunlight, water, and carbon dioxide to create glucose and oxygen."
},
{ 
    cat: "Gen Ed", 
    q: "Ano ang kasingkahulugan ng salitang 'MARIKIT'?", 
    a: ["A. Mabait", "B. Maganda", "C. Masaya", "D. Matapang"], 
    c: 1, 
    rat: "Ang 'marikit' ay tumutukoy sa katangiang biswal na kaaya-aya o maganda."
},
{ 
    cat: "Gen Ed", 
    q: "Which state of matter has particles that are closest together?", 
    a: ["A. Gas", "B. Liquid", "C. Solid", "D. Plasma"], 
    c: 2, 
    rat: "In a solid, particles are packed tightly, giving it a definite shape and volume."
},
{ 
    cat: "Gen Ed", 
    q: "The sum of the angles in any triangle is always:", 
    a: ["A. 90 degrees", "B. 180 degrees", "C. 270 degrees", "D. 360 degrees"], 
    c: 1, 
    rat: "The three interior angles of a triangle always add up to exactly 180°."
},
{ 
    cat: "Gen Ed", 
    q: "Who is the 'Great Plebeian' and founder of the Katipunan?", 
    a: ["A. Antonio Luna", "B. Marcelo del Pilar", "C. Andres Bonifacio", "D. Juan Luna"], 
    c: 2, 
    rat: "Andres Bonifacio is the 'Father of the Philippine Revolution' and leader of the KKK."
},
{ 
    cat: "Gen Ed", 
    q: "Which element has the chemical symbol 'Au'?", 
    a: ["A. Silver", "B. Gold", "C. Aluminum", "D. Copper"], 
    c: 1, 
    rat: "Au comes from the Latin word 'Aurum,' which means gold."
},
{ 
    cat: "Gen Ed", 
    q: "I _________ a teacher for ten years now.", 
    a: ["A. am", "B. was", "C. have been", "D. had been"], 
    c: 2, 
    rat: "The present perfect 'have been' is used for actions starting in the past and continuing now."
},
{ 
    cat: "Gen Ed", 
    q: "Alin sa mga sumusunod ang isang 'PANG-URI'?", 
    a: ["A. Tumakbo", "B. Mabilis", "C. Kanina", "D. Siya"], 
    c: 1, 
    rat: "Ang 'Mabilis' ay naglalarawan (pang-uri). 'Tumakbo' ay pandiwa."
},
{ 
    cat: "Gen Ed", 
    q: "If a shirt costs 500 pesos and has a 20% discount, how much will you pay?", 
    a: ["A. 400 pesos", "B. 450 pesos", "C. 480 pesos", "D. 100 pesos"], 
    c: 0, 
    rat: "20% of 500 is 100. 500 minus 100 equals 400 pesos."
},
{ 
    cat: "Gen Ed", 
    q: "What is the largest desert in the world?", 
    a: ["A. Gobi", "B. Sahara", "C. Arabian", "D. Antarctic"], 
    c: 3, 
    rat: "Antarctica is classified as a polar desert and is the largest overall."
},
{ 
    cat: "Gen Ed", 
    q: "Who is the 'Father of Local Government Code' in the Philippines?", 
    a: ["A. Aquilino Pimentel Jr.", "B. Jovito Salonga", "C. Miriam Santiago", "D. Juan Ponce Enrile"], 
    c: 0, 
    rat: "Senator Aquilino Pimentel Jr. authored RA 7160, the Local Government Code of 1991."
},
{ 
    cat: "Gen Ed", 
    q: "What is the result of 0.5 x 0.5?", 
    a: ["A. 2.5", "B. 0.25", "C. 0.05", "D. 0.025"], 
    c: 1, 
    rat: "5x5=25, move two decimal places: 0.25."
},
{ 
    cat: "Gen Ed", 
    q: "Which planet is known as the 'Red Planet'?", 
    a: ["A. Venus", "B. Mars", "C. Jupiter", "D. Saturn"], 
    c: 1, 
    rat: "Mars appears red because of the iron oxide (rust) on its surface."
},
{ 
    cat: "Gen Ed", 
    q: "Ano ang kasalungat ng 'MAAMO'?", 
    a: ["A. Mabait", "B. Mailap", "C. Matulungin", "D. Masunurin"], 
    c: 1, 
    rat: "Ang mailap ay kasalungat ng maamo (halimbawa: isang hayop na mahirap hulihin)."
},
{ 
    cat: "Gen Ed", 
    q: "The line 'The proper study of mankind is man' was written by:", 
    a: ["A. Alexander Pope", "B. William Shakespeare", "C. Geoffrey Chaucer", "D. John Milton"], 
    c: 0, 
    rat: "This is a famous quote from Alexander Pope's 'An Essay on Man'."
},
{ 
    cat: "Gen Ed", 
    q: "What is the process of water changing into gas?", 
    a: ["A. Condensation", "B. Sublimation", "C. Evaporation", "D. Freezing"], 
    c: 2, 
    rat: "Evaporation is the phase transition from liquid to gas."
},
{ 
    cat: "Gen Ed", 
    q: "How many sides does a nonagon have?", 
    a: ["A. 7", "B. 8", "C. 9", "D. 10"], 
    c: 2, 
    rat: "A nonagon is a polygon with 9 sides."
},
{ 
    cat: "Gen Ed", 
    q: "Who was the 'Brain of the Katipunan'?", 
    a: ["A. Apolinario Mabini", "B. Emilio Jacinto", "C. Andres Bonifacio", "D. Antonio Luna"], 
    c: 1, 
    rat: "Emilio Jacinto wrote the Kartilya ng Katipunan and was Bonifacio's advisor."
},
{ 
    cat: "Gen Ed", 
    q: "What is the pH level of pure water?", 
    a: ["A. 1", "B. 5", "C. 7", "D. 14"], 
    c: 2, 
    rat: "A pH of 7 is considered neutral."
},
{ 
    cat: "Gen Ed", 
    q: "Which of the following is an example of a Homonym?", 
    a: ["A. Big/Small", "B. Happy/Glad", "C. Bear (animal)/Bear (carry)", "D. Fast/Slow"], 
    c: 2, 
    rat: "Homonyms are words that have the same spelling/sound but different meanings."
},
{ 
    cat: "Gen Ed", 
    q: "What is the value of square root of 144?", 
    a: ["A. 10", "B. 12", "C. 14", "D. 16"], 
    c: 1, 
    rat: "12 times 12 equals 144."
},
{ 
    cat: "Gen Ed", 
    q: "Who is the first Filipino saint?", 
    a: ["A. Lorenzo Ruiz", "B. Pedro Calungsod", "C. Pope John Paul II", "D. Padre Pio"], 
    c: 0, 
    rat: "San Lorenzo Ruiz was martyred in Japan and canonized in 1987."
},
{ 
    cat: "Gen Ed", 
    q: "Which law of motion states every action has an equal reaction?", 
    a: ["A. 1st Law", "B. 2nd Law", "C. 3rd Law", "D. Gravity"], 
    c: 2, 
    rat: "Newton's 3rd Law (Interaction) describes force pairs."
},
{ 
    cat: "Gen Ed", 
    q: "Ano ang kahulugan ng idyomang 'NAGBIBILANG NG POSTE'?", 
    a: ["A. Inhinyero", "B. Walang trabaho", "C. Masipag", "D. Mayaman"], 
    c: 1, 
    rat: "Ito ay paraan ng pagsasabi na ang isang tao ay walang trabaho."
},
{ 
    cat: "Gen Ed", 
    q: "What is the average of 10, 20, and 30?", 
    a: ["A. 15", "B. 20", "C. 25", "D. 60"], 
    c: 1, 
    rat: "Sum (60) divided by count (3) equals 20."
},
{ 
    cat: "Gen Ed", 
    q: "Who discovered Penicillin?", 
    a: ["A. Louis Pasteur", "B. Marie Curie", "C. Alexander Fleming", "D. Gregor Mendel"], 
    c: 2, 
    rat: "Alexander Fleming discovered the first antibiotic in 1928."
},
{ 
    cat: "Gen Ed", 
    q: "The first Asian to win the Nobel Prize for Literature was:", 
    a: ["A. Rabindranath Tagore", "B. Yasunari Kawabata", "C. Jose Rizal", "D. Confucius"], 
    c: 0, 
    rat: "Tagore, an Indian poet, won the Nobel Prize in 1913."
},
{ 
    cat: "Gen Ed", 
    q: "What is the main function of red blood cells?", 
    a: ["A. Fight infection", "B. Clot blood", "C. Carry oxygen", "D. Produce hormones"], 
    c: 2, 
    rat: "Red blood cells use hemoglobin to transport oxygen."
},
{ 
    cat: "Gen Ed", 
    q: "Alin ang 'PANG-ABAY' sa: 'Siya ay mabilis na tumakbo'?", 
    a: ["A. Siya", "B. Ay", "C. Mabilis", "D. Tumakbo"], 
    c: 2, 
    rat: "Ang 'Mabilis' ay naglalarawan kung PAANO tumakbo (pandiwa)."
},
{ 
    cat: "Gen Ed", 
    q: "Which layer of the atmosphere contains the Ozone Layer?", 
    a: ["A. Troposphere", "B. Stratosphere", "C. Mesosphere", "D. Thermosphere"], 
    c: 1, 
    rat: "The Stratosphere protects Earth from harmful UV rays."
},
{ 
    cat: "Gen Ed", 
    q: "What is 25% of 200?", 
    a: ["A. 25", "B. 40", "C. 50", "D. 100"], 
    c: 2, 
    rat: "25% is one-fourth. 200 divided by 4 is 50."
},
{ 
    cat: "Gen Ed", 
    q: "Who was the first woman president of the Philippines?", 
    a: ["A. Gloria Arroyo", "B. Corazon Aquino", "C. Imelda Marcos", "D. Miriam Santiago"], 
    c: 1, 
    rat: "Corazon Aquino took office after the 1986 EDSA Revolution."
},
{ 
    cat: "Gen Ed", 
    q: "What is the study of fossils?", 
    a: ["A. Archaeology", "B. Paleontology", "C. Biology", "D. Geology"], 
    c: 1, 
    rat: "Paleontology is the scientific study of prehistoric life."
},
{ 
    cat: "Gen Ed", 
    q: "Identify the tense: 'I will have finished my work by 5pm.'", 
    a: ["A. Simple Future", "B. Future Continuous", "C. Future Perfect", "D. Present Perfect"], 
    c: 2, 
    rat: "The 'will have + past participle' denotes Future Perfect."
},
{ 
    cat: "Gen Ed", 
    q: "The power of the state to take private property for public use is:", 
    a: ["A. Taxation", "B. Police Power", "C. Eminent Domain", "D. Escheat"], 
    c: 2, 
    rat: "Eminent Domain allows the government to acquire land for public projects."
},
{ 
    cat: "Gen Ed", 
    q: "What is the binary representation of decimal 5?", 
    a: ["A. 100", "B. 101", "C. 110", "D. 111"], 
    c: 1, 
    rat: "Binary 101 translates to (1*4) + (0*2) + (1*1) = 5."
},
{ 
    cat: "Gen Ed", 
    q: "Who wrote 'The Iliad' and 'The Odyssey'?", 
    a: ["A. Sophocles", "B. Virgil", "C. Homer", "D. Dante"], 
    c: 2, 
    rat: "Homer is the legendary blind poet of ancient Greece."
},
{ 
    cat: "Gen Ed", 
    q: "Which organ produces insulin?", 
    a: ["A. Liver", "B. Pancreas", "C. Stomach", "D. Kidney"], 
    c: 1, 
    rat: "The Pancreas regulates blood sugar."
},
{ 
    cat: "Gen Ed", 
    q: "Ano ang tawag sa tulang may labing-apat na taludtod?", 
    a: ["A. Soneto", "B. Oda", "C. Elehiya", "D. Haiku"], 
    c: 0, 
    rat: "Ang Soneto (Sonnet) ay may eksaktong 14 na linya."
},
{ 
    cat: "Gen Ed", 
    q: "Solve for x: x/4 + 2 = 5.", 
    a: ["A. 4", "B. 8", "C. 12", "D. 16"], 
    c: 2, 
    rat: "Subtract 2 (x/4 = 3), then multiply by 4 (x = 12)."
},
{ 
    cat: "Gen Ed", 
    q: "Who is the proponent of the Theory of Evolution?", 
    a: ["A. Gregor Mendel", "B. Charles Darwin", "C. Lamarck", "D. Malthus"], 
    c: 1, 
    rat: "Darwin introduced natural selection in 'On the Origin of Species'."
},
{ 
    cat: "Gen Ed", 
    q: "The 'Cry of Pugad Lawin' marked the start of:", 
    a: ["A. Spanish Rule", "B. Phil. Revolution", "C. American Rule", "D. WWII"], 
    c: 1, 
    rat: "It was the event where Katipuneros tore their cedulas."
},
{ 
    cat: "Gen Ed", 
    q: "What is the chemical formula for table salt?", 
    a: ["A. H2O", "B. NaCl", "C. CO2", "D. HCl"], 
    c: 1, 
    rat: "NaCl stands for Sodium Chloride."
},
{ 
    cat: "Gen Ed", 
    q: "Choose the correct pronoun: 'Between you and ________, I don't trust him.'", 
    a: ["A. I", "B. Me", "C. We", "D. They"], 
    c: 1, 
    rat: "The objective pronoun 'me' must be used after the preposition 'between'."
},
{ 
    cat: "Gen Ed", 
    q: "What is the sum of 1/2 and 1/4?", 
    a: ["A. 2/6", "B. 1/8", "C. 3/4", "D. 2/4"], 
    c: 2, 
    rat: "1/2 (or 2/4) + 1/4 = 3/4."
}
// --- PROFESSIONAL EDUCATION --- //
    cat: "Prof Ed", 
 {
    q: "A teacher rewards students with 'stars' for good behavior. This is:", 
    a: ["A. Classical Conditioning", "B. Operant Conditioning", "C. Social Learning", "D. Insight Learning"], 
    c: 1, 
    rat: "B.F. Skinner's Operant Conditioning uses reinforcement (rewards like stars) to increase desired behaviors."
},
{ 
    cat: "Prof Ed", 
    q: "Which law is known as the 'Magna Carta for Public School Teachers'?", 
    a: ["A. RA 7836", "B. RA 4670", "C. RA 9155", "D. RA 10533"], 
    c: 1, 
    rat: "RA 4670 provides guidelines for employment, tenure, and welfare of public school teachers."
},
{ 
    cat: "Prof Ed", 
    q: "Who is the proponent of the 'Multiple Intelligences' theory?", 
    a: ["A. Jean Piaget", "B. Howard Gardner", "C. Lev Vygotsky", "D. Erik Erikson"], 
    c: 1, 
    rat: "Gardner proposed that people have different ways of learning (Musical, Kinesthetic, etc.)."
},
{ 
    cat: "Prof Ed", 
    q: "Which assessment is done DURING instruction to monitor student progress?", 
    a: ["A. Summative", "B. Diagnostic", "C. Formative", "D. Placement"], 
    c: 2, 
    rat: "Formative assessment provides feedback during the lesson to help improve learning."
},
{ 
    cat: "Prof Ed", 
    q: "In Maslow's Hierarchy, what is the highest level of human needs?", 
    a: ["A. Safety", "B. Esteem", "C. Self-Actualization", "D. Belonging"], 
    c: 2, 
    rat: "Self-Actualization is the pinnacle, representing the realization of one's full potential."
},
{ 
    cat: "Prof Ed", 
    q: "Which philosophy focuses on the '3 Rs' (Reading, Writing, Arithmetic)?", 
    a: ["A. Progressivism", "B. Essentialism", "C. Perennialism", "D. Existentialism"], 
    c: 1, 
    rat: "Essentialism emphasizes core knowledge and basic skills that are 'essential' for all."
},
{ 
    cat: "Prof Ed", 
    q: "The K-12 curriculum was institutionalized by which law?", 
    a: ["A. RA 9155", "B. RA 10533", "C. RA 7722", "D. RA 4670"], 
    c: 1, 
    rat: "RA 10533 is the Enhanced Basic Education Act of 2013."
},
{ 
    cat: "Prof Ed", 
    q: "What is the 'Brain' of the teaching-learning process?", 
    a: ["A. The Student", "B. The Teacher", "C. The Curriculum", "D. The School"], 
    c: 1, 
    rat: "The teacher facilitates, plans, and manages the entire learning environment."
},
{ 
    cat: "Prof Ed", 
    q: "Which pillar of learning is about working together and respecting diversity?", 
    a: ["A. Learning to Know", "B. Learning to Do", "C. Learning to Live Together", "D. Learning to Be"], 
    c: 2, 
    rat: "This pillar focuses on social harmony, understanding others, and conflict resolution."
},
{ 
    cat: "Prof Ed", 
    q: "Who is known as the 'Father of Behaviorism'?", 
    a: ["A. John Watson", "B. B.F. Skinner", "C. Ivan Pavlov", "D. Edward Thorndike"], 
    c: 0, 
    rat: "John B. Watson founded Behaviorism, focusing on observable and measurable behaviors."
},
{ 
    cat: "Prof Ed", 
    q: "What does 'Tabula Rasa' mean according to John Locke?", 
    a: ["A. Born with knowledge", "B. Empty vessel", "C. Blank slate", "D. Gifted mind"], 
    c: 2, 
    rat: "It implies that the mind is born empty and knowledge comes from experience."
},
{ 
    cat: "Prof Ed", 
    q: "Which assessment determines strengths and weaknesses BEFORE instruction?", 
    a: ["A. Summative", "B. Diagnostic", "C. Formative", "D. Criterion-referenced"], 
    c: 1, 
    rat: "Diagnostic tests identify specific learning gaps before the lesson starts."
},
{ 
    cat: "Prof Ed", 
    q: "According to Erikson, what is the crisis of adolescence?", 
    a: ["A. Trust vs Mistrust", "B. Identity vs Role Confusion", "C. Initiative vs Guilt", "D. Intimacy vs Isolation"], 
    c: 1, 
    rat: "Teens seek a sense of self and personal identity through exploration."
},
{ 
    cat: "Prof Ed", 
    q: "Which philosophy believes that 'Learning by Doing' is best?", 
    a: ["A. Progressivism", "B. Perennialism", "C. Realism", "D. Idealism"], 
    c: 0, 
    rat: "John Dewey’s Progressivism emphasizes experiential learning and problem solving."
},
{ 
    cat: "Prof Ed", 
    q: "What is the study of how people learn?", 
    a: ["A. Sociology", "B. Pedagogy", "C. Educational Psychology", "D. Philosophy"], 
    c: 2, 
    rat: "It explores the cognitive and emotional processes behind knowledge acquisition."
},
{ 
    cat: "Prof Ed", 
    q: "Which Piagetian stage is characterized by logical thinking about concrete objects?", 
    a: ["A. Sensorimotor", "B. Pre-operational", "C. Concrete Operational", "D. Formal Operational"], 
    c: 2, 
    rat: "This stage (7-11 yrs) allows children to understand conservation and logic."
},
{ 
    cat: "Prof Ed", 
    q: "What is the required CPD units for LET renewal currently?", 
    a: ["A. 15 units", "B. 30 units", "C. 45 units", "D. 60 units"], 
    c: 0, 
    rat: "Under current PRC guidelines for teachers, 15 units are required every 3 years."
},
{ 
    cat: "Prof Ed", 
    q: "Which type of curriculum is unintended and unwritten (values, norms)?", 
    a: ["A. Written", "B. Taught", "C. Hidden", "D. Null"], 
    c: 2, 
    rat: "The Hidden Curriculum refers to social behaviors learned through the school culture."
},
{ 
    cat: "Prof Ed", 
    q: "Who proposed the 'Social Learning Theory' (observation and imitation)?", 
    a: ["A. Bandura", "B. Piaget", "C. Vygotsky", "D. Skinner"], 
    c: 0, 
    rat: "Albert Bandura's theory emphasizes modeling and observational learning."
},
{ 
    cat: "Prof Ed", 
    q: "What does the 'Zone of Proximal Development' (ZPD) represent?", 
    a: ["A. Learning alone", "B. Gap between assisted and independent task", "C. Memory limit", "D. IQ score"], 
    c: 1, 
    rat: "Vygotsky's ZPD is the range where a child can learn with guidance from a 'more knowledgeable other'."
},
{ 
    cat: "Prof Ed", 
    q: "In a 'Positively Skewed' distribution, where are most scores?", 
    a: ["A. High end", "B. Low end", "C. Middle", "D. Evenly spread"], 
    c: 1, 
    rat: "Most scores are low (the tail goes to the right), meaning the test was difficult."
},
{ 
    cat: "Prof Ed", 
    q: "What is the primary purpose of a Table of Specifications (TOS)?", 
    a: ["A. Record grades", "B. Ensure content validity", "C. Discipline students", "D. Schedule classes"], 
    c: 1, 
    rat: "A TOS ensures the test covers all objectives taught in the correct proportion."
},
{ 
    cat: "Prof Ed", 
    q: "Which philosophy believes truth is universal and found in 'Great Books'?", 
    a: ["A. Essentialism", "B. Perennialism", "C. Existentialism", "D. Progressivism"], 
    c: 1, 
    rat: "Perennialists focus on enduring truths and classic literature."
},
{ 
    cat: "Prof Ed", 
    q: "In classroom management, 'Roll-call' is an example of a:", 
    a: ["A. Discipline", "B. Strategy", "C. Routine", "D. Punishment"], 
    c: 2, 
    rat: "Routines are established procedures that save time and minimize confusion."
},
{ 
    cat: "Prof Ed", 
    q: "Which pillar of learning focuses on developing your own personality?", 
    a: ["A. Learning to Know", "B. Learning to Do", "C. Learning to Live Together", "D. Learning to Be"], 
    c: 3, 
    rat: "Learning to Be emphasizes the complete development of the individual."
},
{ 
    cat: "Prof Ed", 
    q: "Who is the 'Father of Modern Psychology'?", 
    a: ["A. Wilhelm Wundt", "B. William James", "C. Sigmund Freud", "D. John Dewey"], 
    c: 0, 
    rat: "Wundt established the first experimental psychology lab in 1879."
},
{ 
    cat: "Prof Ed", 
    q: "Which assessment technique is best for 'Problem Solving' skills?", 
    a: ["A. Multiple Choice", "B. True or False", "C. Performance Task", "D. Fill in the blanks"], 
    c: 2, 
    rat: "Performance tasks require students to apply knowledge to real-world problems."
},
{ 
    cat: "Prof Ed", 
    q: "What is the study of principles and methods of teaching?", 
    a: ["A. Pedagogy", "B. Andragogy", "C. Sociology", "D. Psychology"], 
    c: 0, 
    rat: "Pedagogy refers specifically to the science and art of teaching children."
},
{ 
    cat: "Prof Ed", 
    q: "Which philosophy claims 'Man is the master of his own destiny'?", 
    a: ["A. Realism", "B. Idealism", "C. Existentialism", "D. Essentialism"], 
    c: 2, 
    rat: "Existentialism emphasizes individual choice and responsibility."
},
{ 
    cat: "Prof Ed", 
    q: "What does 'Standard Deviation' measure in test results?", 
    a: ["A. Average", "B. Most frequent score", "C. Variability of scores", "D. Accuracy"], 
    c: 2, 
    rat: "It shows how spread out or clustered the scores are relative to the mean."
},
{ 
    cat: "Prof Ed", 
    q: "A student excels in dance. According to Gardner, they have high:", 
    a: ["A. Visual", "B. Kinesthetic", "C. Naturalist", "D. Interpersonal"], 
    c: 1, 
    rat: "Kinesthetic intelligence is the ability to use the body skillfully."
},
{ 
    cat: "Prof Ed", 
    q: "Which level of Bloom's Revised Taxonomy involves 'Formulating a plan'?", 
    a: ["A. Analyzing", "B. Evaluating", "C. Creating", "D. Applying"], 
    c: 2, 
    rat: "Creating is the highest level, involving the assembly of parts into a new whole."
},
{ 
    cat: "Prof Ed", 
    q: "What is the primary role of a 'Facilitator'?", 
    a: ["A. Lecturer", "B. Authority", "C. Guide", "D. Evaluator"], 
    c: 2, 
    rat: "A facilitator guides the learner to discover knowledge rather than just delivering it."
},
{ 
    cat: "Prof Ed", 
    q: "Which philosophy believes the mind is 'Blank' at birth?", 
    a: ["A. Empiricism", "B. Rationalism", "C. Idealism", "D. Perennialism"], 
    c: 0, 
    rat: "Empiricism (John Locke) states that all knowledge comes from sense experience."
},
{ 
    cat: "Prof Ed", 
    q: "In classroom management, 'with-it-ness' means the teacher:", 
    a: ["A. Has good fashion", "B. Knows everything", "C. Knows what is going on at all times", "D. Is friendly"], 
    c: 2, 
    rat: "Jacob Kounin's term 'With-it-ness' refers to having eyes at the back of the head."
},
{ 
    cat: "Prof Ed", 
    q: "Which law created the Professional Regulatory Board for Professional Teachers?", 
    a: ["A. RA 7836", "B. RA 4670", "C. RA 9155", "D. RA 9293"], 
    c: 0, 
    rat: "RA 7836 is the Philippine Teachers Professionalization Act of 1994."
},
{ 
    cat: "Prof Ed", 
    q: "When a test is 'Reliable,' it means it is:", 
    a: ["A. Accurate", "B. Consistent", "C. Valid", "D. Difficult"], 
    c: 1, 
    rat: "Reliability refers to the consistency of results over multiple administrations."
},
{ 
    cat: "Prof Ed", 
    q: "Who is the proponent of 'Meaningful Verbal Learning' (Advance Organizers)?", 
    a: ["A. Ausubel", "B. Bruner", "C. Gagne", "D. Bloom"], 
    c: 0, 
    rat: "David Ausubel proposed that new learning is related to existing knowledge."
},
{ 
    cat: "Prof Ed", 
    q: "Which assessment technique fits 'Applying skills in a real-life context'?", 
    a: ["A. Essay", "B. True or False", "C. Authentic Assessment", "D. Completion Test"], 
    c: 2, 
    rat: "Authentic assessment measures performance in tasks that mimic real life."
},
{ 
    cat: "Prof Ed", 
    q: "According to Kohlberg, what is the basis of 'Conventional' morality?", 
    a: ["A. Punishment", "B. Social Approval/Laws", "C. Self-interest", "D. Universal Ethics"], 
    c: 1, 
    rat: "Conventional morality is based on following rules and seeking approval."
},
{ 
    cat: "Prof Ed", 
    q: "Which teaching approach starts from General to Specific?", 
    a: ["A. Inductive", "B. Deductive", "C. Inquiry", "D. Discovery"], 
    c: 1, 
    rat: "Deductive reasoning starts with a rule or theory and applies it to examples."
},
{ 
    cat: "Prof Ed", 
    q: "What is the primary function of the 'Code of Ethics for Professional Teachers'?", 
    a: ["A. Raise salary", "B. Guide behavior and dignity", "C. Shorten work hours", "D. Provide insurance"], 
    c: 1, 
    rat: "It sets the standard for conduct, relationship with students, and professionalism."
},
{ 
    cat: "Prof Ed", 
    q: "In 'Spiral Progression' of the K-12, concepts are taught:", 
    a: ["A. Only once", "B. From simple to complex across levels", "C. All at once", "D. Randomly"], 
    c: 1, 
    rat: "Concepts are revisited with increasing depth as students advance."
},
{ 
    cat: "Prof Ed", 
    q: "Who developed the 'Discovery Learning' approach?", 
    a: ["A. Bruner", "B. Piaget", "C. Gagne", "D. Dewey"], 
    c: 0, 
    rat: "Jerome Bruner believed students learn best by discovering facts for themselves."
},
{ 
    cat: "Prof Ed", 
    q: "Which type of test shows if a student has mastered a specific objective?", 
    a: ["A. Norm-referenced", "B. Criterion-referenced", "C. Aptitude", "D. Interest"], 
    c: 1, 
    rat: "Criterion-referenced tests compare a student against a set standard or 'criterion'."
},
{ 
    cat: "Prof Ed", 
    q: "What part of the lesson plan is the 'Hook' or 'Drill'?", 
    a: ["A. Evaluation", "B. Application", "C. Motivation/Review", "D. Generalization"], 
    c: 2, 
    rat: "Motivation is designed to capture interest and activate prior knowledge."
},
{ 
    cat: "Prof Ed", 
    q: "According to Piaget, 'Object Permanence' is mastered during:", 
    a: ["A. Sensorimotor", "B. Pre-operational", "C. Concrete Operational", "D. Formal Operational"], 
    c: 0, 
    rat: "This is the understanding that objects still exist even when hidden (Infancy)."
},
{ 
    cat: "Prof Ed", 
    q: "A teacher/student is held responsible for his actions because s/he has:", 
    a: ["A. Reason", "B. Choice", "C. Maturity", "D. Diploma"], 
    c: 1, 
    rat: "Responsibility comes from the power to choose between alternatives."
},
{ 
    cat: "Prof Ed", 
    q: "Which portfolio showcases a student's best works?", 
    a: ["A. Development", "B. Assessment", "C. Showcase", "D. Process"], 
    c: 2, 
    rat: "The Showcase portfolio is used for celebration and job applications."
},
{ 
    cat: "Prof Ed", 
    q: "What is the required teacher-student interaction style for a child-centered class?", 
    a: ["A. One-way", "B. Two-way/Multi-way", "C. Silent", "D. Authoritarian"], 
    c: 1, 
    rat: "Active engagement requires interaction among students and with the teacher."
}
// --- MATH MAJORSHIP ---
{ 
    cat: "Math Major", 
    q: "Find the value of x: 3(x - 5) = 12.", 
    a: ["A. 4", "B. 9", "C. 7", "D. 15"], 
    c: 1, 
    rat: "Divide by 3: x-5 = 4. Then add 5: x = 9. Plug it back: 3(9-5) = 3(4) = 12."
},
{ 
    cat: "Math Major", 
    q: "What is the slope of the line given by the equation 2x + y = 5?", 
    a: ["A. 2", "B. -2", "C. 5", "D. -5"], 
    c: 1, 
    rat: "Rewrite in y = mx + b form: y = -2x + 5. The slope (m) is -2."
},
{ 
    cat: "Math Major", 
    q: "What is the sum of the interior angles of a hexagon?", 
    a: ["A. 360°", "B. 540°", "C. 720°", "D. 900°"], 
    c: 2, 
    rat: "Use the formula (n-2)180. For a hexagon (n=6): (6-2)180 = 4 * 180 = 720°."
},
{ 
    cat: "Math Major", 
    q: "Solve for x: log₂(x) = 5.", 
    a: ["A. 10", "B. 25", "C. 32", "D. 64"], 
    c: 2, 
    rat: "Convert to exponential form: 2⁵ = x. Since 2 * 2 * 2 * 2 * 2 = 32, then x = 32."
},
{ 
    cat: "Math Major", 
    q: "Which of the following is a prime number?", 
    a: ["A. 51", "B. 81", "C. 91", "D. 97"], 
    c: 3, 
    rat: "97 is only divisible by 1 and itself. (51=3x17, 81=9x9, 91=7x13)."
},
{ 
    cat: "Math Major", 
    q: "If the side of a square is doubled, the area is multiplied by:", 
    a: ["A. 2", "B. 4", "C. 8", "D. 16"], 
    c: 1, 
    rat: "Area = s². If side becomes 2s, new area = (2s)² = 4s². The area quadruples."
},
{ 
    cat: "Math Major", 
    q: "What is the value of sin(30°)?", 
    a: ["A. 1/2", "B. √2/2", "C. √3/2", "D. 1"], 
    c: 0, 
    rat: "Based on the unit circle or special triangles (30-60-90), the sine of 30 degrees is exactly 0.5 or 1/2."
},
{ 
    cat: "Math Major", 
    q: "Simplify: (x³)² / x²", 
    a: ["A. x³", "B. x⁴", "C. x⁵", "D. x⁶"], 
    c: 1, 
    rat: "Apply power rule: (x³)² = x⁶. Apply quotient rule: x⁶ / x² = x⁶⁻² = x⁴."
},
{ 
    cat: "Math Major", 
    q: "The mean of five numbers is 20. If a 6th number, 50, is added, what is the new mean?", 
    a: ["A. 20", "B. 25", "C. 30", "D. 35"], 
    c: 1, 
    rat: "Total of first 5: 5 * 20 = 100. New total: 100 + 50 = 150. New mean: 150 / 6 = 25."
},
{ 
    cat: "Math Major", 
    q: "What is the supplement of an angle measuring 75°?", 
    a: ["A. 15°", "B. 105°", "C. 285°", "D. 180°"], 
    c: 1, 
    rat: "Supplementary angles add up to 180°. 180 - 75 = 105°."
},
{ 
    cat: "Math Major", 
    q: "Find the volume of a cylinder with radius 3 and height 10. (Use π)", 
    a: ["A. 30π", "B. 60π", "C. 90π", "D. 120π"], 
    c: 2, 
    rat: "Volume = πr²h. So, π * (3)² * 10 = π * 9 * 10 = 90π."
},
{ 
    cat: "Math Major", 
    q: "How many permutations are there for the word 'MATH'?", 
    a: ["A. 4", "B. 12", "C. 24", "D. 48"], 
    c: 2, 
    rat: "The letters are all distinct. 4! (4 factorial) = 4 * 3 * 2 * 1 = 24."
},
{ 
    cat: "Math Major", 
    q: "If 3x - 7 = 5x + 3, then x is:", 
    a: ["A. -5", "B. 5", "C. -2", "D. 2"], 
    c: 0, 
    rat: "Subtract 3x: -7 = 2x + 3. Subtract 3: -10 = 2x. Divide by 2: x = -5."
},
{ 
    cat: "Math Major", 
    q: "The set of all possible outcomes in an experiment is called:", 
    a: ["A. Event", "B. Sample Space", "C. Probability", "D. Subset"], 
    c: 1, 
    rat: "The Sample Space is the comprehensive list of every potential result of a random trial."
},
{ 
    cat: "Math Major", 
    q: "What is the distance between points (0,0) and (3,4)?", 
    a: ["A. 5", "B. 7", "C. 12", "D. 25"], 
    c: 0, 
    rat: "Use distance formula: √(3-0)² + (4-0)² = √9 + 16 = √25 = 5."
},
{ 
    cat: "Math Major", 
    q: "Which property is shown: a(b + c) = ab + ac?", 
    a: ["A. Commutative", "B. Associative", "C. Distributive", "D. Identity"], 
    c: 2, 
    rat: "The Distributive Property allows you to multiply a sum by multiplying each addend separately."
},
{ 
    cat: "Math Major", 
    q: "What is the LCM of 12 and 15?", 
    a: ["A. 3", "B. 30", "C. 60", "D. 180"], 
    c: 2, 
    rat: "Multiples of 12: 12, 24, 36, 48, 60. Multiples of 15: 15, 30, 45, 60. The smallest common one is 60."
},
{ 
    cat: "Math Major", 
    q: "Find the median of the set: {5, 2, 9, 1, 7}.", 
    a: ["A. 9", "B. 5", "C. 4.8", "D. 7"], 
    c: 1, 
    rat: "First, arrange in order: 1, 2, 5, 7, 9. The middle number is 5."
},
{ 
    cat: "Math Major", 
    q: "A triangle with three equal sides is called:", 
    a: ["A. Isosceles", "B. Scalene", "C. Equilateral", "D. Right"], 
    c: 2, 
    rat: "An equilateral triangle has all sides equal and all angles equal to 60°."
},
{ 
    cat: "Math Major", 
    q: "Simplify: √50", 
    a: ["A. 5√2", "B. 2√5", "C. 10√5", "D. 25√2"], 
    c: 0, 
    rat: "√50 = √25 * √2 = 5√2."
},
{ 
    cat: "Math Major", 
    q: "What is the value of 5!", 
    a: ["A. 25", "B. 60", "C. 120", "D. 125"], 
    c: 2, 
    rat: "5! = 5 * 4 * 3 * 2 * 1 = 120."
},
{ 
    cat: "Math Major", 
    q: "The hypotenuse of a right triangle with legs 6 and 8 is:", 
    a: ["A. 10", "B. 12", "C. 14", "D. 100"], 
    c: 0, 
    rat: "Pythagorean theorem: 6² + 8² = c². 36 + 64 = 100. √100 = 10."
},
{ 
    cat: "Math Major", 
    q: "What is the domain of the function f(x) = 1/x?", 
    a: ["A. All real numbers", "B. All x > 0", "C. All x except 0", "D. All x < 0"], 
    c: 2, 
    rat: "Division by zero is undefined, so x cannot be 0."
},
{ 
    cat: "Math Major", 
    q: "Find the area of a triangle with base 10 and height 8.", 
    a: ["A. 80", "B. 40", "C. 20", "D. 18"], 
    c: 1, 
    rat: "Area = 1/2 * base * height. 1/2 * 10 * 8 = 40."
},
{ 
    cat: "Math Major", 
    q: "What is the slope of a vertical line?", 
    a: ["A. 0", "B. 1", "C. Undefined", "D. -1"], 
    c: 2, 
    rat: "Vertical lines have no 'run' (change in x = 0). Since you can't divide by zero, the slope is undefined."
},
{ 
    cat: "Math Major", 
    q: "Solve for x: 2^(x+1) = 8.", 
    a: ["A. 1", "B. 2", "C. 3", "D. 4"], 
    c: 1, 
    rat: "8 is 2³. So x + 1 = 3, which means x = 2."
},
{ 
    cat: "Math Major", 
    q: "A deck of 52 cards is shuffled. What is the probability of picking an Ace?", 
    a: ["A. 1/52", "B. 1/13", "C. 4/13", "D. 1/4"], 
    c: 1, 
    rat: "There are 4 Aces in 52 cards. 4/52 simplifies to 1/13."
},
{ 
    cat: "Math Major", 
    q: "Find the 10th term of the arithmetic sequence: 2, 5, 8...", 
    a: ["A. 27", "B. 29", "C. 30", "D. 32"], 
    c: 1, 
    rat: "Use aₙ = a₁ + (n-1)d. a₁₀ = 2 + (9 * 3) = 2 + 27 = 29."
},
{ 
    cat: "Math Major", 
    q: "What is the value of cos(0°)?", 
    a: ["A. 0", "B. 1", "C. 1/2", "D. Undefined"], 
    c: 1, 
    rat: "At 0 degrees, the x-coordinate on the unit circle is 1. Therefore, cos(0°) = 1."
},
{ 
    cat: "Math Major", 
    q: "The number of subsets in a set with 3 elements is:", 
    a: ["A. 3", "B. 6", "C. 8", "D. 9"], 
    c: 2, 
    rat: "Number of subsets = 2ⁿ. For 3 elements, 2³ = 8."
},
{ 
    cat: "Math Major", 
    q: "What is the GCF of 48 and 64?", 
    a: ["A. 8", "B. 12", "C. 16", "D. 24"], 
    c: 2, 
    rat: "Factors of 48: 1, 2, 3, 4, 6, 8, 12, 16, 24, 48. Factors of 64: 1, 2, 4, 8, 16, 32, 64. Highest is 16."
},
{ 
    cat: "Math Major", 
    q: "Simplify: (x + 3)(x - 3)", 
    a: ["A. x² + 9", "B. x² - 9", "C. x² + 6x + 9", "D. x² - 6x + 9"], 
    c: 1, 
    rat: "This is a difference of squares pattern: (a+b)(a-b) = a² - b². Result is x² - 9."
},
{ 
    cat: "Math Major", 
    q: "How many degrees are in a reflex angle?", 
    a: ["A. Less than 90°", "B. Exactly 180°", "C. Between 180° and 360°", "D. Exactly 360°"], 
    c: 2, 
    rat: "A reflex angle is larger than a straight angle (180°) but smaller than a full circle (360°)."
},
{ 
    cat: "Math Major", 
    q: "Find the x-intercept of 3x + 4y = 12.", 
    a: ["A. 3", "B. 4", "C. 12", "D. 0"], 
    c: 1, 
    rat: "Set y = 0: 3x = 12. So x = 4."
},
{ 
    cat: "Math Major", 
    q: "What is the mode of {2, 3, 3, 5, 7, 7, 7, 8}?", 
    a: ["A. 3", "B. 5", "C. 7", "D. 8"], 
    c: 2, 
    rat: "The mode is the number that appears most frequently. 7 appears three times."
},
{ 
    cat: "Math Major", 
    q: "The surface area of a cube with side 2 is:", 
    a: ["A. 8", "B. 12", "C. 16", "D. 24"], 
    c: 3, 
    rat: "Surface Area = 6s². 6 * (2)² = 6 * 4 = 24."
},
{ 
    cat: "Math Major", 
    q: "What is the derivative of f(x) = x²?", 
    a: ["A. x", "B. 2x", "C. 2", "D. x³"], 
    c: 1, 
    rat: "Using the power rule, d/dx(xⁿ) = nxⁿ⁻¹. So for x², the derivative is 2x."
},
{ 
    cat: "Math Major", 
    q: "Solve for x: |x - 3| = 5.", 
    a: ["A. 8 only", "B. -2 only", "C. 8 and -2", "D. 2 and -8"], 
    c: 2, 
    rat: "Set up two equations: x-3 = 5 (x=8) and x-3 = -5 (x=-2)."
},
{ 
    cat: "Math Major", 
    q: "The interior angles of a triangle are in ratio 1:2:3. The angles are:", 
    a: ["A. 30, 60, 90", "B. 40, 50, 90", "C. 20, 40, 120", "D. 10, 20, 150"], 
    c: 0, 
    rat: "1x + 2x + 3x = 180. 6x = 180, so x = 30. Angles: 30, 60, 90."
},
{ 
    cat: "Math Major", 
    q: "What is the period of the function y = sin(x)?", 
    a: ["A. π", "B. 2π", "C. π/2", "D. 4π"], 
    c: 1, 
    rat: "The sine function repeats its pattern every 2π radians (or 360°)."
},
{ 
    cat: "Math Major", 
    q: "Which is the smallest value?", 
    a: ["A. 1/2", "B. 0.55", "C. 45%", "D. 1/3"], 
    c: 3, 
    rat: "Convert to decimals: A=0.5, B=0.55, C=0.45, D=0.33. D is the smallest."
},
{ 
    cat: "Math Major", 
    q: "Find the coordinates of the midpoint between (2,3) and (4,7).", 
    a: ["A. (3,5)", "B. (6,10)", "C. (1,2)", "D. (3,4)"], 
    c: 0, 
    rat: "Midpoint = ((x₁+x₂)/2, (y₁+y₂)/2). (2+4)/2 = 3; (3+7)/2 = 5."
},
{ 
    cat: "Math Major", 
    q: "If tan(θ) = 1, then θ in degrees is:", 
    a: ["A. 0°", "B. 30°", "C. 45°", "D. 90°"], 
    c: 2, 
    rat: "Tangent is 1 when sine and cosine are equal, which occurs at 45°."
},
{ 
    cat: "Math Major", 
    q: "What is the product of (2+3i) and (2-3i)?", 
    a: ["A. 4 - 9i", "B. 4 + 9i", "C. 13", "D. -5"], 
    c: 2, 
    rat: "Difference of squares: 2² - (3i)². 4 - 9i². Since i² = -1, we get 4 + 9 = 13."
},
{ 
    cat: "Math Major", 
    q: "The discriminant of x² + 4x + 4 = 0 is:", 
    a: ["A. 0", "B. 4", "C. 16", "D. -16"], 
    c: 0, 
    rat: "Discriminant D = b² - 4ac. 4² - 4(1)(4) = 16 - 16 = 0."
},
{ 
    cat: "Math Major", 
    q: "How many milliliters are in 2.5 liters?", 
    a: ["A. 25 ml", "B. 250 ml", "C. 2,500 ml", "D. 25,000 ml"], 
    c: 2, 
    rat: "1 Liter = 1,000 ml. So 2.5 * 1,000 = 2,500 ml."
},
{ 
    cat: "Math Major", 
    q: "Find the range of the set: {10, 2, 35, 14, 21}.", 
    a: ["A. 35", "B. 33", "C. 14", "D. 10"], 
    c: 1, 
    rat: "Range = Max - Min. 35 - 2 = 33."
},
{ 
    cat: "Math Major", 
    q: "What is the value of log₁₀(1000)?", 
    a: ["A. 1", "B. 2", "C. 3", "D. 10"], 
    c: 2, 
    rat: "Since 10³ = 1000, the logarithm base 10 of 1000 is 3."
},
{ 
    cat: "Math Major", 
    q: "A polygon with 5 sides is a:", 
    a: ["A. Hexagon", "B. Pentagon", "C. Heptagon", "D. Quadrilateral"], 
    c: 1, 
    rat: "A pentagon has 5 sides and its interior angles sum to 540°."
},
{ 
    cat: "Math Major", 
    q: "What is the derivative of a constant?", 
    a: ["A. 1", "B. x", "C. 0", "D. -1"], 
    c: 2, 
    rat: "Because a constant does not change, its rate of change (derivative) is always 0."
}


// --- ENGLISH MAJORSHIP ---
{ 
    cat: "English Major", 
    q: "What is the study of how sentences are structured?", 
    a: ["A. Semantics", "B. Phonology", "C. Syntax", "D. Morphology"], 
    c: 2, 
    rat: "Syntax is the branch of linguistics that studies the arrangement of words and phrases to create well-formed sentences."
},
{ 
    cat: "English Major", 
    q: "Who is the 'Father of Modern English Poetry'?", 
    a: ["A. William Shakespeare", "B. Geoffrey Chaucer", "C. John Milton", "D. Alexander Pope"], 
    c: 1, 
    rat: "Geoffrey Chaucer, author of The Canterbury Tales, is credited with legitimizing the use of Middle English in literature."
},
{ 
    cat: "English Major", 
    q: "The word 'brunch' (breakfast + lunch) is an example of:", 
    a: ["A. Clipping", "B. Portmanteau", "C. Compounding", "D. Acronym"], 
    c: 1, 
    rat: "A portmanteau is a linguistic blend of words, in which parts of multiple words are combined into a new one."
},
{ 
    cat: "English Major", 
    q: "In the sentence 'The cat sat on the mat,' what is the 'on the mat' part?", 
    a: ["A. Noun Phrase", "B. Prepositional Phrase", "C. Verb Phrase", "D. Adjectival Phrase"], 
    c: 1, 
    rat: "It starts with the preposition 'on' and functions as an adverb of place, making it a prepositional phrase."
},
{ 
    cat: "English Major", 
    q: "Which Philippine novel features the characters Simoun and Basilio?", 
    a: ["A. Noli Me Tangere", "B. El Filibusterismo", "C. The Woman Who Had Two Navels", "D. Without Seeing the Dawn"], 
    c: 1, 
    rat: "El Filibusterismo is Rizal's second novel, where Ibarra returns as the jeweler Simoun."
},
{ 
    cat: "English Major", 
    q: "What is the smallest unit of sound in a language?", 
    a: ["A. Morpheme", "B. Phoneme", "C. Grapheme", "D. Allophone"], 
    c: 1, 
    rat: "A phoneme is the smallest unit of sound that distinguishes one word from another (e.g., /p/ vs /b/)."
},
{ 
    cat: "English Major", 
    q: "Who wrote 'The Waste Land', a landmark of modernist poetry?", 
    a: ["A. Robert Frost", "B. T.S. Eliot", "C. W.B. Yeats", "D. Ezra Pound"], 
    c: 1, 
    rat: "T.S. Eliot's 1922 poem is central to modern literature for its themes of disillusionment and complex style."
},
{ 
    cat: "English Major", 
    q: "Which figure of speech is: 'The classroom was a zoo'?", 
    a: ["A. Simile", "B. Metaphor", "C. Personification", "D. Hyperbole"], 
    c: 1, 
    rat: "This is a metaphor because it makes a direct comparison without using 'like' or 'as'."
},
{ 
    cat: "English Major", 
    q: "What do you call a poem of 14 lines in iambic pentameter?", 
    a: ["A. Ballad", "B. Epic", "C. Sonnet", "D. Haiku"], 
    c: 2, 
    rat: "A sonnet has exactly 14 lines and a specific rhyme scheme, famously used by Shakespeare and Petrarch."
},
{ 
    cat: "English Major", 
    q: "In linguistics, the study of word formation is known as:", 
    a: ["A. Phonology", "B. Syntax", "C. Morphology", "D. Semantics"], 
    c: 2, 
    rat: "Morphology examines how morphemes (roots, prefixes, suffixes) combine to form words."
},
{ 
    cat: "English Major", 
    q: "Who is the Greek poet attributed with 'The Iliad' and 'The Odyssey'?", 
    a: ["A. Sophocles", "B. Homer", "C. Euripides", "D. Virgil"], 
    c: 1, 
    rat: "Homer is the legendary author of the two foundational epics of Ancient Greek literature."
},
{ 
    cat: "English Major", 
    q: "The sentence 'I will have finished the report by noon' is in what tense?", 
    a: ["A. Simple Future", "B. Future Continuous", "C. Future Perfect", "D. Present Perfect"], 
    c: 2, 
    rat: "Future Perfect (will have + past participle) describes an action that will be completed by a specific time."
},
{ 
    cat: "English Major", 
    q: "Which novel by George Orwell is a satire on the Russian Revolution?", 
    a: ["A. 1984", "B. Brave New World", "C. Animal Farm", "D. Lord of the Flies"], 
    c: 2, 
    rat: "Animal Farm uses farm animals to represent key figures and events of the Soviet Union's early years."
},
{ 
    cat: "English Major", 
    q: "What is the primary focus of the 'Audio-Lingual Method' (ALM)?", 
    a: ["A. Grammar translation", "B. Habit formation and drills", "C. Creative writing", "D. Reading comprehension"], 
    c: 1, 
    rat: "ALM is based on behaviorist theory, focusing on repetitive drills and oral imitation to learn a language."
},
{ 
    cat: "English Major", 
    q: "Who is the tragic hero in Shakespeare's 'Hamlet'?", 
    a: ["A. Claudius", "B. Polonius", "C. Hamlet", "D. Laertes"], 
    c: 2, 
    rat: "Prince Hamlet is the protagonist whose 'tragic flaw'—indecision—leads to the play's catastrophic end."
},
{ 
    cat: "English Major", 
    q: "Which literary period focuses on reason, logic, and scientific observation?", 
    a: ["A. Romanticism", "B. Victorian Era", "C. Neoclassicism", "D. Renaissance"], 
    c: 2, 
    rat: "Also called the Enlightenment, Neoclassicism valued order, restraint, and the wisdom of classical thinkers."
},
{ 
    cat: "English Major", 
    q: "The 'Cask of Amontillado' was written by which Master of Macabre?", 
    a: ["A. Nathaniel Hawthorne", "B. Edgar Allan Poe", "C. Mark Twain", "D. Herman Melville"], 
    c: 1, 
    rat: "Edgar Allan Poe is famous for his gothic tales of horror, mystery, and the macabre."
},
{ 
    cat: "English Major", 
    q: "A research paper's 'Abstract' serves what purpose?", 
    a: ["A. Detailed data", "B. Full bibliography", "C. Brief summary", "D. Personal opinion"], 
    c: 2, 
    rat: "An abstract provides a concise summary of the research's objectives, methods, and findings."
},
{ 
    cat: "English Major", 
    q: "Which Philippine writer is known for 'The Woman Who Had Two Navels'?", 
    a: ["A. Jose Garcia Villa", "B. Nick Joaquin", "C. F. Sionil Jose", "D. Bienvenido Santos"], 
    c: 1, 
    rat: "Nick Joaquin, a National Artist, wrote this novel exploring the identity and history of the Philippines."
},
{ 
    cat: "English Major", 
    q: "In writing, 'Cohesion' refers to:", 
    a: ["A. Logical flow of ideas", "B. Grammatical links between sentences", "C. Correct spelling", "D. Choice of topic"], 
    c: 1, 
    rat: "Cohesion is the grammatical and lexical linking within a text that holds it together (e.g., using pronouns or conjunctions)."
},
{ 
    cat: "English Major", 
    q: "Who wrote the Victorian novel 'Jane Eyre'?", 
    a: ["A. Emily Brontë", "B. Charlotte Brontë", "C. Jane Austen", "D. George Eliot"], 
    c: 1, 
    rat: "Charlotte Brontë wrote Jane Eyre under the pseudonym Currer Bell."
},
{ 
    cat: "English Major", 
    q: "Which Japanese poetic form consists of 17 syllables in a 5-7-5 pattern?", 
    a: ["A. Tanka", "B. Haiku", "C. Noh", "D. Kabuki"], 
    c: 1, 
    rat: "Haiku is a traditional form of short poetry that typically focuses on images of nature."
},
{ 
    cat: "English Major", 
    q: "What is the 'Denouement' of a story?", 
    a: ["A. The beginning", "B. The turning point", "C. The resolution", "D. The rising action"], 
    c: 2, 
    rat: "Denouement is the final part of a play, movie, or narrative in which the strands of the plot are drawn together."
},
{ 
    cat: "English Major", 
    q: "Which approach to language teaching emphasizes real-life communication tasks?", 
    a: ["A. Direct Method", "B. Communicative Language Teaching (CLT)", "C. Silent Way", "D. Grammar Translation"], 
    c: 1, 
    rat: "CLT prioritizes interaction and meaningful communication as both the goal and the means of study."
},
{ 
    cat: "English Major", 
    q: "The suffix '-ed' in 'walked' is an example of a/an:", 
    a: ["A. Derivational Morpheme", "B. Inflectional Morpheme", "C. Bound Root", "D. Free Morpheme"], 
    c: 1, 
    rat: "Inflectional morphemes change the grammatical form (like tense) but not the core meaning or part of speech."
},
{ 
    cat: "English Major", 
    q: "Who is known as the 'Morning Star of English Poetry'?", 
    a: ["A. Spenser", "B. Chaucer", "C. Shakespeare", "D. Donne"], 
    c: 1, 
    rat: "Geoffrey Chaucer is given this title as he was the first great poet to write in the English language."
},
{ 
    cat: "English Major", 
    q: "What is an 'Epistolary' novel?", 
    a: ["A. A novel written in verse", "B. A novel written as a series of documents/letters", "C. A novel with no plot", "D. A satirical novel"], 
    c: 1, 
    rat: "Epistolary novels (like Dracula or Frankenstein) use letters, diary entries, or newspaper clippings to tell the story."
},
{ 
    cat: "English Major", 
    q: "Which play by Arthur Miller is a critique of the 'American Dream'?", 
    a: ["A. The Crucible", "B. Death of a Salesman", "C. All My Sons", "D. A View from the Bridge"], 
    c: 1, 
    rat: "Death of a Salesman follows Willy Loman, whose pursuit of success leads to his ultimate downfall."
},
{ 
    cat: "English Major", 
    q: "In literature, 'Irony' occurs when:", 
    a: ["A. Things are exactly as they seem", "B. The outcome is the opposite of what is expected", "C. A character dies", "D. A story ends happily"], 
    c: 1, 
    rat: "Irony involves a contrast between expectation and reality."
},
{ 
    cat: "English Major", 
    q: "The Old English epic 'Beowulf' was written in which language?", 
    a: ["A. Modern English", "B. Middle English", "C. Old English (Anglo-Saxon)", "D. Latin"], 
    c: 2, 
    rat: "Beowulf is the oldest surviving long poem in Old English, dating back to between the 8th and 11th centuries."
},
{ 
    cat: "English Major", 
    q: "Which linguist proposed the 'Innatist' theory (Language Acquisition Device)?", 
    a: ["A. B.F. Skinner", "B. Noam Chomsky", "C. Jean Piaget", "D. Lev Vygotsky"], 
    c: 1, 
    rat: "Chomsky argued that humans are born with a biological predisposition to learn language."
},
{ 
    cat: "English Major", 
    q: "Who is the author of 'Things Fall Apart', a classic of African literature?", 
    a: ["A. Wole Soyinka", "B. Chinua Achebe", "C. Ngugi wa Thiong'o", "D. Nadine Gordimer"], 
    c: 1, 
    rat: "Chinua Achebe's novel depicts the clash between traditional Igbo culture and British colonialism."
},
{ 
    cat: "English Major", 
    q: "What is a 'Malapropism'?", 
    a: ["A. A type of metaphor", "B. Mistaken use of a word in place of a similar-sounding one", "C. A correct grammatical sentence", "D. A rhythmic pattern"], 
    c: 1, 
    rat: "An example is saying 'electrical' instead of 'electoral,' often used for comedic effect."
},
{ 
    cat: "English Major", 
    q: "Which Philippine short story by Paz Marquez Benitez is the first in English?", 
    a: ["A. Dead Stars", "B. Footnote to Youth", "C. The Wedding Dance", "D. May Day Eve"], 
    c: 0, 
    rat: "Published in 1925, 'Dead Stars' is considered the first modern Philippine short story in English."
},
{ 
    cat: "English Major", 
    q: "In the sentence 'I saw the man with a telescope,' the ambiguity is an example of:", 
    a: ["A. Lexical Ambiguity", "B. Structural (Syntactic) Ambiguity", "C. Phonological Ambiguity", "D. Semantic Ambiguity"], 
    c: 1, 
    rat: "The structure makes it unclear if the man had the telescope or if the telescope was used to see the man."
},
{ 
    cat: "English Major", 
    q: "What is 'Alliteration'?", 
    a: ["A. Repetition of vowel sounds", "B. Repetition of initial consonant sounds", "C. Repetition of end rhymes", "D. Repetition of sentence structure"], 
    c: 1, 
    rat: "Example: 'Peter Piper picked a peck of pickled peppers.'"
},
{ 
    cat: "English Major", 
    q: "Who is the Victorian poet who wrote 'Ulysses' and 'The Charge of the Light Brigade'?", 
    a: ["A. Robert Browning", "B. Matthew Arnold", "C. Alfred Lord Tennyson", "D. Christina Rossetti"], 
    c: 2, 
    rat: "Tennyson was the Poet Laureate of Great Britain and Ireland during much of Queen Victoria's reign."
},
{ 
    cat: "English Major", 
    q: "Which 'Hat' in De Bono's Six Thinking Hats deals with facts and information?", 
    a: ["A. Red Hat", "B. White Hat", "C. Black Hat", "D. Green Hat"], 
    c: 1, 
    rat: "The White Hat is neutral and objective, focusing solely on data and available information."
},
{ 
    cat: "English Major", 
    q: "In a plot diagram, what follows the 'Climax'?", 
    a: ["A. Exposition", "B. Rising Action", "C. Falling Action", "D. Resolution"], 
    c: 2, 
    rat: "Falling action deals with the consequences of the climax and leads toward the story's end."
},
{ 
    cat: "English Major", 
    q: "Which American poet is famous for 'The Road Not Taken'?", 
    a: ["A. Walt Whitman", "B. Emily Dickinson", "C. Robert Frost", "D. Langston Hughes"], 
    c: 2, 
    rat: "Robert Frost is highly regarded for his realistic depictions of rural life and command of American colloquial speech."
},
{ 
    cat: "English Major", 
    q: "The study of the social use of language is called:", 
    a: ["A. Psycholinguistics", "B. Sociolinguistics", "C. Computational Linguistics", "D. Historical Linguistics"], 
    c: 1, 
    rat: "Sociolinguistics examines how factors like class, gender, and ethnicity affect language use."
},
{ 
    cat: "English Major", 
    q: "Who wrote 'The Great Gatsby'?", 
    a: ["A. Ernest Hemingway", "B. F. Scott Fitzgerald", "C. William Faulkner", "D. John Steinbeck"], 
    c: 1, 
    rat: "Fitzgerald's novel is a definitive portrait of the Jazz Age and the 'Roaring Twenties'."
},
{ 
    cat: "English Major", 
    q: "What is an 'Oxymoron'?", 
    a: ["A. A long exaggeration", "B. A figure of speech combining contradictory terms", "C. A direct address to an absent person", "D. A word that sounds like its meaning"], 
    c: 1, 
    rat: "Examples include 'deafening silence' or 'bittersweet'."
},
{ 
    cat: "English Major", 
    q: "Which method uses the native language as the medium of instruction for grammar?", 
    a: ["A. Direct Method", "B. Grammar Translation Method", "C. Suggestopedia", "D. Total Physical Response"], 
    c: 1, 
    rat: "This traditional method focuses on translating texts and learning grammar rules in the student's L1."
},
{ 
    cat: "English Major", 
    q: "Who is the National Artist for Literature known as the 'Poet of the People'?", 
    a: ["A. Jose Garcia Villa", "B. Amado V. Hernandez", "C. Edith Tiempo", "D. Virgilio Almario"], 
    c: 1, 
    rat: "Amado V. Hernandez was known for his socially committed works written in Tagalog."
},
{ 
    cat: "English Major", 
    q: "What is 'Syntax'?", 
    a: ["A. Study of sound", "B. Study of sentence structure", "C. Study of meaning", "D. Study of word origins"], 
    c: 1, 
    rat: "Syntax refers to the rules that govern how words are combined to form phrases and sentences."
},
{ 
    cat: "English Major", 
    q: "Which novel by Jane Austen begins with the famous line about a 'single man in possession of a good fortune'?", 
    a: ["A. Sense and Sensibility", "B. Pride and Prejudice", "C. Emma", "D. Mansfield Park"], 
    c: 1, 
    rat: "Pride and Prejudice follows Elizabeth Bennet as she deals with issues of manners, upbringing, and marriage."
},
{ 
    cat: "English Major", 
    q: "What is the 'Monitor Model' in second language acquisition?", 
    a: ["A. Skinner's theory", "B. Krashen's theory", "C. Piaget's theory", "D. Vygotsky's theory"], 
    c: 1, 
    rat: "Stephen Krashen's model includes five hypotheses, such as the Input Hypothesis and the Affective Filter."
},
{ 
    cat: "English Major", 
    q: "Who is the author of 'To Kill a Mockingbird'?", 
    a: ["A. Harper Lee", "B. Truman Capote", "C. Flannery O'Connor", "D. Alice Walker"], 
    c: 0, 
    rat: "Harper Lee's Pulitzer Prize-winning novel deals with racial injustice and the loss of innocence in the American South."
},
{ 
    cat: "English Major", 
    q: "A 'Soliloquy' in a play is when:", 
    a: ["A. Two characters talk quietly", "B. A character speaks their thoughts aloud alone on stage", "C. The narrator speaks to the audience", "D. A character disguises their voice"], 
    c: 1, 
    rat: "Soliloquies (like Hamlet's 'To be or not to be') are used to reveal a character's inner state to the audience."
}


// --- SCIENCE MAJORSHIP ---
{ 
    cat: "Science Major", 
    q: "Which law explains why you slide to the side when a car turns quickly?", 
    a: ["A. Law of Acceleration", "B. Law of Interaction", "C. Law of Inertia", "D. Universal Gravitation"], 
    c: 2, 
    rat: "Law of Inertia (Newton's 1st Law) states your body wants to continue in a straight line even when the car turns."
}
];
{ 
    cat: "Science Major", 
    q: "Which law of thermodynamics states that energy cannot be created or destroyed?", 
    a: ["A. Zeroth Law", "B. First Law", "C. Second Law", "D. Third Law"], 
    c: 1, 
    rat: "The First Law of Thermodynamics (Law of Conservation of Energy) states that energy can only be transformed from one form to another."
},
{ 
    cat: "Science Major", 
    q: "What is the process by which a cell divides into two identical daughter cells?", 
    a: ["A. Meiosis", "B. Mitosis", "C. Fertilization", "D. Budding"], 
    c: 1, 
    rat: "Mitosis is for somatic (body) cell repair and growth, resulting in two cells with the same number of chromosomes."
},
{ 
    cat: "Science Major", 
    q: "Which element is the most abundant in the Earth's crust?", 
    a: ["A. Iron", "B. Silicon", "C. Oxygen", "D. Aluminum"], 
    c: 2, 
    rat: "Oxygen makes up nearly 46.6% of the Earth's crust by weight, usually found combined with other elements as silicates."
},
{ 
    cat: "Science Major", 
    q: "What is the acceleration due to gravity on Earth (standard value)?", 
    a: ["A. 8.9 m/s²", "B. 9.8 m/s²", "C. 10.5 m/s²", "D. 32 m/s²"], 
    c: 1, 
    rat: "The standard gravity is 9.80665 m/s². It represents the acceleration of an object in free fall near Earth's surface."
},
{ 
    cat: "Science Major", 
    q: "Which organelle is responsible for protein synthesis?", 
    a: ["A. Mitochondria", "B. Lysosome", "C. Ribosome", "D. Vacuole"], 
    c: 2, 
    rat: "Ribosomes translate genetic code from mRNA into amino acid chains to form proteins."
},
{ 
    cat: "Science Major", 
    q: "What is the chemical name for common household bleach?", 
    a: ["A. Sodium Chloride", "B. Sodium Hypochlorite", "C. Calcium Carbonate", "D. Sodium Bicarbonate"], 
    c: 1, 
    rat: "Sodium Hypochlorite (NaClO) is the active ingredient used for whitening and disinfection."
},
{ 
    cat: "Science Major", 
    q: "Which planet has the most moons in our solar system?", 
    a: ["A. Jupiter", "B. Saturn", "C. Uranus", "D. Neptune"], 
    c: 1, 
    rat: "As of recent astronomical counts, Saturn has overtaken Jupiter as the planet with the highest number of confirmed moons."
},
{ 
    cat: "Science Major", 
    q: "What type of bond is formed when atoms share electrons?", 
    a: ["A. Ionic Bond", "B. Covalent Bond", "C. Metallic Bond", "D. Hydrogen Bond"], 
    c: 1, 
    rat: "Covalent bonding involves the sharing of electron pairs between atoms, typically between non-metals."
},
{ 
    cat: "Science Major", 
    q: "Which part of the brain controls balance and coordination?", 
    a: ["A. Cerebrum", "B. Cerebellum", "C. Brainstem", "D. Thalamus"], 
    c: 1, 
    rat: "The cerebellum (little brain) coordinates voluntary movements such as posture, balance, and speech."
},
{ 
    cat: "Science Major", 
    q: "What is the SI unit of electric current?", 
    a: ["A. Volt", "B. Watt", "C. Ampere", "D. Ohm"], 
    c: 2, 
    rat: "The Ampere (A) measures the flow of electric charge per second through a conductor."
},
{ 
    cat: "Science Major", 
    q: "What is the most common gas in the Sun?", 
    a: ["A. Helium", "B. Hydrogen", "C. Oxygen", "D. Nitrogen"], 
    c: 1, 
    rat: "The Sun is roughly 73% hydrogen and 25% helium by mass."
},
{ 
    cat: "Science Major", 
    q: "Which layer of the Earth is liquid?", 
    a: ["A. Crust", "B. Mantle", "C. Outer Core", "D. Inner Core"], 
    c: 2, 
    rat: "The Outer Core is composed of liquid iron and nickel; its movement creates Earth's magnetic field."
},
{ 
    cat: "Science Major", 
    q: "What is the functional unit of the kidney?", 
    a: ["A. Neuron", "B. Nephron", "C. Alveoli", "D. Villus"], 
    c: 1, 
    rat: "Nephrons filter blood and remove waste products to form urine."
},
{ 
    cat: "Science Major", 
    q: "Which color of light has the shortest wavelength?", 
    a: ["A. Red", "B. Yellow", "C. Green", "D. Violet"], 
    c: 3, 
    rat: "In the visible spectrum, violet has the shortest wavelength (~400nm) and the highest energy."
},
{ 
    cat: "Science Major", 
    q: "Who is known as the Father of Genetics?", 
    a: ["A. Charles Darwin", "B. Gregor Mendel", "C. Louis Pasteur", "D. James Watson"], 
    c: 1, 
    rat: "Mendel's work with pea plants established the laws of inheritance."
},
{ 
    cat: "Science Major", 
    q: "What is the pH of a neutral solution at 25°C?", 
    a: ["A. 0", "B. 1", "C. 7", "D. 14"], 
    c: 2, 
    rat: "A pH of 7 indicates that the concentration of hydrogen ions equals the concentration of hydroxide ions."
},
{ 
    cat: "Science Major", 
    q: "Which rock type is formed from the cooling of lava or magma?", 
    a: ["A. Sedimentary", "B. Metamorphic", "C. Igneous", "D. Fossilized"], 
    c: 2, 
    rat: "Igneous rocks (like basalt or granite) crystallize from molten rock."
},
{ 
    cat: "Science Major", 
    q: "What is the main function of the large intestine?", 
    a: ["A. Digestion of fats", "B. Absorption of water", "C. Production of bile", "D. Nutrient absorption"], 
    c: 1, 
    rat: "While the small intestine absorbs nutrients, the large intestine primarily absorbs water and forms stool."
},
{ 
    cat: "Science Major", 
    q: "What is the constant value for the speed of light in a vacuum?", 
    a: ["A. 3.0 x 10⁸ m/s", "B. 1.5 x 10⁸ m/s", "C. 3.0 x 10⁶ m/s", "D. 9.8 m/s"], 
    c: 0, 
    rat: "Light travels at approximately 300,000 kilometers per second in a vacuum."
},
{ 
    cat: "Science Major", 
    q: "Which element is found in all organic compounds?", 
    a: ["A. Nitrogen", "B. Oxygen", "C. Carbon", "D. Sulfur"], 
    c: 2, 
    rat: "Organic chemistry is defined as the study of carbon-containing compounds."
},
{ 
    cat: "Science Major", 
    q: "What type of lens is used to correct nearsightedness (myopia)?", 
    a: ["A. Convex", "B. Concave", "C. Bifocal", "D. Cylindrical"], 
    c: 1, 
    rat: "A concave (diverging) lens spreads out light rays before they reach the eye, helping them focus on the retina."
},
{ 
    cat: "Science Major", 
    q: "What is the chemical formula for Ozone?", 
    a: ["A. O₂", "B. O₃", "C. CO₂", "D. H₂O₂"], 
    c: 1, 
    rat: "Ozone is a triatomic molecule consisting of three oxygen atoms."
},
{ 
    cat: "Science Major", 
    q: "Which blood vessel carries deoxygenated blood from the body to the heart?", 
    a: ["A. Aorta", "B. Pulmonary Vein", "C. Vena Cava", "D. Carotid Artery"], 
    c: 2, 
    rat: "The Superior and Inferior Vena Cava return oxygen-poor blood to the right atrium."
},
{ 
    cat: "Science Major", 
    q: "What is the study of fungi called?", 
    a: ["A. Botany", "B. Zoology", "C. Mycology", "D. Phycology"], 
    c: 2, 
    rat: "Mycology is the branch of biology concerned with the study of fungi, including mushrooms and yeasts."
},
{ 
    cat: "Science Major", 
    q: "What instrument is used to measure atmospheric pressure?", 
    a: ["A. Thermometer", "B. Anemometer", "C. Barometer", "D. Hygrometer"], 
    c: 2, 
    rat: "Barometers are used by meteorologists to forecast short-term changes in weather."
},
{ 
    cat: "Science Major", 
    q: "Which state of matter has the highest kinetic energy?", 
    a: ["A. Solid", "B. Liquid", "C. Gas", "D. Plasma"], 
    c: 3, 
    rat: "Plasma contains highly energized, ionized particles, giving it higher energy than gas."
},
{ 
    cat: "Science Major", 
    q: "What is the term for animals that eat both plants and meat?", 
    a: ["A. Herbivores", "B. Carnivores", "C. Omnivores", "D. Detritivores"], 
    c: 2, 
    rat: "Omnivores (like humans and bears) have a diverse diet consisting of various food sources."
},
{ 
    cat: "Science Major", 
    q: "Which scientist proposed the planetary model of the atom?", 
    a: ["A. John Dalton", "B. J.J. Thomson", "C. Ernest Rutherford", "D. Neils Bohr"], 
    c: 3, 
    rat: "Bohr proposed that electrons move in fixed circular orbits (energy levels) around the nucleus."
},
{ 
    cat: "Science Major", 
    q: "What is the primary gas responsible for the greenhouse effect?", 
    a: ["A. Nitrogen", "B. Oxygen", "C. Carbon Dioxide", "D. Argon"], 
    c: 2, 
    rat: "CO₂ traps heat in the atmosphere, leading to global warming."
},
{ 
    cat: "Science Major", 
    q: "Which vitamin is synthesized by the skin when exposed to sunlight?", 
    a: ["A. Vitamin A", "B. Vitamin C", "C. Vitamin D", "D. Vitamin K"], 
    c: 2, 
    rat: "Vitamin D is produced when UV rays from sunlight strike the skin and trigger synthesis."
},
{ 
    cat: "Science Major", 
    q: "What is the value of one mole of a substance (Avogadro's number)?", 
    a: ["A. 6.02 x 10²³", "B. 3.14 x 10¹⁰", "C. 9.8 x 10²", "D. 1.6 x 10⁻¹⁹"], 
    c: 0, 
    rat: "6.022 x 10²³ is the number of particles (atoms/molecules) in one mole."
},
{ 
    cat: "Science Major", 
    q: "What type of plate boundary occurs when two plates slide past each other?", 
    a: ["A. Convergent", "B. Divergent", "C. Transform", "D. Subduction"], 
    c: 2, 
    rat: "The San Andreas Fault is a famous example of a transform plate boundary."
},
{ 
    cat: "Science Major", 
    q: "Which acid is found in the human stomach?", 
    a: ["A. Sulfuric Acid", "B. Nitric Acid", "C. Hydrochloric Acid", "D. Acetic Acid"], 
    c: 2, 
    rat: "HCl helps break down food and kills bacteria in the digestive tract."
},
{ 
    cat: "Science Major", 
    q: "What is the hardest natural substance on Earth?", 
    a: ["A. Gold", "B. Iron", "C. Diamond", "D. Quartz"], 
    c: 2, 
    rat: "Diamond is a solid form of carbon with its atoms arranged in a crystal structure, making it extremely hard."
},
{ 
    cat: "Science Major", 
    q: "Which planet is known as the 'Morning Star'?", 
    a: ["A. Mercury", "B. Venus", "C. Mars", "D. Jupiter"], 
    c: 1, 
    rat: "Venus is the brightest object in the sky after the Sun and Moon, often visible at dawn or dusk."
},
{ 
    cat: "Science Major", 
    q: "What is the term for the movement of water across a semi-permeable membrane?", 
    a: ["A. Diffusion", "B. Osmosis", "C. Active Transport", "D. Filtration"], 
    c: 1, 
    rat: "Osmosis specifically refers to the movement of solvent (water) to equalize concentrations."
},
{ 
    cat: "Science Major", 
    q: "Which scientist discovered the electron?", 
    a: ["A. Ernest Rutherford", "B. Neils Bohr", "C. J.J. Thomson", "D. James Chadwick"], 
    c: 2, 
    rat: "Thomson discovered the electron using cathode ray tube experiments."
},
{ 
    cat: "Science Major", 
    q: "What is the boiling point of water at sea level in Celsius?", 
    a: ["A. 0°C", "B. 50°C", "C. 100°C", "D. 212°C"], 
    c: 2, 
    rat: "Under standard atmospheric pressure, water boils at exactly 100°C."
},
{ 
    cat: "Science Major", 
    q: "Which human organ can regenerate itself if part of it is removed?", 
    a: ["A. Heart", "B. Liver", "C. Lung", "D. Brain"], 
    c: 1, 
    rat: "The liver has a unique ability to grow back to its original size even after 75% of it is removed."
},
{ 
    cat: "Science Major", 
    q: "What type of energy is stored in a compressed spring?", 
    a: ["A. Kinetic Energy", "B. Elastic Potential Energy", "C. Chemical Energy", "D. Thermal Energy"], 
    c: 1, 
    rat: "Potential energy is stored due to the deformation of an elastic object."
},
{ 
    cat: "Science Major", 
    q: "Which endocrine gland is known as the 'Master Gland'?", 
    a: ["A. Thyroid", "B. Adrenal", "C. Pituitary", "D. Pancreas"], 
    c: 2, 
    rat: "The pituitary gland produces hormones that control many other endocrine glands."
},
{ 
    cat: "Science Major", 
    q: "What is the name of the process where gas turns directly into a solid?", 
    a: ["A. Sublimation", "B. Deposition", "C. Condensation", "D. Freezing"], 
    c: 1, 
    rat: "Deposition is the reverse of sublimation (e.g., frost forming on a window)."
},
{ 
    cat: "Science Major", 
    q: "Which color has the longest wavelength in the visible spectrum?", 
    a: ["A. Red", "B. Blue", "C. Yellow", "D. Green"], 
    c: 0, 
    rat: "Red light has wavelengths around 700nm and the lowest frequency in the visible range."
},
{ 
    cat: "Science Major", 
    q: "What is the most common element in the universe?", 
    a: ["A. Helium", "B. Oxygen", "C. Carbon", "D. Hydrogen"], 
    c: 3, 
    rat: "Hydrogen accounts for about 75% of the baryonic mass of the universe."
},
{ 
    cat: "Science Major", 
    q: "What is the speed of sound in air (approximate)?", 
    a: ["A. 343 m/s", "B. 3.0 x 10⁸ m/s", "C. 1,500 m/s", "D. 9.8 m/s"], 
    c: 0, 
    rat: "Sound travels at about 343 meters per second in dry air at 20°C."
},
{ 
    cat: "Science Major", 
    q: "Which scientist formulated the Three Laws of Motion?", 
    a: ["A. Galileo", "B. Newton", "C. Einstein", "D. Kepler"], 
    c: 1, 
    rat: "Sir Isaac Newton's laws describe the relationship between a body and the forces acting upon it."
},
{ 
    cat: "Science Major", 
    q: "What is the functional unit of the nervous system?", 
    a: ["A. Nephron", "B. Neuron", "C. Alveoli", "D. Sarcomere"], 
    c: 1, 
    rat: "Neurons are specialized cells that transmit nerve impulses."
},
{ 
    cat: "Science Major", 
    q: "Which celestial body is at the center of the Milky Way galaxy?", 
    a: ["A. The Sun", "B. A Supermassive Black Hole", "C. Polaris", "D. Alpha Centauri"], 
    c: 1, 
    rat: "Sagittarius A* is the supermassive black hole located at the Galactic Center."
},
{ 
    cat: "Science Major", 
    q: "What is the noble gas used in balloons to make them float?", 
    a: ["A. Argon", "B. Neon", "C. Helium", "D. Krypton"], 
    c: 2, 
    rat: "Helium is lighter than air (less dense), causing balloons to rise."
}

    let current = 0;
    let timeLeft = 30;
    let timerInterval;

    function startTimer() {
        clearInterval(timerInterval);
        timeLeft = 30;
        updateTimerUI();
        timerInterval = setInterval(() => {
            timeLeft--;
            updateTimerUI();
            if (timeLeft <= 0) {
                clearInterval(timerInterval);
                document.getElementById('timer-text').innerText = "TIME'S UP!";
            }
        }, 1000);
    }

    function updateTimerUI() {
        document.getElementById('timer-text').innerText = timeLeft + "s";
        const width = (timeLeft / 30) * 100;
        document.getElementById('timer-bar').style.width = width + "%";
        document.getElementById('timer-bar').style.background = timeLeft < 10 ? "#ef4444" : "#22c55e";
    }

    function render() {
        const item = quizData[current];
        document.getElementById('cat').innerText = item.cat;
        document.getElementById('count').innerText = `Question ${current + 1} of ${quizData.length}`;
        document.getElementById('q-text').innerText = item.q;
        document.getElementById('rat-text').innerText = item.rat;
        document.getElementById('rat-box').style.display = 'none';
        
        const opts = document.getElementById('options');
        opts.innerHTML = '';
        item.a.forEach((text, i) => {
            opts.innerHTML += `<div class="opt" id="opt-${i}">${text}</div>`;
        });
        startTimer();
    }
    function checkAns(idx) {
    if(answeredInThisTurn) return; 
    answeredInThisTurn = true; 
    clearInterval(timerInterval);

    const correctIdx = quizData[current].c;
    if(idx === correctIdx) {
        userScore++; 
        catScore++; // Add point to both total and category scores
        document.getElementById(`opt-${idx}`).classList.add('correct');
    } else {
        document.getElementById(`opt-${idx}`).classList.add('wrong');
        document.getElementById(`opt-${correctIdx}`).classList.add('correct');
    }
    function move(step) {
    let nextIdx = current + step;
    answeredInThisTurn = false;

    // Check if the NEXT question is a different category
    if (nextIdx < quizData.length && nextIdx >= 0) {
        if (quizData[nextIdx].cat !== quizData[current].cat) {
            showCategoryResults(); // Show intermediate score
            return;
        }
    }

    if (nextIdx >= quizData.length) {
        showFinalResults();
        return;
    }
    
    current = nextIdx;
    render();
}

    <!-- Category Break Screen -->
<div id="cat-screen" style="display:none; text-align:center; padding: 50px;">
    <h1 style="font-size: 4rem; color: var(--accent);">CATEGORY FINISHED!</h1>
    <h2 id="cat-finished-name" style="font-size: 3rem; color: #fff;">MATH MAJORSHIP</h2>
    <p style="font-size: 3rem; margin: 20px 0;">Subject Score: <span id="cat-score" style="color: var(--correct);">0</span> / 50</p>
    <p id="cat-feedback" style="font-size: 2rem; color: #cbd5e1;"></p>
    <button onclick="startNextCategory()" style="background: var(--correct); margin-top: 40px; padding: 20px 50px; font-size: 2rem; color: #000;">CONTINUE TO NEXT SUBJECT</button>
</div>

    function reveal() {
        clearInterval(timerInterval);
        document.getElementById(`opt-${quizData[current].c}`).classList.add('correct');
    }

    function showRat() {
        document.getElementById('rat-box').style.display = 'block';
    }

    function move(step) {
        current = (current + step + quizData.length) % quizData.length;
        render();
    let catScore = 0; // Tracks score for the current category only

}

function showCategoryResults() {
    document.getElementById('quiz-ui').style.display = 'none';
    document.getElementById('cat-screen').style.display = 'block';
    
    document.getElementById('cat-finished-name').innerText = quizData[current].cat;
    document.getElementById('cat-score').innerText = catScore;
    
    // Simple feedback based on the 50 items
    const feedback = document.getElementById('cat-feedback');
    if (catScore >= 40) feedback.innerText = "Excellent mastery of this subject!";
    else if (catScore >= 35) feedback.innerText = "Good job! You passed this section.";
    else feedback.innerText = "You might need more focus on this area.";
}

function startNextCategory() {
    catScore = 0; // Reset category counter for the new subject
    document.getElementById('cat-screen').style.display = 'none';
    document.getElementById('quiz-ui').style.display = 'block';
    current++;
    render();
}
    

    render();
</script>
</body>
</html>
