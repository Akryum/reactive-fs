# reactive-fs

A reactive filesystem interface based on Vue 3 reactivity system.

```js
import { createReactiveFileSystem } from 'reactive-fs'

// Create a reactive filesystem
const fs = await createReactiveFileSystem({
  baseDir: __dirname,
  glob: ['**/*.js'],
  ignored: ['excluded'],
})

// List files
fs.list()
fs.list('.', { excludeSubDirectories: true })
fs.list('sub')

// List is reactive
let results = []
fs.effect(() => {
  // This will update `results` each time a file is added or removed
  results = fs.list('.', { excludeSubDirectories: true }).sort()
})

// Create a file
fs.createFile('foo.js', '')

// Remove a file
fs.files['foo.js'].remove()

// Read a file
const content = await fs.files['meow.js'].waitForContent

// `content` is a reactive property
let result
fs.effect(() => {
  result = fs.files['meow.js'].content
})

// Write to a file
fs.files['meow.js'].content = 'waf'

// Destroy the filesystem
await fs.destroy()
```
