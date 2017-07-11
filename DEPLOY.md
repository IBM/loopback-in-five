## Loopback in Five More - Deployment

This is the git repository that shows you how to take a LoopBack server you've already set up (like [this]() one), and deploy it to Bluemix.

## Installation

In addition to everything you installed [here](https://github.com/StrongLoop-Evangelists/loopback-in-five/blob/master/README.md#installation), you'll need to do the following:

- sign up for a free Bluemix [account](https://console.ng.bluemix.net)
- download the Bluemix [CLI](https://console.bluemix.net/docs/cli/index.html) and log into your account

For the sake of this tutorial, we'll be using the server we created according to [this]() video. You can find the code for it [here]().

## Setup

### Step 1 - Create your Cloud Foundry instance

Log into your Bluemix account and navigate to your [dashboard](https://console.ng.bluemix.net). In the upper right hand corner of the dashboard, click on the "Create App" button that looks like this:

![photo1](./tutorialPhotos/createApp.png)

On the left hand side of the next screen, you'll want to click on "Cloud Foundry Apps", and you'll come to a collection of runtimes you can start up. Click on the Node.js SDK button:

![photo2](./tutorialPhotos/sdkForNodejs.png)

On the next screen, you'll be prompted to enter a name for your application. By default, your URL will be `your-app-name + mybluemix.net`, so make sure you choose something that will be unique. Create the application, and you'll be taken to documentation for how to use Cloud Foundry on Bluemix. I recommend reading this in your downtime!

### Step 2 - Set your API up for deployment

You'll be creating and editing three files next:

- `.cfignore`
- `manifest.yml`
- `package.json`

#### .cfignore

Create a file in your API's root directory titled `.cfignore`, and type this for the first and only line of the file:

```
node_modules/
```

The purpose of this file is much like a `.gitignore` file, but for making sure you don't upload unnecessary files to your Cloud Foundry instance. CLoud Foundry will run `npm install` for you, as well as whatever startup commands you list in your `package.json` file for the Node.js runtime.

#### manifest.yml
Next, you'll want to create another file in your API's root directory titled `manifest.yml`, and you'll want the file to look like this:

```
applications:
- name: david-okun-loopback-testing
  memory: 256M
```

This file will be searched for by the Bluemix CLI to link your API with the right deployed instance in Cloud Foundry.

The name of the application in this file should exactly correspond to what you listed in your Bluemix dashboard. Also, the memory needs to be a multiple of 128, and starts at a minimum of 128M. Bluemix will not charge you for more than 512 megabytes of memory on your account.

#### package.json

Open up your package.json file from your API's root directory in your text editor, and find the following node:

```
"engines": {
    "node": "8.1.0",
    "npm": "5.1.0"
  },
```

If you can't find this node in your file, you can add it. The goal is to make sure that you specify the version of Node.js you are using locally, so that Cloud Foundry will download the correct buildpack and version of Node.js on your deployed server. This will help you make sure that you don't run into any unexpected issues on your deployment.

### Step 3 - Pushing to Bluemix

By now, you should have set up your Bluemix CLI on your machine. Before you log in for the first time, you'll need to set your API location, according to what you did with your account online by running `bx api` and setting it to one of the following URLs:

- `https://api.ng.bluemix.net` **(US South)**
- `https://api.eu-gb.bluemix.net` **(United Kingdom)**
- `https://api.au-syd.bluemix.net` **(Sydney)**
- `https://api.eu-de.bluemix.net` **(Frankfurt)**

After that, you'll need to make sure you login with `bx login`, and you'll want to make sure you choose the same organization and space that you deployed your Cloud Foundry app when you sign in with the CLI. To check this, run `bx app list`, and you should see the name of your application in the output. If you don't run `bx logout` and `bx login` again to choose the right organization and development space.

If you've completed all of the above steps, run `bx app push`, and watch the output as Bluemix uploads your API and starts the container. Within a couple of minutes, your API should be live! You can go to the [dashboard](https://console.ng.bluemix.net) to check the status of your application.

### Notes

Once again, feel free to email me at [david.okun@ibm.com](mailto:david.okun@ibm.com) if you have any questions!!!