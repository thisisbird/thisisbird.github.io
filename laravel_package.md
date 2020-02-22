###### tags: `教學` `laravel`

# laravel 做自己的package

1. ## 先 開啟/建立 laravel專案  
    `composer create-project laravel/laravel MyPackage --prefer-dist`
3. ## 建立資料夾  
    `/packages/[開發商名]/[套件名]/src`
3. ## 建立composer.json  
    ```
    cd /packages/virtualorz/hello
    composer init
    ```
    輸入 `composer init` , 它會問你一系列的問題，但你可以參考我的內容，產生出你的 composer.json:  
    ```json
        {
         "name": "virtualorz/hello",
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
               "Virtualorz\\Hello\\": "src"
             }
           }
        }
    ```
    laravel/framdwork 這邊要特別注意，這邊指的是使用者下載使用你套件時的必要環境，以上例來說，就是規定使用者的 Laravel 版本必須是 5.1.* 使用者將無法下載該套件。  
    
    然後 autoload 指定凡使用 virtualorz\hello 的來源是 src  
   
4. ## 設定PSR-4命名空間  
   我們必須回到上述管理package的project根目錄將剛剛建立好的package加入到composer.json中，讓他可以在這個project中運行
   ![](https://i.imgur.com/SzV5Xfk.png)

   **請注意PSR-4大小寫的問題**
   
   接著回到MyProject根目錄來產生新的autoload檔案  
   
   `composer dump-autoload`
5. ## 建立ServiceProvider  
    接下來要建立HelloServiceProvidor.php，套件與專案Project會透過ServiceProvider連結起來  
    
    `php artisan make:provider HelloServiceProvider`  
    
    這時候會在/app/providers中建立好HelloServiceProvider.php 但是因為我們是要套件中使用  
    所以必須要將這個檔案移動到 /packages/virtualorz/hello/src 底下，也因為移動了所以必須要修改namespace  
    
    `namespace Virtualorz\Hello`
    
    **請注意PSR-4大小寫的問題**
    ![](https://i.imgur.com/PG7SAvn.png)
    
    接著在config/app.php 中註冊HelloServiceProvidor
    
    `Virtualorz\Hello\HelloServiceProvider::class`
    
    ![](https://i.imgur.com/T68jkL0.png)
6. ## 建立Controller  
    一樣可以透過指令的方式建立Controller，相同的建立完成之後我們會將Controller移動到 MyPackage/packages/virtualorz/hello/src 底下
    
    `php artisan make:controller HelloController`
    
    同樣的也必須要修改namespace，也因為extend Controller的關係，所以必須將App\Http\Controllers\Controller 拿進來用
    ![](https://i.imgur.com/jWrtEQy.png)  
    **上圖中 ~~Hello::welcome~~ 改成 HelloWorld::welcome **
    
7. ## 建立Route  
    Route可以根據不同的狀況來建立所需要的存取路徑，本次範例在 MyPackage/virtualorz/hello/src 底下建立routes.php，填入以下路徑  
    ```php 
    Route::get('hello', 'Virtualorz\Hello\HelloController@index');
    ```
    
8. ## 建立View
    本次範例也不特別建立view，僅拿現成的welcome頁面來使用，我們將resources/views/welcome.blade.php 複製到 package/virtualorz/hello/src/views

    並且小修一下內容用來顯示controller中的message
    ![](https://i.imgur.com/rIEGTir.png)

    
9. ## 修改ServiceProvider  
    最後我們還要修改一下SeviceProvidor檔案，讓系統可以知道從哪邊將view讀進來
    ![](https://i.imgur.com/ESUC37y.png)

10. ## 測試package是否可行  
    打開瀏覽器，輸入MyPackage所在的網址後面夾入/hello，本次的範例是這樣子的
    
    `http://127.0.0.1:8888/hello`  
    
    如果有看到這樣的畫面就是正確的，我們把message字串接在Laravel後面
    ![](https://i.imgur.com/07lNPWX.png)

11. ## 上傳到github  
    因為之後提交到packagist後使用者可以自行下載，當然不能讓使用者自行修改PSR-4才能使用，因此我們將hello 資料夾中的composer.json修改一下
    ```json
    {
        "name": "virtualorz/hello",
        "authors": [
            {
                "name": "virtualorz",
                "email": "virtualorz@gmail.com"
            }
        ],
        "require": {},
        "autoload": {
            "psr-4": {
                "Virtualorz\\Hello\\": "src/"
            }
        }
    }
    ```
    接著在hello 資料夾下建立git，並且push到遠端github上
    ```
    git init
    git add .
    git commit -m "Initial commit."
    git remote add origin git@github.com:virtualorz/hello.git
    git push -u origin master
    git tag -a 1.0.0 -m "First version"
    git push --tags
    ```
    
    請一定要記得下tag，並且在之後所有push的版本都加上tag，這樣packagist才有依據要抓哪一個版本的程式碼給使用者

    最後記得提交到Packagist上就可以啦

 

    注意事項 ：在開發過程中如果要變更版本號碼請記得砍掉composer.lock，讓系統重新抓composer.json中定義的版本號碼，這樣才會重新下載新的
    
12. ## 提交至packagist  
    接著去 packagist 把我們的 git url 貼過去  
    [https://packagist.org/login/](https://packagist.org/login/)  
    用github帳號登入  
    貼上github網址  
    ![](https://i.imgur.com/BnSpyzZ.png)

13. ## 安裝套件  
    只要輸入composer require thisisbird/hello 即可安裝"自己"的套件到vendor資料夾  
    
    然後他們就可以跟你一樣，直接用
    
    `use Virtualarz\Hello\HelloController;`
    
    或輸入網址  
    
    `http://127.0.0.1:8888/hello`