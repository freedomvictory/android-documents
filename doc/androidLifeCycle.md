# Andriod Life Cycle 


## Andriod activity instrucion 

1. andriod activity lifecycle 

![lifecycle picture looks ](https://koenig-media.raywenderlich.com/uploads/2015/09/activity_lifecycle_pyramid.png)


- `onCreate()` 


- `onStart()`

    request sources, set up something 
- `onResume()`

    when app status changed, that is  `background -> foreground` , this function will be called.Maybe input focus resumed 


- `onPause()`

    when app status changed, that is  `foreground -> background` , this function will be called.Maybe input focus has changed 

- `onStop()`

    This method is called right after `onPause`.  When the activity is no longer visible to the user , and it's a good place ts save data that you want to commit to the disk.

      The screen is already not show this APP
    relase sources, stop something 


- `onRestart()`

- `onDestroy()`    

    This is the final callback you'll receive from the OS before the activity is destroyed.

[CLick it, Refer ](https://www.raywenderlich.com/2705552-introduction-to-android-activities-with-kotlin/)

[Click it , more good Refer!](https://codelabs.developers.google.com/codelabs/kotlin-android-training-lifecycles-logging/#4)



  




## Andriod Life Cycle library  

3 main parts of the lifecycle library 

1. `lifecycle Owner`

    which are the components that have a lifecycle,` Activity `and `Fragment` are lifecycle owners, Lifecycle owners implement the `LifecycleOwner` infterface 
2. `Lifecycle class`

    which holds the actual state of a lifecycle owner and triggers events when lifecycle changes happen 



3. `Lifecycle observer`

    Which obser the lifecycle state and perform tasks when the lifecycle changes.Lifecycle observers implements the `LifecycleObserver`


```kotlin 
class DessertTimer(lifecycle: Lifecycle) : LifecycleObserver {

    // The number of seconds counted since the timer started
    var secondsCount = 0

    /**
     * [Handler] is a class meant to process a queue of messages (known as [android.os.Message]s)
     * or actions (known as [Runnable]s)
     */
    private var handler = Handler(Looper.getMainLooper())
    private lateinit var runnable: Runnable
    init{
        lifecycle.addObserver(this)
    }


    @OnLifecycleEvent(Lifecycle.Event.ON_START)
    fun startTimer() {
        // Create the runnable action, which prints out a log and increments the seconds counter
        runnable = Runnable {
            secondsCount++
            Timber.i("Timer is at : $secondsCount")
            // postDelayed re-adds the action to the queue of actions the Handler is cycling
            // through. The delayMillis param tells the handler to run the runnable in
            // 1 second (1000ms)
            handler.postDelayed(runnable, 1000)
        }

        // This is what initially starts the timer
        handler.postDelayed(runnable, 1000)

        // Note that the Thread the handler runs on is determined by a class called Looper.
    }

    @OnLifecycleEvent(Lifecycle.Event.ON_STOP)
    fun stopTimer() {
        // Removes all pending posts of runnable from the handler's queue, effectively stopping the
        // timer
        handler.removeCallbacks(runnable)
    }
}



```



## Process shutdowns and saving activity state 

- `onSaveInstanceState()`

This callback to save other data you want to retain, even if the app was automatically shutdown. You can use `putInt` method to put data into the `bundle`(it's in RAM)

This callback called between `OnPause` and `OnStop`


**save data**
```kotlin
   override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)

        outState.putInt(KEY_REVENUE, revenue)
        outState.putInt(KEY_DESSERT_SOLD, dessertsSold)
        outState.putInt(KEY_TIMER_SECONDS, dessertTimer.secondsCount)
        Timber.i("onSaveInstanceState called")

    }
```
**loda data**

```kotlin
 if (savedInstanceState != null){
    revenue = savedInstanceState.getInt(KEY_REVENUE, 0)
    dessertsSold = savedInstanceState.getInt(KEY_DESSERT_SOLD, 0)
    dessertTimer.secondsCount = savedInstanceState.getInt(KEY_TIMER_SECONDS, 0)
}
```

- `onRestoreInstranceState`

It's called after `OnStart()`, if you ever need to restore some state after `OnCreate` is called, you can use this callback.But most of time , you restore the activity state in `onCreate()`



