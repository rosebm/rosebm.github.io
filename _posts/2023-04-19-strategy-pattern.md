---
layout: post
title: 'Strategy Pattern'
tags: [Android, Kotlin]
featured_image_thumbnail: assets/images/posts/2023/strategy_pattern/strategy_pattern_3_roads.png
featured_image: assets/images/posts/2023/strategy_pattern/strategy_pattern_3_roads.png
---

Simplifying Interchangeable Behaviors in Your Code.

<!--more-->

Imagine you’re driving to a destination, and you have several routes to choose from: the highway, the scenic route, or the city streets. Each route has its own way of getting you there, with different speeds, views, and potential traffic. Here, the goal is to reach your destination, and the strategy is the route you choose. Depending on the situation (like time of day or traffic), you might decide to change routes to fit your needs. This way, you can adapt your approach at any time without changing the main goal—just like the Strategy Pattern lets you change specific behaviors in your code without altering the overall structure.

Something similar happens on an app, you may have several sources that you want to interchange without requiring major updates to the code. Let's say an app stores data locally using a database like SQLite, but later decides to switch to Room. Ideally, this change would only require swapping out the database layer without affecting the rest of the code. The Strategy Pattern makes this possible, allowing your app to switch between different implementations of a specific function without breaking or changing the code that depends on it.


Let's say your app stores data locally using a database like SQLite  but later you decide to switch to Room, imagine the update if all the code is highly attached such as, you have to make changes is every single class that connect and calls the database (what a pain!). Ideally would be something like unplugging a black cable and plugging a red one to the outlet, well, this is where the Strategy Pattern comes to rescue.

### So, what is exactly the Strategy Pattern?
<br />

![Switch anatomy](assets/images/posts/2023/strategy_pattern/strategy_pattern_diagram.png)

The definition says that, Strategy Pattern is a behavioral design pattern that defines a family of algorithms, encapsulates each one as a separate class, and makes them interchangeable. In other words, the Strategy Pattern lets you switch between different behaviors or algorithms at runtime by swapping out the implementation behind the scenes, without needing to modify the main code.

It consists of three main components:

- **Context:** The main class that uses different strategies. It holds a reference to the strategy interface and delegates the work to the current strategy.
- **Strategy Interface:** The common interface that each strategy implements. This interface ensures that the Context can use any strategy without knowing the specifics of each.
- **Concrete Strategies:** Classes that implement the strategy interface, each containing a unique way to execute the algorithm or behavior.
 
### When Should You Use the Strategy Pattern?

The Strategy Pattern is perfect when:

- You have multiple ways to perform a similar action and want flexibility in choosing which to use.
- You want to avoid large conditional statements (e.g., if-else or switch-case) for choosing between behaviors.
- You want to enable interchangeable components in your app, so switching between them is seamless.

Enough of concepts, let's code!

We're going to create a pattern for sending user's event to different analytics providers (let's say Google Analytics, Amplitude, Amazon PinPoint), this allows you to measure and understand how users interact with your website or app.

Step 1. Create the strategy interface:

{% highlight kotlin %}
interface AnalyticsHelper {

    fun sendEvent(event: Event)
    fun sendUserProperties(key: String, value: String?)
    fun setUserId(id: String)
}
{% endhighlight %}

Step 2. Context

{% highlight kotlin %}
class AnalyticsManager(private val providers: List<AnalyticsHelper>) {
    fun sendEvent(event: Event) {
        providers.forEach {
            it.sendEvent(event)
        }
    }

    fun sendUserProperties(key: String, value: String?) {
        providers.forEach {
            it.sendUserProperties(key, value)
        }
    }

    fun setUserId(id: String) {
        providers.forEach {
            it.setUserId(id)
        }
    }
}
{% endhighlight %}

Step 3. Concrete strategies

{% highlight kotlin %}
class AnalyticsFirebaseImpl(private val application: Application): AnalyticsHelper {

    private var firebaseAnalytics: FirebaseAnalytics? = null

    init {
        initializeFirebaseAnalytics()
    }

    private fun initializeFirebaseAnalytics() {
        firebaseAnalytics = FirebaseAnalytics.getInstance(application)
    }

    override fun sendEvent(event: Event) {
        firebaseAnalytics?.logEvent(event.name, convertToBundle(event))
    }

    override fun sendUserProperties(key: String, value: String?) {
        firebaseAnalytics?.setUserProperty(key, value)
    }

    override fun setUserId(id: String) {
        firebaseAnalytics?.setUserId(id)
    }

    private fun convertToBundle(event: Event): Bundle {
        return Bundle().apply {
            putString("Description", event.description)
            event.parameter_string?.let {
                for ((paramKey, paramValue) in it) {
                    putString(paramKey, paramValue)
                }
            }
        }
    }

}
{% endhighlight %}



Now you're ready to customize it! Thanks for reading.

You can find a sample here [GitHub](https://github.com/rosebm/Samples/tree/main/customswitch)
