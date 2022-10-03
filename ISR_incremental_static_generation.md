
ISR will update page on request after that time on revalidate, it will update only that page which has been request 

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
    },
    revalidate: 30,
  }
}

export default TripDetails;
```