# Smelte and sapper

## Steps to reproduce the error

```
npx degit "sveltejs/sapper-template#rollup" my-app
cd my-app
npm install smelte
```

Add in rollup.config.js (client section):

```
smelte({
	purge: production,
	output: "public/global.css", // it defaults to static/global.css which is probably what you expect in Sapper
	postcss: [], // Your PostCSS plugins
	whitelist: [], // Array of classnames whitelisted from purging
	whitelistPatterns: [], // Same as above, but list of regexes
	tailwind: {
		theme: {
			extend: {
				spacing: {
					72: "18rem",
					84: "21rem",
					96: "24rem"
				}
			}
		}, // Extend Tailwind theme
		colors: {
			primary: "#b027b0",
			secondary: "#009688",
			error: "#f44336",
			success: "#4caf50",
			alert: "#ff9800",
			blue: "#2196f3",
			dark: "#212121"
		}, // Object of colors to generate a palette from, and then all the utility classes
		darkMode: true,
	}, // Any other props will be applied on top of default Smelte tailwind.config.js
}),
```

Change the public/global.css to static/global.css

Add in src/client.js
```
import "smelte/src/tailwind.css";
```

Add in src/template.html
```
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
```

Add in src/routes/index.svelte:
```
<script>
  import { Button } from "smelte";
</script>

<h6 class="mb-3 mt-6">Basic</h6>
<div class="py-2">
  <Button>Button</Button>
</div>
```

npm run dev

```
(node:26992) UnhandledPromiseRejectionWarning: ReferenceError: smelte is not defined
    at Object.<anonymous> (/tmp/test/my-app/rollup.config.js:68:28)
    at Module._compile (internal/modules/cjs/loader.js:868:30)
    at Object.require.extensions..js (/tmp/test/my-app/node_modules/sapper/dist/core.js:881:12)
    at Module.load (internal/modules/cjs/loader.js:731:32)
    at Function.Module._load (internal/modules/cjs/loader.js:644:12)
    at Module.require (internal/modules/cjs/loader.js:771:19)
    at require (internal/modules/cjs/helpers.js:68:18)
    at Function.load_config (/tmp/test/my-app/node_modules/sapper/dist/core.js:887:18)
    at async Object.create_compilers (/tmp/test/my-app/node_modules/sapper/dist/core.js:1118:18)
    at async Watcher.init (/tmp/test/my-app/node_modules/sapper/dist/dev.js:215:21)

```
