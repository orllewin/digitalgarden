Note. work-in-progress as of 18th January 2024

Shannon is a [Kotlin script](../computers/Kotlin%20script.md) tool to find high entropy string literals in source files using the [Shannon Entropy](https://rosettacode.org/wiki/Entropy#Kotlin) algorithm. 

It's not finished, still to do:

* Handle none-C style literals, it currently only matches "Hello, World" format strings
* Absolute paths, only relative paths from the script directory work currently
* Hard-coded entropy value of 5, should be an optional arg
* Implement recursive flag (default impl. is to walk all directories)
* Arg error checking... 
* etc etc
## Status

Archived.

## Usage

`./shannon.main.kts -r -min 5 -max 256 -p ../../orllewin/Lento2/ -f .kt`

* `-r` turns recursive searching on (not implemented yet).
* `-min` followed by a number, the minimum length of the string literal to find.
* `-max` followed by a number, the maximum length of the string literal to find.
* `-s` followed by a file path, to only search a single file, not really useful, may remove.
* `-p` followed by a path, the directory to search
* `-f` followed by a file extension, a file type filter

## Source

```
#!/usr/bin/env kotlin

import java.io.File
import java.nio.file.Files
import java.nio.file.Paths
import java.util.Scanner
import java.util.regex.Pattern
import kotlin.io.path.pathString
import kotlin.math.ln

//From https://rosettacode.org/wiki/Entropy#Kotlin
fun log2(d: Double) = ln(d) / ln(2.0)
fun shannon(s: String): Double {
  val counters = mutableMapOf<Char, Int>()
  for (c in s) {
    if (counters.containsKey(c)) counters[c] = counters[c]!! + 1
    else counters.put(c, 1)
  }
  val nn = s.length.toDouble()
  var sum = 0.0
  for (key in counters.keys) {
    val term = counters[key]!! / nn
    sum += term * log2(term)
  }
  return -sum
}

data class Result(val entropy: Double, val literal: String, val line: String, val filePath: String)

println("\nShannon - API Key finder\n")

args.forEachIndexed{ index, arg ->
  println("$index: $arg")
}

val recursive = when {
  args.contains("-r") -> true
  else -> false
}

val minLength: Int = if(args.contains("-min")){
  args[args.indexOf("-min") + 1].toInt()
}else{
  5
}

val maxLength: Int = if(args.contains("-max")){
  args[args.indexOf("-max") + 1].toInt()
}else{
  512
}

val singleFile = when {
  args.contains("-s") -> true
  else -> false
}

val filePath: String? = if(singleFile){
  args[args.indexOf("-s") + 1]
}else{
  null
}

val fileType: String? = if(args.contains("-f")){
  args[args.indexOf("-f") + 1]
}else{
  null
}

println("\nRecursive: $recursive")
println("\nMin length: $minLength")
println("\nMax length: $maxLength")
println("\nSingle file: $singleFile")

fileType?.let{
  println("\nFiletype: $fileType")
}

val filePaths = mutableListOf<String>()

val path = if(!singleFile && !args.contains("-p")){
  "./"
}else{
  args[args.indexOf("-p") + 1]
}

if(singleFile){
  println("\nFile: $filePath")
  when (filePath) {
    null -> throw Exception("Single file command but no file arg")
    else -> filePaths.add(filePath)
  }
}else{
  println("\nPath: $path")
  Files.walk(Paths.get(path)).use { stream ->
    stream.filter(Files::isRegularFile).filter {
      if(fileType != null){
        it.fileName.toString().endsWith(fileType)
      }else{
        true
      }
    }.forEach{ filteredPath ->
      filePaths.add(filteredPath.pathString)
    }
  }
}

val patternDoubleQuote: Pattern = Pattern.compile("\"[^\"\\\\]*(\\\\.[^\"\\\\]*)*\"")

val entropyResults = mutableListOf<Result>()

filePaths.forEach { foundPath ->
  println("Found path: $foundPath")
  Scanner(File(foundPath)).use { scanner ->
    while (scanner.hasNextLine()) {
      val line = scanner.nextLine()
      if(line.contains("\"")) {
        val doubleQuoteMatcher = patternDoubleQuote.matcher(line)
        while (doubleQuoteMatcher.find()) {
          repeat(doubleQuoteMatcher.groupCount()) { count ->
            val match = doubleQuoteMatcher.group(count)
            val literal = match.dropLast(1).drop(1)
            if (literal.length in minLength..maxLength){
              val shannonEntropy = shannon(literal)
              entropyResults.add(Result(shannonEntropy, literal, line, foundPath))
            }
          }
        }
      }
    }
  }
}

entropyResults.sortByDescending { result ->
  result.entropy
}

entropyResults.forEach { result ->
  if(result.entropy > 5) {
    println()
    println("Entropy: ${result.entropy}")
    println("Literal: ${result.literal}")
    println("Line: ${result.line}")
    println("File: ${result.filePath}")
    println()
  }
}

println("End")
```