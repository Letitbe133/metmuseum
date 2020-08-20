# Make a dream !

Dreams are a human thing. Or so we believe ;&#41;

This simple web app will pick a random image from the Metropolitan Museum of Art Collection API (https://metmuseum.github.io/) and pass it to the DeepAI API (https://deepai.org/) that will use it to generate a "dream".

Weird stuff ahead ;&#41;

Original idea by miplayer1 (https://github.com/miplayer1)

## How to use

1. clone project

```
git clone git@github.com:Letitbe133/metmuseum.git
```

2. cd into project folder

```
cd met metmuseum
```

3. edit `env.example.local`
   with your DeepAI api key && rename file to `.env.local`

   Get your free DeepAI api key by creating a free account here : https://deepai.org/

```
VUE_APP_API_KEY = YOUR_API_KEY
```

4. install project dependencies

```
npm install
```

5. start dev server

```
npm run serve
```

6. once project is ready for production run

```
npm run build
```

This will create a `dist/` folder containing production ready files that you can `ssh` or `ftp` to your production server

Tip : if your project needs to be installed in a sub folder on your server (eg: `http://example.com/project_folder`), make sure to edit `vue.config.js` accordingly :

```js
  publicPath: process.env.NODE_ENV === "production" ? "/project_folder/" : "/",
//   default is "/"

```
