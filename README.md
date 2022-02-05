# Dependencies
npm i express dotenv
<br/>
npm i -D typescript ts-node nodemon rimraf eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin jest ts-jest @types/jest supertest @types/supertest @types/express

# package.json scripts
"dev": "nodemon",
<br/>
"build": "rimraf ./dist && tsc",
<br/>
"start": "npm run build && node dist/src/index.js",
<br/>
"test": "NODE_ENV=test jest"

# Files

### .env
PORT=8080

### .env.example
PORT=

### .env.testing
PORT=8080

### .prettierrc.json
{<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "singleQuote": true<br/>
}

### tsconfig.json
{<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "compilerOptions": {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "target": "es2016",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "lib": ["es6"],<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "module": "commonjs",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "rootDir": ".",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "resolveJsonModule": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "allowJs": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "outDir": "dist",<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "esModuleInterop": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "forceConsistentCasingInFileNames": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "strict": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "noImplicitAny": true,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "skipLibCheck": true<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; },<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "include": ["src/\*\*/\*"],<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "exclude": ["node_modules", "\*\*/\*.test.ts"]<br/>
}

### nodemon.json
{<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "watch": ["src"],<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "ext": ".ts",<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "exec": "ts-node ./src/index.ts"<br/>
}

### .eslintrc
{<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "root": true,<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "parser": "@typescript-eslint/parser",<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "plugins": [<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "@typescript-eslint"<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ],<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "extends": [<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "eslint:recommended",<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "plugin:@typescript-eslint/eslint-recommended",<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; "plugin:@typescript-eslint/recommended"<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ]<br/>
}

### .eslintignore
node_modules<br/>
dist

### jest.config.ts
export default {<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; clearMocks: true,<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; coverageProvider: 'v8',<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; moduleFileExtensions: ['js', 'ts'],<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; testMatch: ['\<rootDir\>/tests/**/*.test.ts'],<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; transform: {<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; '^.+\\\\.ts$': 'ts-jest',<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; },<br/>
};

### .gitignore
node_modules<br/>
dist<br/>
.env

### src/index.ts
import app from './app';<br/><br/>

const PORT = process.env.PORT || 8080;<br/><br/>

app.listen(PORT, (): void => {<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; console.log(\`URL server: http://localhost:${PORT}`);<br/>
});<br/>

### src/app.ts
import * as dotenv from 'dotenv';<br/>
import express, { Application } from 'express';<br/><br/>

import routes from './routes';<br/><br/>

dotenv.config({<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path: process.env.NODE_ENV === 'test' ? '.env.testing' : '.env',<br/>
});<br/><br/>

const app: Application = express();<br/>
app.use(express.json());<br/>
app.use(routes);<br/><br/>

export default app;

### src/routes.ts
import express, { Request, Response } from 'express';<br/><br/>

const routes = express.Router();<br/><br/>

routes.get('/', (req: Request, res: Response) => {<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return res.json({ message: 'Hello Friend!' });<br/>
});<br/><br/>

export default routes;

### tests/index.test.ts
import request from 'supertest';<br/><br/>

import app from '../src/app';<br/><br/>

describe('GET /', () => {<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; it('return success message', async () => {<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; const res = await request(app).get('/');<br/><br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expect(res.body).toEqual({ message: 'Hello Friend!' });<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; expect(res.status).toEqual(200);<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; });<br/>
});
