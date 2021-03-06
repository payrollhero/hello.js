


# hello.js

A client-side Javascript SDK for authenticating with [OAuth2](http://tools.ietf.org/pdf/draft-ietf-oauth-v2-12.pdf) (and **OAuth1** with a [oauth proxy](#oauth-proxy)) web services and querying their REST API's. HelloJS standardizes paths and responses to common API's like Google Data Services, Facebook Graph and Windows Live Connect. Its **modular** so that list is [growing](modules.html). No more spaghetti code! 



## Features

Looking for more? HelloJS supports a lot more actions than just getting the users profile. Like, matching users with a users social friends list, sharing events with the users social streams, accessing and playing around with a users photos. Lets see if these whet your appetite ...

<table>
	<thead>
		<tr>
			<th></th>
			<th>Windows</th>
			<th>FaceBook</th>
			<th>Google</th>
			<th><a href="#helloapi">More..</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<th class="method get"><a href="demos/profile.html">Profile: name, picture (email)</a></th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method get"><a href="demos/friends.html">Friends/Contacts: name, id (email)</a></th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method get">Albums, name, id, web link</th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method get"><a href="demos/albums.html">Photos in albums, names, links</a></th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method get">Photo file: url, dimensions</th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method post">Create a new album</th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td></td>
		</tr>
		<tr>
			<th class="method post"><a href="demos/upload.html">Upload a photo</a></th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td></td>
		</tr>
		<tr>
			<th class="method delete">Delete an album</th>
			<td>&#10003;</td>
			<td>&#10007;</td>
			<td></td>
		</tr>
		<tr>
			<th class="method get"><a href="demos/activities.html">Status updates</a></th>
			<td>&#10007;</td>
			<td>&#10003;</td>
			<td>&#10003;</td>
		</tr>
		<tr>
			<th class="method post"><a href="demos/share.html">Update Status</a></th>
			<td>&#10003;</td>
			<td>&#10003;</td>
			<td>&#10007;</td>
		</tr>
	</tbody>
</table>



- Items marked with a &#10003; are fully working and can be [tested here](./tests/). 
- Items marked with a &#10007; aren't provided by the provider at this time. 
- Blank items are work in progress, but there is good evidence that they can be done.
- Anything not listed i have no knowledge of and would appreciate input.



## Install

Download: [HelloJS](dist/hello.all.js) | [HelloJS (minified)](dist/hello.all.min.js)

Compiled source, which combines all the modules can be obtained from [Github](https://github.com/MrSwitch/hello.js/tree/master/dist), and source files can be found in [Source](https://github.com/MrSwitch/hello.js/tree/master/src).

### Bower Package

```bash
# Install the package manager, bower
npm install bower

# Install hello
bower install hello
```

The [Bower](http://bower.io/) package shall install the aforementioned "/src" and "/dist" directories. The "/src" directory provides individual modules which can be packaged as desired.

**Note:** Some services require OAuth1 or server-side OAuth2 authorization. In such case HelloJS communicates with an [OAuth Proxy](#oauth-proxy).

## Help &amp; Support


- [GitHub](https://github.com/MrSwitch/hello.js/issues) for reporting bugs and feature requests.
- [Stackoverflow](http://stackoverflow.com/questions/tagged/hello.js) to reach out for help.



## Quick Start
Quick start shows you how to go from zero to loading in the name and picture of a user, like in the demo above.


- [Register your app domain](#1-register)
- [Include hello.js script](#2-include-hellojs-script-in-your-page)
- [Create the signin buttons](#3-create-the-signin-buttons)
- [Setup listener for login and retrieve user info.](#4-add-listeners-for-the-user-login)
- [Initiate the client_ids and all listeners](#5-configure-hellojs-with-your-client_ids-and-initiate-all-listeners)


### 1. Register

Register your application with atleast one of the following networks. Ensure you register the correct domain as they can be quite picky


<!-- 
- [Windows Live](http://msdn.microsoft.com/en-us/library/hh243641.aspx)
- [Google+](http://code.google.com/apis/accounts/docs/OAuth2UserAgent.html)
- [FaceBook](http://developers.facebook.com/docs/reference/dialogs/oauth/)
-->
- [FaceBook](https://developers.facebook.com/apps)
- [Windows Live](http://dev.live.com/) (click on dashboard)
- [Google +](https://code.google.com/apis/console/b/0/#:access)


### 2. Include Hello.js script in your page

```html
<script class="pre" src="./dist/hello.all.min.js"></script>
```

### 3. Create the signin buttons
Just add onclick events to call hello( network ).login(). Style your buttons as you like, i've used [zocial css](http://zocial.smcllns.com), but there are many other icon sets and fonts

```html
<button onclick="hello( 'windows' ).login()">windows</button>
```


### 4. Add listeners for the user login

Lets define a simple function, which will load a user profile into the page after they signin and on subsequent page refreshes. Below is our event listener which will listen for a change in the authentication event and make an API call for data.


```javascript
hello.on('auth.login', function(auth){
	
	// call user information, for the given network
	hello( auth.network ).api( '/me' ).success(function(r){
		var $target = $("#profile_"+ auth.network );
		if($target.length==0){
			$target = $("<div id='profile_"+auth.network+"'></div>").appendTo("#profile");
		}
		$target.html('<img src="'+ r.thumbnail +'" /> Hey '+r.name).attr('title', r.name + " on "+ auth.network);
	});
});
```

### 5. Configure hello.js with your client_id's and initiate all listeners

Now let's wire it up with our registration detail obtained in step 1. By passing a [key:value, ...] list into the `hello.init` function. e.g....

```javascript
hello.init({ 
	facebook : FACEBOOK_CLIENT_ID,
	windows  : WINDOWS_CLIENT_ID,
	google   : GOOGLE_CLIENT_ID
},{redirect_uri:'redirect.html'});
```

That's it. The code above actually powers the demo at the start so, no excuses.


# Core Methods

## hello.init()

Initiate the environment. And add the application credentials 

### hello.init( {facebook: *id*, windows: *id*, google: *id*, .... } )

<table>
	<thead>
		<tr>
			<th>name</th>
			<th>type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>credentials</td>
			<td><i>object( key =&gt; value, ...&nbsp; )</i>
				<table>
					<thead>
						<tr>
							<th>name</th>
							<th>type</th>
							<th>example</th>
							<th>description</th>
							<th>argument</th>
							<th>default</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>key</td>
							<td><i>string</i></td>
							<td><q>windows</q>, <q>facebook</q> or <q>google</q></td>
							<td>App name"s</td>
							<td><em>required</em></td>
							<td>n/a</td>
						</tr>
						<tr>
							<td>value</td>
							<td><i>string</i></td>
							<td><q>0000000AB1234</q></td>
							<td>ID of the service to connect to</td>
							<td><em>required</em></td>
							<td>n/a</td>
						</tr>
					</tbody>
				</table>
			</td>
		</tr>
		<tr>
			<td>options</td>
			<td>set's default <i>options</i>, as in hello.login()</i></td>
		</tr>
	</tbody>
</table>

### Example:

```js
hello.init({
	facebook : '359288236870',
	windows : '000000004403AD10'
});
```

## hello.login()



If a network string is provided: A consent window to authenticate with that network will be initiated. Else if no network is provided a prompt to select one of the networks will open. A callback will be executed if the user authenticates and or cancels the authentication flow.

### hello.login( [network] [,options] [, callback() ] )

<table>
	<tr>
		<th>name</th>
		<th>type</th>
		<th>example</th>
		<th>description</th>
		<th>argument</th>
		<th>default</th>
	</tr>
	<tr>
		<td>network</td>
		<td><i>string</i></td>
		<td><q>windows</q>, <q>facebook</q></td>
		<td>One of our services.</td>
		<td><em>required</em></td>
		<td><em>null</em></td>
	</tr>
	<tr>
		<td>options</td>
		<td colspan="5"><i>object</i>
			<table>
				<tr>
					<th>name</th>
					<th>type</th>
					<th>example</th>
					<th>description</th>
					<th>argument</th>
					<th>default</th>
				</tr>
				<tr>
					<td>display</td>
					<td><i>string</i></td>
					<td><q>popup</q>, <q>page</q> or <q>none</q></td>
					<td>How the signin flow should work, "none" is used to refresh the access_token in the background</td>
					<td><em>optional</em></td>
					<td><q>popup</q></td>
				</tr>
				<tr>
					<td>scope</td>
					<td><i>string</i></td>
					<td><q>email</q>, <q>publish</q> or <q>photos</q></td>
					<td>Comma separated list of <a href="#scope">scopes</a></td>
					<td><em>optional</em></td>
					<td><em>null</em></td></tr>
				<tr>
					<td>redirect_uri</td>
					<td><i>string</i></td>
					<td><q><a href="redirect.html" target="_blank">redirect.html</a></q></td>
					<td>
					A full or relative URI of a page which includes this script file hello.js</td>
					<td>
					<em>optional</em></td>
					<td>
					<em>window.location.href</em></td>
				</tr>
				<tr>
					<td>response_type</td>
					<td><i>string</i></td>
					<td><q>token</q>, <q>code</q></td>
					<td>Mechanism for skipping auth flow</td>
					<td><em>optional</em></td>
					<td><q>token</q></td>
				</tr>
				<tr>
					<td>force</td>
					<td><i>Boolean</i></td>
					<td><i>false</i>: return current session else initiate auth flow; <i>true</i>: Always initiate auth flow</td>
					<td>Mechanism for authentication</td>
					<td><em>optional</em></td>
					<td><i>true</i></td>
				</tr>
			</table>
		</td>
	</tr>
	<tr>
		<td>callback</td>
		<td><i>function</i></td>
		<td><code>function(){alert("Logged in!");}</code></td>
		<td>A callback when the users session has been initiated</td>
		<td><em>optional</em></td>
		<td><em>null</em></td>
	</tr>
</table>

### Examples:

```js
hello( "facebook" ).login( function(){
	alert("You are signed in to Facebook");
});
```


## hello.logout()



Remove all sessions or individual sessions.

### hello.logout( [network] [, options ] [, callback() ] )

<table>
	<tr>
		<th>name</th>
		<th>type</th>
		<th>example</th>
		<th>description</th>
		<th>argument</th>
		<th>default</th>
	</tr>
	<tr>
		<td>network</td>
		<td><i>string</i></td>
		<td>
			<q>windows</q>,
			<q>facebook</q>
		</td>
		<td>One of our services.</td>
		<td>
			<em>optional</em>
		</td>
		<td>
			<em>null</em>
		</td>
	</tr>
	<tr>
		<td>options</td>
		<td colspan="5"><i>object</i>
			<table>
				<tr>
					<th>name</th>
					<th>type</th>
					<th>example</th>
					<th>description</th>
					<th>argument</th>
					<th>default</th>
				</tr>
				<tr>
					<td>force</td>
					<td><i>boolean</i></td>
					<td><i>true</i></td>
					<td>If set to true, the user will be logged out of the providers site as well as the local application. By default the user will still be signed into the providers site.</td>
					<td>
						<em>optional</em>
					</td>
					<td><i>false</i></td>
				</tr>
			</table>
		</td>
	</tr>
	<tr>
		<td>callback</td>
		<td><i>function</i></td>
		<td>
			<code>function(){alert("Logged in!");}
			</code>
		</td>
		<td>
			A callback when the users session has been initiated</td>
		<td>
			<em>optional</em>
		</td>
		<td>
			<em>null</em>
		</td>
	</tr>
</table>

### Example:

```js
hello( "facebook" ).logout(function(){
	alert("Signed out");
});
```


## hello.getAuthResponse()



Get the current status of the session, this is an synchronous request and does not validate any session cookies which may have expired.

### hello.getAuthResponse( network );

<table>
	<tr>
		<th>name</th>
		<th>type</th>
		<th>example</th>
		<th>description</th>
		<th>argument</th>
		<th>default</th>
	</tr>
	<tr>
		<td>network</td>
		<td><i>string</i></td>
		<td><q>windows</q>, <q>facebook</q></td>
		<td>One of our services.</td>
		<td><em>optional</em></td>
		<td><em>current</em></td>
	</tr>
</table>

### Examples:

```js
var online = function(session){
	var current_time = (new Date()).getTime() / 1000;
	return session && session.access_token && session.expires > current_time;
};

var fb = hello( "facebook" ).getAuthResponse();
var wl = hello( "windows" ).getAuthResponse();

alert(( online(fb) ? "Signed":"Not signed") + " into FaceBook, " + ( online(wl) ? "Signed":"Not signed")+" into Windows Live");
```

## hello.api()



Make calls to the API for getting and posting data

### hello.api( [ path ], [ method ], [ data ], [ callback(json) ] )

<table>
	<tr>
		<th>name</th>
		<th>type</th>
		<th>example</th>
		<th>description</th>
		<th>argument</th>
		<th>default</th>
	</tr>
	<tr>
		<td>path</td>
		<td><i>string</i></td>
		<td>
			<q>/me</q>,
			<q>/me/friends</q>
		</td>
		<td>Path or URI of the resource. See <a href="#REST API">REST API</a>, Can be prefixed with the name of the service</td>
		<td>
			<em>required</em>
		</td>
		<td>null</td>
	</tr>
	<tr>
		<td>method</td>
		<td>
			<q>get</q>,
			<q>post</q>,
			<q>delete</q>,
			<q>put</q>
		</td>
		<td>See
			<em>type</em>
		</td>
		<td>HTTP request method to use.</td>
		<td>
			<em>optional</em>
		</td>
		<td>
			<q>get</q>
		</td>
	</tr>
	<tr>
		<td>data</td>
		<td><i>object</i></td>
		<td>
			<code>{name:<q>Hello</q>, description:<q>Fandelicious</q>}</code>
		</td>
		<td>
			A JSON object of data, FormData, HTMLInputElement, HTMLFormElment to be sent along with a
			<q>get</q>,
			<q>post</q>or
			<q>put</q>request
		</td>
		<td>
			<em>optional</em>
		</td>
		<td>
			<em>null</em>
		</td>
	</tr>
	<tr>
		<td>callback</td>
		<td><i>function</i></td>
		<td>
			<code>function(json){console.log(json);}</code>
		</td>
		<td>
			A function to call with the body of the response returned in the first parameter as an object, else boolean false
		</td>
		<td>
			<em>optional</em>
		</td>
		<td>
			<em>null</em>
		</td>
	</tr>
</table>


### Examples:

```js
hello( "facebook" ).api("me").success(function(json){
	alert("Your name is "+ json.name);
}).error(function(){
	alert("Whoops!");
});
```

# Event subscription

## hello.on()

Bind a callback to an event. An event maybe triggered by a change in user state or a change in some detail. 

### hello.on( event, callback );


<table>
	<thead>
		<tr>
			<th>event</th>
			<th>description</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>auth</td>
			<td>Triggered whenever session changes</td>
		</tr>
		<tr>
			<td>auth.login</td>
			<td>Triggered whenever a user logs in</td>
		</tr>
		<tr>
			<td>auth.logout</td>
			<td>Triggered whenever a user logs out</td>
		</tr>
		<tr>
			<td>auth.update</td>
			<td>Triggered whenever a users credentials change</td>
		</tr>
	</tbody>
</table>


### Example:

```js
var sessionstart =  function(){
	alert("Session has started");
};
hello.on("auth.login",sessionstart);
```


## hello.off()

Remove a callback, both event name and function must exist

### hello.off( event, callback );

```js
hello.off("auth.login",sessionstart);
```


# Misc

## Pagination, Limit and Next Page
A convenient function to get the `next` resultset is provided in the second parameter of any `GET` callback. Calling this function recreates the request with the original parameters and event listeners. Albeit the original path is augmented with the parameters defined in the `paging.next` property.

```js
hello( "facebook" ).api( "me/friends", {limit: 1} ).success( function( json, next ){
	if( next ){
		if( confirm( "Got friend "+ json.data[0].name + ". Get another?" ) ){
			next();
		}
	}
	else{
		alert( "Got friend "+ json.data[0].name + ". That's it!" );
	}
}).error( function(){
	alert("Whoops!");
});
```


## Scope
The scope property defines which privileges an app requires from a network provider. The scope can be defined globally for a session through `hello.init(object, {scope:'string'})`, or at the point of triggering the auth flow e.g. `hello('network').login({scope:'string'});`
An app can specify multiple scopes, seperated by commas - as in the example below.

```js
hello( "facebook" ).login( {scope: "friends,photos,publish" } );
```

Scopes are tightly coupled with API requests, which will break if the session scope is missing or invalid. The best way to see this is next to the API paths in the [hello.api reference table](http://adodson.com/hello.js/#helloapi).

The table below illustrates some of the default scopes HelloJS exposes. Additional scopes may be added which are proprietary to a service, but take careful not to mix proprietary scopes with other services which dont know how to handle them.
<table>
	<thead>
	<tr>
		<th>Scope</th>
		<th>Description</th>
	</tr>
	</thead>
	<tbody>
		<tr>
			<th><i>default</i></th>
			<td>Read basic profile</td>
		</tr>
		<tr>
			<th><q>friends</q></th>
			<td>Read friends profiles</td>
		</tr>
		<tr>
			<th><q>photos</q></th>
			<td>Read users albums and photos</td>
		</tr>
		<tr>
			<th><q>files</q></th>
			<td>Read users files</td>
		</tr>
		<tr>
			<th><q>publish</q></th>
			<td>Publish status updates</td>
		</tr>
		<tr>
			<th><q>publish_files</q></th>
			<td>Publish photos and files</td>
		</tr>
	</tbody>
</table>

Its good practice to limit the use of scopes and also to make users aware of why your app needs certain privilieges. Try to update the permissions as a user delves further into your app. For example: If the user would like to share a link with a friend; Include a button which the user has to click to trigger the hello.login with the 'friends' scope, and then the handler triggers the API call after authorisation.


## Error handling

For hello.api([path], [*callback*]) the first parameter of *callback* 
upon error will be either *boolean (false)* or be an error *object* as 
described below.

### Error Object

<table>
	<thead>
		<tr>
			<th>name</th>
			<th>type</th>
		</tr>
	</thead>
	<tbody>
		<tr>
			<td>error</td>
			<td><i>object</i>
				<table>
					<thead>
						<tr>
							<th>name</th>
							<th>type</th>
							<th>example</th>
							<th>description</th>
							<th>argument</th>
							<th>default</th>
						</tr>
					</thead>
					<tbody>
						<tr>
							<td>code</td>
							<td><i>string</i></td>
							<td>
								<q>request_token_unauthorized</q>
							</td>
							<td>Code</td>
							<td>
								<em>required</em>
							</td>
							<td>n/a</td>
						</tr>
						<tr>
							<td>message</td>
							<td><i>string</i></td>
							<td class="auto-style2">The provided access token....</td>
							<td>
								Error message</td>
							<td>
								<em>required</em>
							</td>
							<td>n/a</td>
						</tr>
					</tbody>
				</table>
			</td>
		</tr>
	</tbody>
</table>



## Extending the services
Services are added to HelloJS as "modules" for more information about creating your own modules and examples, go to [Modules](./modules.html)

## OAuth Proxy

Services which rely on the OAuth 1 or OAuth2 token exchange authentication methods require a server side handshake with the secret key. This is unlike client-side OAuth 2 which doesn't need a secret and verifies the app based on the redirect_uri property.

Making HelloJS work with secret keys requires a web services where the secret key is used to authorize the user and sign subsequent API requests in the case of OAuth1. As a shim HelloJS uses a service hosted at [https://auth-server.herokuapp.com/](https://auth-server.herokuapp.com/). Developers may add their own network registration AppID/client_id and secret to this service in order to get up and running.

The aforementioned service uses [//node-oauth-shim](https://npmjs.org/package/oauth-shim), so if you want to roll your own please look there for installation instructions and usage. Override the default path for this service in HelloJS at the point of calling hello.init, e.g. `hello.init(credentials, {oauth_proxy:'//path/to/oauth-shim-proxy'})`.


## Browser Support

<table>
	<thead>
	<tr>
		<td>Browser</td>
		<th><div class="ie9"></div>IE10</th>
		<th><div class="ie9"></div>IE9</th>
		<th><div class="ie8"></div>IE8</th>
		<th><div class="ie7"></div>IE7</th>
		<th><div class="ff"></div>FF</th>
		<th><div class="cr"></div>CR</th>
		<th><div class="sa"></div>SA</th>
		<th><div class="op"></div>OP</th>
		<th><div class="op"></div>Mob</th>
		<th><div class="op"></div>Mini5</th>
		<th>iOS</th>
		<th>WP 7</th>
	</tr>
	</thead>
	<tbody>
	<tr>
		<th>hello.js</th>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;<sup>1,2</sup></td>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;</td>
		<td>&#10003;<sup>3</sup></td>
		<td>&#10003;</td>
		<td>&#10003;<sup>4</sup></td>
	</tr>
	</tbody>
</table>



- IE7: Makes beeping sounds whenever the POST, PUT or DELETE methods are 
used - because of the XD, IFrame+Form+hack.- 
- IE7: Requires JSON.js and localStorage shims
- Opera Mini: Supports inline consent only, i.e. reloads original page.
- WP7: Supports inline consent only, i.e. reloads original page.




## Phonegap Support

HelloJS can also be run on phonegap applications. Checkout the demo [hellojs-phonegap-demo](https://github.com/MrSwitch/hellojs-phonegap-demo)


## Contributing

**"No, It's perfect!"**.... If you believe that then give it a [star](https://github.com/MrSwitch/hello.js).

Having read this far you have already invested your time, why not contribute!?

HelloJS is constantly evolving, as are the services which it connects too. So if you think something could be said better, find something buggy or missing from either the code, documentation or demos then please put it in, no matter how trivial.


### Changing code?
Ensure you setup and test your code on a variety of browsers.

```bash
# Using NodeJS on your dev environment
# cd into the project root and install dev dependencies 
npm install -l

# run continuous integration tests
grunt test
```



