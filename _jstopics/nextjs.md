---
topic: Next.js
desc: "A framework based on node and React"
category_prefix: "nextjs: "
---

TODO: Fill this in

# Troubleshooting strategies

Sometimes, you may need to "regenerate" the files that are automatically created by the next.js ecosystem.

There isn't a universal `npm clean` built into the `npm` ecosystem (similar to a `make clean`).

But, if you need to do something like a `make clean` or a `mvn clean`, the equivalent would be something like this:

```
rm -rf .next node_modules
```

After doing this, you'll need to do the following to regenerate these files:

```
npm install
npm run dev
```
