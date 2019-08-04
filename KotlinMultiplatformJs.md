slidenumbers: true
theme: simple
text: Roboto
text-strong: Roboto Bold
text-emphasis: Roboto Light Italic
header: Roboto
header-strong: Roboto Strong,#EE783F
code: Fira Code Medium, #EE783F, #8B3D90, #2E59A2, #DF393F, #1EA8D9
footer: Kotlin Multiplatform **|** @rdbatch02
build-lists: true

# [fit] Kotlin/Mulitplatform
<br>
### By: Ryan Batchelder
#### @rdbatch02

[.hide-footer]
[.header: alignment(left)]
[.slidenumbers: false]

---

# ⚠️ Warning! ⚠️

## Kotlin/Multiplatform is still considered "**experimental**". The API changes **often** and documentation is slow to catch up. Use this at your own risk!

---
[.autoscale: true]
# What is Multiplatform?

* JetBrains has stated that a goal for Kotlin is for it to run on all platforms
* The JVM gets 80% of the way there, but 80% != 100%
* Kotlin/JS and Kotlin/Native projects seek to bring Kotlin to the last 20%
* Kotlin/Multiplatform provides tooling to unify different build targets into one project

---
[.autoscale: true]

# What about _______?

* Other projects attempt to chase the dream of "write once, run everywhere"
    * Xamarin, React Native, Appcelerator
* These projects try to create wrappers over platform calls, usually with compromises
* Platform wrapper developers often spend too much time "catching up" with platform capabilities instead of new functionality
* Writing platform-specific code can be hard or awkward

---
[.autoscale: true]

# How is Kotlin different?

* Kotlin/JS and Kotlin/Native provide their own platform wrappers, but confine them to thier own platforms
    * Ex: `kotlin.dom` package wraps over Browser DOM calls, but is *only* available in Kotlin/JS
* In practice: "business logic" can be shared code, while platform-specific code is explicitly written for each target platform
* **Bottom-Up Code Sharing**


---

[.autoscale: true]
[.slidenumbers: false]
[.hide-footer]

# Bottom-Up Code Sharing

![inline](images/bottom-up-1.png)

<sub>
Source: KotlinConf 2018 - Architecting a Kotlin JVM and JS Multiplatform Project by Felipe Lima
[https://www.youtube.com/watch?v=pcwIs749KSE]()
</sub>

---

[.autoscale: true]
[.slidenumbers: false]
[.hide-footer]

# Bottom-Up Code Sharing

![inline](images/bottom-up-2.png)

<sub>
Source: KotlinConf 2018 - Architecting a Kotlin JVM and JS Multiplatform Project by Felipe Lima
[https://www.youtube.com/watch?v=pcwIs749KSE]()
</sub>

---

[.autoscale: true]
[.slidenumbers: false]
[.hide-footer]

# Bottom-Up Code Sharing

![inline](images/bottom-up-3.png)

<sub>
Source: KotlinConf 2018 - Architecting a Kotlin JVM and JS Multiplatform Project by Felipe Lima
[https://www.youtube.com/watch?v=pcwIs749KSE]()
</sub>

---
[.autoscale: true]

# Bottom-Up Code Sharing

Kotlin adds two keywords to make this happen: `expect` and `actual`

* Strict implementation of the `interface` concept with key differences:
    * Classes marked with `expect` have no common implementation, but the compiler will enforce that a platform-specific implementation exists for all platforms
    * Classes marked with `actual` may only have one instance per platform and must implement all `expect`'ed members

---
[.autoscale: true]

# Bottom-Up Code Sharing - Example

```kotlin
// Common Code
expect class HttpClient() {
    fun get(url: String): Response
}

// JVM Code
actual class HttpClient() {
    actual fun get(url: String): Response {
        // Implement GET using some HTTP lib: http4k, Spring RestTemplate, etc. 
    }
}

// JS Code
actual class HttpClient() {
    actual fun get(url: String): Response {
        // Implement GET using some JS lib: axios, superagent, Node HTTP, etc. 
    }
}
```
---

# Breaking Promises 🧐

```kotlin
// JS Code
actual class HttpClient() {
    actual fun get(url: String): Response {
        ...
    }
}
```

---
[.autoscale: true]

# Keeping Promises 🤞

```kotlin
// Common Code
expect class HttpClient()

// JVM Code
actual class HttpClient() {
    fun get(url: String): Response {
        // Implement GET using some HTTP lib: http4k, Spring RestTemplate, etc. 
    }
}

// JS Code
actual class HttpClient() {
    fun get(url: String): Promise<Response> {
        // Implement GET using some JS lib: axios, superagent, Node HTTP, etc. 
    }
}
```

---
[.autoscale: true]
# Kotlin/Multiplatform

* `expect` and `actual` are supported on Native as well
* Share common code across platforms with easy hooks to write platform specific code when needed
* Compiler-enforced safety for platform code
* Platform-specific dependencies
    * **Java/Android** - Maven
    * **JS** - NPM
    * **iOS** - Cocoa Pods
    * **Native** - ...good luck
* One project produces multiple artifacts: jar, compiled js, binary files

---
[.autoscale: true]
# Kotlin/Multiplatform Caveats ⚠️

* Kotlin/MP API should be stable, build tooling is not
* Gradle plugins are just starting to settle on an API

---
[.autoscale: true]
# Kotlin/Multiplatform Caveats ⚠️

* Kotlin/JS and Kotlin/Native are still considered "experimental"
    * Native is in need of major performance improvements
    * JS needs better tool-chain integrations with tools like NPM and Webpack
* Not all JVM features are properly supported on JS, and the compiler doesn't really help you
    * Function overloads

---
[.autoscale: true]
# Kotlin/Multiplatform Caveats ⚠️

* Documentation is a major problem
    * Docs get outdated quickly, and it's hard to know if what you're looking at is latest
    * Gradle plugin documentation is notoriously bad
    * Most support comes from Kotlin Slack

---
[.autoscale: true]

# Kotlin/Multiplatform - Learn More

* Boston-area: Kotlin Office Hours meetup
**Bottom-Up Code Sharing with Kotlin Multiplatform** by Russell Wolf on 8/8/19
[https://www.meetup.com/kotlin-office-hours/events/262084616/]()

* Official Kotlin Slack - [slack.kotlinlang.org](http://slack.kotlinlang.org/)
* Kotlin Docs - [https://kotlinlang.org/docs/reference/multiplatform.html]()