---
title: |
    Creating a Mobile Research Database using Open Data Kit
authors:
- Rob Blades
date: 2015-07-18
reviewers:
-
layout: default
---
## Requirements
Before setting up a database, users must have:
  1. A Gmail account (you will be using [Google's App Engine](https://appengine.google.com/))
  2. A mobile device or tablet running Android 1.6 and higher (for other operating systems, such as iPhone's iOS, see the [Open Data Kit FAQ page](https://opendatakit.org/help/faq/)). Alternatively, you could use the [emulator set-up page on Open Data Kit github](https://github.com/opendatakit/opendatakit/wiki/DevEnv-Setup). The emulator simulates what you would see on an Android device.

## Lesson Goals
This lesson is meant to be a primer for historians to create their own functioning databases for field work, from the archives, to oral interviews, site work, etc. The beauty of Open Data Kit is its versatility and inherent mobility - Open Data Kit is designed for mobile use by research from all disciplines. In this lesson you will learn how to create a database server using Google's App Engine. While users can deploy Open Data Kit on other servers, this lesson will only focus on using Google's server capabilities. Advanced users can consult the [Aggregate install page](https://opendatakit.org/use/aggregate/#Installing_VM) for more information.

## What is Open Data Kit and Why Should Historians Use it?
[Open Data Kit (ODK)](https://opendatakit.org) . ODK is not limited to historical research. In fact, it is primarily used by researchers in science and medicine. However, ODK is a tool that historians can employ widely.

ODK uses forms created by users to collect information. These forms can have any number of multiple choice questions, text fields, video, photo, or audio capture, etc. Users create their own form in Excel, convert it to XML (See #Convert XLS Form to XML), and upload the form to their personal ODK server. Once the form is on the server it will sync across your devices.

### Open Data Kit Examples
#### Use by Historians

## Getting Started
For this lesson we will use three main ODK tools.
1. ODK's official Android app to collect data into forms: [ODK Collect](https://opendatakit.org/use/collect/)
2. ODK's form management tool, working in tandem with [Google's App Engine](https://appengine.google.com/): [ODK Aggregate](https://opendatakit.org/downloads/download-category/aggregate/)
3. A spreadsheet editor such as Microsoft Excel, Numbers (on Mac OSX) or Google Spreadsheets to create forms.

In this lesson we will do the steps a bit differently than ODK's official instructions. We will set up your server before dealing with forms.

#### Remember Usernames and Passwords
Remember to record any and all usernames/passwords you use in this tutorial. From gmail to your server login information. This will save you a lot of frustration throughout the process.

### Download the App
To begin, download the ODK Collect Android app. The easiest way to get ODK Collect on your Android device is through the [Google Play Store.](https://play.google.com/store/apps/details?id=org.odk.collect.android&hl=en) Alternatively, you could refer to the [download page](https://opendatakit.org/downloads/download-info/odk-collect-apk/) to manually install the apk (Android application file) directly.

Once the app is working on your device, you can set it aside. We will deal with this later.

### Create a Server for your Forms and Database
Now, we will use ODK Aggregate to create a server for your forms. Here you will upload your blank and completed forms. ODK's installation instructions are thorough here:

Under "Sign-in and Security" select "Connected apps and sites" and then turn "Allow less secure apps" from OFF to ON.
On the App Engine account page, press the "Create Application button" and then follow the link to the "Google Developers Console."
On the ODK installer, select "Google App Engine". When creating the ODK Aggregate Username and password, RECORD this information!! You will need it later
Mac Users: you will need to add an exception in Java's security settings. In System Preferences, select Java to open the Java Control Panel. Select the tab labeled "Security" and under "Exception Site List" click "Edit Site List." Here you will add the file path to your ODK Aggregate folder you created earlier. For this tutorial, the exception I created was under "file://users/robertblades/Desktop/ODK Aggregate" Click OK to save and exit the Java Control Panel.
The uploadAggregateToAppEngine installer will open your Terminal (MacOSX/Linux) or Command Prompt (Windows) and ask for your Gmail username and password. Enter your email first and then your password. NOTE: The Terminal will not show your password as you type. Don't worry. Your computer did not freeze. Just type your password anyways and press enter. Once you have entered the correct login information, let the Terminal run its scripts.
>
1. Make sure Java 7 or higher is installed on the computer you plan to use. If it is not, [download and install it](https://java.com/en/download/). If you are using MacOSX, it may require special care and attention. See [MacOSX Java install](https://docs.oracle.com/javase/7/docs/webnotes/install/mac/mac-jdk.html) and [MacOSX Java FAQ](https://docs.oracle.com/javase/7/docs/webnotes/install/mac/mac-install-faq.html).
2. You will need a Gmail account to use AppEngine. The way the upload script authenticates to AppEngine is now considered a "less secure application" usage by Gmail. You only need to enable reduced security levels during the running of the upload script (at the very end of the installer). After you have uploaded ODK Aggregate to AppEngine, you can restore the secure settings to your Gmail account. You might want to create one specifically for AppEngine if you are not comfortable with weakening your Gmail security for this brief interval of time; or, perhaps, you have already allowed less secure apps access to your Gmail account, and are therefore already running with weakened security. To weaken your security (or verify your settings), go to [Google MyAccount](https://myaccount.google.com/) and, under the "Signing in" heading, click on "Access for less secure apps" and choose "Turn on".
3. You'll need to setup an [App Engine account](https://appengine.google.com/). These accounts are free [(under these terms)](https://cloud.google.com/terms/). You may need to be able to receive a text message from Google to verify your account.
4. If you are creating an App Engine account for the first time, you will be directed to the Google Developers Console. Otherwise, you will on a screen displaying all your existing application ids, and have the option of creating a new application. These two cases are described below:
Developers Console (first time) | On that console
                                | click on the 'Create an empty project' option.
                                | Enter your application's title for the Project Name (e.g., 'South Sudan Water Project'), then edit the project ID. The project ID will be your App Engine application Identifier; it determines the url and can never be changed; these are globally unique, so the common ones like 'test' will all have been taken.
                                | Once you have created the project ID, you can go back to [App Engine Console](https://appengine.google.com/) and see a less cluttered view of your available applications.
App Engine Console (existing/legacy) | On that console
                                      | click on the "Create Application" button,
                                      | choose an application identifier (e.g., my-app-id; the Google Developers Console project ID). The application identifier determines your url and can never be changed.
                                      | enter your application's title (e.g., 'South Sudan Water Project'; Google Developers Console Project Name), and
                                      | click on "Save."
5. Download [ODK Aggregate v1.N.N](https://opendatakit.org/downloads/). Select the latest Featured release for your operating system. These downloads are wizard-based installers for the various operating systems. If you are running OSX, you must unzip the downloaded file before running the installer within it. If you are on MacOSX Mountain Lion or onward, you will need to fiddle with [GateKeeper settings](http://osxdaily.com/2012/07/27/app-cant-be-opened-because-it-is-from-an-unidentified-developer/) in order to run the installer. Please consider using a non-Featured release during forms development (to help us identify issues prior to a production release).
6. The installer will guide you through configuring ODK Aggregate for App Engine and then launch a script to upload this configured ODK Aggregate to App Engine. NOTE: Beginning with Java 7 Update 51, there are security level settings [described here](https://www.java.com/en/download/help/jcp_security.xml) that may prevent the upload script from running. A reported work-around is to add the file: path (e.g., file:///) to the Exception Site list. When using a Google account with two step-authentication then the installation using the ODK Aggregate v1.x.0 installers with fail with the message "Unable to update app: Use an application-specific password instead of your regular account". To over come this you need to enter you Google account name (myaccount@gmail.com) and obtain and use an application-specific password instead of your normal one. See [application-specific password how-to](https://support.google.com/accounts/answer/185833?hl=en).
7. Finally, after successfully uploading ODK Aggregate to AppEngine, update your Gmail account using the same link in step 2, above, to not allow "less secure applications".


### Build your Form

#### Convert XLS Form to XML
ODK uses [XML, a common markup language](http://www.w3schools.com/xml/xml_whatis.asp), to create working forms across platforms. To create a working form on your mobile device, upload the xls form to the [conversion page on the ODK website.](http://opendatakit.org/xiframe/) Because this converter parses data for XML, it tells you whether or not the form is valid. A valid form will appear green, even if it contains warning notifications in yellow (Fig.). Here you can download the converted XML form. An invalid form will appear red, showing you where the error is in your form.

#### Example Error
Because I believe in learning through mistakes, the following is a simple example of an invalid form.

The hard part is over. Now all we have to do is use your form and upload it to your server.

## Test Drive your form

## Reap what you Sow
