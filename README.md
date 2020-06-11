<!--DO NOT EDIT : this file has been generated-->

# rollup-plugin-inject-process-env

> Inject `process.env` environment variables in a browser rollup bundle.

## Why ?

Because replacing a string typically with `rollup-plugin-replace` works in one case :

```js
    console.log(process.env.NODE_ENV);
```

...but not in all other cases :

```js
    console.log(process.env['NODE_ENV']);
```

```js
    const { NODE_ENV, NODE_PORT } = process.env;
    console.log(NODE_ENV);
```

Worse : sometimes, such substitution :

```js
    if (process.env.NODE_ENV === 'production') {
```

...will be expand to :

```js
    if ('production' === 'production') {
```

...and make some linter complain.

## How ?

### Installation

```bash
npm install --save-dev rollup-plugin-inject-process-env
```

### Usage

Pass any JSON object to the plugin that will be set as the `process.env` value. This object accept members value of any type.

```typescript
    injectProcessEnv(
        env: {},
        options: {
            include?: string | string[],
            exclude?: string | string[],
            verbose?: boolean
        }
    )
```

#### Example :

```js
import injectProcessEnv from 'rollup-plugin-inject-process-env';

// ... usual rollup stuff

    plugins: [
        typescript(),
        injectProcessEnv({ 
            NODE_ENV: 'production',
            SOME_OBJECT: { one: 1, two: [1,2], three: '3' },
            UNUSED: null
         }),
		nodeResolve(),
		commonjs()
	],
```

#### Example with environment variables passed in the CLI :

```js
        injectProcessEnv({ 
            NODE_ENV: process.env.NODE_ENV,
            SOME_OBJECT: JSON.parse(process.env.SOME_OBJECT),
            UNUSED: null
         }),
```

#### Options

* The `verbose` option allows to show which file is included in the process and which one is excluded.
* The `include` and `exclude` options allow to explicitely specify with a [minimatch pattern](https://github.com/isaacs/minimatch) the files to accept or reject. By default, all files are targeted and no files are rejected.

Example :

```js
        injectProcessEnv({
            NODE_ENV: 'production',
            SOME_OBJECT: { one: 1, two: [1,2], three: '3' },
            UNUSED: null
        }, {
            exclude: '**/*.css',
            verbose: true
        }),
        postcss({
            inject: true,
            minimize: true,
            plugins: [],
        }),
```

Output example of the `verbose` option :

```
[rollup-plugin-inject-process-env] Include /path/to/src/index.ts
[rollup-plugin-inject-process-env] Exclude rollup-plugin-inject-process-env
[rollup-plugin-inject-process-env] Exclude /path/to/src/style.3.css
[rollup-plugin-inject-process-env] Include /path/to/node_modules/style-inject/dist/style-inject.es.js
```

#### Icing on the cake

You might notice that as mentionned in the documentation https://nodejs.org/api/process.html#process_process_env
environment variables are always `string`, `number` or `boolean`.

With **rollup-plugin-inject-process-env**, you may inject safely any JSON object to a `process.env` property.

## Reports

> Code quality reports

### Metrics

| Files | 1 | |
| ----- | -: | - |
| Lines of code | 59 | (w/o comments) |
| Comments | 4 | (+ 1 with code) |
| Empty lines | 4 | |
| **Total lines** | **67** | (w/o tests) |
| TODO | 0 | lines |
| Tests | 455 | (w/o comments) |

### Linter

**✅ 0 problems**



### Tests

|   | Tests suites | Tests |
| - | ------------ | ----- |
| ❌ Failed | 0 | 0 |
| ✅ Passed | 4 | 12 |
| ✴ Pending | 0 | 0 |
| ☢ Error | 0 | |
| **Total** | **4** | **12** |




#### ✅ `/test/browser.test.ts` **1.964s** 


| Status | Suite | Test |
| ------ | ----- | ---- |
| ✅ | Browser | get NODE_ENV |
| ✅ | Browser | get SOME_OBJECT |
| ✅ | Browser | get MISSING |



#### ✅ `/test/node.2.test.ts` **0.251s** 


| Status | Suite | Test |
| ------ | ----- | ---- |
| ✅ | Node | get NODE_ENV |
| ✅ | Node | get SOME_OBJECT |
| ✅ | Node | get MISSING |



#### ✅ `/test/node.test.ts` **0.209s** 


| Status | Suite | Test |
| ------ | ----- | ---- |
| ✅ | Node | get NODE_ENV |
| ✅ | Node | get SOME_OBJECT |
| ✅ | Node | get MISSING |



#### ✅ `/test/node.3.test.ts` **0.21s** 


| Status | Suite | Test |
| ------ | ----- | ---- |
| ✅ | Filter out CSS | get NODE_ENV |
| ✅ | Filter out CSS | get SOME_OBJECT |
| ✅ | Filter out CSS | get MISSING |




## License

MIT

## Who ?

* [Inria](http://inria.fr)
* [Inria @ Github](https://github.com/inria)

