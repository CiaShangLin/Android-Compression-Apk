# ***減少APK Size***
為何要壓縮APK : 根據google資料統計,APK的Size越小對於使用者安裝的意願越高,尤其是使用低階手機的地區,例如:印度,非洲...

# **如何減少APK Size**
1. 壓縮png圖檔,一個apk通常圖檔佔了五到七成的部分,所以壓縮圖檔佔了一個很重要的部分,有幾種方法可以使用
    - 第一種[TinyPNG](https://tinypng.com/ "TinyPNG")透過這個網站可以壓縮png,但是要注意這個網站的壓縮比滿高的,如果你肉眼都能看出差距了還是建議問一下設計或是PM接不接受。
    - 第二種window有FileOptimizer,mac有ImageOptim,這個是我比較常使用的,壓縮比比較低幾乎是看不出差異,跟TinyPng比較來就是減少的大小比較少。
    - 第三種Android Studio自身有提供png轉webP的功能,大小又能在更小一點。
    - 第四種把hdpi跟mdpi drawable刪掉,一般來說drawable有五種大小來提供各種不同機型的使用,但是hdpi跟mdpi在目前的android來說已經太小了,不過如果APP有很多低階的android用戶還是不建議刪除,另一種方案就是使用SVG向量圖這種就只有一個xml更省大小但是要看設計能不能出SVG的。
    - 第五種google play bundle他能夠依使用者的手機打包出對應的APK,但我實際沒什麼用過,所以不是很了解。

2. 混淆程式碼 : 將混淆打開 minifyEnabled true,他可以將程式碼的程度減少,例如:MyClass->a,相對來說字節就會少很多,混淆不只可以減少程式碼所佔的空間,也有最基本的防破解功能,可是有些東西要避免混淆。
    例如:Gson要用的model class,Jni native要用的function,用到反射的地方...,這些地方要特別注意避免混淆,不然debug好好的release馬上爆開給你看,爾且錯誤訊息都會是很奇怪的那種,比如說第五行imageview找不到這種。

3. 混淆資源檔 : 抖音的[AabResGuard](https://github.com/bytedance/AabResGuard "AabResGuard")道理和上面的程式碼一樣,但是要記得加入白名單,不然有些第三方的被混淆到你也不知道,但是我不推薦使用啦,尤其是當你接手某個專案你不知道那些該加白名單,會造成自己很大的困擾。

4. 減少so庫種類 : 有用過opencv或是其他第三方的so庫的通常都會放入一個叫做.so檔的東西,這東西其實滿佔空間的,在新版的NDK 20.xx版本支援的ndk只剩下四種了,有一些APP會只放armeabi-v7a因為它支援全部,但是使用不同的指令集架構效能肯定低於專用的,又或者是進去了APP之後再去抓取對應它指令集的so。
>     ndk {
            abiFilters "armeabi-v7a", "arm64-v8a", "x86", "x86_64"
        }

5. 清除無用的程式碼 : 推薦使用Lint,這東西可以幫你產出一份html和xml的檔案告訴妳很多東西,例如:無用的程式碼,無用的資源檔...,但是特別注意Lint有提供自動清除功能強烈建議不要使用,如果要用的話建議開個分支測試,這東西非常恐怖可能會清除到有在的,實際經歷。


<!--more-->
以後有想到什麼再補充
