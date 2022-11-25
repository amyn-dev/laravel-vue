# Laravel With VueJs SPA

## installation :
after install the source code open Terminal to use these command line:
- composer install
- cp .env.example .env
- create new database
- php artisan key:generate
- php artisan storage:link
- php artisan migrate --seed
- php artisan passport:install
- copy the of Client ID value and paste it in .evn Ex. 2
- copy the of Client secret value and paste it in .evn Ex. CLFlyd84jgusluIbFDwWQ4XLMiuMr4NCh6kE2XSd 
- npm install
- npm run dev or watch
- php artisan serve
- mkdir storage/app/public/assets/avatars
- sudo cp public/img/users/avatar.png storage/app/public/assets/avatars/  
- add your default image (should be the name is 'avatar.png' or custom the default name from ProductMedia migration table)
- important( make sure the APP_URL in .env file same actually URL )
- 
## Permission folder ( Linux | Mac)
- sudo chmod -R 777 settings.json
- sudo chmod -R 777 storage/app/public/images/


