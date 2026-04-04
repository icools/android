android tools emerge apk 
snapshot
https://www.emergetools.com/

這blog將meta最近推出的Threads拆解了一下，得到以下結果
![alt text](image.png)
Become a Medium member
Android 用了部分的react native
2.有一個很大的JNI so ,18.4Mb
3.dex很小，代表程式碼很少，然後很大量的使用了Compose
4.把debug code包了進去
5.IOS版本超肥,244Mb，Android 72.3Mb
Press enter or click to view image in full size

然後再仔細一看，原來這間公司是提供上傳apk然後幫你分析成dashboard的，可以看到很多更詳細的資訊

Emerge | Com.instagram.barcelona App Size Analysis
This is an example of an Emerge Tools app size analysis of the Com.instagram.barcelona Android app. Companies like…
www.emergetools.com

android asset 類的居然比resource 還大。

https://www.emergetools.com/app/example/android/com.instagram.barcelona?buildContent=breakdown&source=post_page-----fddb76e2173d---------------------------------------