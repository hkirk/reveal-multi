# reveal-multi

Generate static site with index page from reveal.js slides from multiple directories.

* Multiple slide projects
* Autogenerated index.html with all projects
* Markdown by default
* Dev static server with livereload
* Multiplex works out of box, just run reveal-multi --server in you slides project, open index page and copy two links: master and client.

## Usage

### Install

```
npm i -g reveal-multi
```

### Create you projects

```
/my-slides
  /docs
  /src
    reveal-multi.json
    /project1
      /i
        pic.png
      index.md
    /project2
      /some
        some-file.png
      index.md
```

For reveal-multi.json syntax see [IConfig](./src/interfaces.js) and [default config](./src/defaultConfig.js). [Example project root](./examples)

### Build static site in docs

```
cd my-slides
reveal-multi
```

Commit and push generated my-slides/docs to github pages.

or

### Build and run static server

```
reveal-multi --server
```

Goto http://localhost:8080

Open master link for master presentation and client for synced client presentation.

## Options

reveal-multi options:

```
  -h, --help     This help
  -e  --examples Run dev server with examples
  -s, --server   Run dev server
  -v, --verbose  Verbose output
  -i, --in       Root directory with projects              [default: "src"]
  -o, --out      Destination directory                    [default: "docs"]

Ex:
  reveal-multi --server --in=./src --out=./docs
```

## As library

```js
import {createServer, buildStatic} from 'reveal-multi'
import type {IRunOptions, IBuildOptions, IBuildInfo} from 'reveal-multi'

function revealGenCli(options: IRunOptions): Promise<void> {
    const staticPromise = buildStatic(options)
    let result: Promise<void>
    if (options.runServer) {
        result = staticPromise
            .then(createServer)
            .then((server: Server) => {
                console.log(`open http://localhost:${server.address().port}/`)
            })
    } else {
        result = staticPromise
            .then((info: IBuildInfo) => {
                console.log(`${info.dirs.join(', ')} builded in ${info.options.destDir}`)
            })
    }

    return result
        .catch((e: Error) => {
            console.error(e)
        })
}
```
