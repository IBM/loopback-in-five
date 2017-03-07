## Loopback in Five

This is the git repository that shows you how to get a Loopback server set up in no time. This server will:

- serve up information about animals
- be secured by user authentication
- link to a persistent data store hosted by [Compose](https://compose.io)

This repository contains a completed product, but the steps below will show you how to replicate it.

## Installation
You'll need to have Node.js and npm installed on your machine. Go [here](https://nodejs.org/en/download/) if you haven't done that yet.  

Go to your command line and run the following:

```
npm install -g loopback-cli
```

## Setup

### Step 1 - API Scaffolding
Navigate to an empty folder using your command line. When you're there, type:

```
lb
```

It will start the Yeoman generator, and allow you to make selections about your API using the command line. You want to select the following options:

```
     _-----_
    |       |    ╭──────────────────────────╮
    |--(o)--|    │  Let's create a LoopBack │
   `---------´   │       application!       │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What's the name of your application? marketplace
? Enter name of the directory to contain the project: (workshop-marketplace)
   create marketplace/
     info change the working directory to marketplace
? Which version of LoopBack would you like to use? 3.x (current)
? What kind of application do you have in mind? api-server (A LoopBack API server with local User auth)
```

Once you hit enter once more, `npm install` will run automatically and pull down all of your dependencies for you.

### Step 2 - Linking a Datasource

Next, from the same directory as step 1, you want to type the following into your command line:

```
lb datasource
```

Just like the previous step, Loopback will walk you through the necessary configuration steps to do. You'll enter the following:

```
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

```
mongodb://animalsUsername:loopbackRocks123@aws-us-east-1-portal.26.db
layer.com:17243/animalsDemo?ssl=true
```

Once you install the connector, you're ready to move on.

### Step 3 - Making your Model Object

In the same directory as the previous step, you want to type the following into your command line:

```
lb model
```

Just like before, you'll be walked through the process of making a model object, all from the command line. You should get to this point:

```
? Enter the model name: Animal
? Select the data-source to attach Animal to: ComposeMongo (mongodb)
? Select model's base class PersistedModel
? Expose Animal via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? common
```

After we type all of this, we will then start adding properties. Here is what your three properties should look like:

```
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

### Step 4 - Securing your API

In the same directory as the previous step, you want to type the following into your command line:

```
lb acl
```

Now, you'll be walked through the process of setting up an ACL (access control list) for your API. You want to start with the following configuration:

```
? Select the model to apply the ACL entry to: Animal
? Select the ACL scope: All methods and properties
? Select the access type: All (match all types)
? Select the role All users
? Select the permission to apply Explicitly deny access
```

This means your API is practically inaccessible to anyone, but you can run the same command again to start selectively allowing access to it. Type `lb acl` again, and set up the following configuration:

```
? Select the model to apply the ACL entry to: Animal
? Select the ACL scope: A single method
? Enter the method name find
? Select the role Any authenticated user
? Select the permission to apply Explicitly grant access
```

Now your API is up to date and ready to try out.

### Step 5 - Explore your API

Type `node .` into your command line, and enter the API Explorer URL into your web browser. It should be located, by default, at `http://0.0.0.0:3000/explorer`.

Once you are here, you can play around right in the API Explorer with both of your objects. Since you've set up user authentication, you'll want to navigate to the `/POST` method for your User object, and create a new user. After you do this, try the `/GET/login` method on the same class to get an access token. You can post this access token right into the API Explorer, and you can start making authenticated requests right in your browser.

### Notes

Feel free to email me at [david.okun@ibm.com](mailto:david.okun@ibm.com) if you have any questions!!!
