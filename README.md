# cordova-iframe-test

* clone repository
* cd to root directory
* run `cordova build` (run `npm install -g cordova` if cordova is not already installed)
* open the XCode project in platforms/ios
* put a breakpoint in `Classes/MainViewController:shouldStartLoadWithRequest` (line 133-ish)
* Run the app
* The breakpoint will be hit a few times as the app launches (loading the main html file, loading the iframe, etc.)
* Click on the first link and when the breakpoint hits, in the XCode output window type "po navigationType" (it should be "...Clicked")
* The first link will open inside the iframe
* Relaunch the app
* Click the second link... in XCode `po navigationType` will still be "...Clicked" but the url will replace the app
* Relaunch the app
* Click the third item (window.open)... in Xcode `po navigationType` will be "...Other" and the url will replace the app
* Relaunch the app
* Click the fourth item (window.open)... in Xcode `po navigationType` will be "...Other" and the url will replace the app

* Static ads we've seen have followed the pattern of the second link (good)
* Third party ads are doing something along the lines of the third link (not good)

The key problem is that even though we are clicking the third item that has an event listener on click, it is not registering as a click event in the app (it appears the webview will only register a click when it is actually a link).
We want to know when it is a click because ALL requests go through here (as evidenced by the iframe loading initially on app launch).
