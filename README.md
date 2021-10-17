# Svelte + Tailwind + Daisy

A template project that has 
[Svelte](https://svelte.dev), 
[TailwindCSS](https://tailwindcss.com/) and 
[DaisyUI](https://daisyui.com/).

Follow [How To](https://blog.logrocket.com/how-to-use-tailwind-css-with-svelte/) if preferred.

When you complete this it should look like this:

![Results in browser](/docs/img/results.png)

## Steps

* Start with a template Svelte project.
<pre>
npx degit sveltejs/template tauri-svelte-tailwind/
cd tauri-svelte-tailwind
npm i
</pre>

* add [TailwindCSS](https://tailwindcss.com/docs/installation)

<pre>
npm install -D tailwindcss@latest postcss@latest autoprefixer@latest
npx tailwindcss init -p
npm i concurrently cross-env
npm i postcss postcss-cli
npm install -D svelte-preprocess postcss-load-config
</pre>

* add [DaisyUI](https://daisyui.com/)
<pre>
npm i daisyui
</pre>

### Additional manual steps

* Add the **purge** in `tailwind.config.js` file (not sure this is related):
<pre>
purge: {
    enabled: !process.env.ROLLUP_WATCH,
    content: ['./public/index.html', './src/**/*.svelte'],
    options: {
    defaultExtractor: content => [
        ...(content.match(/[^<>"'`\s]*[^<>"'`\s:]/g) || []),
        ...(content.match(/(?<=class:)[^=>\/\s]*/g) || []),
    ],
    },
},
</pre>

* Add the **require** for daisy in `tailwind.config.js` file:

<pre>plugins: [require("daisyui")],</pre>

* `postcss.config.js` should look like this:

<pre>
const tailwindcss = require('tailwindcss');
const autoprefixer = require("autoprefixer");
module.exports = {
  plugins: [ 
    require("tailwindcss"), 
    require("autoprefixer")
  ]
} 
</pre>

* `package.json` needs these scripts:

<pre>
  "scripts": {
    "watch:tailwind": "postcss public/tailwind.css -o public/index.css -w",
    "build:tailwind": "cross-env NODE_ENV=production postcss public/tailwind.css -o public/index.css",
    "build": "npm run build:tailwind && rollup -c",
    "start": "sirv public",
    "serve": "serve public -p 80",
    "dev": "concurrently \"rollup -c -w\" \"npm run watch:tailwind\""
  },
</pre>

* add the `index.css` that is output from tailwind to `index.html` after `global.css`:

<pre>
&lt;link rel='stylesheet' href='/index.css'&gt;
</pre>

* `global.css` should have nothing in it.

* Generate the `tailwind.css` file

<pre>
npx tailwindcss -o tailwind.css
</pre>

* Add this to `tailwind.css`:

<pre>
@tailwind base;
@tailwind components;
@tailwind utilities;
</pre>
