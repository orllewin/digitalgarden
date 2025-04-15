## Platform

Android.  

## About

Control is an Android app intended to accompany 1st party apps that control connected devices in the home (namely: audio streamers), it can also be used for anything that can be called over http, or websites, or with Android Intents. Think of it as a lightweight [Postman](https://www.postman.com/) for Android with a slick frontend. There are a few Postman-like 'api tester' apps for Android but they're all aimed at developers. Control instead aims to provide friction-free operation with a fantastic user interface. 

## Status

Under active development.

## Group import

Control allows import of groups of controls via distributed .json files. This means anyone can reverse engineer any branded hardware and offer up a 'recipe' for other people to use.

## Streamers/http

Audio streamers from Cambridge Audio, Naim, etc tend to expose endpoints over plaintext on your local network, this means it's easy to mimic the commands sent from 1st party applications.

## Web

For convenience there's also a 'web' control type, this allows you to have a group in Control that opens your favourite websites

## Intents

There's (very) basic support for an Intent control type, the target for this is really VLC so you can create controls to stream the same radio stations on Android as you setup for your streamers.

