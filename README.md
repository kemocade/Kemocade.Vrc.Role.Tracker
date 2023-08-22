# Kemocade VRC Role Tracker

-----
# Introduction

Video Introduction on YouTube:

[![Intro Video](http://img.youtube.com/vi/YedZa5gvlZg/0.jpg)](http://www.youtube.com/watch?v=YedZa5gvlZg "Introduction")

This is a template repository for quickly configuring your own GitHub Actions powered workflow to automatically track your desired VRChat Group(s) and/or Discord Server(s).
Data about your tracked items will be produced as a JSON file hosted via GitHub pages.

The JSON file produced by this workflow is designed to be consumed at runtime in VRChat Worlds by the [Kemocade VRC Role Reader](https://github.com/kemocade/Kemocade.Vrc.Role.Reader) VPM package.

You can configure this repository to track multiple VRChat Groups and/or Discord Servers, and the resulting data will be combined into one file.

# Prerequisites

In order to track a particular VRChat Group, you will need access to a VRChat account that meets all of the following requirements for each group:
* Is a member
* Has any combination of roles that grant at least these permissions:
  * Manage Group Member Data
  * View All Members

In order to track a particular Discord Server, you will need permission to add a Discord Bot to that server.

# Setup

## 1. Create Your Role Tracker

Click the "[Use This Template](https://github.com/kemocade/Kemocade.Vrc.Role.Tracker/generate)" button to create your own repository based on this template repository.
Don't set your repository's visibility to private, as this will break compatibility with GitHub pages (unless you have a GitHub Pro subscription).

## 2. Configure GitHub Pages

Open your repository's "Settings" tab in the top bar.
Open the "Pages" section from the settings side bar.
Under "Build and deployment", set "Source" to "GitHub Actions".

## 3. Configure VRChat API Access

Create repository variables with the names and values described below.
For details on how to create repository variables, see [Creating Configuration Variables for a Repository](https://docs.github.com/en/actions/learn-github-actions/variables#creating-configuration-variables-for-a-repository).

* `VRCHAT_USERNAME`: Your VRChat Username.
  * Make sure you are using your VRChat Username, not your Display Name or User ID.
  * For more information on the various different VRChat account identifiers, see [Account Names and Identifiers](https://help.vrchat.com/hc/articles/4408181867027-Account-Names-and-Identifiers-Usernames-Display-Names-and-User-IDs).

Create repository secrets with the names and values described below.
For details on how to create repository secrets, see [Creating Encrypted Secrets for a Repository](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository).

> :warning: **These secrets contain sensitive information that could compromise your VRChat account if exposed!**
> To keep them safe, you should only ever input these values as GitHub Actions encrypted secrets!
> For extra security, consider using an alternate VRChat account instead of your main account.

* `VRCHAT_PASSWORD`: Your VRChat Password.

* `VRCHAT_KEY`: Your VRChat Two-Factor-Authentication (2FA) Key.
  * You can get this key while [setting up 2FA on your VRChat account](https://docs.vrchat.com/docs/setup-2fa#set-up-two-factor-authentication).
    * If you do not have 2FA set up, you will need to enable it now.
    * If you already have 2FA set up and did not previously save this key, you will need to disable and re-enable 2FA to get it again.
  * When you reach [Step 2 of the 2FA setup process](https://docs.vrchat.com/docs/setup-2fa#step-2-add-your-vrchat-account-to-your-authenticator-app), click on "enter the key manually" and copy the provided key string.
    * It should look something like `aaaa aaaa aaaa aaaa aaaa aaaa aaaa aaaa`, likely containing both letters and numbers.
  * Make sure you also scan the 2FA QR code with your normal 2FA app before proceeding.
    * If you disabled and re-enabled 2FA as part of this setup process, you will need to also re-scan the QR code, as your previous 2FA registration will no longer work.

## 4. Configure Tracked VRChat Groups (Optional)

If you do not wish to track any VRChat Groups, you can skip this step.

Create the following repository variable:

* `VRCHAT_GROUP_IDS`: The Group ID(s) of the VRChat Group you want to track.
  * If tracking more than one group, use a comma-delimeted string of all Group IDs.

## 5. Configure Tracked Discord Servers (Optional)

If you do not wish to track any VRChat Groups, you can skip this step.

Use the [Discord Developer Portal](https://discord.com/developers) to [create a new app registration](https://discord.com/developers/docs/getting-started#step-1-creating-an-app).
The app can be named anything.
You will only need to complete step 1 from this linked article.
Make sure you copy your app's Bot Token during this process and save it for later.

Configure your Bot's [Privileged Gateway Intents](https://support-dev.discord.com/hc/articles/6207308062871-What-are-Privileged-Intents) to include the following permissions:
* SERVER MEMBERS INTENT
* MESSAGE CONTENT INTENT

Next, add your new Discord Bot to any servers that you wish to track.

Finally, for each Discord Server that you wish to track, create a channel in which users can post their VRChat User IDs.
The VRChat User IDs posted in these channels by your users will be scraped by your Discord Bot.

Create the following repository variables:

* `DISCORD_GUILD_IDS`: The Guild ID(s) of the Discord Servers you want to track.
  * If tracking more than one server, use a comma-delimeted string of all Guild IDs.

* `DISCORD_CHANNEL_IDS`: The Channel ID(s) of the channels from each tracked server in which users will post their VRChat User IDs.
  * If tracking more than one server, use a comma-delimeted string of all Channel IDs.

Create the following repository secrets:

> :warning: **This secret contains sensitive information that could allow someone else to access your Discord Bot if exposed!**
> To keep is safe, you should only ever input this value as a GitHub Actions encrypted secret!

* `DISCORD_BOT_TOKEN`: The Bot Token of the Discord Bot you created previouisly.

## 6. Run the Action

By default, this template is scheduled to run every hour on the hour (but may take a few minutes after the hour to actually start).
You can adjust this schedule by [adjusting the schedule trigger](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule) in your workflow.
If you don't want to wait, you can also [run the workflow manually](https://docs.github.com/en/actions/using-workflows/manually-running-a-workflow).

> :warning: **Important note for users located outside of The United States of America!**
> GitHub Actions run on servers located in the USA.
> If you have never previously logged in to your VRChat account from the USA, this action's first execution will cause you to receive an e-mail from VRChat asking you to confirm your login attempt from the United States.
> This check will cause the action to fail until you click the confirmation link in that e-mail.
> Subsequent executions will work properly after this and you will not need to repeat this process.

## 7. Get the Results

After your workflow has executed successfully, your repository's GitHub Pages site will publish updated data about your tracked items.

By default, the link to your GitHub Pages site will be formatted as `https://<YOUR_GITHUB_HERE>.github.io/<YOUR_REPOSITORY_HERE>`.
For example, this template repository's site is located at [https://kemocade.github.io/Kemocade.Vrc.Role.Tracker](https://kemocade.github.io/Kemocade.Vrc.Role.Tracker).

This site publishes a `data.json` file containing the tracked information from your tracked items.
For example, this template repository's `data.json` file is located at [https://kemocade.github.io/Kemocade.Vrc.Role.Tracker/data.json](https://kemocade.github.io/Kemocade.Vrc.Role.Tracker/data.json).

You can consume this `data.json` file in your VRChat world by using the [Kemocade VRC Role Reader](https://github.com/kemocade/Kemocade.Vrc.Role.Reader) VPM package.

# FAQs

## Will the author of this repository be able to view my Username/Password/etc?

**No!**
When you use this template to make your own repository, you are creating your own entirely seperate system.
Secrets you create in your repository will be encypted and unavailable to anyone else.
To keep it this way, make sure you follow the instructions carefully and never commit secrets to source control directly.

## Why is two-factor-authentication required?

If 2FA is not enabled on an account, VRChat will sometimes send 2FA codes via e-mail instead of a 2FA app.
These codes can not be supplied to the GitHub Actions workflow, so it will always fail in these cases.
By providing your 2FA key to the GitHub Actions workflow, it can generate 2FA codes when needed.

## Is it okay to use the VRChat API for something like this?

The [Official VRChat Creator Guidelines](https://hello.vrchat.com/creator-guidelines) contain guidelines on acceptable usage of the VRChat API.
The author of this repository believes that it conforms to all of these guidelines.
You should take care not to modify this code in ways that could break these guidlines, such as by adjusting your workflow's schedule to run too frequently.

## What exactly is happening when I run this workflow?

This workflow consumes the internal [Kemocade VRC Role Tracker Action](https://github.com/kemocade/Kemocade.Vrc.Role.Tracker.Action) repository to gather and process information.
You can view that repository's source code to see everything that will happen with your supplied information in each workflow execution.
If this source action is updated, your copied workflow will not change until you manually update it.
This is a security feature to ensure that the authors of GitHub Actions such as this one can't maliciously update and change their code to collect or expose your secrets.

# Disclaimers

* In accordance with the API Guidelines, the author of this package is not requesting log-in information from you and does not intend to provide any mechanism by which you could share this secret information.
* Instead, by using this template, you aknowledge that you are using it to create and host your own separate solution that privately contains your log-in information.
* You are fully responsible for your own usage of this code.
