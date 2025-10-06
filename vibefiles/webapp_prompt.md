Create a PHP single-page application for generating digital business cards with the following requirements:

TECHNOLOGY:
- Pure PHP with HTML/CSS form
- Use PicoCSS for styling
- PHP GD library for image processing
- ZipArchive for packaging

FEATURES:
1. Form with drag-and-drop image uploads:
   - Portrait image (JPG) - auto-resize to 640x630px with smart cropping (keeps faces centered)
   - Logo (PNG/GIF) - must be square, validate dimensions, scale to 300x300px
   
2. Form fields:
   - Name: first, last
   - Professional: organization, title
   - Contact: email, phone, cell, home, website
   - Social: facebook, yelp, twitter, linkedin, bluesky, skype
   - Note: textarea for additional info
   
3. URL inputs should show "https://" prefix in light gray, users only enter domain

4. Generate two files from these templates:
   - index.html using carousel card layout with navigation
   - {firstname}_{lastname}_vcard.vcf with proper vCard format

5. On submit, show preview of both HTML (in iframe) and VCF (as text)

6. Download button creates ZIP containing:
   - index.html
   - personalized .vcf file
   - images/portrait.jpg (processed)
   - images/logo.png or .gif (processed)
   - Copy entire /styles directory from server
   - Copy entire /images directory from server

7. Error handling:
   - Enable full error logging
   - Check for GD and ZIP extensions
   - Validate logo is square, show helpful error if not
   - Include try-again button on errors

8. Use these exact templates:
```<!doctype html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="./favicon.png" />
		<meta name="viewport" content="width=device-width, initial-scale=1" />
		<meta property="og:title" content="{{title}}" />
		<meta property="og:type" content="Digital Business Card" />
		<meta property="og:url" content="{{}}" />
		<meta property="og:image" content="./images/og-site-image.png" />
		<meta name="msapplication-TileColor" content="#ffffff">
		<meta name="msapplication-TileImage" content="/ms-icon-144x144.png">
		<meta name="theme-color" content="#ffffff">
		<link rel="apple-touch-icon" sizes="57x57" href="/apple-icon-57x57.png">
		<link rel="apple-touch-icon" sizes="60x60" href="/apple-icon-60x60.png">
		<link rel="apple-touch-icon" sizes="72x72" href="/apple-icon-72x72.png">
		<link rel="apple-touch-icon" sizes="76x76" href="/apple-icon-76x76.png">
		<link rel="apple-touch-icon" sizes="114x114" href="/apple-icon-114x114.png">
		<link rel="apple-touch-icon" sizes="120x120" href="/apple-icon-120x120.png">
		<link rel="apple-touch-icon" sizes="144x144" href="/apple-icon-144x144.png">
		<link rel="apple-touch-icon" sizes="152x152" href="/apple-icon-152x152.png">
		<link rel="apple-touch-icon" sizes="180x180" href="/apple-icon-180x180.png">
		<link rel="icon" type="image/png" sizes="192x192"  href="/android-icon-192x192.png">
		<link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
		<link rel="icon" type="image/png" sizes="96x96" href="/favicon-96x96.png">
		<link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
		<link href="./styles/card.css" rel="stylesheet">
		<link href="./styles/entypo.css" rel="stylesheet">
	</head>
	<body>

		<nav class="top">
			<a href="#" onclick="showCard('id')"
					><img src="images/navigation/v-card.svg" width="25" height="25" alt="Codepen logo" /></a
				>
			<a href="#" onclick="showCard('qr')"
					><img src="images/navigation/qr-code.svg" width="25" height="25" alt="Codepen logo" class="off" /></a
				>
			<a href="#" onclick="showCard('info')"
					><img src="images/navigation/info.svg" width="25" height="25" alt="Codepen logo" class="off" /></a
				>
		</nav>
		<main>

			<div class="cardCarousel">

				<div class="card id">

					<figure class="portrait">
						<img src="images/portrait.jpg" width="640" height="630" alt="Portrait of {{firstName}} {{lastName}}">
					</figure>

					<div class="text">
						<h1>{{organization}}</h1>
						<h2>{{title}}</h2>
					</div>

				</div>
				
				<div class="card qr">
					<figure class="qr">
						<img src="images/qrcode.png" alt="Business Raccoon vcf qr code">
					</figure>
				</div>

				<div class="card info">
					<div class="text">
						<h1>About</h1>
						<h2>Some kind of about informaton.</h2>
					</div>
				</div>
				
			</div>
			
			<nav class="inline">
				<div class="button"><a href="https://www.linkedin.com/company/raccoonlogic/"><img src="images/navigation/linkedin-white-circle.svg" width="30" height="30" alt="Linkedin logo" /><label>Raccoon Logic</label></a></div>
				<div class="button"><a href="https://www.pinterest.com/belovedleader/allen/"><img src="images/navigation/instagram-white-circle.svg" width="30" height="30" alt="Instagram logo" /><label>Allen!</label></a></div>
				<div class="button"><a href="https://www.instagram.com/awesome.raccoon/"><img src="images/navigation/github-white-circle.svg" width="30" height="30" alt="Github logo" /><label>Awesome Raccoon</label></a></div>
				<div class="button"><a href="#"><img src="images/navigation/facebook-white-circle.svg" width="30" height="30" alt="Github logo" /><label>Awesome Raccoon</label></a></div>
				<div class="button"><a href="#"><img src="images/navigation/weblink-white-circle.svg" width="30" height="30" alt="Github logo" /><label>Awesome Raccoon</label></a></div>
			</nav>

		</main> 
			
		<script>
			function showCard(cardId) {
				const carousel = document.querySelector('.cardCarousel');
				const cards = document.querySelectorAll('.card');
				const mainWidth = document.querySelector('main').offsetWidth;
				
				// Find the index of the target card
				let targetIndex = 0;
				cards.forEach((card, index) => {
					if (card.classList.contains(cardId)) {
						targetIndex = index;
					}
				});
				
				// Calculate the translation needed
				const translation = (-targetIndex * mainWidth) + 3;
				
				// Apply the translation
				carousel.style.transform = `translateX(${translation}px)`;
				
				// Update navigation icons
				document.querySelectorAll('nav.top img').forEach(img => {
					img.classList.add('off');
				});
				document.querySelector(`nav.top a[onclick="showCard('${cardId}')"] img`).classList.remove('off');
			}
			
			// Show initial card
			showCard('id');
		</script>
		
	</body>
</html>
```
```// File: templates/contact.vcf.hbs
BEGIN:VCARD
VERSION:3.0
FN:{{firstName}} {{lastName}}
N:{{lastName}};{{firstName}};;;
{{
#if
 organization}}ORG:{{organization}}{{
/if
}}
{{
#if
 title}}TITLE:{{title}}{{
/if
}}
{{
#if
 email}}EMAIL;type=INTERNET;type=WORK:{{email}}{{
/if
}}
{{
#if
 phone}}TEL;type=CELL:{{phone}}{{
/if
}}
{{
#if
 address}}ADR;type=WORK:;;{{address.street}};{{address.city}};{{address.state}};{{address.zip}};{{address.country}}{{
/if
}}
{{
#if
 url}}URL:{{url}}{{
/if
}}
{{
#if
 note}}NOTE:{{note}}{{
/if
}}
END:VCARD```

9. Image processing:
   - Portrait: crop/scale to 640x630, prioritize face centering (crop 30% from top for tall images)
   - Logo: validate square dimensions first, then scale (no crop), preserve transparency

10. Code structure:
   - Define all functions at the top
   - Use array() syntax for PHP 5.x compatibility
   - Use isset() instead of ?? operator
   - No heredoc syntax, use string concatenation