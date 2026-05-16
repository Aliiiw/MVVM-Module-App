# MVVM Module App

MVVM Module App is a compact Kotlin Android sample that demonstrates a simple Model-View-ViewModel flow with Jetpack Compose, Retrofit, and LiveData. The app fetches posts from JSONPlaceholder and renders them in a Compose `LazyColumn`.

The Android project is stored inside the `MyApp/` directory.

## Features

- Native Android app written in Kotlin
- Jetpack Compose UI
- Basic MVVM-style separation
- Retrofit client for JSONPlaceholder
- Gson converter for JSON parsing
- ViewModel with `MutableLiveData` for data, loading, and error state
- Coroutine-based network call on `Dispatchers.IO`
- Compose list rendering for fetched posts
- Internet permission configured in the manifest
- Gradle wrapper included

## Project Structure

```text
.
+-- README.md
+-- MyApp/
+    +-- build.gradle
+    +-- settings.gradle
+    +-- gradlew
+    +-- app/
+        +-- build.gradle
+        +-- src/main/
+            +-- AndroidManifest.xml
+            +-- java/com/example/mymvvpapp/
+                +-- MainActivity.kt
+                +-- data/
+                |   +-- model/
+                |   +-- network/
+                +-- viewmodel/
+                +-- ui/theme/
+```

## Architecture

The app follows this simple flow:

```text
MainActivity / Compose UI
   -> PostsViewModel
      -> ApiClient
         -> ApiInterface
            -> JSONPlaceholder API
```

`MainActivity` observes `LiveData` from `PostsViewModel` and updates Compose state when posts arrive.

## Main Components

| File | Purpose |
| --- | --- |
| `MainActivity.kt` | App entry point, theme setup, ViewModel observation, and post list UI. |
| `viewmodel/PostsViewModel.kt` | Fetches posts and exposes data/loading/error through LiveData. |
| `data/network/ApiClient.kt` | Retrofit and OkHttp client setup. |
| `data/network/ApiInterface.kt` | Defines the `/posts` endpoint. |
| `data/model/Post.kt` | Data model for JSONPlaceholder post objects. |
| `ui/theme/*.kt` | Compose theme files from the Android template. |

## API

The sample uses JSONPlaceholder:

```text
https://jsonplaceholder.typicode.com/posts
```

Retrofit endpoint:

```kotlin
@GET("/posts")
suspend fun getAllPosts(): Response<List<Post>>
```

Post model:

```kotlin
data class Post(
    val body: String,
    val id: Int,
    val title: String,
    val userId: Int
)
```

## UI Behavior

When the app starts:

1. `MainActivity` renders `ObservePostsViewModel()`.
2. A `PostsViewModel` is created with `ViewModelProvider`.
3. `getAllPostsRequest()` starts the network request.
4. `postsList`, `postsListError`, and `loading` are observed.
5. `PostView()` renders posts in a `LazyColumn`.

Each post card currently shows:

- title in white text
- body in dark text
- blue block background
- basic spacing between title and body

## Tech Stack

- Kotlin
- Android
- Jetpack Compose
- Compose Material
- Android ViewModel
- LiveData
- Kotlin Coroutines
- Retrofit `2.9.0`
- Gson converter `2.9.0`
- Gson `2.9.1`
- Gradle wrapper
- Android Gradle Plugin `7.4.2`
- Kotlin Android plugin `1.7.0`
- Compose UI `1.2.0`

## Android Configuration

| Setting | Value |
| --- | --- |
| Namespace | `com.example.mymvvpapp` |
| Application ID | `com.example.mymvvpapp` |
| Min SDK | `21` |
| Target SDK | `33` |
| Compile SDK | `33` |
| Version | `1.0` |
| App label | `MyMVVPApp` |
| Main activity | `com.example.mymvvpapp.MainActivity` |
| Permission | `android.permission.INTERNET` |

## How to Run

Open this folder in Android Studio:

```text
MyApp/
```

Then sync Gradle and run the `app` configuration on an emulator or Android device.

From the command line, with Android SDK configured:

```bash
cd MyApp
./gradlew :app:assembleDebug
```

## Notes

- This is a learning/sample project for MVVM basics.
- The package and app label use `mymvvpapp`/`MVVP`, which appears to be a typo of `mvvm`.
- `ApiClient.BASE_URL` is missing a trailing slash. Retrofit normally expects base URLs to end with `/`, so runtime API usage may need `https://jsonplaceholder.typicode.com/`.
- The ViewModel creates a standalone `CoroutineScope` instead of using `viewModelScope`.
- Loading and error states are logged but not rendered as user-facing UI.
- The default Android starter tests are still present, but this README update did not run Gradle or tests.

## Possible Improvements

- Rename package/app strings from `mymvvpapp` to `mymvvmapp`
- Add the trailing slash to the Retrofit base URL
- Use `viewModelScope` inside `PostsViewModel`
- Show loading and error UI in Compose
- Move networking behind a repository layer
- Add previews for the post list item
- Add ViewModel tests for success and error responses
- Style post rows as reusable Compose components
