# snykapps
To begin, create a directory to house the project somewhere on your device. From within the newly created directory, we'll initialize a package.json manifest for our application to keep track of our dependencies and ensure our project is portable:
1 
mkdir my-snyk-app
2 
cd my-snyk-app
3 
npm init
4 
npm install typescript --save-dev
5 
vi tsconfig.json
6 
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
7 
mkdir ./dist
mkdir ./src
8 
vi ./src/index.ts
9
const world = 'world';

export function hello(world: string = world): string {
  return `Hello ${world}! `;
}
10
npx tsc
11
ls
12
npm install --save \
  express \
  express-rate-limit \
  express-session \
  http
13
npm install --save-dev \
  @types/express \
  @types/express-rate-limit \
  @types/express-session \
  @types/node
14
vi ./src/app.ts
15
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
16
npx tsc
17
mkdir -p ./src/routes/index
touch ./src/routes/index/indexController.ts
