## Version 2.1.1
 * feature - 
    1. Three new calls added to this release. getBeacons, getBeacon, fireBeacon
    2. Caching mechanism implement for beacon calls
    3. Logging retry process update
    4. delete cached beacon periodically 
    
## Version 2.0.0
Changes for this release are backward incompaitble
 * feature - 
    1. ConfigurationSettings() class visibility access update. 
    ```
    previous format: EQLibrary.Settings(applicationContext).configurationSettings.setAppDevToken(appDevToken)
    present format: ConfigurationSettings(applicationContext).setAppDevToken(appDevToken)
    ```
    2. LocationC class relocate
    ``` 
    previous format: EQLibrary.LocationC(-45.767898, 23.6754433)
    present format: LocationC(-45.767898, 23.6754433)
    ```
    3. Product class relocate
    ```
    previous format: EQLibrary.Product("PRD45678", "Laptop","PRD45677654OTI", 2550.30F, "", "16 inch monitor")
    present format: Product("PRD45678", "Laptop","PRD45677654OTI", 2550.30F, "", "16 inch monitor")
    ```
    4. Events class update
    ```
    previous format: EQLibrary.Events.ITEM_ADD_TO_WISHLIST
    present format:  EventName.ITEM_ADD_TO_WISHLIST
    ```
                
## Version 1.1.5
 * feature - logUser and logEvent methods can receive valid Json String as attributes/ properties. If provided String is not a valid Json, SDK will throw an exception  

## Version 1.1.3
 * Optional feature - Android AdvertisingId. The client app can enable this feature after receiving consent from users to use AdvertisingId.

## Version 1.0.7
 * feature - logUser, logEvent, settings method to set JWT access token and queue size

