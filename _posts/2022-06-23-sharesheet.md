---
layout: post
title: 'Sending data to other apps with Sharesheet'
tags: [Android, Kotlin]
featured_image_thumbnail: assets/images/posts/2022/sharesheet_pigeon_thumbnail.jpg
featured_image: assets/images/posts/2022/sharesheet_pigeon.jpg
---

A simple way to send a link to another app!

<!--more-->

A few days ago I needed to be able to send a link to an external app, and Intents are perfect for such task, Android through the Sharesheet allows you to do so (unless you want to implement a custom one).

Just need to call Intent.createChooser(intent, null) and start the activity; now, if you want to be able to detect which app the user chooses to share the link with, we would need to include the broadcaster (only available from API 22) and once the onReceived is called, obtain the ClickedComponent by getting the extra from the intent with **Intent.EXTRA_CHOSEN_COMPONENT**, this will give you access to the package name of the app selected.

The main call would look something like this (Don't forget the MIME type, in my case is "text/plain"):

{% highlight kotlin %}

val pendingIntent = PendingIntent.getBroadcast(context, 0,
   Intent(context, ShareSheetBroadcastReceiver::class.java), PendingIntent.FLAG_MUTABLE)
Intent.createChooser(intent,  null, pendingIntent.intentSender)

{% endhighlight %}
  

On your BroadcastReceiver onReceived method:

{% highlight kotlin %}

val clickedComponent = intent?.getParcelableExtra<ComponentName>(Intent.EXTRA_CHOSEN_COMPONENT)

clickedComponent?.let { componentName ->
    packageManager?.let {
        val appInfo = packageManager.getApplicationInfo(
            componentName.packageName, PackageManager.GET_META_DATA)
        packageManager.getApplicationLabel(appInfo) as String
    }
}
{% endhighlight %}

What about if I want to listen when the user decides to just copy the link? In that case, we could do so by listening changes on the clipboard:


{% highlight kotlin %}
val clipManager = context?.getSystemService(Context.CLIPBOARD_SERVICE) as ClipboardManager
clipManager.addPrimaryClipChangedListener {
    Timber.tag("FirstFragment").d("Link shared via Copy link")
}
{% endhighlight %}


[![ShareSheet sample](/assets/images/posts/2022/samsung-sharesheet.png)](/sharesheet "ShareSheet sample")

The logcat output would be:

{% highlight kotlin %}

D/FirstFragment: Link shared via copy link
D/ShareSheetBroadcastReceiver: Sharing the link via: WhatsApp
D/ShareSheetBroadcastReceiver: Sharing the link via: Messages

{% endhighlight %}

You can find a mini example here [GitHub](https://github.com/rosebm/ShareSheet)

<p align="left">
<small>Pigeon image credit: iStockphoto/IEEE Spectrum</small>
</p>


