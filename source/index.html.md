---
title: AuthMe

language_tabs:
  - PHP
  - Java
  - Python
  - JS
  - Go


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

## 2. Build your project.

## 3. Create a config object, set Environment, API Key and user email Id.

```Java
	Config config = new Config(MainActivity.this);
    config.setEnvironment(Config.SANDBOX); //Change this to Config.PRODUCTION when you are ready
    config.setAPIKey("YOUR_API_KEY_HERE"); //Remember that the keys are different for sandbox and production
    config.setEmailId("USER_EMAIL_ID");
```

## 4. Call AuthMe

```Java
   Intent intent = new Intent(MainActivity.this, AuthScreen.class);
   startActivityForResult(intent, RESULT);
```

## 5. Callback

We let you know 4 types of results.

### 1. Sign up successfull

In case the user didn't have a pattern set earlier, we sign up the user.

### 2. Login Trust Score

In case the user swiped to login, we calculate the trust score and give it back to you.

### 3. Reset Pattern

In case the user already exists on the platform, but forgot the pattern, we redirect you to reset the pattern.

### 4. Failed to Identify

In case the user failed to identify with AuthMe.

Here's the code snippet that shows how to handle these cases.

```Java
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

## 6. Trust Score

In case the user tried to login, we send the trust score which looks as follows:

```Json

```

### What's a trust score?

Trust score is the indication of how much the user matches to his behavioural profile.

Signature score 90, user matches 90% to his signature behaviour.

Velocity score 82, user matches 82% to his score behaviour.

### What's hash? Why is it so important?

You are expected to process the trust score on the server and implement the business logic on the server. 

When you get a trust score in the LOGIN_PATTERN case above, post this on your server and calculate the hash to verify if the trust score is not tampered with.

In the server integration, we will demonstrate how hash is calculated.

#Server Integration

Here's the logici to calculate hash. 

Modules on the right side, should help you do this quickly.

<aside class="notice">
Remember to replace secret key with your secret key before deploying.
</aside>

<aside class="warning">
If you bypass the logic of hash calculation, you are vulnarable to a security loophole.
</aside>

# Add Ons

## 1. How do I put my logo on the AuthMe Screen?

### a. Upload your logo in the drawable section of your app.

### b. Create a bitmap of your logo and load it into a file accessible only to your app.

```Java
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
```

### c. Add the filename in the intent before calling `startActivityForResult(intent, RESULT);`

`intent.putExtra("logo", fileName);`
