To use `getStaticPaths` in addition to `getStaticProps` in your Next.js code, you can implement dynamic routing for your pages. Here's a step-by-step guide:

1. **Create a Page with Dynamic Routing**:

   First, create a page file with square brackets in the filename to indicate dynamic routing. For example, create a file named `[slug].js` inside the `pages` directory.

   ```javascript
   // pages/[slug].js
   import { getStaticProps, getStaticPaths } from 'next';

   export async function getStaticPaths() {
     // Fetch a list of possible values for the dynamic route parameter
     const paths = [
       { params: { slug: 'page1' } },
       { params: { slug: 'page2' } },
       // Add more possible values as needed
     ];

     return {
       paths,
       fallback: false, // Set to 'false' to return a 404 for unknown paths
     };
   }

   export async function getStaticProps({ params }) {
     // Use the 'slug' parameter to fetch data for the specific page
     const slug = params.slug;
     // Fetch data for the specified page
     const data = await fetch(`https://api.example.com/pages/${slug}`);
     const jsonData = await data.json();

     return {
       props: {
         data: jsonData,
       },
     };
   }

   function DynamicPage({ data }) {
     // Render the data for the dynamic page
     return (
       <div>
         {/* Render your data here */}
       </div>
     );
   }

   export default DynamicPage;
   ```

   In this example, we're using `[slug].js` to create dynamic routes based on a `slug` parameter.

2. **Define `getStaticPaths` Function**:

   In the code above, the `getStaticPaths` function is used to specify the possible values for the `slug` parameter. These values are returned as an array of `params` objects. You can add as many possible values as needed.

   - `fallback: false`: Setting `fallback` to `false` means that Next.js will return a 404 error for paths that aren't defined in the `paths` array. If you want to enable dynamic routes with fallback behavior, you can set `fallback` to `'blocking'` or `true`.

3. **Use the Data in Your Component**:

   You can access the `data` prop in the `DynamicPage` component to render the data specific to each dynamic route.

4. **Build and Start the Development Server**:

   Build your Next.js project using `next build` and start the development server with `next start`.

Now, your Next.js application will generate static pages based on the dynamic routes defined in `getStaticPaths`. When a user accesses a URL like `/page1`, Next.js will pre-render the page using the `getStaticProps` function with the corresponding `slug` parameter.


=========================
=========================


without ISR no page will update after first time of generation

**First basic**
```js
const TripDetails = ({ data: trip }) => {
  return (
    <div>
      <h1>{trip.title}</h1>
      <p>{trip.price}</p>
      <p>{trip.loc}</p>

    </div>
  );
}

export const getStaticPaths = async () => {


  return {
    paths: [
      { params: { tripId: '1' } },
      { params: { tripId: '2' } },
      { params: { tripId: '3' } }
    ],
    fallback: false
  }
}


export const getStaticProps = async (ctx) => {
  let { params } = ctx

  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips/${params.tripId}`
  );
  const data = await res.json();

  return {
    props: {
      data
    }
  }
}

export default TripDetails;

```

**2nd basic**

```js
const TripDetails = ({ data: trip }) => {
  return (
    <div>
      <h1>{trip.title}</h1>
      <p>{trip.price}</p>
      <p>{trip.loc}</p>

    </div>
  );
}

export const getStaticPaths = async () => {
  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips`
  );

  const data = await res.json();
  console.log(data)

  const paths = data.map(item => (
    { params: { tripId: `${item.id}` } }
  ))

  return {
    paths,
    fallback: false
  }
}


export const getStaticProps = async (ctx) => {
  let { params } = ctx

  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips/${params.tripId}`
  );

  const data = await res.json();

  return {
    props: {
      data
    }
  }
}

export default TripDetails;
```

-----------------------------------------------------------------
**fallback True approac**


```js
import { useRouter } from "next/router";

const TripDetails = ({ data: trip }) => {

  const router = useRouter()

  if (router.isFallback) {
    return <div>Loading...</div>
  }
  return (
    <div>
      <h1>{trip.title}</h1>
      <p>{trip.price}</p>
      <p>{trip.loc}</p>

    </div>
  );
}

export const getStaticPaths = async () => {
  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips`
  );

  const data = await res.json();

  const paths = data.map(item => (
    { params: { tripId: `${item.id}` } }
  ))

  return {
    paths,
    fallback: true
  }
}


export const getStaticProps = async (ctx) => {
  let { params } = ctx

  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips/${params.tripId}`
  );

  const data = await res.json();

  if (!data.id) {
    return {
      notFound: true
    }
  }

  return {
    props: {
      data
    }
  }
}

export default TripDetails;
```

**fallback blocking**

will not show loading page

```js

const TripDetails = ({ data: trip }) => {

  return (
    <div>
      <h1>{trip.title}</h1>
      <p>{trip.price}</p>
      <p>{trip.loc}</p>

    </div>
  );
}

export const getStaticPaths = async () => {
  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips`
  );

  const data = await res.json();

  const paths = data.map(item => (
    { params: { tripId: `${item.id}` } }
  ))

  return {
    paths,
    fallback: 'blocking'
  }
}


export const getStaticProps = async (ctx) => {
  let { params } = ctx

  const res = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/trips/${params.tripId}`
  );

  const data = await res.json();

  if (!data.id) {
    return {
      notFound: true
    }
  }

  return {
    props: {
      data
    }
  }
}

export default TripDetails;
```
