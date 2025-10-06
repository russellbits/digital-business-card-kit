# Overview
Given these assets (CSS and HTML templates) your job is to ask the user questions in order to fill out the card.yaml file, insert that information into the proper places in the .vcf and index.html templates and create a ZIP file that will contain a small web site that the user can then upload to a server. The small web site is what is known as a digital business card and it has key information about the user including, their .vcf file, a photo, ways to contact them and social media links.

# Guidelines
+ Use only the HTML template, the CSS, and the images provided. Do not add anything unless instructed.
+ Do not make any edits or changes to the CSS files

# Instructions
1. Once given this prompt, ask any series of questions that will allow you to fill out the `card.tmpl.vcf` file. End the series of questions with a request for a profile bio. 
2. Create a folder called `build` in which you will place final items for the digital business card web site. `build` should have two sub-folders called `build/images` and `build/styles`. Use the `images` folder included with this prompt top initially populate `build/images` and then add the images from steps 4 and 5.
3. Fill out the `card.tmpl.vcf` with the information you gleaned from the question in Step 1. `card.tmpl.vcf` has mustache-style template tags. Fill in any tags (example: `{{tag}}`) Save the file to  `/build` as `${firstName}_${lastName}_card.vcf`.
4. In the same manner as the `card.tmpl.vcf` file, there is a `index.tmpl.html` file. It also has mustache-style tags to be filled in. Use the same information as the .vcf file as well as the profile bio. Save the fille-in file as `index.html` and place it in `build`.
4. Find the file called portrait.jpg and crop and scale it to a width of 630px and a height of 640px. Try to keep the person's face in the center of the image. Place the final image in the `build/images`
5. Find the file called `logo.png` and scale (only scale, do not crop) to 300px by 300px. Place that image in the `build/images` folder.
6. Copy the included `styles` folder into `build/styles`
7. Generate a QR code (as a .png file) that has the URL `${firstName}_${lastName}_card.vcf`. In other words, generate a QR code that will link to the local .vcf file inside `build`. If possible, add the logo to the QR code at the center; scale `logo.png` however needed. Generate that QR code image and place it in `build/images`
8. Create the `build` directory and compress it into a zip file.