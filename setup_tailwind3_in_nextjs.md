## Create your project

Start by creating a new Next.js project if you don’t have one set up already. The most common approach is to use [Create Next App](https://nextjs.org/docs/api-reference/create-next-app).

```perl
npx create-next-app@latest my-project --typescript --eslintcd my-project
```

Install `tailwindcss` and its peer dependencies via npm, and then run the init command to generate both `tailwind.config.js` and `postcss.config.js`.

```kotlin
npm install -D tailwindcss@3 postcss autoprefixer
npx tailwindcss init -p
```

## Configure your template paths

Add the paths to all of your template files in your `tailwind.config.js` file.

```java
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    "./app/**/*.{js,ts,jsx,tsx,mdx}",
    "./pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./components/**/*.{js,ts,jsx,tsx,mdx}",
 
    // Or if using `src` directory:
    "./src/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

## Add the Tailwind directives to your CSS

Add the `@tailwind` directives for each of Tailwind’s layers to your `globals.css` file.

```less
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Start your build process

Run your build process with `npm run dev`.

## Start using Tailwind in your project

Start using Tailwind’s utility classes to style your content.

```javascript
export default function Home() {
  return (
    <h1 className="text-3xl font-bold underline">
      Hello world!
    </h1>
  )
}
```
