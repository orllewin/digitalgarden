## About

While looking at an old bash build script at work we decided to look at alternatives. Usually my preference for a bash script replacement had been node.js but I took a look at Kotlin Script and am impressed.

## Setup

I used [sdkman.io/](https://sdkman.io/)to install Kotlin, which I'd seen recommended on the official Kotlin site somewhere and got a quick test Hello, World running:

```
curl -s "https://get.sdkman.io/" | bash
source "/Users/FOOBAR/.sdkman/bin/sdkman-init.sh"
sdk version  
sdk install kotlin
echo "print(\"Hello, World\")" >> test.main.kts
kotlin test.main.kts

Hello, World
```

## Shebang

You can add the usual shebang to a script to make it executable:

```
#!/usr/bin/env kotlin

println("Hello, World\n")
```

I think I `chmod +x test.main.kts`, can't remember, then run: `./test.main.kts`

## JVM standard lib + shell commands

Kotlin Script runs in the JVM, you have access to the full Java standard lib, and I also found a utility method to run shell commands somewhere online:

```
#!/usr/bin/env kotlin

import java.io.File

fun shell(command: String) {
	val process = ProcessBuilder("/bin/bash", "-c", command).inheritIO().start()
	process.waitFor()
}


File("hello.txt").createNewFile()

shell("ls")
```

## Dependencies

It's easy to add any well known Kotlin dependency, maven central is added by default, but you can also specify a repository:

```
#!/usr/bin/env kotlin

@file:Repository("https://repo.maven.apache.org/maven2/")
@file:DependsOn(
	"com.squareup.okio:okio:3.6.0"
)

import okio.FileSystem
import okio.Path.Companion.toPath

fun shell(command: String) {
	val process = ProcessBuilder("/bin/bash", "-c", command).inheritIO().start()
	process.waitFor()
}

val system = FileSystem.SYSTEM

system.write("hello2.txt".toPath()) {
  writeUtf8("World")
}

shell("ls")
```

## Extensions

Kotlin extension syntax is of course supported so you ca do things like:

```
#!/usr/bin/env kotlin

@file:DependsOn("com.squareup.okhttp3:okhttp:4.12.0")

import okhttp3.OkHttpClient
import okhttp3.Request
import okhttp3.Response

fun String.get(client: OkHttpClient): String? {
	val request = Request.Builder().url(this).build()
	val response = client.newCall(request).execute()
	return response.body?.string()
}

val client = OkHttpClient()

val html = "[https://bbc.co.uk/index.html](https://bbc.co.uk/index.html)".get(client)

println(html)
```

 (^fairly nasty and ill-advised, but you get the idea) 