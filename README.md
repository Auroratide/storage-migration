# Storage Migration

Got a nifty custom domain for your github site? Cool beans! Let's just switch it and...

Not so fast!

If people have data stored locally in the Github domain, then it'll get lost when redirected to the new domain. We have to somehow migrate data **across domains** so people do not lose their stuff.

## Script on New Site

This script moves data into local storage.

```javascript
const target = "poke5e"
const hasMigrated = localStorage.getItem("migrated") != null
if (!hasMigrated) {
   const iframe = document.createElement("iframe")

   window.addEventListener("message", (event) => {
      if (event.origin = "https://auroratide.github.io") {
         console.log("Migrating data to new domain...")

         if (Array.isArray(event.data)) {
            event.data.forEach(([key, value]) => {
               localStorage.setItem(key, value)
            })

            localStorage.setItem("migrated", "true")
            console.log("...Done migrating!")
         }
      }

      iframe.remove()
   }, { once: true })

   iframe.style.position = "absolute"
   iframe.style.width = "1px"
   iframe.style.height = "1px"
   iframe.style.clipPath = "inset(50%)"
   iframe.style.overflow = "hidden"
   iframe.style.left = "-9999px"
   iframe.src = `https://auroratide.github.io/storage-migration/${target}`

   document.body.appendChild(iframe)
}
```

## Test Locally First

Make sure your migration strategy works locally first. We can use [serve](https://www.npmjs.com/package/serve) for this:

```
npm i -g serve

serve -l 3001 .
```
