**basic**

profile/index.js
```js
function ProfileList({ data: profiles }) {
  return (
    <>
      <h1>List of News Articles</h1>
      {profiles.map(item => {
        return (
          <div key={item.id}>
            <h2>
              {item.id} {item.name} | {item.age} | {item.loc}
            </h2>
            <hr />
          </div>
        )
      })}
    </>
  )
}

export default ProfileList

export async function getServerSideProps() {
  // console.log('Pre-rendering ProfileList')
  const response = await fetch('https://my-json-server.typicode.com/mozsocia/json_db/profile')
  const data = await response.json()

  return {
    props: {
      data
    }
  }
}


```
----------------------------

**dynamic params**

profile/[location].js

```js
function ProfileById({ data: profiles, location }) {
  return (
    <>
      <h1>Showing news for category "{location}"</h1>
      {profiles.map(item => {
        return (
          <div key={item.id}>
            <h2>
              {item.name} {item.age}
            </h2>
            <p>{item.loc}</p>
            <hr />
          </div>
        )
      })}
    </>
  )
}

export default ProfileById

export async function getServerSideProps(context) {
  const { params } = context
  const { location } = params
  const response = await fetch(
    `https://my-json-server.typicode.com/mozsocia/json_db/profile?loc=${location}`
  )
  const data = await response.json()

  // console.log(`Pre-rendering News Articles for category ${categoryId}`)

  return {
    props: {
      data,
      location
    }
  }
}
```