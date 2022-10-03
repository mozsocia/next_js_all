
Header.js
```js

function Header() {
  return <div className='header'>Header</div>
}

export default Header
```

Footer.js
```js
function Footer() {
  return <div className='footer'>Footer</div>
}

export default Footer

```


**_app.js**
```js
import Footer from '../components/Footer'
import Header from '../components/Header'
import '../styles/globals.css'
import '../styles/layout.css'

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
  return (
    <>
      <Header />
      <Component {...pageProps} />
      <Footer />
    </>
  )
}

export default MyApp
```
----------------------------------------------------------------------
-------------------------------------------------------------------
---------------------------------------------------------

layout per page 

about.js
```js
import Head from 'next/head'
import Footer from '../components/layout/Footer'

function About() {
  return (
    <>
      <Head>
        {/* <title>About Codevolution</title> */}
        <meta name='description' content='Free tutorials on web development' />
      </Head>
      <h1 className='content'>About</h1>
    </>
  )
}

export default About

About.getLayout = page => (
  <>
    {page}
    <Footer />
  </>
)
```
_app.js
```js
import Head from 'next/head'
import Footer from '@/layout/Footer'
import Header from '@/layout/Header'
import 'styles/globals.css'
import 'styles/layout.css'

function MyApp({ Component, pageProps }) {
  if (Component.getLayout) {
    return Component.getLayout(<Component {...pageProps} />)
  }
  return (
    <>
      <Head>
        <title>Codevolution</title>
        <meta name='description' content='Awesome YouTube channel' />
      </Head>
      <Header />
      <Component {...pageProps} />
      <Footer />
    </>
  )
}

export default MyApp

```


