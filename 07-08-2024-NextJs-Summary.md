#### Prerendering

- NextJS has two forms of pre-rendering
	- Fetching Data
		- Client-side
			- Per normal, just make HTTP calls using javascript
		- Server-side
			- getInitialProps
				- Pre-NextJS 9.3
				- Server will pre-render page with the fetched data and serve it together with the HTML page
				- Executed on the server side during the initial page request and on subsequent client-side navigations
			- getServerSideProps
				- Pre-NextJS 13
				- Only executed on the server side during the initial page request and not on subsequent client-side navigations
				- Used for Server Side Rendering
			-  getStaticProps
				- Pre-NextJS 13
				- Pre-renders the page at build time and fetches the data during the build process
				- Used for Static Site Generation
				- Add revalidate prop to getStaticsProps for Incremental Static Regeneration
			- Using fetch API with server components to replace getStaticProps, getServerSideProps ([Rendering: Server Components | Next.js (nextjs.org)](https://nextjs.org/docs/app/building-your-application/rendering/server-components#server-rendering-strategies))
				- Static rendering via fetch is by default unless we opt (or it is detected) for dynamic rendering
				- Incremental Static Site Regeneration can be replicated for using cache invalidation
	- Static Generation
		- Default mode
		- Generates HTML at build time
		- Pre-rendered HTML is reused at each request
		- E.g. For my previous application project hosted on IIS server, we did not use getStaticProps/getServerSideProps but we still are able to "dynamically" fetch data via asynchronous HTTP calls, these calls are not server-sided at all but are ran using javascript dynamically via the browser (i.e. they are slower than server side fetched data)
	- Server-side Rendering
		- Generates HTML on each request
	- Incremental Static Regeneration
		- Pages are generated as requested but then cached


React Suspense
- This is how you fetch data in NextJS to get loading.js (and underlying React Suspense) to show fallback spinner whilst data is loading
https://medium.com/@chaitanya99j/a-react-developers-basic-guide-to-data-fetching-in-next-js-13-6cda3d0d853a
```javascript
// Fetch external data
function fetchPosts(userId) {
  let url = `https://jsonplaceholder.typicode.com/posts${
    userId ? "?userId=" + userId : ""
  }`;
  let fetching = fetch(url)
    .then((res) => res.json())
    // Fetch request has gone well
    .then((success) => {
      status = "fulfilled";

      result = success;
    })
    // Fetch request has failed
    .catch((error) => {
      status = "rejected";

      result = error;
    });

  return () => {
    if (status === "pending") {
      throw fetching; // Suspend(A way to tell React data is still fetching)
    } else if (status === "rejected") {
      throw result; // Result is an error
    } else if (status === "fulfilled") {
      return result; // Result is a fulfilled promise
    }
  };
}
```