---
title: AuthMe

language_tabs:
  - android
  - java
  - php
  - javascript
  - golang

includes:
  - errors

search: false
---

# Introduction

Users keep same passwords everywhere. Despite you following security best practices, a data breach on an unrelated app can compromise your users.

AuthMe helps you do away with passwords. With AuthMe pattern lock, bring UX and security together.

# Getting Started

Before you start off, make sure that you have the API Key and Secret Key. If not, get them from [here.](https://account.authme.authme.host/generatekeys)

It can be found on the left panel of the dashboard.

API Key looks like this: `k-162b77f3-5937-4856-af1b-7950a001f732` and can be exposed in public clients (Android, iOS, Windows apps)

Secret Key looks like this: `s-001b77f3-5938-0056-af1b-7950a001f712` and should not be put in any public artifact.

Integration has 2 parts.

1. Client Integration (Android/iOS/Window)

2. Server Integration (PHP/Java/Go/Python/Js)

#Android Integration

## 1. Gradle import

Add the following line to your app build.gradle file

`compile 'io.authme:patternlock:0.1.1'`

Build your project.

## 2. Config
```java
    Config config = new Config(MainActivity.this);
    config.setEnvironment(Config.SANDBOX); 
    config.setAPIKey("YOUR_API_KEY_HERE"); 
    config.setEmailId("USER_EMAIL_ID");
```
Create a config object. 

Set environment to `Config.SANDBOX`, if you are testing. 

Set it to `Config.PRODUCTION` when you are ready.

<aside class="notice">
Remember to replace API key with your API key before running the program.<br>
</aside>
<aside class="notice">
Ensure that the environment and keys are appropriate.
</aside>

## 3. Call AuthMe
```java
   Intent intent = new Intent(MainActivity.this, AuthScreen.class);
   startActivityForResult(intent, RESULT);
```

Call AuthScreen activity wherever you are signing up or signing in the user. 

If the use has set the patter already, AuthMe will offer a trust score to you in the callback. 

If not, AuthMe will help the user set the pattern. 

## 4. Callback
```java
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
  switch (requestCode) {
    case RESULT : {
        switch (resultCode) {
            case Config.SIGNUP_PATTERN : {
                Toast.makeText(getApplicationContext(), "Sign up successfull", Toast.LENGTH_LONG)
                        .show();
                 } break;

            case Config.LOGIN_PATTERN : {
                Toast.makeText(getApplicationContext(), data.getStringExtra("response"), Toast.LENGTH_LONG)
                        .show(); //you will get a trust score in the response here.
                } break;

            case Config.RESET_PATTERN: {
                Toast.makeText(getApplicationContext(), "Reset Pattern", Toast.LENGTH_LONG)
                        .show();
                } break;            

            case Config.RESULT_FAILED : {
                Toast.makeText(getApplicationContext(), "Failed To Identify", Toast.LENGTH_LONG)
                        .show();
                if (data.hasExtra("response")) {
                            Toast.makeText(getApplicationContext(), data.getStringExtra("response"), Toast.LENGTH_LONG)
                                    .show();
                        }
                } break;

                default: break;
                }

            } break;

            default: break;

      }
    }
```

You will get the callback in onActivityResult. 

### a. Sign up successfull

In case the user didn't have a pattern set earlier, we sign up the user. 

Check for case Config.SIGNUP_PATTERN in onActivityResult.

### b. Login Trust Score

In case the user swiped to login, we calculate the trust score and give it back to you.

Check for case Config.LOGIN_PATTERN in onActivityResult.

### c. Reset Pattern

In case the user already exists on the platform, but forgot the pattern, we redirect you to reset the pattern.

Check for case Config.RESET_PATTERN in onActivityResult.

### d. Failed to Identify

In case the user failed to identify with AuthMe.

Check for case Config.RESULT_FAILED in onActivityResult.

The code snippet on the right side shows how to handle these cases.

## 5. Trust Score
```json
{
"Accept":true,
"Reason":"Human readable reason for decision",
"Hash":"0352044c22fac6455b46de681701a733f86bca1f8477bf015d3ebd9817e2b9dc",
"Motion":0.93,
"Path":0.9,
"Speed":0.95,
"ReferenceId": "echo-order-id-from-request"
}
```
In case the user tried to login, we send the trust score which looks as follows:

### What's a trust score?

Trust score is the indication of how much the user matches to his behavioural profile.

Path score 0.9 means that user matches 90% to his signature behaviour.

Speed score 0.95 means that user matches 95% to his velocity behaviour.

Motion sensor 0.93 means that user matches 93% to his movements behaviour.

### What's hash? Why is it so important?

You are expected to process the trust score on the server and implement the business logic on the server. 

When you get a trust score in the LOGIN_PATTERN case above, post this on your server and calculate the hash to verify if the trust score is not tampered with.

<aside class="warning">
If you bypass the logic of hash calculation, you are vulnarable to a security loophole.
</aside>

In the server integration, we will demonstrate how hash is calculated.

#Server Integration

Here's the logic to calculate hash. 

Modules on the right side, should help you do this quickly.

<aside class="notice">
Remember to replace secret key with your secret key before deploying.
</aside>

# Add Ons

## How do I put my logo on the AuthMe Screen?
```java
String fileName = "mylogo";
Bitmap bitmap = BitmapFactory.decodeResource(MainActivity.this.getResources(), R.drawable.nestaway_bird_logo);
        try {
            ByteArrayOutputStream bytes = new ByteArrayOutputStream();
            bitmap.compress(Bitmap.CompressFormat.JPEG, 100, bytes);
            FileOutputStream fo = openFileOutput(fileName, Context.MODE_PRIVATE); // MODE_PRIAVE so that no one else can access this file
            fo.write(bytes.toByteArray());
            fo.close();
        } catch (Exception e) {
            e.printStackTrace();
            fileName = null;
        }
        intent.putExtra("logo", fileName);
        startActivityForResult(intent, RESULT);
```
a. Upload your logo in the drawable section of your app.

b. Create a bitmap of your logo and load it into a file accessible only to your app.

c. Add the filename in the intent before calling `startActivityForResult(intent, RESULT)`

## How do I change the color of statusbar?
```java
intent.putExtra("statusbar", "#443FFF");
startActivityForResult(intent, RESULT);
```

Add the intent extra 'statusbar' before calling `startActivityForResult(intent, RESULT)`

## How do I change the color of titlebar?
```java
intent.putExtra("titlebar", "#443FFF");
startActivityForResult(intent, RESULT);
```

Add the intent extra 'titlebar' before calling `startActivityForResult(intent, RESULT)`

## How do I change the titlebar text?
```java
intent.putExtra("titletext", "Authentication Screen");
startActivityForResult(intent, RESULT);
```

Add the intent extra 'titletext' before calling `startActivityForResult(intent, RESULT)`