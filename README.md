[Add Webex photo]: # 
# Embedding a Webex SDK video 'Space Widget' into a web app

Take a standard HTML web page and embed a Webex SDK 'Space Widget' to easily enable Webex collaboration.
 
[Add a gif of end product]: #  

### Pre-Reqs
- Firefox web browser: https://www.mozilla.org/
**or**
- Google Chrome web browser: https://www.google.com/chrome/
- (Recommended) Git software version control: https://git-scm.com/
- Webex account

<details>
<summary>Tutorial</summary>

### Installation
To embed a Space Widget into a web page, we first need to have a web page! A basic HTML/CSS project for the fictitious 'OneBank' company can be downloaded from GitHub, or cloned via Git:
 
#### Obtain your Webex Personal Access Token 

[Webex Access Token!](https://developer.webex.com/docs/getting-started)
> Important: Copy Personal Access Token for later use
 
![CleanShot-Google Chrome202207-14 at 11 17 36](https://user-images.githubusercontent.com/9085386/179029658-ecdb4a93-b1a1-47c9-8d87-4a0af12e15db.png)

#### Clone repo 
```
git clone https://github.com/CiscoDevNet/devnet-express-cloud-collab-code-samples.git
cd /devnet-express-cloud-collab-code-samples/itp/collab-spark-video-sdk-widget-meet/onebank
```

#### Configure

1. In the `/devnet-express-cloud-collab-code-samples/itp/collab-spark-video-sdk-widget-meet/onebank` folder
2. Open `webex-teams.html` in an editor
  -- add your Webex personal access token and an email address to a webex user that is not yourself. Save the file
3. Load up the main web application file onebank.html into your editor
  - Find Line 69, which defines the blue button on the page labeled 'Ask Sandy':
 `<input type='submit' value='Ask Sandy' name='submit' class='submit' onclick='' />`
> Note: the `oneclick` handler is empty
4. Place `window.open("webex-teams.html","","height=500,width=450")` inside the `onclick=''`
> Example:  
```
<input type='submit' value='Ask Sandy' name='submit' class='submit' onclick='window.open("webex-teams.html","","height=500,width=450")' />

```
#### Test Configuration

1. Open the onebank.html file in your browser
2. Right-click on the 'Ask Sandy' button, and choose Inspect Element
> Note: You should see the window.open JavaScript code you entered above:
3. Click 'Ask Sandy'
4. Check to see that your pop-up window appears, and automatically starts loading the Webex Space Widget. Collaborate away!
</details>

<details>
<summary>How-to</summary>
Overview
Webex is a cloud service providing persistent chat, room-based collaboration, WebRTC video conferencing, and more. Developers can easily integrate solutions with Webex via the Webex REST API - for example to add Webex messaging features to an app user interface, or to automate sending Webex messages to rooms based on business system or real-world events.

Screen Shot

In addition to automating the messaging features of Webex via the REST API, developers can also integrate the rich real-time collaboration features of Webex - including voice, video, file-sharing, and more - via the Webex widgets docs.

In this Lab we will focus on the Space Widget, which makes it easy to place a full-featured Webex user-interface directly into any HTML web application.

What's a widget?
A Webex SDK widget provides a (mostly) self-contained HTML/CSS user-interface (built using the React JavaScript framework) providing Webex functionality that lives in an HTML "container" element in a web page.

Embedded

Typically the container is a <div> element which is given a position in the page layout by virtue of its inclusion in the page HTML, as well as a specified height and width that can be adjusted to fit the application needs and/or responsive design.

All of the functionality inside the widget container - the JavaScript logic, WebRTC media management, event-processing, back-end signaling, etc. - is handled by a JavaScript bundle that is incorporated into the HTML via a <script> import element, such as:

<script src="https://code.s4d.io/widget-space/latest/bundle.js"></script>
Widget types
Currently there are two Webex widgets. The first is a Space Widget that encapsulates the messaging, video calling, file-management, etc. functionality of a Webex 1-to-1 or group Space:

Embedded

Note that the Space Widget will represent usage of Webex from one user, and to either another user (for a 1-on-1 collaboration) or a group Space. When the widget is initialized (which can happen statically based on info in the HTML at the time the page loads, or dynamically via JavaScript) it automatically connects to the Webex cloud and launches into the 1-on-1 or Space user-interface.

The second available widget is the Recents Widget which wraps the functionality of the 'recent conversation' list found in the Webex clients. This Widget lists recent/unread Spaces, as well as provides JavaScript hooks and events for tying the Recents Widget to a Space Widget to coordinate selecting Spaces and updating the UI on incoming calls, messages, etc.

Embedded

In this Lab we will be experimenting with the Space Widget.

Widget code management
There are two basic methods for delivering widget source code (which will end up being a bundle.js JavaScript library imported into the web app):

You can download and compile the Webex widget source code project from GitHub, which will output a bundle.js file, then manage and deliver that bundle.js along with the rest of your web app source files.
You can rely on the Webex cloud content delivery network (CDN) to serve the bundle.js dynamically via a simple URL reference.
There are advantages to either approach. By compiling/managing the widget code yourself you can guarantee a set of functionality and features that you and your users have verified, but you will have to maintain the widget code base and manage any future updates or fixes. If you use the CDN delivery method, then your app does not have to manage (or even serve) the widget code and any new features or security updates are available immediately and automatically; however, you lose some control over the exact version/features of the widget and are reliant on the CDN being reachable/available.

In this lab we will use the Webex CDN to serve the widget code and CSS.

Step 1: Download the sample code project
To embed a Space Widget into a web page, we first need to have a web page! A basic HTML/CSS project for the fictitious 'OneBank' company can be downloaded from GitHub, or cloned via Git:

git clone https://github.com/CiscoDevNet/devnet-express-cloud-collab-code-samples.git
Note: we will be using the project in the folder ../devnet-express-cloud-collab-code-samples/itp/collab-spark-video-sdk-widget-meet

  Step 2: Obtaining the Webex access token
Webex for Developers

https://developer.webex.com
To try out a 1-on-1 widget video call, we will need information about two Webex user accounts: one being the From user - on whose behalf we will be making the call - the other being the To user, that is, the target person who we want to make a call to. (A Webex widget can also connect to a group Space, but in this exercise we will be performing a 1-on-1 connection.)

As a secure platform, Webex requires that our web application has the proper authorization to act on behalf of the From user. If you are familiar with the Webex REST API, you may know that you will need an access token to perform API operations - the widgets are no different: you will need to obtain a Webex access token for your From user to use the widget.

There are a several ways of obtaining a Webex API access token:

You can grab a "personal access token" by simply logging into the Webex for Developers site, then clicking Documentation / Getting Started: https://developer.webex.com/docs/api/getting-started

Note, this 'developer' access token has full API permissions, but expires after a relatively short time. You can use this token for development purposes (as we will here), but you should not use this kind of token in production

By creating a Webex 'bot' application account. Anyone can create a bot account, and use the generated bot access token to perform actions on behalf of the bot user.

However, note that any calls made using this access token will appear to be coming from the bot account (not an actual user), which may not be what you want. In addition, your app will need to ship with this access token embedded in it and send it to client-side code, making it vulnerable to exposure

By using the Webex OAuth mechanism and request flow, your web application can allow individual Webex users to login to your app, providing your application with a limited-life-time access token which your app can use to perform API operations on the user's behalf (like place a video call)

Note, the widgets require that you define your Webex integration application with the spark:all permission scope

For more details on Webex OAuth2 authentication, see Webex for Developers - Apps & OAuth

By dynamically creating (or reusing) a Guest User Webex account and authenticating it via a JSON Web Token (JWT).

This authentication mechanism lets you create application users on-the-fly, and requires creating a Guest Issuer application type

Let's grab the 'From' access token
Log in to the Webex developer page using the From user account (which could just be your personal account)

Once logged in, navigate to the Documentation / Getting Started page: https://developer.webex.com/docs/api/getting-started

Copy your developer access token and place it in a safe place, as you'll be using it in future steps of this lab:

Developer Access Token

The target user
Specifying the target user is a lot easier! To use the Space Widget to place a 1-on-1 video call, we will need either the Webex user email account or the user's Webex API unique ID. In this exercise we will simply use the Webex email address of another account you want to try to make a call to.

Double Check: Don't use the same account for both the From access token and the To target email account - your widget will not be able to load. You may want to create a second free account with a separate email

Makes sure you have a device (like your mobile phone, or a friend's phone) that is logged into Webex using the To user account, so we can test our video call functionality in a bit...
  
Step 3: Making a simple widget video test call
Now that you have identified your From user's access token, and your To user's email address, let's create a super simple web page to test the Webex Space Widget video call functionality:

From your file explorer or from the command line, navigate into the sample project code downloaded previously with Git. Look for a file named webex-teams.html in the directory:

 ../devnet-express-cloud-collab-code-samples/itp/collab-spark-video-sdk-widget-meet/onebank
Using your favorite text editor, go ahead and open up webex-teams.html for editing

Note: You may find a light-weight 'developer' editor with syntax-highlighting helpful, like Visual Studio Code

 <!-- Latest compiled and minified CSS -->
 <link rel="stylesheet" href="https://code.s4d.io/widget-space/production/main.css">
 <!-- Latest compiled and minified JavaScript -->
 <script src="https://code.s4d.io/widget-space/production/bundle.js"></script>

 <div
 class="webexteams-widget"
 data-toggle="webex-space"
 data-access-token="WEBEX_TEAMS_ACCESS_TOKEN"
 data-destination-id="TARGET_USER_EMAIL"
 data-destination-type="email"
 />
Let's take a look at the HTML code for this page and see what's going on:

Line 2: Via this <link> reference to the Webex CDN server, the page retrieves the latest CSS style data for the widget:

<link rel="stylesheet" href="https://code.s4d.io/widget-space/production/main.css">
Line 4: Via this <script> element, the core Space Widget JavaScript code is downloaded from the Webex CDN:

<script src="https://code.s4d.io/widget-space/production/bundle.js"></script>
Line 6-12: Here an HTML <div> element is defined, which represents the browser layout 'real estate' where the Space Widget loads and runs (in this case, taking up the entire browser window).

Note there is no explicit content in the <div> itself, but there are some special HTML element attributes that identify this <div> as the target for loading the widget code, as well as identifying the From and To users:

<div
class="webexteams-widget"
data-toggle="webex-space"
data-access-token="WEBEX_TEAMS_ACCESS_TOKEN"
data-destination-id="TARGET_USER_EMAIL"
data-destination-type="email"
/>
Looking more closely at the special <div> attributes:

data-toggle: this attribute identifies the type of Webex widget that should be loaded. webex-space represents the Space Widget type we're experimenting with in this lab

data-access-token: this value should be the Webex access token of the From user previously identified

data-destination-id: this is the unique identifier of the To user - the person we want to make a video call to

data-destination-type: this is the "type" of the destination Id just above - in this case it will be email

Let's go ahead and make the needed modifications to this file and test it out:

Go ahead and modify the attribute values for WEBEX_TEAMS_ACCESS_TOKEN and TARGET_USER_EMAIL with the appropriate values identified in the previous steps.

Be sure to Save the file!

Using either Firefox or Google Chrome, open the webex-teams.html file and let's see what happens!

If everything went well, you should see a Webex-like user interface representing the details of a 1-on-1 collaboration session between your From user and your To user.

Note you can send messages, share files, and even switch to the Call activity via the activities menu button activity button and make a video call

Note: as this is a simple page where the <div> element is not constrained to any particular set of height/width parameters, the Webex widget UI takes up the entire browser window area

Go ahead and terminate any calls in progress and close your widget browser window.

  Step 4: Modifying the main application page
Let's use this bit of code we just tested to embed Webex Space Widget functionality in a 'real' web page!

Load up the main web application file onebank.html into your editor

Find Line 69, which defines the blue button on the page labeled 'Ask Sandy':

 <input type='submit' value='Ask Sandy' name='submit' class='submit' onclick='' />
Note that the onclick event-handler attribute is currently empty

Carefully place the following JavaScript code inside the onclick handler quotes:

 window.open("webex-teams.html","","height=500,width=450")
like so:

 <input type='submit' value='Ask Sandy' name='submit' class='submit' onclick='window.open("webex-teams.html","","height=500,width=450")' />
Double Check: be sure to keep your single-quotes and double-quotes straight (and matched up), and make sure your result looks exactly like the above snippet

Go ahead and Save the file

What Does the Code Do?
By providing some JavaScript code for the <input> button's onclick handler, we have turned what was a previously function-less 'Ask Sandy' button into an active button.

When this button is clicked, it executes the specified JavaScript code, which will open a 'pop-up' window (or possibly a new tab, depending on your browser preferences) and load it with the contents of the webex-teams.html page.

Note we've also provided height and a width parameters to keep the size of the pop-up window constrained so we can continue to view most of the main web app content

Check your work
Open the onebank.html file in your browser

Right-click on the 'Ask Sandy' button, and choose Inspect Element

Note that you should see the window.open JavaScript code you entered above:

inspect

(You can close the browser tools window at this point)

Click 'Ask Sandy'

Check to see that your pop-up window appears, and automatically starts loading the Webex Space Widget. Collaborate away!
  
Step 5: Don't try this at home
Please note that this sample app is quite simplistic, in order to quickly demonstrate the basic widget functionality. A production-quality application would likely implement many of the below enhancements:

Use OAuth to dynamically (and securely) obtain the From user's Webex access token: Remember to configure your Webex integration to use and request the spark:all scope

Dynamically create/load the widget container: This sample 'hard codes' all of the HTML code and special attributes needed to instantiate the widget. It is certainly possible to use JavaScript and the browser DOM, along with some special objects present in the Webex widget JavaScript import, to create/instantiate/destroy widgets and their containers as needed by the app

Integrate the page and widgets with Webex widget JavaScript events and 'hook' functions: The widgets expose events (such as when new messages are created, or a new video call is started) that you can use to more tightly integrate your app's user experience with the widget functionality

Re-skin or customize the look and feel of the widgets: The Webex widget code is fully open source, and,depending on your level of experience and willingness to get your hands dirty, it is quite possible to modify that code (that is, to change the CSS and other graphics) to suit your application needs

For more details about the Webex widgets functionality, API references and links to the source code repositories, see the Webex Widgets page.

</details>

### Links
[webex]: https://developer.webex.com
[emacs]: https://www.gnu.org/software/emacs/

## Contributing
See [contributing guide](.github/CONTRIBUTING.md)
