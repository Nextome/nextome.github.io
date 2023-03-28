# Observe events

If events are set up in your venue, you can observe when user enters or exits from event radius using those two observers:
=== "Android"
    ```kotlin
        nextomeSdk.getEnterEventObservable().asLiveData().observe(this) { event ->
            Log.e("event_test", "Received on enter with id ${event.event.id} and data: ${event.event.data}")            
        }
    
        nextomeSdk.getExitEventObservable().asLiveData().observe(this) { event ->
            Log.e("event_test", "Received on exit with id ${event.event.id} and data: ${event.event.data}")
        }
    ```
=== "iOS"
    ```swift
    let enterObserver = nextomeSdk.getEnterEventObservable().watch(){ event in
        guard let event = event else{
            return
        }
    
        print("Received on enter with id \(event.event.id) and data: \(event.event.data)")
    }

    let exitObserver = nextomeSdk.getEnterEventObservable().watch() { event in
        guard let event = event else{
            return
        }
        print("Received on exit with id \(event.event.id) and data: \(event.event.data)")
    }
    ```

Optionally, after exiting from event, it's possible to set a timeout value in seconds before signaling that event again.
This allows you to ignore when user exits and enters again in a event radius in a short time span. For example, if timeout is set to 60 seconds, the same event onEnter after onExit is received will be not triggered again for at least for 1 minute.
If set to 0, Nextome SDK will signal onEnter and onExit event realtime.
This parameter is only configurable during the SDK initialization.

=== "Android"
    ``` kotlin
    nextomeSdk = NextomePhoenixSdk(
        clientId = CLIENT_ID,
        clientSecret = CLIENT_SECRET,
        context = context as ApplicationContext,
        eventTimeoutDurationInSeconds = 10
    )
    ```
=== "iOS"
    ``` swift
    NextomePhoenixSdk.Builder(clientId: CLIENT_ID, clientSecret: CLIENT_SECRET)
        .setEventTimeoutDuration(millis: 60)
        .build()
    ```
