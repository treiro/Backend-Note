# FlexiPlayer 

[![N|Solid](http://wiki.sigma-solutions.vn/resources/assets/wiki-logo.png?d6e47)](http://wiki.sigma-solutions.vn/resources/assets/wiki-logo.png?d6e47)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Implement Flexiplayer for Android

  - Java language
  - Android Studio 3.x

### Library

   [![N|Solid](https://i.imgur.com/8OszATV.png)](https://i.imgur.com/8OszATV.png)

### Component

   [![N|Solid](https://i.imgur.com/JX6mr99.png)](https://i.imgur.com/JX6mr99.png)

  



### Class

  - ViewGui: Player controller UI for video type use as factory
>>VodGui: Use for fullscreen player with VOD content
>>VodGuiMini: Use for miniplayer with VOD content
>>LiveGui: Fullscreen player for LIVE
>>LiveGuiMini: Mini player for LIVE
>>AdvertGui: Fullscreen player for Ads video
>>AdvertGuiMini: Mini player for Ads video
>>VodGuiTablet: Player Controller UI for VOD on Tablet
>>LiveGuiTablet: Player Controller UI for LIVE on Tablet
  - PlaybackItem: Object with video data(ads, streaming info, ...)
  - FlexiPlayer: Core Player
  - FlexiUtils: Manage all resource for player
  - FlexiGui: Interface define GUI that Flexiplayer draw video
  - IplaFragmentGui: Fragment contain player and implement FlexiGui
  - PlayerFragment: Class User define for your project it will include FlexiGui on GUI
  - FullScreenPlayerOverLayFragmentDialog: OverlayDialogFragment for full screen player
  - IFlexiGuiUiManager: Interface define UI contain IplaFragmentGui, PlayerFragment and FullScreenPlayerOverLayFragmentDialog implement it

### Implement

  - Prepare player:
```java
    private void preparePlayer(final PlaybackItem playbackItem) {
        flexiUtils = new FlexiUtils(playbackItem);
        addCallbacks(playbackItem);
        flexiUtils.prepareAdsAndPlayer(new AdvertDownloader.Config(
                        "null (Sony C6603; 5.1.1 Android)", new int[]{1920, 1080},
                        getLastSeen(playbackItem)),
                new AdvertDownloader.AdvertListener() {
                    @Override
                    public void onFinished(final List<Advert> videos, final List<Advert> overlays,
                                           final Advert swirly) {
                        new Handler(Looper.getMainLooper()).post(new Runnable() {
                            @Override
                            public void run() {
                                initPlayer(videos, overlays, swirly, resolveStartPosition(playbackItem));
                            }
                        });
                    }
                });
    }
```
  - Pause, Resume, Release
  ```java
    public void pause() {
        if (flexiPlayer != null)
            flexiPlayer.pause();
    }
    
    public void resume() {
        if (flexiPlayer != null)
            flexiPlayer.play();
    }
    
    public void release() {
        if (flexiPlayer != null)
            flexiPlayer.release();
        context = null;
        fmcExternalListener = null;
        qualityChangedListener = null;
        mExternalListener = null;
        guiCallback = null;
        flexiUtils = null;
        fmcModule.release();
        fmcModule = null;
    }
```
 - Switch player from mini to full screen
 ```javascript
public class UiUtil {
    public static final void openFullPlayer(Context ctx) {
        if (ctx != null) {
            MemoryCache.getDefault().setPlayFullShow(true);
            if (ctx.getResources().getConfiguration().orientation == Configuration.ORIENTATION_PORTRAIT) {
                ((Activity) ctx).setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);
            } else {
                if (MemoryCache.getDefault().getPlayerScreenType() != PlayerScreenType.MINI_PLAYER_TABLET_LANSCAPE)
                ((Activity) ctx).setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
            }
            if (MemoryCache.getDefault().getiFlexiGuiUiManager() != null)
                MemoryCache.getDefault().getiFlexiGuiUiManager().onRemoveFlexiFragment();
            new FullScreenPlayerOverLayFragmentDialog().show(((AppCompatActivity) ctx).getSupportFragmentManager(), "full_player");
        }
    }

    public static final void closeFullPlayer(Context ctx) {
        if (ctx != null) {
            MemoryCache.getDefault().setPlayFullShow(false);
            if (ctx.getResources().getConfiguration().orientation == Configuration.ORIENTATION_PORTRAIT) {
                ((Activity) ctx).setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);
            } else {
                Log.e("getPlayerScreenType", MemoryCache.getDefault().getPlayerScreenType()+"");
                if (MemoryCache.getDefault().getPlayerScreenType() != PlayerScreenType.MINI_PLAYER_TABLET_LANSCAPE) {
                    ((Activity) ctx).setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_PORTRAIT);
                }
            }
            if (MemoryCache.getDefault().getiFlexiGuiUiManagerFull() != null)
                MemoryCache.getDefault().getiFlexiGuiUiManagerFull().onRemoveFlexiFragment();
            if (MemoryCache.getDefault().getiFlexiGuiUiManager() != null)
                MemoryCache.getDefault().getiFlexiGuiUiManager().onAddFlexiFragment();

        }
    }
}

```

### Note
 -Don't forget release flexiPlayer and flexiUtils before play new video
 -MemoryCache: Use for singleton Player on app
 -Switch player from mini to full player and vesus: Don't create new player. FlexyPlayer don't support switch UI for player. We move IplaFragmentGui from PlayerFragment and attach to FullScreenPlayerOverLayFragmentDialog and keep IplaFragmentGui instance alive.
 



