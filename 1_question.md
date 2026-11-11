**Question_1.** You need to test new feature on a mobile client, but API for it is implemented only on the staging environment at that moment. URL of the staging environment - https://staging.test.planner5d.com/ URL of the production environment - https://production.test.planner5d.com/ How can you test this new feature using the staging environment?

**Answer**
To test the new feature using the staging environment If the client I have is configured for production, I would utilize either a configuration-based approach or a network proxy tool, depending on the available builds.

1. Environment Specific Build: I would request a staging  build (APK/IPA) from the development team where the base URL is natively set to  https://staging.test.planner5d.com/. This is the preferred approach because it is the most reliable, requires no additional network setup or third-party tools, and completely avoids potential SSL certificate issues. 

2. App Debug Menu: Alternatively, if the application has a built-in developer/QA menu, I would use it to switch the environment endpoints from prod to staging directly on the device without reinstalling the app.


3. Network Proxy Approach (Using Charles Proxy)
If the above options are not available and testing must be performed on a build with a hardcoded production URL, I would use a web debugging proxy, such as Charles Proxy, to intercept and reroute the traffic. The configuration involves the following steps:

Step 1: Network Setup
Connect both the mobile device and the computer running Charles Proxy to the same Wi-Fi network. Configure the mobile device's Wi-Fi settings to use a manual proxy, entering the computer's local IP address and the default Charles port (8888).

Step 2: SSL Decryption
To intercept HTTPS traffic, download the Charles Root Certificate via the mobile browser (chls.pro/ssl) and install it on the device (ensuring full trust is enabled in the device settings). Then, in Charles Proxy, navigate to SSL Proxying Settings and enable it specifically for production.test.com.

Step 3: Traffic Rerouting (Map Remote)
Open the Map Remote tool in Charles and create a new rule with the following parameters:

Map From:
Protocol: https
Host: production.test.planner5d.com
Map To:
Protocol: https
Host: staging.test.planner5d.com

**Result:**
Once configured, any request sent by the mobile application to the production URL will be intercepted by Charles and seamlessly redirected to the staging API. The mobile client will receive and process the response from the staging environment, allowing full validation of the new feature without requiring any code modifications.