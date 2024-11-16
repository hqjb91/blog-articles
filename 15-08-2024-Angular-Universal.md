# Angular Universal: Enhancing SEO with Server-Side Rendering

Angular applications are typically packaged and rendered on the client side, which can lead to poor Search Engine Optimization (SEO). **Angular Universal** addresses this limitation by pre-rendering the application on the server and delivering a static HTML file to the user.

---

## Adding Angular Universal to Your Application

To integrate Angular Universal into your project, run the following command:

```bash
ng add @nguniversal/express-engine
```

This will add several files to your project, including:

- `app.server.module.ts`
- `main.server.ts`
- `tsconfig.server.json`
- `server.ts`

The `server.ts` file runs an Express application to generate the frontend and serve it to the user. You can modify configurations such as the port within this file.

---

## Testing the Server-Side Rendering Locally

Use the following command to test the server-side rendering in development:

```bash
npm run dev:ssr
```

You can then access the server at `http://localhost:4200` on your local machine.

---

## Deploying Angular Universal to Production

To deploy the application to production, follow these steps:

1. **Build the Application**:
   Run the following command to build both browser and server bundles:

   ```bash
   ng build && ng run app-name:server
   ```

   This will generate `browser` and `server` folders under the `dist` directory.

2. **Run the Server**:
   Use a process manager like `pm2` to run the generated Node.js server:

   ```bash
   pm2 start dist/app-name/server/main.js
   ```

---

## Common Issues and Solutions

### 1. **Accessing the `Window` Object**
   Since server-side rendering does not have access to the browser's `window` object, you may encounter issues. To address this, avoid direct use of the `window` object. Instead, use Angular’s dependency injection or conditional logic to ensure compatibility.

---

### 2. **Relative Path URLs**
   Relative paths might fail during server-side rendering. To resolve this, use an `HttpInterceptor` to convert relative paths to absolute URLs. 

   Example:

   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpInterceptor, HttpRequest, HttpHandler } from '@angular/common/http';

   @Injectable()
   export class AbsoluteUrlInterceptor implements HttpInterceptor {
       intercept(req: HttpRequest<any>, next: HttpHandler) {
           const absoluteReq = req.clone({ url: 'https://yourdomain.com' + req.url });
           return next.handle(absoluteReq);
       }
   }
   ```

---

### 3. **DOM Manipulation**
   When interacting with the DOM, always use Angular's `Renderer2`. For example:

   **Before** (Direct DOM manipulation):
   ```typescript
   @Directive({
     selector: '[x-large]'
   })
   export class XLarge {
       constructor(element: ElementRef) {
           element.nativeElement.style.fontSize = 'x-large';
       }
   }
   ```

   **After** (Using `Renderer2`):
   ```typescript
   @Directive({
     selector: '[x-large]'
   })
   export class XLarge {
       constructor(element: ElementRef, renderer: Renderer2) {
           renderer.setStyle(element.nativeElement, 'fontSize', 'x-large');
       }
   }
   ```

---

### 4. **Local Storage**
   Server-side rendering does not support `localStorage` or `sessionStorage`. To handle this, add the following polyfills to `server.ts`:

   ```typescript
   import 'localstorage-polyfill';

   (global as any).sessionStorage = (global as any).localStorage;
   (global as any).document = { cookie: '' };
   (global as any).window = {
       location: {}
   };
   ```
