# Rails assets precompile

Assets Pipeline 是 Rails 3.1 一個重要的功能，一直並沒有很去了解其特性，但因為最近都在寫前端的東西在 assets pipeline 的東西上跌跌撞撞了不少次(尤其在 deploy 上 production 後常爆炸，爆到我無處自容)，這篇就是好好研究後的心得以及筆記。

### Assets Pipeline 有什麼好處，不用會怎樣?

不用當然不會怎樣，你可以在 `confing/application.rb` 中把他關掉：

```
config.assets.enabled = false
```

但是 Assets Pipeline 有著許多優良的好處，幫助你處理的過去一些需要由第三方元件來處理的事情，像是：

- 將所有的 js 或是 css 壓縮打包成單一檔案，減少 http request 的大小與數量，增加你網站的效能及速度。
- 支援像是 SCSS 及 CoffeeScript 這樣的 high-lever 語言，你可以用更簡單更棒的方式來寫 css 及 js。
- 取代原先不可靠的 query string 改用 MD5 的 fingerprint，query string 的用意在於當檔案內容更動的時候也會一併更動檔案的 query string，這樣可以分辨檔案是否有更動過，因此客戶端可以保留快取並比對自己擁有的版本以及伺服器上的版本是否一致，減少每次的 request，無奈 query string 的作法還是有些問題，像是在部分 CDN 上根本不會快取、在多伺服器的環境中檔案名稱可能會變動、以及許多無效的 cache 問題，因此在 Rails 3.1 改使用 MD5 的 Fingerprinting 來解決了這個問題。
Assets Pipeline 的功能主要由兩個重要的元件提供：Sprockets 以及 Tilt。Sprockets 用來從你的 assets 路徑中打包壓縮你所有的 assets 後包裝成一個檔案，然後放到你目的地路徑(public/assets)，而 Tilt 主要是一個樣板引擎，用來讓 Sprockets 可以去解析像是 SCSS、CoffeeScript 或是 ERB 等各種樣板，你可以參考 Tilt 的 Readme 來了解支援哪些樣板。

### Assets 的結構

首先必須了解 Assets 的結構，在 Rails 的目錄結構中有三個地方：

- app/assets(通常放置我們自己為了自己的程式所寫的 js、css 或是 images)
- lib/assets(通常是我們所使用的套件中去用到的 assets)
- vendor/assets(通常是放一些我們從別的地方借用的 assets，例如說一些 jQuery 的套件)
這三個目錄，在預設情況下這三個資料夾的東西是共通的(因為都會被打包成一個檔案)，你可以把你的 rails app 跑起來後在 http://localhost:3000/assets/application.js 中看到你所有的 js 都在這支檔案中，css 同理亦然，你可以在 terminal 中輸入 Rails.application.config.assets.paths 來查看所有的 assets 路徑。你可以發現，除了原本我們剛剛說的三個 assets 目錄之外，還出現了包含在我們 GemFile 中的 jquery，這代表你的 assets 現在也可以包成 gem 來用，如果你有很多個 projects 常重複使用一些共通的 assets，不妨考慮包成 gem 來使用，方便又愉快。

### Assets 的載入

再來是 assets 目錄下的檔案 import 方式，以 app/asset/javascripts/application.js 這支檔案為例，這是一支 manifest 檔案，主要用來告訴 Sprockets 說哪些檔案是要被載入最後要被包起來壓縮的，最後這支檔案裡面所有的東西就會被包成 application.js 這支檔案，也是我們 layout/application.html.erb 中的 javascript_include_tag 'application' 中的檔案，打開這支檔案除了上面的說明外只有這三行：

```
//= require jquery
//= require jquery_ujs
//= require_tree .
```

上面兩行很明顯的就是要載入 jquery 以及 jquery_ujs 這兩支檔案，這兩支檔案剛剛有提到他其實是被包含在我們所使用的 Gem 中，而下面那行 require_tree . 表示是把三個 assets/javascript 目錄下的檔案或是子目錄內的檔案全部都包進來，這時候你一定會想問如果有些 js 或是 css 我只想在某些特定頁面中使用的話該怎麼辦，例如說假設我們今天有個 admin_functions.js 的檔案只想在我們的後台使用，有兩種方法可以使用：

你可以將 require_tree 的目錄改成其他目錄，例如在 app/assets/javascript 目錄下建個 common 資料夾，把 require_tree . 改成 require_tree ./common，這樣子所產生的 application.js 這支檔案就不會用到 admin_functions.js 這支檔案。
你可以建立一個新的資料夾來放你不想要被 application.js 載入的檔案，例如我們在 app/assets/javascript 下建立一個 admin 資料夾把剛剛的 admin_functions.js 檔案放進去，然後把原先 application.js 中的 require_tree . 改為 require_directory . 這樣子只會抓與 apllication.js 檔案同目錄底下的所有檔案而不會去載入子目錄中的檔案。
最後再建立另外一支 manifest 用來 import 那些我們要獨立出來的 assets，例如我們建立一支 admin.js 的檔案用來載入其他功能，一樣使用 require_tree 或是 require_directory 的方式來載入，然後在你需要用到的頁面中使用 javascript_include_tag 'admin' 來存取。

千千萬萬要記得，當你使用 application.js 以外的 manifest 檔案時，一定要在你環境設定檔中將這支檔案加入 precompile 的清單，否則上了 staging 或是 production 時你就會收到一堆 500 Error XXXX isn’t precompiled，加入的位置在環境設定檔像是 production.rb 中的 config.assets.precompile += %w( search.js ) 中。

除了 require_tree 及 require_directory 之外，還有其他的用法，你都可以使用絕對或是相對路徑來指定檔案位置，副檔名可有可無：

require [路徑] 載入某支特定檔案，如果這支檔案被載入多次，Sprockets 也會很聰明的只幫你載入一次。
include [路徑] 與 require 一樣，差別在即使是被載入過的檔案也會再被載入。
require_directory [路徑] 將路徑下不包含子目錄的檔案按照字母順序依次載入。
require_tree [路徑] 會將路徑下包含子目錄的檔案全部載入。
require_self [路徑] 告訴 Sprockets 再載入其他的檔案前，先將自己的內容插入。
depend_on [路徑] 宣告依賴於某支 js，在需要通知某支快取的 assets 過期時非常實用。
stub [路徑] 將路徑中的 assets 加入黑名單，所有其他的 require 都不會將他載入。
你可以看 Sprockets 的 Readme 來獲得更多的資訊。

### Preprocessing

另外就是 Sprockets 在 Tilt 的協助下有 preprocessing 的功能，例如你可以使用像是 something.js.coffee.erb 這樣的檔名，Sprockets 會從檔名的最後面一直解析回去成最後的檔案，因此你可以在 js 中使用 CoffeeScript 的寫法來寫 js，並在裡面寫 ruby code 來產生你想要的東西，例如：

```
jQuery ->
  number = <%= 1 + 1 %>
```

不用我說我想你也知道會有什麼結果。

### Helper

Assets 提供了很多路徑 helper 來讓你指向你的 assets:

```
audio_path("horse.wav")   # => /audios/horse.wav
audio_tag("sound")        # => <audio src="/audios/sound" />
font_path("font.ttf")     # => /fonts/font.ttf
image_path("edit.png")    # => "/images/edit.png"
image_tag("icon.png")     # => <img src="/images/icon.png" alt="Icon" />
video_path("hd.avi")      # => /videos/hd.avi
video_tag("trailer.ogg")  # => <video src="/videos/trailer.ogg" />
```

Sass 還提供了像是 -url 和 -path 這樣的 helper 來協助你，因此你也可以這樣使用：

```
image-url("rails.png")         # => url(/assets/rails.png)
image-path("rails.png")        # => "/assets/rails.png".
asset-url("rails.png", image)  # => url(/assets/rails.png)
asset-path("rails.png", image) # => "/assets/rails.png"
Production
```

### Production

在還不熟悉的情況下，很容易上了 production 環境後發現原本在本機好好的東西全部炸掉了，因此我們必須了解一下在 production 運作時的情形，如果你直接在 console 中打 rails s -e production 來啟動 production 環境時，你會發現馬上就噴錯誤 application.css isn't precompiled，這是因為在 production 的環境下我們的 assets 是必須被 compile 過後存在 public/assets 底下的，你可以在 console 中打 rake assets:precompile，Rails 會幫你把所有的 assets 檔案依照你 manifests 以及環境設定打包壓縮成單一的檔案後放在 public/assets 目錄底下，所有的檔案名稱並會加入 MD5 的 fingerprinting 用來表示其內容供快取，這些 assets 會被 Rack Cache middleware 自動的被快取，如果你想要使用自己的 server 來取代 middleware 的功能你也可以自己 precompile 後上傳。

在你 precompile 後你再打開 localhost:3000 會發現這時已經沒有錯誤，但是網站看起來就是沒有 css 的感覺，這時候查看 log 你會看到 Rails 找不到 assets 檔案的錯誤，這是因為在 production 環境中是不處理靜態檔案的，因此你必須先在 production.rb 中將 config.serve_static_assets = false 改成 true，這時候重開一次就會看到一切正常，我們已經順利在本機上跑起 production 環境了，你就可以在本機上測試你的 assets pipeline 是否正常，如果你去檢視 precompile 後的檔案你會發現它們都加上了我們之前提到的 MD5 digest，用來辨認其檔案內容是否有所更動，因此你可以將你的 Server 設定中的 expires 時間調到最長，因為在檔案沒有更動的情況下他都會保持相同的檔名，讓使用者可以達到最快的效能。

### Deploy 的小技巧

#### 在本機 precompile 來節省在 server 上 precompile 的記憶體使用量

一般我們都會使用 Capistrano 來 Deploy，記得要將 Capfile 中的 load 'deploy/assets' 的註解取消，Deploy 的過程中會使用 production server 來 compile 你的 assets，如果你很在意 production server 的效能，你可以在本機先 compile 後再上傳到 Server，我們只需要覆寫原本 Capistrano 所提供的 assets:precompile 功能，在你的 deploy.rb 中加入下面的 Code：

```
namespace :deploy do
  namespace :assets do
    desc "Precompile assets on local machine and upload them to the server."
    task :precompile, :roles => web, :except => {:no_release => true} do
      run_locally "bundle exec rake assets:precompile"
      find_servers_for_task(current_task).each do |server|
        run_locally "rsync -vr --exclude='.DS_Store' public/assets #{user}@#{server.host}:#{shared_path}/"
      end
    end
  end
end
```

這樣會在本機 precompile 後使用 rsync 將檔案上傳上去，如果你有使用 Git 的話別忘了把 public/assets 加到 .gitignore 中。

#### 如果 assets 沒有更新時，就不要跑 precompile

precompile 應該是整個 deploy 過程中最漫長的一段時間，即使你沒有更新 assets 時也免不了給你跑一下，你可以一樣覆寫 assets:precompile 來判斷是否有更新 assets，如果沒有才執行 precompile：

```
namespace :deploy do
  namespace :assets do
    task :precompile, :roles => :web, :except => { :no_release => true } do
      from = source.next_revision(current_revision)
      if capture("cd #{latest_release} && #{source.local.log(from)} vendor/assets/ app/assets/ | wc -l").to_i > 0
        run %Q{cd #{latest_release} && #{rake} RAILS_ENV=#{rails_env} #{asset_env} assets:precompile}
      else
        logger.info "Skipping asset pre-compilation because there were no asset changes"
      end
    end
  end
end
```

當然你可以將上面兩段 Code 合而為一，有更新 assets 的情況就在本機 precompile 後才上傳到伺服器：

```
namespace :deploy do
  namespace :assets do
    task :precompile, :roles => :web, :except => { :no_release => true } do
      from = source.next_revision(current_revision)
      if capture("cd #{latest_release} && #{source.local.log(from)} vendor/assets/ app/assets/ | wc -l").to_i > 0
        run_locally "bundle exec rake assets:precompile"
        find_servers_for_task(current_task).each do |server|
          run_locally "rsync -vr --exclude='.DS_Store' public/assets #{user}@#{server.host}:#{shared_path}/"
        end
      else
        logger.info "Skipping asset pre-compilation because there were no asset changes"
      end
    end
  end
end
```

參考：

RailsGuides Asset Pipeline
Asset Pipeline for Dummies
#279 Understanding the Asset Pipeline
#341 Asset Pipeline in Production
Speed up assets:precompile with Rails 3.1/3.2 Capistrano deployment
