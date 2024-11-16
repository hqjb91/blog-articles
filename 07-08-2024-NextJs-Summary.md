# Next.js Primer

Basic Notes on NextJS.

---

## Prerendering in Next.js

Next.js supports two main forms of prerendering:

1. **Static Generation**  
   HTML is generated at build time and reused on each request.
   
2. **Server-Side Rendering (SSR)**  
   HTML is generated on each request based on server-side fetched data.

---

### Data Fetching Approaches

#### Client-Side Fetching
Data is fetched dynamically using JavaScript in the browser.  
Example: Asynchronous HTTP calls executed after the page has been rendered.

#### Server-Side Fetching
Data is fetched on the server to render the page before sending it to the client.

- **Pre-Next.js 9.3**
  - **`getInitialProps`**
    - Fetches data during both server-side rendering (initial request) and client-side navigation.
    - Pre-renders the page with fetched data.

- **Post-Next.js 9.3**
  - **`getServerSideProps`**
    - Fetches data only on the server during the initial request.
    - Used for **Server-Side Rendering (SSR)**.

  - **`getStaticProps`**
    - Fetches data during the build process for **Static Site Generation (SSG)**.
    - Add a `revalidate` property for **Incremental Static Regeneration (ISR)** to periodically regenerate the page.

  - **Using Server Components (Next.js 13+)**
    - Leverages the `fetch` API within server components to replace `getStaticProps` and `getServerSideProps`.
    - Automatically static renders data unless dynamic rendering is opted for or detected.
    - Cache invalidation replicates Incremental Static Regeneration.

---

### Rendering Strategies in Next.js

#### Static Generation (SSG)
- **Default Mode**: HTML is pre-rendered at build time.
- Example: Applications hosted on platforms like IIS without `getStaticProps` or `getServerSideProps` rely on client-side data fetching instead.

#### Server-Side Rendering (SSR)
- HTML is generated on each request using server-side data fetching methods like `getServerSideProps`.

#### Incremental Static Regeneration (ISR)
- Pages are generated as requested and then cached for reuse.

---

## React Suspense for Data Fetching

React Suspense enables smooth handling of asynchronous data by displaying fallback content (like spinners) while data is loading.  

Example of using Suspense in Next.js:

### Fetch Data Function
```javascript
function fetchPosts(userId) {
  let url = `https://jsonplaceholder.typicode.com/posts${
    userId ? "?userId=" + userId : ""
  }`;
  let status = "pending"; // Tracks the state of the fetch
  let result;

  let fetching = fetch(url)
    .then((res) => res.json())
    .then((success) => {
      status = "fulfilled";
      result = success;
    })
    .catch((error) => {
      status = "rejected";
      result = error;
    });

  return () => {
    if (status === "pending") {
      throw fetching; // Suspends the React render process
    } else if (status === "rejected") {
      throw result; // Returns the error
    } else if (status === "fulfilled") {
      return result; // Returns the fetched data
    }
  };
}
```

### Key Points
- **Suspension**: Tells React that data is still being fetched.
- **Fallback Handling**: Displays a fallback spinner or placeholder while the data loads.

Refererence is taken from : [React Suspense in Next.js](https://medium.com/@chaitanya99j/a-react-developers-basic-guide-to-data-fetching-in-next-js-13-6cda3d0d853a).