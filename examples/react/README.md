# typesafe-i18n React

This is a small project demonstrating a `typesafe-i18n` integration with React.

> This repository was set up using [Create React App](https://github.com/facebook/create-react-app) with the typescript option.


## Get started

Start the project in development mode:

```bash
npm run dev
```

Navigate to [http://localhost:4000](http://localhost:4000). You should see the example app running.

# Overview
 - [add `typesafe-i18n` to existing projects](#configure-typesafe-i18n-for-an-existing-react-project)
 - [generated component & context](#generated-component--context)

<!-- ------------------------------------------------------------------------------------------ -->
<!-- ------------------------------------------------------------------------------------------ -->
<!-- ------------------------------------------------------------------------------------------ -->

# Configure `typesafe-i18n` for an existing React project

Configure `typesafe-i18n` by creating the file `.typesafe-i18n.json` with following contents:

```json
{
   "adapter": "react"
}
```

Run the watcher e.g. by adding a new script inside your `package.json` file.
You could configure your development script to run the watcher in parallel to `react-scripts start`.

```json
// ... other options

"scripts": {
   "dev": "npm-run-all --parallel start typesafe-i18n-watcher",
   "typesafe-i18n-watcher": "node ./node_modules/typesafe-i18n/node/watcher.js",
   "start": "react-scripts start"
}
```

The watcher will generate a custom React component and context inside `i18n-react.ts` that you can use inside your application.

Wrap your application root with the `TypesafeI18n` component:

```typescript
import React from 'react'
import TypesafeI18n from './i18n/i18n-react'

function App() {

   return (
      <TypesafeI18n initialLocale="en">

         <!-- your app goes here -->

		</TypesafeI18n>
   )
}

export default App
```

That's it. You can then start using `typesafe-i18n` inside your React components.

```typescript
import React from 'react'
import { I18nContext } from './i18n/i18n-react'

function Greeting(props) {
   const { LL } = useContext(I18nContext)

   return <h1>{LL.HI({ name: props.name })}</h1>
}

export default Greeting
```

<!-- ------------------------------------------------------------------------------------------ -->
<!-- ------------------------------------------------------------------------------------------ -->
<!-- ------------------------------------------------------------------------------------------ -->

## generated component & context


### TypesafeI18n

When running the [watcher](https://github.com/ivanhofer/typesafe-i18n#typesafety), the file `i18n-react.tsx` will export a React component you can wrap around your application. It accepts the (optional) prop `initialLocale` where you can pass a locale to initialize the context.

```typescript
import React from 'react'
import TypesafeI18n from './i18n/i18n-react'

function App() {

   return (
      <TypesafeI18n initialLocale="en">

         <!-- your app goes here -->

		</TypesafeI18n>
   )
}

export default App
```


### I18nContext

Also a React context is exported by the generated file `i18n-react.tsx`. You can use it with the `useContext` hook.

```typescript
import React from 'react'
import { I18nContext } from './i18n/i18n-react'

function MyComponent(props) {
   const { LL, isLoadingLocale, locale, setLocale } = useContext(I18nContext)


   return // ...
}

export default MyComponent
```

The context gives you access to following variables:

#### LL

An initialized [`i18nObject`](https://github.com/ivanhofer/typesafe-i18n#i18nobject) you can use to translate your app.

```typescript
import React from 'react'
import { I18nContext } from './i18n/i18n-react'

function ProjectOverview(props) {
   const { LL } = useContext(I18nContext)

   return LL.NR_OF_PROJECTS(5) // will output e.g => '5 Projects'
}

export default ProjectOverview
```

#### locale

A `string` containing the current selected locale.

#### isLoadingLocale

A `boolean` that indicates if the locale is currently loading. Can be useful if you have set `loadLocalesAsync` and a network request is beeing performed when switching locales.

#### setLocale

A function to set another locale for the context.


```typescript
import React from 'react'
import { I18nContext } from './i18n/i18n-react'

function LanguageSelection(props) {
   const { locale, isLoadingLocale, setLocale } = useContext(I18nContext)

   if (isLoadingLocale) {
      return <div>loading...</div>
   }

   return (
      <ul className="language-selection">
         <li className={'en' === locale ? 'selected' : ''} onClick={() => setLocale('en')}>
            english
         </li>
         <li className={'de' === locale ? 'selected' : ''} onClick={() => setLocale('de')}>
            deutsch
         </li>
         <li className={'it' === locale ? 'selected' : ''} onClick={() => setLocale('it')}>
            italiano
         </li>
      </ul>
   )
}

export default LanguageSelection
```