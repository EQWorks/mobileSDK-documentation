## Version 2.0.0
Changes for this release are backward incompaitble
 * feature - 
    1. ConfigurationSettings() class visibility access update. 
    ```
    previous format to access configuration methods: EQLibrary.Settings(applicationContext).configurationSettings.setAppDevToken(appDevToken)
    present format to access the  configuration methods: ConfigurationSettings(applicationContext).setAppDevToken(appDevToken)
    ```
    1. LocationC class relocate
    ``` 
    previous format to access LocationC: EQLibrary.LocationC(-45.767898, 23.6754433)
    present format to access LocationC: LocationC(-45.767898, 23.6754433)
    ```
    1. Product class relocate
    ```
    previous format to access Product: EQLibrary.Product("PRD45678", "Laptop","PRD45677654OTI", 2550.30F, "", "16 inch monitor")
    present format to access Product: Product("PRD45678", "Laptop","PRD45677654OTI", 2550.30F, "", "16 inch monitor")
    ```
    1. Events class update
    ```
    previous format to access Events: EQLibrary.Events.ITEM_ADD_TO_WISHLIST
    present format to access Events:  EventName.ITEM_ADD_TO_WISHLIST
    ```
                
## Version 1.1.5
 * feature - logUser and logEvent methods can receive valid Json String as attributes/ properties. If provided String is not a valid Json, SDK will throw an exception  

## Version 1.1.3
 * Optional feature - Android AdvertisingId. The client app can enable this feature after receiving consent from users to use AdvertisingId.

## Version 1.0.7
 * feature - logUser, logEvent, settings method to set JWT access token and queue size

