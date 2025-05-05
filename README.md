# 🚀 Getting started with Strapi

Strapi comes with a full featured [Command Line Interface](https://docs.strapi.io/dev-docs/cli) (CLI) which lets you scaffold and manage your project in seconds.

### `develop`

Start your Strapi application with autoReload enabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-develop)

```
npm run develop
# or
yarn develop
```

### `start`

Start your Strapi application with autoReload disabled. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-start)

```
npm run start
# or
yarn start
```

### `build`

Build your admin panel. [Learn more](https://docs.strapi.io/dev-docs/cli#strapi-build)

```
npm run build
# or
yarn build
```

## ☁️ Cloud Deployment

Strapi gives you many possible deployment options for your project including [Strapi Cloud](https://cloud.strapi.io). Browse the [deployment section of the documentation](https://docs.strapi.io/dev-docs/deployment) to find the best solution for your use case.

```
yarn strapi deploy
```

## ⚙️ Self-Hosting Deployment

### 1. Make sure your project is appropriately Setup

Make sure that you have created the Super Admin Account when you first make the project.

> It's the account that you usually make when you first started creating the Strapi project

If you haven't then you must make one by running:

```
npm run develop
# or
yarn develop
```

open the Strapi admin on your development machine _localhost_ and make one Super Admin Account

### 2. Build the Strapi Project

After that we can start by making sure that we have the latest build of our Strapi project by running:

```
npm run build
# or
yarn build
```

make sure that your build is working correctly in the development machine by running:

```
npm run start
# or
yarn start
```

and checking all the features that you have made inside it.

### 3. Zip your project files

Zip all of your files and folder, with the node_modules **EXCLUDED**

### 4. Dump, Bakcup, Extract, or Export Database

Now you need to dump, backup, extract, or export your Strapi connected Database.
Make sure the resulting format is in `.SQL` or `.sql`

### 5. Making Dedicated Database and Database User in your cPanel

Make your dedicated Database and its user that have all the privilage for the dedicated database.
Then import the `.SQL` or `.sql` that you have exported before into the new database that you have created.

### 5. Making Dedicated Subdomain in your cPanel

> _Note: only do this if you haven't done it already_

In your cPanel Dashboard, look for `Domains` then click on `Domains`

`Domains > Domains`

Click **Create A New Domain**

Then fill the empty boxes with the domain that you wants.
It can be one of the following:

```
strapi.yourserver.com

cms.yourserver.com

yoursubdomainchoice.yourserver.com
```

Uncheck the box saying `Share document root (/home/yourusername/public_html) with “yourserver.com”.` **THIS IS IMPORTANT**

Then click `Submit` and wait until it has finished creating your new subdomain.

> After you're done with the submission process, make sure that the SSL/TLS Certificate has also been created for your new Subdomain
> by going into:
> `Security > SSL/TLS Status`.
> If you see that it's still running or generating for the certificate then wait until it's finished before continuing to the next step.

### 6. Upload and Extrack your project files, then adjust the .env

If you have finished the previous process correctly then there should be a new folder created.
You can check by looking for your `Files > File Manager` then navigate into:

`/home/yourusername/public_html/yoursubdomainchoice.yourserver.com`

Upload your zip file into that folder and extract all of the files and folders inside it.

Edit the `.env` and only change the following:

```
HOST=yoursubdomainchoice.yourserver.com
PORT=80
```

Make sure to hit **Save** after you've done editing.

### 6. Setup Node.js App in your cPanel

In your cPanel dashboard go into `Software > Setup Node.js App` then click on **Create Application**

Then fill as the following:

- Select `NodeJS Version 20+`
- Set Application mode as `Production`
- Set Application root as `/home/yourusername/public_html/yoursubdomainchoice.yourserver.com`
- Pick Application URL as `yoursubdomainchoice.yourserver.com`
- Set Application startup file as `server.js`

Click **Create**

After you the creation process has been finished, **stop the application** and note down the command to enter the virtual enviroment.
It usually looks like this:

> Enter to the virtual environment.To enter to virtual environment, run the command: `source /home/yourusername/nodevenv/public_html/yoursubdomainchoice.yourserver.com/yournodejsversion/bin/activate && cd /home/yourusername/public_html/yoursubdomainchoice.yourserver.com`

### 7. In cPanel Terminal do npm install

Go to `Advanced > Terminal`, then paste the command that you have noted down before.

After succesfully entering the virtual enviroment, the command line should change, there should be a node version and your subdomain.
It should aproximately look like this:

> [/public_html/yoursubdomainchoice.yourserver.com (yournodejsversion)] [username yoursubdomainchoice.yourserver.com]$

Then do `npm install` and wait until it has finished **completely**

#### Troubleshooting Problems

<details>

<summary>Fails because out of memory</summary>

- make sure that you have you're using NodeJS Version 20+, if you already did then change it into NodeJS Version 20.x.x specifically. Then do the npm install process throug **the Terminal** again.
- If you have make sure the NodeJS Version and the memory is still not enough then in your cPanel terminal run `ps faux` and check if there is any other Node process running.

- If there are other Node process running then clean them using this command `kill -9 $(ps faux | grep node | grep -v grep | awk {'print $2'})` after it's done, do the npm install process through **the Terminal** again.

</details>

### 7. Editing the server.js

Go back to `Files > File Manager` and navigate again into `/home/yourusername/public_html/yoursubdomainchoice.yourserver.com`.

Inside there should now be server.js, if you have done **Step 6** correctly. Edit the file and replace the content with the following:

```js
async function main() {
  const { createStrapi, compileStrapi } = require("@strapi/strapi");

  const appContext = await compileStrapi();
  const app = await createStrapi(appContext).start();

  app.log.level = "error";
}

main().catch((error) => {
  console.error(error);
  process.exit(1);
});
```

Make sure you succesfully **Save** it then navigate back into your cPanel Dashboard and go staright into the `Software > Setup Node.JS App > yoursubdomainchoice.yourserver.com`.

Then hit **start** now your project should have run correctly. You can now just enter your `yoursubdomainchoice.yourserver.com` and the Strapi should be deployed correctly.

## 📚 Learn more

- [Resource center](https://strapi.io/resource-center) - Strapi resource center.
- [Strapi documentation](https://docs.strapi.io) - Official Strapi documentation.
- [Strapi tutorials](https://strapi.io/tutorials) - List of tutorials made by the core team and the community.
- [Strapi blog](https://strapi.io/blog) - Official Strapi blog containing articles made by the Strapi team and the community.
- [Changelog](https://strapi.io/changelog) - Find out about the Strapi product updates, new features and general improvements.

Feel free to check out the [Strapi GitHub repository](https://github.com/strapi/strapi). Your feedback and contributions are welcome!

## ✨ Community

- [Discord](https://discord.strapi.io) - Come chat with the Strapi community including the core team.
- [Forum](https://forum.strapi.io/) - Place to discuss, ask questions and find answers, show your Strapi project and get feedback or just talk with other Community members.
- [Awesome Strapi](https://github.com/strapi/awesome-strapi) - A curated list of awesome things related to Strapi.

---

<sub>🤫 Psst! [Strapi is hiring](https://strapi.io/careers).</sub>
