profile2.js

```js

import { useState } from 'react'
import { useRouter } from 'next/router'

function ProfileList({ data: profileList }) {
  const [profiles, setProfiles] = useState(profileList)
  const router = useRouter()

  const handleClick = async (_loc) => {
    const response = await fetch(`https://my-json-server.typicode.com/mozsocia/json_db/profile?loc=${_loc}`)
    const Ndata = await response.json()
    setProfiles(Ndata)
    router.push(`/profile2?loc=${_loc}`, undefined, { shallow: true })
  }
  return (
    <>
      <br />
      <button onClick={() => handleClick("khulna")}>Filter Khulan</button>
      <br />
      <button onClick={() => handleClick("dhaka")}>Filter Dhaka</button>
      <h1>List of events</h1>
      {profiles.map(item => {
        return (
          <div key={item.id}>
            <h2>
              {item.id} {item.name} {item.age} | {item.loc}
            </h2>
            <p>{item.description}</p>
            <hr />
          </div>
        )
      })}
    </>
  )
}

export default ProfileList

export async function getServerSideProps(context) {
  const { loc } = context.query

  const queryString = loc ? `loc=${loc}` : ""

  const response = await fetch(`https://my-json-server.typicode.com/mozsocia/json_db/profile?${queryString}`)
  const data = await response.json()

  return {
    props: {
      data
    }
  }
}

```