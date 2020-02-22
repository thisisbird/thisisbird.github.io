###### tags: `教學` `laravel`

# laravel 做自己的package

1. 先開啟laravel專案
2. 建立資料夾  
    `/packages/[開發商名]/[套件名]/src`
3. cd /packages/sexyoung/plus  
    輸入 `composer init` , 它會問你一系列的問題，但你可以參考我的內容，產生出你的 composer.json:  
    ```
        {
         "name": "sexyoung/plus",
         "keywords": ["framework", "laravel"],
         "license": "MIT",
         "authors": [
         {
         "name": "你的名字",
         "email": "你的email"
         }
         ],
           "minimum-stability": "dev",
           "require": {
           "php": ">=5.4.0",
           "laravel/framework": "5.1.*"
         },
           "autoload": {
             "psr-4": {
               "Sexyoung\\Plus\\": "src"
             }
           }
        }
    ```
    laravel/framdwork 這邊要特別注意，這邊指的是使用者下載使用你套件時的必要環境，以上例來說，就是規定使用者的 Laravel 版本必須是 5.1.* 使用者將無法下載該套件。  
4. 

