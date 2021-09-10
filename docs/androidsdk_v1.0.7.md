* [EQWorks Android MobileSDK](#eqworks-android-mobilesdk)
* [Installation](#installation)
* [Permission](#permission)
* [Configuration](#configuration)
* [Client Methods](#client-methods)
	- [logUser](#loguser)
	- [logEvent](#logevent)
* [TroubleShooting](#troubleshooting)

### EQWorks Android MobileSDK

EQWorks Android MobileSDK allows app developers to log events from client applications to EQ servers. The SDK and the examples provided in this document are developed in Kotlin.

### Installation

We will use JitPack to distribute the Android MobileSDK. To install the library in an Android project, add the following lines.

Project-level build.gradle file

```
repositories {
	maven { url 'https://jitpack.io' }
}
```
Application-level build.gradle file
```
dependencies {
	implementation 'com.github.eqworks(........to be updated))'
}
```
### Permission

Add below permissions in AndroidManifest.xml for SDK
```
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```
### Configuration

Please configure the SDK to work seamlessly with EQ servers. There are two steps to follow.

1. EQWorks will provide a JWT access token to client application developers for authentication. Please Set this token in the SDK by calling "setAppDevToken". It is sufficient to set the token in SDK only once. Afterwards, if you need to update the token in SDK, you can use this method
```
EQLibrary.Settings(applicationContext).configurationSettings.setAppDevToken(appDevToken)
```
2. The MobileSDK supports data caching. Client applications can decide to upload a single data or batch data to the server. The SDK supports caching of 1 to 20 data in the SDK database. Using the ‘setQueueSize’ method, you can set any number between 1 to 20 as cache size. Default is 20.
```
EQLibrary.Settings(applicationContext).configurationSettings.setQueueSize(queueSize)
```
3. The SDK uses a background WorkManager that runs every six hours to check if anything has been stored in the queue and not uploaded to the server.

### Client Methods

#### logUser

This SDK call helps to identify a user and relates him to his activities. It is sufficient to call this method once. You can also call this method to update attributes for a user. This is a suspend function. Please call from a coroutine or another suspend function.

Method

```
suspend fun logUser(appUserId: String, 
attributes: JsonObject = emptyJsonObject, location: LocationC?): Int
```
Method call example

```
val eqObj = EQLibrary.getInstance(applicationContext)

CoroutineScope(Dispatchers.IO).launch{val res = eqObj.logUser("abc45678", null)}

CoroutineScope(Dispatchers.IO).launch{
		val res = eqObj.logUser("abc45678", param12, null)}

CoroutineScope(Dispatchers.IO).launch{
		val res = eqObj.logUser("abc45678", param12, EQLibrary.LocationC(34.56788976,-24.6754336)}
```
<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>appUserId

</td>

<td>Can be any id, through which a client application identifies a user

</td>

</tr>

<tr>

<td>attributes

</td>

<td>Can be a map/ JSON object/ class/ SDK provided class, which holds attributes of a user. If you are using a JSON object or class declared on a client application, then ensure that you are using <strong>kotlinx.serialization</strong> in the client application to serialize. If that is not possible due to platform constraints, use a map/ UserInfo class provided by SDK. Below are some valid input formats for "attributes". This is an optional parameter. App developers are encouraged to use "attributes" to record as much useful user information available to them.

</td>

</tr>

<tr>

<td>location

</td>

<td>If location service is enabled in the client app, please provide this information. A class has been provided in SDK.

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

<tr>

<td>

</td>

<td>The call returns an Int(HTTP status code). Use this parameter to test if the call is working properly. This output parameter is only reliable when the queue size is set to 1. In the case of batch upload, the output parameter is set to 200 by default. Which means the record has been cached for future upload.

</td>

</tr>

</table>


**Example of “attributes” parameter**

UserInfo class provided by SDK

```
data class UserInfo(
var name: String?,
var age: Int?,
var email: String?,
var company: String?,
var interest: String?,
var occupation: String?
)

val param12 = EQLibrary.UserInfo("Sam", 25, "", null, "sports", "student")
```
Map
```
val param = mapOf<String,String>( "email" to "abcdxyz@gmail.com",
			          "customer" to "Walmart",
                                  "age" to "28",
                                  "area" to "Ottawa, Canada")
```
JSON object (serializable with kotlinx.serialization)
```
val param = buildJsonObject { put("business", "financial")
			      put("interest", "stock market")
			      put("area", "Waverly Heights, Montreal")
			    }
```
Any class declared on client-side application(serializable with kotlinx.serialization)
```
@Serializable
data class UserAttributes(var age: Int, var company: String, 
var referred by: String)

val param = UserAttributes(23, "ABC company", "xyz store")
```

#### logEvent

logEvent helps to record a user’s activity. Additionally, has an optional “properties” parameter to log any details of the activity. App developers are encouraged to use “properties” to provide activity-related information available in the app. This is a suspend function. Please call from a coroutine or another suspend function.

Method

```
suspend fun logEvent(name: String, 
properties: JsonObject = emptyJsonObject, location: LocationC?): Int
```
Example

```
val eqObj = EQLibrary.getInstance(applicationContext)

CoroutineScope(Dispatchers.IO).launch {
	val res = eqObj.logEvent( EQLibrary.Events.ITEM_ADD_TO_WISHLIST, 
				  param5,
			          EQLibrary.LocationC(56.789665, -34.89765544))}

CoroutineScope(Dispatchers.IO).launch {
	val res = eqObj.logEvent( EQLibrary.Events.ADD_TO_CART, param5, null)}
	
EQLibrary.getInstance(applicationContext).logEvent("item purchased", null)
```

<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>name

</td>

<td>This is the event name. It can be any string or event that is supported by the SDK.

<p>
	
List of supported events in mobileSDK:

<p>
	
ADD_TO_CART

<p>

REMOVE_FROM_CART 

<p>

ITEM_PURCHASED

<p>

ITEM_VIEWED 

<p>

ITEM_ADD_TO_WISHLIST

<p>

ITEM_REMOVE_FROM_WISHLIST

<p>

ITEM_SHARED

<p>

ITEM_SEARCHED

</td>

</tr>

<tr>

<td>properties

</td>

<td>Can be a map/ JSON object/ class/ SDK provided class. If you are using a JSON object or class declared on a client application, then ensure that you are using <strong>kotlinx.serialization</strong> in the client application to serialize. If that is not possible due to platform constraints, use a map/ Product class provided by SDK. Below are some valid input formats for "properties". 

</td>

</tr>

<tr>

<td>location

</td>

<td>If location service is enabled in the client app, please provide this information. A class has been provided in SDK.

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

<tr>

<td>

</td>

<td>The call returns an Int(HTTP status code). Use this parameter to test if the call is working properly. This output parameter is only reliable when the queue size is set to 1. In the case of batch upload, the output parameter is set to 200 by default. Which indicates that the record has been cached for future upload. 

</td>

</tr>

</table>

**Example of “properties” parameter**

Product class provided in SDK

```
val param7 = EQLibrary.Product("PRD45678", "Laptop","PRD45677654OTI",
 2550.30F, "", "16 inch monitor")
```

Map

```
val param11 = mapOf<String,String>("productId" to "PRD98234567",
				   "price" to "225.30F",
				   "size" to "S")
```

JSON object (serializable with kotlinx.serialization)

```
val param1 = buildJsonObject { put("prod_id", "PRD5691234")
			       put("prod_name", "Samsung Tab")
                             }
```

Any class declared in a client application (serializable with kotlinx.serialization)

```
@Serializable
data class Item(var name: String, var price: Float, var sku: String)

val param6 = Item("toaster", 225.56F, "TOAST67856435677")
```

### TroubleShooting

1. For 400 to 499 series error in response, please ensure you are providing the JWT access token correctly. If the error exists, please contact EQWorks for assistance.
2. 413 status code is returned when uploaded data is too large. For a single record upload, the max payload size is 1000KB. For batch payload upload, the size of a single payload cannot exceed 1000KB, and the total size of a batch payload cannot exceed 4MB. 


