const express = require('express');
const multer = require('multer');
const fs = require('fs');
const Tesseract = require('tesseract.js');
const app = express();

// Set the view engine to EJS
app.set('view engine', 'ejs');

// Serve static files from the 'public' directory
app.use(express.static(__dirname + '/public'));

// Middleware for parsing URL-encoded and JSON request bodies
app.use(express.urlencoded({ extended: true }));
app.use(express.json());

// Define multer storage for file uploads
const Storage = multer.diskStorage({
  destination: (req, file, callback) => {
    callback(null, __dirname + '/images');
  },
  filename: (req, file, callback) => {
    callback(null, file.originalname);
  },
});

// Create a multer instance for handling file uploads
const upload = multer({
  storage: Storage,
}).single('image');

// Define routes
app.get('/', (req, res) => {
  res.render('pages/index');
});

app.post('/upload', (req, res) => {
  upload(req, res, (err) => {
    if (err) {
      console.log(err);
      return res.send('Something went wrong');
    }

    const image = fs.readFileSync(__dirname + '/images/' + req.file.originalname, {
      encoding: null,
    });

    Tesseract.recognize(image)
      .progress(function (p) {
        console.log('progress', p);
      })
      .then(function (result) {
        res.send(result.html);
      });
  });
});

app.get('/ttslearning', (req, res) => {
  res.render('pages/ttslearning');
});

app.get('/Brailleedit', (req, res) => { 
  res.render('pages/Brailleedit'); 
});

app.get('/braille', (req, res) => { 
  res.render('pages/braille'); 
});

app.get('/optical', (req, res) => {
  res.render('pages/optical');
});

app.get('/lang', (req, res) => {
  res.render('pages/lang');
});

app.get('/showdata', (req, res) => {
  // Handle your 'showdata' route logic here
});

// Initiate the server on port 80
const PORT = process.env.PORT || 80;
app.listen(PORT, () => {
  console.log(`Server running on Port ${PORT}`);
});


{
	"optOut": false,
	"lastUpdateCheck": 1694759594872
}


nav {
	background-color: var(--primaryDark);
	color: var(--white);
	height: auto;
	padding: 10px;
	/* width: 100%;
	display: flex;
	justify-content: end; */
	position: relative;
	z-index: 1000;
}

.tempLogo{
  font-size: 1.2em;
  font-weight: bold;
}

nav .navWrap {
	display: flex;
	justify-content: space-between;
	padding: 1px 3px;
	align-items: center;
}

nav .navLinksWrap {
	display: flex;
	flex-direction: column;
	width: 100%;
	padding: 1px 3px;
}

nav .navLinksWrap ul {
	display: flex;
	flex-direction: column;
	gap: 4px;
	padding: 2px 0px;
}

nav .navLinksWrap ul * {
	list-style-type: none;
	text-decoration: none;
	color: var(--white);
	cursor: pointer;
	font-size: 17px;
	padding: 6px 8px;
	border-radius: 7px;
	transition: all 200ms ease-out;
	display: flex;
	white-space: nowrap;
}

nav .navLinksWrap ul li a {
	display: flex;
	width: 100%;
}

nav .navLinksWrap ul li:hover, nav .navLinksWrap ul li:active {
	/* transition: all 200ms ease-out; */
	background-color: var(--primary);
}

/* hamburger menu */

.hamburger {
	display: flex;
	width: max-content;
	height: auto;
	justify-content: center;
	align-items: center;
	border: none;
	background-color: transparent;
	cursor: pointer;
	padding: 7px;
	border-radius: 7px;
	transition: all 350ms ease-out;
}

.hamburger:hover, .hamburger:active {
	transition: all 200ms ease-out;
	background-color: var(--primary);
}

@media only screen and (min-width: 480px) {
	nav {
		padding: 10px 20px;
		display: flex;
		flex-direction: row;
		width: 100%;
	}
	nav .navLinksWrap ul {
		flex-direction: row;
		justify-content: flex-end;
		display: flex !important;
		gap: 8px;
	}
	nav .navLinksWrap ul *:hover, nav .navLinksWrap ul *:active {
		background-color: inherit !important;
	}
	.hamburger {display: none;}
}



@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');

*, *::before, *::after {
	box-sizing: border-box;
	margin: 0;
	padding: 0;
  scroll-behavior: smooth;
}

:root {
	--primary:  rgb(207, 170, 212);
	--primaryLight: #ddafeedc;
	--primaryDark: rgb(150, 84, 174);
  
	/* --secondary: ; */
  --text:#343A40;
  --textLight:#6C7686;
  --white: #ffffff;
  --shadow: 0 0 1.25rem rgb(108 118 134 / 10%) ;
}

html, body {
	font-family: 'Poppins', sans-serif;
	font-weight: 400;
	width: 100vw;
	min-height: 100vh;
  background: #ffffff;
  overflow-x: hidden;
	position: relative;
}

/* ________hero_________ */

.hero {
	margin: 5em auto;
}

.hero .heroWrap {
	position: relative;
	z-index: 100;
	display: flex;
  flex-direction: column;
	justify-content: center;
  gap:3em;
}

.hero .topItem, .topItemLarge {
	position: absolute;
	top: 0;
	left: 0;
  width: 90%;
	max-width: 400px;
	z-index: 0;
}

.topItemLarge {
	max-width: 660px !important;
}

.hero .heroWrap article {
	max-width: 550px;
	padding: 8px 34px;
}

.hero .heroWrap article h1, .cam h1, .vrHeader h1, .speechRecHeader h1 {
	font-size: 2.5rem;
	font-weight: bold;
	line-height: 4rem;
}

.hero .heroWrap article h1 .headerSecondaryText {
	color: var(--primaryDark);
}

.hero .heroWrap article p {
	font-size: 18px;
	padding: 1em 0px 2em 0;
	font-weight: 200;
}

.hero .heroWrap .art {
	max-width: 700px;
  width: 90%;
}

/* ________cards________ */

.features {
	width: 100%;
	padding: 0 15px;
	text-align: center;
  position: relative;
	z-index: 50;
}

.heading{
	margin: 4em auto;
}

.heading p {margin-bottom: 1rem;}

.cards{
  max-width: 800px;
	display:flex;
	justify-content:center;
  align-items: stretch;
	flex-wrap:wrap;
	gap: 3em;
  margin: 0 auto;
}

.cards  img{
	width: 80px;
	border-radius: 100%;
	margin:0 auto;
	margin-bottom: 1.5em;
}

.cards h3 {
  max-width: 200px;
  margin: 0 auto;
  font-size: 1.2rem;
  color: var(--primaryDark);
}

.card {
	width: 90%;
	max-width: 350px;
  display: flex;
  flex-direction: column;
	background:#fff;
  border: .0625rem solid rgba(0, 0, 0, .08);
  border-radius: .375rem;
	box-shadow:var(--shadow);
	gap: .7em;
	padding:2em;
  z-index: 2;
}


.cards a{
  display: contents;
  left:0px;
}

/* _____dots_____ */
.dots{position: absolute;}

.features .dots{
  left: 22%;
  top: 8%;
}
.arCardsWrap .dots{
  left: 5%;
  top: 0%;
}

/* ________arCards________ */

.arCardsWrap {
	position: relative;
	display: flex;
	flex-direction: column;
	flex-wrap: wrap;
	align-items: center;
	justify-content: center;
	gap: 14px;
	padding: 20px 0px;
}

.arCards {
	max-width: 400px;
	width: 80%;
	height: 600px;
	text-align: center;
	background-color: var(--white);
	box-shadow: var(--shadow);
	border-radius: .375rem;
  border: .0625rem solid rgba(0, 0, 0, .08);
	position: relative;
	z-index: 300;
	padding: 0px 0px 12px 0px;
}

.arCards article {
	padding: 24px 0px;
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
}

.arCards article h3 {
	font-size: 1.8em;
	font-weight: bold;
}

.arCards article p {
	padding: 14px 0px;
	font-size: 15px;
	max-width: 200px;
	text-align: center !important;
}

.arCards .arCardsAWrap {
	display: flex;
	justify-content: center;
	align-items: flex-end;
	width: 100%;
}

.arCards .arCardsAWrap a {
	position: absolute;
	bottom: 22px;
}

/* .arCards .arCardsAWrap {
	display: flex;
	align-items: center;
	justify-content: center;
} */

/* .arCardWrap {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100%;
	background-color: #0000;
} */

.arCards .imgContainer {
	/* width: 100% !important; */
	height: 200px;
	background: #000;
	border-top-left-radius: .375rem;
	border-top-right-radius: .375rem;
}

.arCards .arCard1 {
	background: url('../media/skeleton.png') !important;
	background-size: contain !important;
	background-repeat: no-repeat !important;
	background-position: center right !important;
	margin: 0 auto;
}

.arCards .arCard2 {
	background: url('../media/brain.png') !important;
	background-size: contain !important;
	background-repeat: no-repeat !important;
	background-position: center center !important;
	margin: 0 auto;
}

.arCards .arCard3 {
	background: url('../media/skull.png') !important;
	background-size: contain !important;
	background-repeat: no-repeat !important;
	background-position: center center !important;
	margin: 0 auto;
}

.arCards .arCard4 {
	background: url('../media/lungs.png') !important;
	background-size: contain !important;
	background-repeat: no-repeat !important;
	background-position: center center !important;
	margin: 0 auto;
}

.arCards .arCard5 {
	background: url('../media/elephant.png') !important;
	background-size: cover !important;
	background-repeat: no-repeat !important;
	background-position: center center;
	margin: 0 auto;
}

.arCards .arCard6 {
	background: url('../media/articFox.png') !important;
	background-size: cover !important;
	background-repeat: no-repeat !important;
	background-position: center center;
	margin: 0 auto;
}

/* ________Cam________ */

.cam {
	padding: 40px 0px;
	z-index: 3000;
	position: relative;
	display: flex !important;
	gap: 22px;
	flex-direction: column;
	justify-content: center;
	align-items: center;
	max-width: 420px;
	margin: 0 auto;
}

.camDisplay {
	background-color: var(--primary);
	width: 100%;
	height: 240px;
	display: flex;
	align-items: center;
	justify-content: center;
	overflow: hidden;
}

.camDisplay > * {
	width: 80%;
	max-width: 380px; 
}

.labelContainer {
	display: flex;
	width: 100%;
	justify-content: start;
	flex-direction: column;
	gap: 2px;
}



/*________SpeechRec________  */

.speechRecContainer {
	position: relative;
	z-index: 400;
	padding: 14px;
	display: flex;
	flex-direction: column;
	justify-content: center;
	align-items: center;
}

.speechRecContainer .app {
	width: 100%;
}

.speechRecContainer .app .input-single {
	width: 100%;
	display: flex;
	justify-content: center;
	align-items: center;
	padding: 12px 0px;
}

.speechRecContainer .app .input-single textarea {
	width: 100%;
	padding: 6px 10px;
	font-size: 15px;
	border: 1px solid rgba(0, 0, 0, .08);
	border-radius: .375rem;
	box-shadow: var(--shadow);
	font-family: 'Poppins', 'sans-serif';
}

.speechRecContainer .app .input-single textarea:focus {
	outline: none !important;
	border: 1px solid var(--primary);
}

.speechRecContainer .app .speechRecBtnsWrap {
	display: flex;
	flex-direction: column;
	gap: 12px;
	padding: 4px 0px;
}

.speechRecContainer .app .speechRecBtnsWrap button {
	width: max-content !important;
}

.speechRecContainer .app .speechRecRecogBtnsWrap {
	display: flex;
	flex-direction: row;
	gap: 12px;
	flex-wrap: wrap;
}

.speechRecContainer .app p {
	padding: 18px 0px;
	font-size: 18px;
}

.speechRecContainer .app .notes {
	padding: 14px;
	background-color: var(--primary);
	color: var(--white);
	width: 100%;
	border-radius: .375rem;
	overflow: auto;
	height: 600px;
}

.speechRecContainer .app .notes h3 {
	font-size: 2em;
	font-weight: bold;
	padding: 10px 0px;
}

.speechRecContainer .app .notes ul li {
	list-style-type: none;
	margin-bottom: 2px;
	width: 100%;
	border-radius: .375rem;
	padding: 6px 8px;
	transition: 200ms ease-in-out;
	position: relative;
}

.speechRecContainer .app .notes ul li:hover, .speechRecContainer .app ul li:active {
	background-color: var(--primaryLight);
	transition: 200ms ease-in-out;
}

.speechRecContainer .app .notes ul li p {
	display: flex;
	width: 70%;
}

.speechRecContainer .app ul li .noteBtnWrap {
	display: flex;
	width: 100%;
	flex-direction: row;
	justify-content: center;
	gap: 20px;
	flex-wrap: wrap;
	/* padding: 8px 0px; */
}

.speechRecContainer .app ul li .noteBtnWrap * {
	display: flex;
	justify-content: center;
	font-size: 13px;
	white-space: nowrap;
}

/* ______Slider_______ */

.carouselContainer, .carouselContainer2 {
	display: flex;
	padding: 28px 0px;
	justify-content: center;
	align-items: center;
	flex-direction: column;
	gap: 40px;
	position: relative;
	z-index: 200;
}

.carouselContainer h2, .carouselContainer2 h2, .heading h2 {
	text-align: center;
	font-weight: bold;
	font-size: 2.5em;
}


.carouselContainer p, .carouselContainer2 p {
	text-align: center;
	font-weight: 200;
	font-size: 15px;
	max-width: 420px;
	padding: 1.3rem 0;
}

.nslider {
  max-width: 600px;
	width: 100%;
	height: 560px;
	position: relative;
	z-index: 500;
	padding: 8em 0px;
}

.nslider .nslider-dots .nslider-dot {
	background-color: var(--primaryDark) !important;
}

.nslider-button-prev, .nslider-button-next {
	display: none !important;
}

.galleryCard {
	background-color: var(--white);
	margin: 0 auto;
	width: 100%;
	max-width: 380px;
	height: 460px;
	color: var(--text);
	border: .0625rem solid rgba(0, 0, 0, .08);
  border-radius: .375rem;
	box-shadow:var(--shadow);
	overflow-y: auto;
	overflow-x: hidden;
	position: relative;
}

.galleryCard img {
	max-width: 380px;
	width: 100%;
}

.galleryCard article {
	padding: 2em;
}

.galleryCard article h2 {
	text-align: center;
	font-weight: bolder;
	font-size: 2em !important;
}

.galleryCard article p {
	font-size: 15px;
	padding: 40px 0;
}

/*  */

.bottomItem {
	position: absolute;
	bottom: 0;
	right: 0;
	width: 80%;
	max-width: 400px;
	z-index: 0;
}

.bottomItemBig {
	position: absolute;
	bottom: 0;
	left: 0;
	width: 90%;
	max-width: 540px;
	z-index: 0;
}

.ellipse, .ellipseTwo {
	position: absolute;
	right: 0;
	top: 50%;
	z-index: 0;
	width: 70%;
	max-width: 220px !important;
}

.ellipseTwo {
	top: 35%;
}

.ellipseLeft {
	position: absolute;
	left: 0;
	bottom: 15%;
	z-index: 0;
	width: 70%;
	max-width: 200px !important;
}


.okWrap, .arImgWrap {
	display: flex;
	justify-content: center;
	align-items: center;
	margin-top: 80px !important;
}

.ok {
	width: 80%;
	max-width: 400px;
	position: relative;
	z-index: 300;
}

.logo {
	width: 70px;
	cursor: pointer;
	margin-left: 16px;
}

.arImg {
	margin: 0 auto;
	margin-top: 20px !important;
	width: 80%;
	max-width: 1000px;
}

.buttonPrimary {
	background-color: var(--primaryDark);
	color: var(--white);
	font-size: 16px;
	padding: 4px 20px;
	border-radius: 3.5px;
	cursor: pointer;
	border: none;
	outline: none;
	transition: 200ms ease-in-out;
}

.buttonPrimary:hover {
	background-color: var(--primary);
	transition: 200ms ease-in-out;
}

.homeText {
	/* max-width: 400px; */
	text-align: center;
	gap: 12px;
	padding: 60px 0px;
	display: flex;
	justify-items: center;
	align-items: center;
	flex-direction: column;
	position: relative;
	z-index: 300;
}

.homeText h2 {
	text-align: center;
	font-weight: bold;
	font-size: 2.5em;
}

.homeText p {
	max-width: 650px;
	width: 80%;
	font-size: 14px;
	font-weight: 200;
}

.vrHeader, .speechRecHeader {
	position: relative;
	z-index: 300;
	text-align: center;
	padding: 60px 0px;
	display: flex;
	justify-content: center;
	align-items: center;
	flex-direction: column;
}

.vrHeader p, .speechRecHeader p {
	max-width: 900px;
  padding: 1em 2em;
}

/* ________footer________ */

.footer {
	/* position: absolute;
	bottom: 0; */
	margin-top: 20px;
	background: var(--primaryDark);
	padding: 20px 40px;
	width: 100%;
	position: relative;
	z-index: 900;
  display: inline-flex;
  justify-content: space-between;
  color: var(--white);
}

.footer a{
  color:#fafbfe;
  width:50%;
  padding: 0 .2em;
}
/* mobile first design :lilygunemote: */

@media only screen and (min-width: 480px) {
	nav {
		padding: 10px 20px;
		display: flex;
		flex-direction: row;
		width: 100%;
	}
	nav .navLinksWrap ul {
		flex-direction: row;
		justify-content: flex-end;
		display: flex !important;
		gap: 8px;
	}
	nav .navLinksWrap ul *:hover, nav .navLinksWrap ul *:active {
		background-color: inherit;
	}
	.hamburger {display: none;}
	/* .hero  */
	.cam {
		padding: 80px 0px;
	}
	.ok {
		max-width: 480px;
	}
	.speechRecContainer .app {
		width: 88%;
	}
	.speechRecContainer .app ul li .noteBtnWrap {
		gap: 50px !important;
	}
	.speechRecContainer .app ul li .noteBtnWrap {
		font-size: 18px !important;
		width: 100% !important;
	}
	.nslider {
		width: 70% !important;
	}
	.galleryCard article h2 {
		font-size: 2.2em;
		font-weight: bold;
	}
}

@media only screen and (min-width: 620px) {
	.hero .heroWrap {
    flex-wrap: wrap;
    flex-direction: row;
    justify-content: space-evenly;
	}
	.cam {
		max-width: 560px;
	}
	.camDisplay {
		height: 300px;
	}
	.ok {
		max-width: 600px;
	}
	.homeText p {
		font-size: 18px;
	}
	.arCardsWrap {
		flex-direction: row;
		justify-content: center;
		gap: 34px;
    max-width: 1000px;
    margin: auto;
	}
	.arCards {
		max-width: 300px;
	}
	.arCards article p {
		font-size: 18px;
	}
	.speechRecBtnsWrap .app {
		width: 60% !important;
	}
  .arCardsWrap .dots{
  left: -3%;
  top: -2%;
  }
}

a {
  color: black;
  text-decoration: none;
}

                  <!DOCTYPE html>
                  <html lang="en">
                  <head>
                      <meta charset="UTF-8">
                      <meta name="viewport" content="width=device-width, initial-scale=1.0">
                      <title>Braille Editor - DotCompanion</title>
                      <link rel="stylesheet" href="/styles/style.css" type="text/css" />
                      <link rel="stylesheet" href="/styles/nav.css" type="text/css" />
                      <script src="https://kit.fontawesome.com/43c1b0f304.js" crossorigin="anonymous"></script>

                      <style>
                          body {
                              font-family: Arial, sans-serif;
                          }

                          /* Styles for the Text to Braille Converter */
                          .converter-container {
                              text-align: center;
                              display: flex;
                              flex-direction: column;
                              justify-content: center;
                              align-items: center;
                              height: 100vh; /* Center vertically */
                          }

                          .buttonPrimary {
                              background-color: var(--primaryDark);
                              color: var(--white);
                              font-size: 16px;
                              padding: 8px 20px;
                              border-radius: 3.5px;
                              cursor: pointer;
                              border: none;
                              outline: none;
                              transition: 200ms ease-in-out;
                          }

                          .input-container {
                              margin: 20px;
                          }

                          label {
                              font-weight: bold;
                          }

                          input {
                              padding: 5px;
                              margin-right: 10px;
                          }

                          /* Style for the button group */
                          .button-group {
                              display: flex;
                              gap: 10px; /* Adjust the gap as needed */
                          }

                          /* Style for the individual buttons in the group */
                          .button-group .buttonPrimary {
                              padding: 5px 10px;
                          }

                          .braille-output {
                              margin-top: 20px;
                              font-size: 36px; /* Increase font size for larger Braille dots */
                          }
                      </style>
                    
                  </head>
                  <body>
                      <header>
                          <%# Import nav %>
                          <%- include('../partials/nav') %>
                      </header>

                      <main class="converter-container">
                          <img src="/media/topItem.svg" class="topItemLarge" />
                          <img src="/media/ellipse.svg" class="ellipse" />

                          <h1>Braille to Text Converter</h1>
                          <div class="input-container">
                              <label for="braille-input">Enter Braille code:</label>
                              <input type="text" id="braille-input" placeholder="Enter Braille code">
                              <button id="convert-button" class="buttonPrimary" onclick="convertBraille()">Convert</button>
                          </div>
                          <div class="braille-output" id="braille-output"></div>

                          <div class="button-group">
                              <button id="copy-button" class="buttonPrimary">Copy</button>
                              <button id="share-button" class="buttonPrimary">Share</button>
                          </div>
                      </main>

                            <script>
                                function convertBraille() {
                                    const brailleInput = document.getElementById('braille-input').value;
                                    const textResult = brailleToText(brailleInput);
                                    document.getElementById('braille-output').textContent = `Text: ${textResult}`;
                                }

                                function brailleToText(braille) {
                                    const brailleMap = {
                                        "⠃": "a",
                                        "⠉": "b",
                                        "⠙": "c",
                                        "⠑": "d",
                                        "⠋": "e",
                                        "⠛": "f",
                                        "⠓": "g",
                                        "⠊": "h",
                                        "⠚": "i",
                                        "⠅": "j",
                                        "⠕": "k",
                                        "⠽": "l",
                                        "⠇": "m",
                                        "⠳": "n",
                                        "⠞": "o",
                                        "⠥": "p",
                                        "⠚": "q",
                                        "⠮": "r",
                                        "⠧": "s",
                                        "⠤": "t",
                                        "⠜": "u",
                                        "⠻": "v",
                                        "⠳": "w",
                                        "⠾": "x",
                                        "⠌": "y",
                                        "⠱": "z",
                                        // Add more mappings as needed
                                    };

                                    return braille.split(' ').map(code => brailleMap[code] || code).join('');
                                }
                            </script>


                      <footer>
                          <%- include('../partials/footer') %>
                      </footer>
                  </body>
                  </html>


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Braille Editor - DotCompanion</title>
    <link rel="stylesheet" href="/styles/style.css" type="text/css" />
    <link rel="stylesheet" href="/styles/nav.css" type="text/css" />
    <script src="https://kit.fontawesome.com/43c1b0f304.js" crossOrigin="anonymous"></script>

    <style>
        body {
            font-family: Arial, sans-serif;
        }

        /* Styles for the Text to Braille Converter */
        .converter-container {
            text-align: center;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100vh; /* Center vertically */
        }

        .buttonPrimary {
            background-color: var(--primaryDark);
            color: var(--white);
            font-size: 16px;
            padding: 8px 20px;
            border-radius: 3.5px;
            cursor: pointer;
            border: none;
            outline: none;
            transition: 200ms ease-in-out;
        }

        .input-container {
            margin: 20px;
        }

        label {
            font-weight: bold;
        }

        input {
            padding: 5px;
            margin-right: 10px;
        }

        /* Style for the button group */
        .button-group {
            display: flex;
            gap: 10px; /* Adjust the gap as needed */
        }

        /* Style for the individual buttons in the group */
        .button-group .buttonPrimary {
            padding: 5px 10px;
        }

        .braille-output {
            margin-top: 20px;
            font-size: 36px; /* Increase font size for larger Braille dots */
        }
    </style>
</head>
<body>
    <header>
        <%# Import nav %>
        <%- include('../partials/nav') %>
    </header>

    <main class="converter-container">
        <!-- Absolute SVGs that are displayed on the page -->
        <img src="/media/topItem.svg" class="topItemLarge" />
        <img src="/media/ellipse.svg" class="ellipse" />

        <h1>Text to Braille Converter</h1>
        <div class="input-container">
            <label for="text-input">Enter text:</label>
            <input type="text" id="text-input" placeholder="Enter text">
            <button id="convert-button" class="buttonPrimary">Convert</button>
        </div>
        <div class="braille-output" id="braille-output"></div>

        <!-- Button group for "Copy" and "Share" buttons -->
        <div class="button-group">
            <button id="copy-button" class="buttonPrimary">Copy</button>
            <button id="share-button" class="buttonPrimary">Share</button>
        </div>
    </main>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            const textInput = document.getElementById('text-input');
            const convertButton = document.getElementById('convert-button');
            const brailleOutput = document.getElementById('braille-output');
            const copyButton = document.getElementById('copy-button');
            const shareButton = document.getElementById('share-button');

            convertButton.addEventListener('click', function () {
                const text = textInput.value;
                const brailleText = convertToBraille(text);
                brailleOutput.textContent = brailleText;
            });

            copyButton.addEventListener('click', function () {
                const brailleText = brailleOutput.textContent;
                const textarea = document.createElement('textarea');
                textarea.value = brailleText;
                document.body.appendChild(textarea);
                textarea.select();
                document.execCommand('copy');
                document.body.removeChild(textarea);
                alert('Braille text copied to clipboard.');
            });

            shareButton.addEventListener('click', function () {
                const brailleText = brailleOutput.textContent;

                if (navigator.share) {
                    navigator.share({
                        title: 'Shared Braille Text',
                        text: brailleText,
                    }).then(() => {
                        console.log('Braille text shared successfully.');
                    }).catch((error) => {
                        console.error('Error sharing Braille text:', error);
                    });
                } else {
                    alert('Sharing is not supported in this browser.');
                }
            });

            function convertToBraille(text) {
                const brailleMap = {
                    'a': '⠁', 'b': '⠃', 'c': '⠉', 'd': '⠙', 'e': '⠑',
                    'f': '⠋', 'g': '⠛', 'h': '⠓', 'i': '⠊', 'j': '⠚',
                    'k': '⠅', 'l': '⠇', 'm': '⠍', 'n': '⠝', 'o': '⠕',
                    'p': '⠏', 'q': '⠟', 'r': '⠗', 's': '⠎', 't': '⠞',
                    'u': '⠥', 'v': '⠧', 'w': '⠺', 'x': '⠭', 'y': '⠽', 'z': '⠵',
                    'A': '⠁', 'B': '⠃', 'C': '⠉', 'D': '⠙', 'E': '⠑',
                    'F': '⠋', 'G': '⠛', 'H': '⠓', 'I': '⠊', 'J': '⠚',
                    'K': '⠅', 'L': '⠇', 'M': '⠍', 'N': '⠝', 'O': '⠕',
                    'P': '⠏', 'Q': '⠟', 'R': '⠗', 'S': '⠎', 'T': '⠞',
                    'U': '⠥', 'V': '⠧', 'W': '⠺', 'X': '⠭', 'Y': '⠽', 'Z': '⠵',
                    ' ': ' ',
                };

                return text.split('').map(char => {
                    if (brailleMap[char]) {
                        return brailleMap[char];
                    } else {
                        return char;
                    }
                }).join('');
            }
        });
    </script>
          <%# Import footer %>
		<footer>
			<%- include('../partials/footer') %>
		</footer>
</body>
</html>



<!DOCTYPE html>
<html lang="en">
	<%# Head %>
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, 	initial-scale=1.0">
		<title>Home - DotCompanion</title>
		<link rel="stylesheet" href="/styles/style.css" type="text/css" />
		<link rel="stylesheet" href="/styles/nav.css" type="text/css" />
    <script src="https://kit.fontawesome.com/43c1b0f304.js" crossOrigin="anonymous"></script>
	</head>
	<%# End of head %>

	<body>
		<%# Import Navbar from partials (EJS equivalent to React components) %>
		<header>
			<%- include('../partials/nav') %>
		</header>

		<%# Main Content %>
		<main>
			<%# Import Hero %>
			<%- include('../partials/hero') %>
			<%# Absolute SVG that is displayed on page %>
			
			
		</main>
		<%# End of main content %>

		<%# Import Footer %>
		<footer>
			<%- include('../partials/footer') %>
		</footer>
	</body>
</html>



<!DOCTYPE html>
<html lang="en">
	<%# Head %>
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, 	initial-scale=1.0">
		<title>Home - DotCompanion</title>
		<link rel="stylesheet" href="/styles/style.css" type="text/css" />
		<link rel="stylesheet" href="/styles/nav.css" type="text/css" />
    <script src="https://kit.fontawesome.com/43c1b0f304.js" crossOrigin="anonymous"></script>
	</head>
	<%# End of head %>

	<body>
		<%# Import Navbar from partials (EJS equivalent to React components) %>
		<header>
			<%- include('../partials/nav') %>
		</header>

		<%# Main Content %>
		<main>
			<%# Import Hero %>
			<%- include('../partials/hero') %>
			<%# Absolute SVG that is displayed on page %>
			
			
		</main>
		<%# End of main content %>

		<%# Import Footer %>
		<footer>
			<%- include('../partials/footer') %>
		</footer>
	</body>
</html>


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Optical - DotCompanion</title>
  <link rel="stylesheet" href="/styles/style.css" type="text/css" />
  <link rel="stylesheet" href="/styles/nav.css" type="text/css" />
  <script src="https://kit.fontawesome.com/43c1b0f304.js" crossorigin="anonymous"></script>
  <style>
    /* CSS for Sticky Footer */
    html, body {
      height: 100%;
      margin: 0;
      padding: 0;
    }

    body {
      display: flex;
      flex-direction: column;
    }

    /* Main content container */
    .main-container {
      flex: 1;
      display: flex;
      flex-direction: column;
      justify-content: center; /* Center vertically */
      align-items: center; /* Center horizontally */
    }

    /* Center-align OCR heading and style file input and upload button */
    .center-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      text-align: center;
      margin-top: 20px; /* Adjust the margin as needed */
    }

    /* Style the "Choose File" and "Upload" buttons */
    input[type="file"] {
      display: none; /* Hide the default file input */
    }

    label.buttonPrimary {
      background-color: var(--primaryDark);
      color: var(--white);
      font-size: 16px;
      padding: 1px 10px;
      border-radius: 3.5px;
      cursor: pointer;
      border: none;
      outline: none;
      transition: 200ms ease-in-out;
      margin-top: 20px; /* Adjust the margin as needed */
      display: inline-block;
    }

    label.buttonPrimary:hover {
      background-color: var(--primary);
      transition: 200ms ease-in-out;
    }
  </style>
</head>
<body>
  <header>
    <%# Import nav %>
    <%- include('../partials/nav') %>
  </header>
  <div class="main-container">
    <main>
      <!-- Center-align OCR heading and style file input and upload button -->
      	<%# Absolute SVGs that are displayed on page %>
				<img src="/media/topItem.svg" class="topItemLarge" />
				<img src="/media/ellipse.svg" class="ellipse" />

      <div class="center-content">
        <h1>Optical Character Recognition</h1>
        <form
          id="frmUploader"
          enctype="multipart/form-data"
          action="/upload"
          method="post"
        >
          <label for="fileInput" class="buttonPrimary">
            Choose File
            <input type="file" name="image" id="fileInput" multiple />
          </label>
          <input type="submit" name="submit" id="btnSubmit" value="Upload" class="buttonPrimary" />
        </form>
      </div>
    </main>
  </div>
  <footer>
    <%# Import footer %>
    <%- include('../partials/footer') %>
  </footer>
</body>
</html>


<!DOCTYPE html>
<html lang="en">
	<%# Head %>
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>TTS - DotCompanion</title>
		<link rel="stylesheet" href="/styles/style.css" type="text/css" />
		<link rel="stylesheet" href="/styles/nav.css" type="text/css" />
    <script src="https://kit.fontawesome.com/43c1b0f304.js" crossOrigin="anonymous"></script>
	</head>
	<%# End of head %>
	<body>
		<%# Import nav %>
		<header>
			<%- include('../partials/nav') %>
		</header>

			<%# Main content %>
			<main>
				<%# Absolute SVGs that are displayed on page %>
				<img src="/media/topItem.svg" class="topItemLarge" />
				<img src="/media/ellipse.svg" class="ellipse" />

				<%# Text header at the top of the page %>
				<article class="speechRecHeader">
					<h1>Speech - Text Converter</h1>
					
				</article>

				<%# Container for notes section %>
				<div class="speechRecContainer">
						<%# Another container %>
            <div class="app"> 
                <h3></h3>
								<%# Textarea for user to input notes %>
                <div class="input-single">
                    <textarea id="note-textarea" placeholder="Create a new note by typing or using voice recognition." rows="6"></textarea>
                </div> 

						<%# Buttons %>        
						<div class="speechRecBtnsWrap">
							<div class="speechRecRecogBtnsWrap">
               <!--Start Recognition Button -->
                <button class="buttonPrimary" id="start-record-btn" title="Start Recording">Start </button>

                <!--Stop Recognition Button -->
                <button class="buttonPrimary" id="pause-record-btn" title="Pause Recording">Stop </button>
							</div>

                <!--Saving the note Button -->
                <button class="buttonPrimary" id="save-note-btn" title="Save Note">Save </button>  
						</div> 

                <!--Description for using the buttons -->
                <p id="recording-instructions">Press the <strong>Start </strong> button and allow access.</p>
                

							<%# Notes %>
							<div class="notes">
                <h3>Speech - Text Notes</h3>
                <ul id="notes">

                    <!--Error message for no notes or -->
                    <li>
                        <p class="no-notes">You don't have any notes.</p>
                    </li>
                </ul>
							</div>

            </div>

        </div>
			</main>
			<%# End of main content %>

		<%# Import footer %>
		<footer>
			<%- include('../partials/footer') %>
		</footer>
		<!-- Start of Javascript -->
		<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
        <script>
            try {
              var SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
              var recognition = new SpeechRecognition();
            }
            catch(e) {
              console.error(e);
              $('.no-browser-support').show();
              $('.app').hide();
            }


            var noteTextarea = $('#note-textarea');
            var instructions = $('#recording-instructions');
            var notesList = $('ul#notes');

            var noteContent = '';

            /* Get all notes from previous sessions and display them.*/
            var notes = getAllNotes();
            renderNotes(notes);



         /*voice recognition*/            
            recognition.continuous = true;

            
            recognition.onresult = function(event) {

              var current = event.resultIndex;
              var transcript = event.results[current][0].transcript;

              var mobileRepeatBug = (current == 1 && transcript == event.results[0][0].transcript);

              if(!mobileRepeatBug) {
                noteContent += transcript;
                noteTextarea.val(noteContent);
              }
            };

            recognition.onstart = function() { 
              instructions.text('Voice recognition activated. Try speaking into the microphone.');
            }

            recognition.onspeechend = function() {
              instructions.text('You were quiet for a while so voice recognition turned itself off.');
            }

            recognition.onerror = function(event) {
              if(event.error == 'no-speech') {
                instructions.text('No speech was detected. Try again.');  
              };
            }

            /*taking user input*/
            $('#start-record-btn').on('click', function(e) {
              if (noteContent.length) {
                noteContent += ' ';
              }
              recognition.start();
            });


            $('#pause-record-btn').on('click', function(e) {
              recognition.stop();
              instructions.text('Voice recognition paused.');
            });

            /*Sync the text inside the text area with the noteContent variable.*/
            noteTextarea.on('input', function() {
              noteContent = $(this).val();
            })

            $('#save-note-btn').on('click', function(e) {
              recognition.stop();

              if(!noteContent.length) {
                instructions.text('Could not save empty note. Please add a message to your note.');
              }
              else {
                /*Using Local storage to save the note */
                saveNote(new Date().toLocaleString(), noteContent);

                noteContent = '';
                renderNotes(getAllNotes());
                noteTextarea.val('');
                instructions.text('Note saved successfully.');
              }
                  
            })


            notesList.on('click', function(e) {
              e.preventDefault();
              var target = $(e.target);

              /*listening to the note */
              if(target.hasClass('listen-note')) {
                var content = target.closest('.note').find('.content').text();
                readOutLoud(content);
              }

              /* Function for deleting the recorded note.*/
              if(target.hasClass('delete-note')) {
                var dateTime = target.siblings('.date').text();  
                deleteNote(dateTime);
                target.closest('.note').remove();
              }
            });

            /*Reading the note by using a voice API*/
            function readOutLoud(message) {
                var speech = new SpeechSynthesisUtterance();

              // Set the text and voice attributes.
                speech.text = message;
                speech.volume = 1;
                speech.rate = 1;
                speech.pitch = 1;
              
                window.speechSynthesis.speak(speech);
            }


            function renderNotes(notes) {
              var html = '';
              if(notes.length) {
                notes.forEach(function(note) {
                  html+=`<li class="note">
                    <p class="header">
                      <span class="date">${note.date}</span>
                    </p>
                    <p class="content">${note.content}</p>
										<div class="noteBtnWrap">
                     	  <a href="#" class="listen-note buttonPrimary" title="Listen to Note">Listen to Note</a>
                    	  <a href="#" class="delete-note buttonPrimary" title="Delete">Delete</a>
											</div>
                  </li>`;    
                });
              }
              else {
                html = '<li style="pointer-events:none;"><p class="content">You don\'t have any notes yet.</p></li>';
              }
              notesList.html(html);
            }


            function saveNote(dateTime, content) {
              localStorage.setItem('note-' + dateTime, content);
            }


            function getAllNotes() {
              var notes = [];
              var key;
              for (var i = 0; i < localStorage.length; i++) {
                key = localStorage.key(i);
                console.log(i)
                console.log(key)

                if(key.substring(0,5) == 'note-') {
                  notes.push({
                    date: key.replace('note-',''),
                    content: localStorage.getItem(localStorage.key(i))
                  });
                } 
              }
              console.log(notes)
              return notes;
            }


            function deleteNote(dateTime) {
              localStorage.removeItem('note-' + dateTime); 
            }
        </script>
	</body>
</html>


<section class="footer">
  <p>Live without seeing, but be what you are 📚 ! </p>
  </div>
</section>



<section class="hero">
	<%# Absolute SVG that is displayed on page %>
	<img src="/media/topItem.svg" class="topItem" />
		<%# Hero Content %>
		<span class="heroWrap">
			<article>
				<h1>Dot
        <span	class="headerSecondaryText">Companion</span>
        </h1>
				<p>Introducing the Enhanced Braille Translator Web Application: Bridging the Gap in Communication Between Sighted and Visually Impaired Individuals. Our mission is to create an inclusive digital environment by developing a cutting-edge web application that seamlessly translates text to Braille and vice versa. By doing so, we empower visually impaired users and promote effortless communication with the sighted community.</p>

				
			</article>
			<%# Absolute SVG that is displayed on page %>
		<img class="art" src="/media/art1.jpg" />
		</span>
</section>


<nav>
	<span class="navWrap">
	<%# Logo %>
		<%# Hamburger Menu %>
		<button onclick="toggleNavLinks()" class="hamburger">
			<svg viewBox="0 0 100 80" width="24" height="24" fill="#FFFFFF">
		  	<rect width="100" height="11"></rect>
		  	<rect y="30" width="100" height="11"></rect>
		  	<rect y="60" width="100" height="11"></rect>
			</svg>
		</button>
	</span>
	<%# Nav links %>
	<span class="navLinksWrap">
		<ul id="navLinks">
			<li><a href="/">Home</a></li>
      <li><a href="/ttslearning">Text to Speech </a></li>
      <li><a href="/Brailleedit"> Text to Braille</a></li>

      <li><a href="/braille">Braille to Text</a></li>
      <li><a href="/optical"> OCR </a></li>
      
		</ul>
	</span>
</nav>

<script>
	// Check if the display of the nav links, and switch to either display: none; or display: block; depending on what the current display value is
	const toggleNavLinks = () => {
		let x = document.getElementById('navLinks');
		(x.style.display === 'none') ? x.style.display = 'block' : x.style.display = 'none';
	}
</script>


