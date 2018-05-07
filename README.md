# LoopBack in 5: Rapid APIs From Your Terminal

Express.js made it easy to roll up a REST API in Node.js, and it's become the de-facto library for standing up a backend. [Strongloop](https://strongloop.com) took that and made it even easier to create your APIs.

Strongloop created LoopBack, a tool that allows you to generate a Node.js API that's architected like Express.js, and adheres to the OpenAPI spec. You can have a production-ready web API online with [Cloud Foundry](https://www.cloudfoundry.org/) and ready to go within 10 quick minutes, and it's flexible and scalable to any needs you might have as it grows.

You can learn how to do this in two short videos. Here's the first video, where you'll create the API: 

[![Create an API in 5 minutes](https://img.youtube.com/vi/Bm6u1f2PUag/0.jpg)](https://www.youtube.com/watch?v=Bm6u1f2PUag)

And here's the second video, where you'll deploy it to Cloud Foundry on IBM Cloud:

[![Deploy your API in 5 more minutes](https://img.youtube.com/vi/nlP8mCcQDDY/0.jpg)](https://www.youtube.com/watch?v=nlP8mCcQDDY)


## Diagram

![](https://media.github.ibm.com/user/1650/files/5961ce34-a504-11e7-9142-488fdeac0713)

## Included Components
- [LoopBack](https://loopback.io)
- [Cloud Foundry](https://www.cloudfoundry.org/)
- [Compose.io](https://compose.io)

# System Preparations

### Compose.io 

It's important to point out that you can use any valid MongoDB service with this, but I've chosen Compose.io for this. Go to their [website](https://compose.io) and sign up for a deployment. **NB**: compose.io charges 31 USD/mo after a free trial month. Create a database, a username and password for access, and get the connection string for it. It should look something like this:

```
mongodb://<user>:<password>@aws-us-east-1-portal.26.dblayer.com:17243/animalsDemo?ssl=true
```

Hang onto this, as you'll need this for later.

### IBM Cloud
Create an account on IBM Cloud for free by going [here](https://console.bluemix.net/registration?cm_sp=developer-_-api-connect-_-github-loopback-tutorial). After that, download the IBM Cloud CLI to your local machine [here](https://console.bluemix.net/docs/cli/reference/bluemix_cli/all_versions.html#bluemix-cli-installer-downloads) and log in with the same account that you just created.

You can deploy this application anywhere that supports Cloud Foundry. Since IBM Cloud supports it out of the box, we'll use it to deploy the application.

### Node.js
You'll also need to make sure you have at least v6.0 of Node.js installed on your local machine. I recommend using [Node Version Manager](https://github.com/creationix/nvm) to handle your Node.js versions. In your command line, type:

```bash
touch ~/.bash_profile
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
source ~/.bash_profile
```

This makes sure you have a `.bash_profile` file in your root directory, installs the version manager, and restarts your terminal instance. You should be ready to install Node.js now by typing:

```bash
nvm install node
nvm use node
```

This will install the latest version of Node.js (v8.3.0 at the time of this writing) and put it into effect on your machine. If you run into problems using nvm, go [here](https://github.com/creationix/nvm) for more information. To confirm you are installed correctly, ensure that you get output for each of the following commands:

```bash
node -v
npm -v
```
### LoopBack

If you're clean so far, you'll now want to install LoopBack. Type the following:

```bash
npm install -g loopback-cli
lb -v
```

Now you're ready to create and deploy your API.

# Steps

### Part A: Create an API in just five minutes

1. [Scaffolding your API](#1-API-Scaffolding)
2. [Linking a Datasource](#2-linking-a-datasource)
3. [Generating your Model Object](#3-generating-your-model-object)
4. [Securing the API with User Authentication](#4-securing-the-api-with-user-authentication)
5. [Explore Your API](#5-explore-your-api)

### Part B: Deploy your API in just five more minutes

6. [Create a Cloud Foundry instance](#6-create-a-cloud-foundry-instance)
7. [Prepare your API Deployment Manifest](#7-prepare-your-api-deployment-manifest)
8. [Push to IBM Cloud](#8-push-to-ibm-cloud)

## Part A: Create an API in just five minutes

### 1. API Scaffolding
Navigate to an empty folder using your command line. When you're there, type:

```bash
lb
```

It will start the Yeoman generator, and allow you to make selections about your API using the command line. You want to select the following options:

```bash
     _-----_
    |       |    ╭──────────────────────────╮
    |--(o)--|    │  Let's create a LoopBack │
   `---------´   │       application!       │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What's the name of your application? AnimalsDemo
? Which version of LoopBack would you like to use? 3.x (current)
? What kind of application do you have in mind? api-server (A LoopBack API server with local User auth)
```

Important note: When you are choosing the name of your application, if you just hit Enter, you'll default to the folder you are currently in. If you want to specify a unique folder, type in the name of the folder you want, and LoopBack will create it for you. Keep in mind that you'll have to do all future steps from that folder.

After you enter all of your choices, `npm install` will run automatically and pull down all of your dependencies for you.

### 2. Linking a Datasource

Next, make sure you are in your project directory at the root folder. You want to type the following into your command line:

```bash
lb datasource
```

Just like the previous step, Loopback will walk you through the necessary configuration steps to do. You'll enter the following:

```bash
? Enter the data-source name: ComposeMongo
? Select the connector for ComposeMongo: MongoDB (supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings (eg: mongodb://username:password@hostname:port/database): mongodb://animalsUsername:loopbackRocks123@aws-us-east-1-portal.26.db
layer.com:17243/animalsDemo?ssl=true
? host:
? port:
? user:
? password:
? database:
? Install loopback-connector-mongodb@^1.4 Yes
```

It's important to point out that, for this demo, the connection string simply overrides whatever options you would enter for the host, port, user, password, and database. You could do either one for your own datasource, but we're using a simple connection string that [Compose](https://compose.io) has generated for us. Again, that string is:

```bash
mongodb://animalsUsername:loopbackRocks123@aws-us-east-1-portal.26.db
layer.com:17243/animalsDemo?ssl=true
```

Once you install the connector, you're ready to move on.

### 3. Generating your Model Object

In the same directory as the previous step, you want to type the following into your command line:

```bash
lb model
```

Just like before, you'll be walked through the process of making a model object, all from the command line. You should get to this point:

```bash
? Enter the model name: Animal
? Select the data-source to attach Animal to: ComposeMongo (mongodb)
? Select model's base class PersistedModel
? Expose Animal via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? common
```

After we type all of this, we will then start adding properties. Here is what your three properties should look like:

```bash
Let's add some Animal properties now.

Enter an empty property name when done.
? Property name: photoURL
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:

Let's add another Animal property.
Enter an empty property name when done.
? Property name: looksFriendly
   invoke   loopback:property
? Property type: boolean
? Required? Yes
? Default value[leave blank for none]: true

Let's add another Animal property.
Enter an empty property name when done.
? Property name: type
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:
```

After you get prompted to add a fourth property, hit Enter and it'll jump out of the model creation dialog.

At this point, you have a working API! Feel free to try it out with `node .`, and navigate to the API Explorer, which will be provided in the command line for you.

**OR**, we can skip ahead to step 4.

### 4. Securing the API with User Authentication

In the same directory as the previous step, you want to type the following into your command line:

```bash
lb acl
```

Now, you'll be walked through the process of setting up an ACL (access control list) for your API. You want to start with the following configuration:

```bash
? Select the model to apply the ACL entry to: Animal
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role All users
? Select the permission to apply Explicitly deny access
```

This means your API is practically inaccessible to anyone, but you can run the same command again to start selectively allowing access to it. Type `lb acl` again, and set up the following configuration:

```bash
? Select the model to apply the ACL entry to: Animal
? Select the ACL scope: A single method
? Enter the method name find
? Select the role Any authenticated user
? Select the permission to apply Explicitly grant access
```

Now your API is up to date and ready to try out.

### 5. Explore your API

Type `node .` into your command line, and enter the API Explorer URL into your web browser. It should be located, by default, at `http://0.0.0.0:3000/explorer`.

Once you are here, you can play around right in the API Explorer with both of your objects. Since you've set up user authentication, you'll want to navigate to the `/POST` method for your User object, and create a new user. After you do this, try the `/GET/login` method on the same class to get an access token. You can post this access token right into the API Explorer, and you can start making authenticated requests right in your browser.

## Part B: Deploy your API in just five more minutes

### 6. Create a Cloud Foundry Instance

It's important to point out here that you can run this anywhere Cloud Foundry is supported. Luckily, IBM Cloud supports Cloud Foundry, so for this tutorial, we'll be using that.

Log into your IBM Cloud account and navigate to your [dashboard](https://console.ng.bluemix.net). In the upper right hand corner of the dashboard, click on the "Create App" button that looks like this:

![photo1](./tutorialPhotos/createApp.png)

On the left hand side of the next screen, you'll want to click on "Cloud Foundry Apps", and you'll come to a collection of runtimes you can start up. Click on the Node.js SDK button:

![photo2](./tutorialPhotos/sdkForNodejs.png)

On the next screen, you'll be prompted to enter a name for your application. By default, your URL will be `your-app-name + mybluemix.net`, so make sure you choose something that will be unique. Create the application, and you'll be taken to documentation for how to use Cloud Foundry on Bluemix. I recommend reading this in your downtime!

### 7. Prepare your API Deployment Manifest

You'll be creating and editing three files next:

- `.cfignore`
- `manifest.yml`
- `package.json`

#### .cfignore

Create a file in your API's root directory titled `.cfignore`, and type this for the first and only line of the file:

```bash
node_modules/
```

The purpose of this file is much like a `.gitignore` file, but for making sure you don't upload unnecessary files to your Cloud Foundry instance. CLoud Foundry will run `npm install` for you, as well as whatever startup commands you list in your `package.json` file for the Node.js runtime.

#### manifest.yml
Next, you'll want to create another file in your API's root directory titled `manifest.yml`, and you'll want the file to look like this:

```yaml
applications:
- name: david-okun-loopback-testing
  memory: 256M
```

This file will be searched for by the IBM Cloud CLI to link your API with the right deployed instance in Cloud Foundry.

The name of the application in this file should exactly correspond to what you listed in your IBM Cloud dashboard. Also, the memory needs to be a multiple of 128, and starts at a minimum of 128M. IBM Cloud will not charge you for more than 512 megabytes of memory on your account.

#### package.json

Open up your package.json file from your API's root directory in your text editor, and find the following node:

```json
"engines": {
    "node": "8.1.0",
    "npm": "5.1.0"
  },
```

If you can't find this node in your file, you can add it. The goal is to make sure that you specify the version of Node.js you are using locally, so that Cloud Foundry will download the correct buildpack and version of Node.js on your deployed server. This will help you make sure that you don't run into any unexpected issues on your deployment.

### 8. Push to IBM Cloud

By now, you should have set up your IBM Cloud CLI on your machine. Before you log in for the first time, you'll need to set your API location, according to what you did with your account online by running `bx api` and setting it to one of the following URLs:

- `https://api.ng.bluemix.net` **(US South)**
- `https://api.eu-gb.bluemix.net` **(United Kingdom)**
- `https://api.au-syd.bluemix.net` **(Sydney)**
- `https://api.eu-de.bluemix.net` **(Frankfurt)**

After that, you'll need to make sure you login with `bx login`, and you'll want to make sure you choose the same organization and space that you deployed your Cloud Foundry app when you sign in with the CLI. To check this, run `bx app list`, and you should see the name of your application in the output. If you don't run `bx logout` and `bx login` again to choose the right organization and development space.

If you've completed all of the above steps, run `bx app push`, and watch the output as Bluemix uploads your API and starts the container. Within a couple of minutes, your API should be live! You can go to the [dashboard](https://console.ng.bluemix.net) to check the status of your application.


# References
[LoopBack](https://loopback.io)


# License
[Apache 2.0](http://www.apache.org/licenses/LICENSE-2.0)
