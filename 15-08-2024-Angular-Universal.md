Normally angular apps are packaged and then rendered by browser on client side. This results in poor SEO(Search engine optimization) results. Angular Universal helps resolve that by pre-rendering the application on the server-side and sent to the user directly as a static HTML file.

We first add Angular universal to our application:

```
ng add @nguniversal/express-engine
```

app.server.module.ts, main.server.ts, tsconfig.server.json and server.ts files are then added. The server.ts will run an express application which generates the frontend application and send it to the user and we can modify the configurations such as port in it.

For testing we then use the following command:

```
npm run dev:ssr
```

We can then access the server in our local machine at http://localhost:4200

How to deploy it into production? First we build the application using the below code:

```
ng build && ng run app-name:server
```

After this step, Angular will generate a browser and server folder under the dist folder. We can then run the generated node server using either node/nodemon/pm2(I'm using pm2):

```
pm2 start dist/app-name/server/main.js
```

Note: If you access the Window object, as the code is server side renderred, there may be some problems resulting. Another commonly faced issue is that relative path URLs will fail. One solution would be to use a HttpInterceptor to convert relative paths to absolute paths.

You will have to use Renderer2 to interact with DOM, so instead of:

```
@Directive({
  selector: '[x-large]' 
})
export class XLarge {
  constructor(element: ElementRef) {
    element.nativeElement.style.fontSize = 'x-large';
  }
}
```

You have to do this instead :

```
@Directive({
  selector: '[x-large]'
})
class XLarge {
  constructor(element: ElementRef, renderer: Renderer2) {
    // we must interact with the dom through Renderer for webworker/server to see the changes
    renderer.setElementStyle(element, 'fontSize', 'x-large');
  }
}
```

Another commonly faced issue is that of localstorage not working with Angular universal, we can add the following code to server.ts to address this issue:

```
import 'localstorage-polyfill';

(global as any).sessionStorage = (global as any).localStorage;
(global as any).document = {cookie:''};
(global as any).window = {
  location: {}
};
```