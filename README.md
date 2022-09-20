# snykapps
To begin, create a directory to house the project somewhere on your device. From within the newly created directory, we'll initialize a package.json manifest for our application to keep track of our dependencies and ensure our project is portable:

1

```
mkdir my-snyk-app
```

2 
```
cd my-snyk-app
```

3 (accept default values)
```
npm init
```
4 
```
npm install typescript --save-dev
```
5 
```
vi tsconfig.json
```

6
```
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "declaration": true,
    "sourceMap": true,
    "moduleResolution": "node",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "skipLibCheck": true,
    "esModuleInterop": true
  },
  "exclude": [
    "./tests",
    "./dist"
  ]
}
```
7 
```
mkdir ./dist
mkdir ./src
```
8 
```
vi ./src/index.ts
```

9
```
const world = 'world';

export function hello(who: string = world): string {
  return `Hello ${who}! `;
}
```
10
```
npx tsc
```
11
```
ls
```
12
```
npm install --save \
  express \
  express-rate-limit \
  express-session \
  http
```
13
```
npm install --save-dev \
  @types/express \
  @types/express-rate-limit \
  @types/express-session \
  @types/node
```
14
```
vi ./src/app.ts
```
15
```
import express from 'express';
import type { Application } from 'express';
import type { Server } from 'http';

class App {
  public app: Application;
  private server: Server;

  constructor() {
    this.app = express();
    this.server = this.listen(3000);
  }

  private listen(port: number) {
    this.server = this.app.listen(port, () => {
      console.log(`App listening on port: ${port}`);
    });

    return this.server;
  }

}

export default App;
```
16
```
npx tsc
```
17
```
mkdir -p ./src/routes/index
touch ./src/routes/index/indexController.ts
```
18
```
mkdir -p ./src/interfaces
vi ./src/interfaces/Controller.ts
```
19
```
import type { Router } from 'express';

export interface Controller {
  path: string;
  router: Router;
}
```
20
```
vi ./src/routes/index/indexController.ts
```
21
```
import type { Controller } from '../../interfaces/Controller';
import type { Request, Response, NextFunction } from 'express';
import { Router } from 'express';

class IndexController implements Controller {
  public path = '/';
  public router = Router();

  constructor() {
    this.initRoutes();
  }

  private initRoutes() {
    this.router.get(`${this.path}`, this.indexPage);
  }

  private indexPage(req: Request, res: Response, next: NextFunction) {
    return res.send('https://bit.ly/3S9vORm');
  }
}

export default IndexController;
```
22
```
vi ./src/app.ts
```
23
```
import express from 'express';
import type { Application } from 'express';
import type { Server } from 'http';
import type { Controller } from './interfaces/Controller';

class App {
  public app: Application;
  private server: Server;

  constructor(controllers: Controller[], port: number) {
    this.app = express();
    this.initRoutes(controllers);
    this.server = this.listen(port);
  }

  private initRoutes(controllers: Controller[]) {
    controllers.forEach((controller: Controller) => {
      this.app.use('/', controller.router);
    });
  }

  private listen(port: number) {
    this.server = this.app.listen(port, () => {
      console.log(`App listening on port: ${port}`);
    });

    return this.server;
  }

}

export default App;
```
24
```
vi ./src/index.ts
```
25 (overwrite with below)
```
import IndexController from './routes/index/indexController';
import App from './app';

new App(
  [
    new IndexController()
  ],
  3000
);
```
26
```
npx tsc && node ./dist/index.js
```
27
```
http://localhost:3000
```


28
cd ..
29
```
git clone https://github.com/snyk/snyk-apps-demo.git
```
30
```
cd snyk-apps-demo
npm install
```
31
```
npm run create-app -- --authToken=<token> --orgId=<orgid> --scopes=org.read org.project.read org.report.read org.project.jira.issue.read org.project.ignore.read --name=<appname>
```
32
```
npm run build
```
33
```
npm run dev
```
