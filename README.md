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
ls dist
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
23 (overwrite with below)
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
const privateKey = '-----BEGIN RSA PRIVATE KEY-----\r\nMIICXAIBAAKBgQDNwqLEe9wgTXCbC7+RPdDbBbeqjdbs4kOPOIGzqLpXvJXlxxW8iMz0EaM4BKUqYsIa+ndv3NAn2RxCd5ubVdJJcX43zO6Ko0TFEZx/65gY3BE0O6syCEmUP4qbSd6exou/F+WTISzbQ5FBVPVmhnYhG/kpwt/cIxK5iUn5hm+4tQIDAQABAoGBAI+8xiPoOrA+KMnG/T4jJsG6TsHQcDHvJi7o1IKC/hnIXha0atTX5AUkRRce95qSfvKFweXdJXSQ0JMGJyfuXgU6dI0TcseFRfewXAa/ssxAC+iUVR6KUMh1PE2wXLitfeI6JLvVtrBYswm2I7CtY0q8n5AGimHWVXJPLfGV7m0BAkEA+fqFt2LXbLtyg6wZyxMA/cnmt5Nt3U2dAu77MzFJvibANUNHE4HPLZxjGNXN+a6m0K6TD4kDdh5HfUYLWWRBYQJBANK3carmulBwqzcDBjsJ0YrIONBpCAsXxk8idXb8jL9aNIg15Wumm2enqqObahDHB5jnGOLmbasizvSVqypfM9UCQCQl8xIqy+YgURXzXCN+kwUgHinrutZms87Jyi+D8Br8NY0+Nlf+zHvXAomD2W5CsEK7C+8SLBr3k/TsnRWHJuECQHFE9RA2OP8WoaLPuGCyFXaxzICThSRZYluVnWkZtxsBhW2W8z1b8PvWUE7kMy7TnkzeJS2LSnaNHoyxi7IaPQUCQCwWU4U+v4lD7uYBw00Ga/xt+7+UqFPlPVdz1yyr4q24Zxaw0LgmuEvgU5dycq8N7JxjTubX0MIRR+G9fmDBBl8=\r\n-----END RSA PRIVATE KEY-----'
export default App;
```
24
```
vi ./src/index.ts
```
25 (overwrite with below, dd is the command in vi to delete a line)
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
```
cd ..
```
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
