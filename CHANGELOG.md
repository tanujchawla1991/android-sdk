Version 1.0.0-beta8
===================

**What's new**

* Login flow in the WebView. It is no longer necessary to open the web browser
  to log users in. See `AuthenticationClient#openLoginActivity` for details.
* `PlayerState` now implements `Parcelable`.
* `Spotify#getPlayer` method is static.

**Known bugs**

* Using queue methods together with regular playback might behave unreliably


Version 1.0.0-beta7
==================

**What's New**

* SDK includes native libraries for ARMv7 devices
* New way to get access to audio data. Instead of subclassing the `Player`
  it is now possible to create a custom `AudioController` and pass it to the
  player during creation. For details refer to `AudioController` class
* Subclassing the `Player` is no longer possible. This is because there's no need for it
  with custom `AudioController`s.
* Initialized `Player` instance is passed to the `Player.InitializationObserver#onInitalized` callback.
* It is possible to set playback bitrate. Default bitrate is set to 160kbit/s.
  For more details see `Player#setPlaybackBitrate` method and `PlaybackBitrate` object.
* Player has two new methods: `login` and `logout` which can be used to log back in
  after losing connectivity or to switch users.
* It is now possible to specify which handler player should post its callback on (default is
  main thread)
* One of the `ConnectionStateCallback` methods has been removed.
* To use all brand new options Player has a brand new Builder.

**Bug fixes**

* Only playback events are delivered to `PlayerNotificationCallback#OnPlaybackEvent`
* Lots of internal Player fixes mostly related to concurrency.

Version 1.0.0-beta6
===================

**What's New:**

* Starting a song from specified position is now possible.
  Check out the new `PlayConfig` class and `Player#play(PlayConfig)` method to see how to use it.
* We added some more error handling. This means you can get new types of errors
  in `PlayerNotificationCallback#onPlaybackError` if something goes wrong.
* Disk cache is now turned on by default. It can be turned off by invoking `Config#useCache`
  with `false`.
* Initialization of the player changed slightly. Now you'll need to pass a `Config` object
  with valid client ID when getting the player:

  ```java
  // Old way:
  Spotify spotify = new Spotify("myauthtoken");
  Player player = spotify.getPlayer(context, "mycompany", referenceObj, initObserver);

  // New way
  Spotify spotify = new Spotify();
  Config playerConfig = new Config(context, "myauthtoken", "myclientid");
  Player player = spotify.getPlayer(playerConfig, referenceObj, initObserver);
  ```

* It is possible to inject a custom player to the `Spotify` class with `Spotify#setPlayer` method.

**Bug fixes**

* We tweaked playback logic a bit. Now the player should much more responsive
  when skipping songs or changing contexts.
* SDK is now tested with Lollipop (Android 5.0)

**Known issues**

* If you try playing a playlist from an index beyond the playlist size, there's no related
  playback error triggered.
* The same issue occurs when trying to play a song with the initial position that is
  beyond its duration.


Version 1.0.0-beta5
===================

**What's New:**

* We introduced two new playback events: `TRACK_START` and `TRACK_END` which
  replace the `TRACK_CHANGED` event.
  The uri of the track that started or finished playing can be read from
  player state sent with those events. ([Issue #8](https://github.com/spotify/android-sdk/issues/8))
* SDK now handles playback errors with new `PlayerNotificationCallback#onPlaybackError` method 
  which can be used to handle the unavailable tracks errors. ([Issue #37](https://github.com/spotify/android-sdk/issues/37))
* As an experimental feature you can now use disk cache in your app. It will store streamed
  tracks locally on the device and read them from disk when played next time.
  To enable disk cache, initialize Spotify as follows:

  ```
  Spotify spotify = new Spotify("myauthtoken");
  spotify.useCache(true);
  ```

**Bugs fixed:**

* `InitializationObserver#onError` callback is now delivered correctly on the UI thread.

Version 1.0.0-beta4
===================

**What's New:**

* You will now get player state with every player event.
* The player state now contains information about currently playing track
  and its duration. ([Issue #22](https://github.com/spotify/android-sdk/issues/22))
* Instead of keeping a copy of player state from native player inside Player object
  `getPlayerState()` is now asynchronous and requires a callback.
* We removed these methods from the Player object:
  * `isPlaying()`
  * `getPlaybackPosition()`
  * `isShuffling()`
  * `isRepeating()`

  This data can now be retrieved asynchronously with `getPlayerState()` or from
  the player state passed with player events.
* New callback for errors while logging in.
* Player initialization callback is now triggered after user successfully logs in.

**Bugs fixed:**

* SDK does not mix playback with Spotify App. ([Issue #28](https://github.com/spotify/android-sdk/issues/28))

Version 1.0.0-beta3
===================

**What's New:**

* We added the possibility to play a list of tracks (for example, an album's tracks).
Playing the list of tracks also supports playing from a specified index ([Issue #11](https://github.com/spotify/android-sdk/issues/11)).
* Improved API reference manual.

**Bugs Fixed:**

* We fixed clearing the buffer after pausing the playback and context switching now works correctly
 ([Issue #21](https://github.com/spotify/android-sdk/issues/21)).
* No more error when context ends ([Issue #20](https://github.com/spotify/android-sdk/issues/20))


Version 1.0.0-beta2
===================

**What's New:**

* SDK now comes as a single aar library.
* Added method to clear queued tracks.

**Bugs Fixed:**

* getPlaybackPosition returns correct values after seeking position
 ([Issue #7](https://github.com/spotify/android-sdk/issues/7)).

**Known Issues:**

* You can't play albums yet.
* Switching between playback contexts is buggy.


Version 1.0.0-beta1
===================

**What's New:**

* Initial release

**Known Issues:**

* SDK limited to authentication and playback. All other functionality is
  currently provided by the Web API.
