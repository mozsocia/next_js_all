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