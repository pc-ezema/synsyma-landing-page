# Synsyma website (one page)

Two files, no build step:  index.html  and  config.js
Sections: Home, Services, How it works, About us, Download app, Apply to join (contractors),
Contact us, Delete your account, footer. The menu has Services, About us, Download app, Contact us
and the highlighted "Apply to join" button.

## Put it online (cPanel)
1. Point your main domain (or a subdomain such as www / join) at a folder, upload both files there.
2. Edit config.js: API_BASE, the two store links, privacy / terms links (see below).
3. Turn on SSL.

## API changes (small)
1. Contact us reads the contacts you manage in the admin (Support contacts). Add ONE public route,
   outside the auth group, in the Support module's routes file:
     use Modules\Support\Http\Controllers\SupportController;
     Route::get('public/support-contacts', [SupportController::class, 'contacts'])->middleware('throttle:60,1');
   (Same controller method the app already uses, so only active contacts are shown.)
2. Replace PublicJoinController.php with the new copy (the service list now includes icon + description).
3. CORS: add your site's address to config/cors.php 'allowed_origins', then  php artisan config:clear
4. The application form uses the contractor endpoints you already added (/admin/public/join/...).

## Store buttons
While APP_STORE_URL / PLAY_STORE_URL are empty the buttons show "Soon". Fill them in when the apps are live.
The buttons are simple custom ones. Apple and Google ask you to use their official badge artwork:
download it from their brand pages and swap it in the two .store links.

## Account deletion page (required by Google Play and the App Store)
The section has the id  delete-account, so the page address is  https://yourdomain/#delete-account
Use that as the "account deletion URL" in Play Console and App Store Connect.
It describes what the app really does today (password + confirm, record kept). If you change that, edit the text.

## Privacy policy and terms
Both stores require a privacy policy link. Put the address in config.js (PRIVACY_URL / TERMS_URL) and the
links appear in the footer.

## QR codes
Admin QR codes open  https://your-site/?ref=CODE  and the page jumps to the application form.
In the admin project set  VITE_JOIN_URL=https://your-site   (no trailing slash) in .env.production.

## Images (img/ folder)
logo.png / favicon.png   the house-and-leaf logo, taken from the shirts in your illustrations
cleaner.webp, handyman.webp   the two characters with the white background removed
lawn.webp                the lawn-mowing scene
Service cards pick a picture by category name (clean/sanitation -> cleaner, repair/technical -> handyman,
outdoor/lawn/garden -> lawn). Other categories show an icon until you add a picture and a line in pickPhoto() in index.html.
For a sharper logo, replace img/logo.png with your original logo file (transparent PNG, square).
