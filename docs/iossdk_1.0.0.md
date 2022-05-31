* [EQWorks iOS MobileSDK](#eqworks-ios-mobilesdk)
* [Installation](#installation)
* [Permission](#permission)
* [Configuration](#configuration)
* [Client Methods](#client-methods)
	- [logUser](#loguser)
	- [logEvent](#logevent)
  - [getBeacons](#getBeacons)
  - [getBeacon](#getBeacon)
  - [fireBeacon](#fireBeacon)
* [TroubleShooting](#troubleshooting)
* [Payload format](#payload-format)

### EQWorks iOS MobileSDK

EQWorks iOS MobileSDK allows app developers to log events from client applications to EQ servers. The SDK and the examples provided in this document are developed in Swift. 

### Installation

TODO

### Permission

TODO

### Configuration

Mobile application need to configure the SDK to work seamlessly with EQ servers. There are two steps to follow.

1. EQWorks will provide a JWT access token to client application developers for authentication. Please Set this token in the SDK by calling ```setJwtToken(token: String)```. It is sufficient to set the token in SDK only once. Afterwards, if you need to update the token in SDK, you can use this method
```Swift
let eqObj = EQLibrary.shared()
eqObj.setJwtToken(token: token)
```
2. The MobileSDK supports data caching. Client applications can decide to upload a single data or batch data to the server. The SDK supports caching of 1 to 20 data in the SDK database. Using the ‘setQueueSize’ method, you can set any number between 1 to 20 as cache size. Default is 1.
```Swift
 let eqObj = EQLibrary.shared()
_ = eqObj.setQueueSize(size: 5)
```

### Client Methods

#### logUser

This method helps to keep track of a user by assinging auto generated id to a user. It is sufficient to call this method once. You can also call this method to update attributes for a user. 

Method

```Swift
public func logUser(appUserId: String, attributes: String? = nil, location: LocationC? = nil) 
public func logUser(appUserId: String, attributes: String? = nil)
public func logUser(appUserId: String, location: LocationC? = nil)
public func logUser(appUserId: String)
```
Method call example

```Swift
let eqObj = EQLibrary.shared()
eqObj.logUser(appUserId: "STW45678899")
```
```Swift
eqObj.logUser(appUserId: "STW45678899", location: LocationC(lat: 23.567889, long: 24.453211))
```
```Swift
let attributes = UserAttribute(age: 26, interest: ["Reading", "Travelling"])
let encoder = JSONEncoder()
do {
    let attributeData = try encoder.encode(attributes)
    let attributeStr = String(data: attributeData, encoding: .utf8)
    eqObj.logUser(appUserId: "STW45678899", attributes: attributeStr)
} catch {
    print("invalid json provided")
}
```
```Swift
let attributes = UserAttribute(age: 26, interest: ["Reading", "Travelling"])
let encoder = JSONEncoder()
do {
    let attributeData = try encoder.encode(attributes)
    let attributeStr = String(data: attributeData, encoding: .utf8)
    eqObj.logUser(appUserId: "STW45678899", attributes: attributeStr, location: LocationC(lat: 23.456755, long: -12.651229))
} catch {
    print("invalid json provided")
}
```
<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>appUserId

</td>

<td>An id that client application uses to identify a user

</td>

</tr>

<tr>

<td>attributes

</td>

<td>A valid json string which holds attributes of a user. App developers are encouraged to use "attributes" to record useful user information.

</td>

</tr>

<tr>

<td>location

</td>

<td>If location service is enabled in the client application, use LocationC class of SDK to provide user lat long.

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

</table>

#### logEvent

logEvent helps to track user's activities. Additionally, provides an optional “properties” parameter to log any details. App developers are encouraged to use “properties” to provide activity-related information available in the app.

Method

```Swift
public func logEvent(name: String, properties: String? = nil, location: LocationC? = nil)
public func logEvent(name: String, properties: String? = nil)
public func logEvent(name: String, location: LocationC? = nil)
public func logEvent(name: String)
```
Example

```Swift
let eqObj = EQLibrary.shared()
eqObj.logEvent(name: "app_download")
```
```Swift
eqObj.logEvent(name: "add_to_cart", location: LocationC(lat: 23.789658, long: -45.564597))
```
```Swift
let obj = ProductDetails(expiryDate: "2022-12-12", weight: 1.13)
let prop = ProductProperties(product: "electric candle lighter", price: 28.92, desc: ["recharge and use", "charge lasts upto 600 sparks", "LED Battery Display", "USB charging cable"], obj: obj)
let encoder = JSONEncoder()
do {
    let propData = try encoder.encode(prop)
    let propStr = String(data: propData, encoding: .utf8)
    eqObj.logEvent(name: "cart_checkout", properties: propStr, location: LocationC(lat: 49.888171, long: -97.131112))
} catch {
    print("invalid json string")
}
```
```Swift
let obj = ProductDetails(expiryDate: "2022-12-12", weight: 1.13)
let prop = ProductProperties(product: "electric candle lighter", price: 28.92, desc: ["recharge and use", "charge lasts upto 600 sparks", "LED Battery Display", "USB charging cable"], obj: obj)
let encoder = JSONEncoder()
do {
    let propData = try encoder.encode(prop)
    let propStr = String(data: propData, encoding: .utf8)
    eqObj.logEvent(name: "cart_checkout", properties: propStr)
} catch {
    print("invalid json string")
}
```
<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>name

</td>

<td>This is the event name. It can be any string.

</td>

</tr>

<tr>

<td>properties

</td>

<td>A valid json string. 

</td>

</tr>

<tr>

<td>location

</td>

<td>If location service is enabled in the client application, use LocationC class from SDK to record user lat, long.

</td>

</tr>

</table>

#### getBeacons

Returns list of beacons associated with the client.

Method

```Swift
public func getBeacons(completion: @escaping([BeaconTag]?, Error?) -> Void) 
```
Example

```Swift
let eqObj = EQLibrary.shared()
eqObj.getBeacons {response, error in
    if let beacons = response {
        print("beacons = \(beacons)")
    } else if let error = error {
        print("error = \(error)")
    }
}
```
<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

<tr>

<td>

</td>

<td>The call returns a List of BeaconTags or an empty list []. 

</td>

</tr>

</table>

#### getBeacon

Returns a beacon associated with the provided id.

Method

```Swift
public func getBeacon(beaconId: Int, completion: @escaping(BeaconTag?, Error?) -> Void)
```
Example

```Swift
eqObj.getBeacon(beaconId: 8145) {response, error in
    if let beaconTag = response {
        print("beaconTag = \(beaconTag)")
    } else if let error = error {
        print("error = \(error)")
    }
}
```

<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>beaconId

</td>

<td>Id of the beacon.

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

<tr>

<td>

</td>

<td> If a record is found for the provided id, a BeaconTag is returned otherwise nil.

</td>

</tr>

</table>

#### getBeacons with campaignId

Returns list of beacons associated with provided campaign id.

Method

```Swift
public func getBeacons(campId: Int, completion: @escaping([BeaconTag]?, Error?) -> Void)
```
Example

```Swift
eqObj.getBeacons(campId: 8527) {response, error in
    if let beacons = response {
        print("beacons = \(beacons)")
    } else if let error = error {
        print("error = \(error)")
    }
}
```

<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>campId

</td>

<td>campaignId associated with beacon.

</td>

</tr>

<tr>

<td colspan="2" >Output

</td>

</tr>

<tr>

<td>

</td>

<td> returns list of beacons associated with The campaignId.

</td>

</tr>

</table>


#### fireBeacon

This call Uploads the beacon information to the server. firBeacon works with a single beaconTag. It is recommended that the Client application calls getBeacons with campaignId once and locally cache the list of beacons. Then the list can be used to call fireBeacons on specific events.

Method

```Swift
public func fireBeacon(beacon: BeaconTag, location: LocationC?, completion: @escaping(Bool?, Error?) -> Void)
```
Example

```Swift
eqObj.fireBeacon(beacon: beaconTag, location: LocationC(lat: 23.67899, long: -34.4567), completion: handleFireBeaconResponse(success:error:))

func handleFireBeaconResponse(success: Bool?, error: Error?) {
  if let error = error {
      print("beacon upload failed")
      print("fireBeacon error: \(error)")
  } else {
      print("beacon upload succeed")
  }
}
```

<table>

<tr>

<td colspan="2" >Input

</td>

</tr>

<tr>

<td>beacon

</td>

<td>
getBeacon/ getBeacons method returns BeaconTag object/ objects. Select a single BeaconTag to upload to the server with fireBeacon call at appropriate event.
</td>

</tr>

<tr>

<td>location

</td>

<td> If location is enabled in the application, then send location data using LocationC class.

</td>

</tr>

</table>

### TroubleShooting

TODO

### Payload Format

**logUser**
```Swift
{
  "location": {
    "longitude": -12.651229,
    "latitude": 23.456755
  },
  "userId": "E330DA6F-BFDA-4F18-8804-C8D76DCA6431",
  "identifierForVendor": "AEE05862-7C8D-47DB-831F-1E7842132AED",
  "attribute": {
    "age": 26,
    "interest": [
      "Reading",
      "Travelling"
    ]
  },
  "context": {
    "locale": "en",
    "device": {
      "manufacturer": "Apple",
      "model": "iPhone",
      "name": "iPhone 12",
      "type": "iOS"
    },
    "os": {
      "name": "iOS",
      "version": "15.5"
    },
    "app": {
      "build": "1",
      "namespace": "com.eqworks.mobilesdk-ios-test",
      "name": "mobilesdk_ios_test",
      "version": "1.0"
    },
    "timezone": "America/Winnipeg",
    "ip": "192.168.100.102",
    "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148"
  },
  "cmd": "logUser",
  "type": "Identify",
  "timestamp": "2022-05-31T17:19:35.092Z",
  "appUserId": "STW45678899",
  "h3Index": "8f55022b0c0538c"
}
```
**logEvent**
```Swift
{
  "location": {
    "longitude": -97.131112,
    "latitude": 49.888171
  },
  "userId": "E330DA6F-BFDA-4F18-8804-C8D76DCA6431",
  "name": "cart_checkout",
  "context": {
    "locale": "en",
    "device": {
      "manufacturer": "Apple",
      "model": "iPhone",
      "name": "iPhone 12",
      "type": "iOS"
    },
    "os": {
      "name": "iOS",
      "version": "15.5"
    },
    "app": {
      "build": "1",
      "namespace": "com.eqworks.mobilesdk-ios-test",
      "name": "mobilesdk_ios_test",
      "version": "1.0"
    },
    "timezone": "America/Winnipeg",
    "ip": "192.168.100.102",
    "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148"
  },
  "cmd": "logEvent",
  "type": "Track",
  "timestamp": "2022-05-31T20:48:32.208Z",
  "properties": {
    "obj": {
      "expiryDate": "2022-12-12",
      "weight": 1.1299999952316284
    },
    "product": "electric candle lighter",
    "price": 28.92,
    "desc": [
      "recharge and use",
      "charge lasts upto 600 sparks",
      "LED Battery Display",
      "USB charging cable"
    ]
  },
  "identifierForVendor": "AEE05862-7C8D-47DB-831F-1E7842132AED",
  "postalcode": {
    "postalcode": "R3C4X1",
    "fsa": "R3C"
  },
  "h3Index": "8f271c973aa4594"
}
```
**batch upload**
```Swift
{
  "cmd": "batch",
  "batch_info": [
    {
      "location": {
        "longitude": -97.131112,
        "latitude": 49.888171
      },
      "userId": "E330DA6F-BFDA-4F18-8804-C8D76DCA6431",
      "name": "cart_checkout",
      "properties": {
        "product": "electric candle lighter",
        "obj": {
          "expiryDate": "2022-12-12",
          "weight": 1.1299999952316284
        },
        "price": 28.92,
        "desc": [
          "recharge and use",
          "charge lasts upto 600 sparks",
          "LED Battery Display",
          "USB charging cable"
        ]
      },
      "cmd": "logEvent",
      "timestamp": "2022-05-31T21:07:56.214Z",
      "type": "Track",
      "identifierForVendor": "AEE05862-7C8D-47DB-831F-1E7842132AED",
      "postalcode": {
        "postalcode": "R3C4X1",
        "fsa": "R3C"
      },
      "h3Index": "8f271c973aa4594"
    },
    {
      "location": {
        "longitude": -97.112786,
        "latitude": 49.880312
      },
      "identifierForVendor": "AEE05862-7C8D-47DB-831F-1E7842132AED",
      "userId": "E330DA6F-BFDA-4F18-8804-C8D76DCA6431",
      "attribute": {
        "age": 26,
        "interest": [
          "Reading",
          "Travelling"
        ]
      },
      "cmd": "logUser",
      "type": "Identify",
      "timestamp": "2022-05-31T21:07:56.220Z",
      "appUserId": "STW45678899",
      "postalcode": {
        "postalcode": "R2H1B9",
        "fsa": "R2H"
      },
      "h3Index": "8f271c9468b2c5c"
    }
  ],
  "context": {
    "locale": "en",
    "device": {
      "manufacturer": "Apple",
      "model": "iPhone",
      "name": "iPhone 12",
      "type": "iOS"
    },
    "os": {
      "name": "iOS",
      "version": "15.5"
    },
    "app": {
      "build": "1",
      "namespace": "com.eqworks.mobilesdk-ios-test",
      "name": "mobilesdk_ios_test",
      "version": "1.0"
    },
    "timezone": "America/Winnipeg",
    "ip": "192.168.100.102",
    "user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 15_5 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Mobile/15E148"
  }
}
```
