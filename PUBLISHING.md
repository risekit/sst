# Publishing @risekit.io/sst

## Setup (one-time)

```bash
# Add npm token to ~/.npmrc
echo "//registry.npmjs.org/:_authToken=YOUR_TOKEN" >> ~/.npmrc

# Add SST remote (if not exists)
git remote add sst https://github.com/sst/sst.git
```

## Update from SST

```bash
git fetch sst
git merge sst/dev
# If conflicts in router.ts: KEEP YOUR CHANGES
git add platform/src/components/aws/router.ts
git commit
```

## Publish

```bash
# 1. Tag (use SST's current version, e.g. v3.17.38)
git tag v3.17.38
git push origin v3.17.38

# 2. Build
rm -rf platform/dist/ dist/ sdk/js/dist/ sdk/js/tmp/
cd platform && bun run build && cd ..
goreleaser build --clean --skip validate
cd sdk/js && bun run build

# 3. Publish (run from sdk/js)
bun run release
```

## Custom Files

- `sdk/js/package.json` - name: `@risekit.io/sst`
- `sdk/js/bin/sst.mjs` - reads package name dynamically
- `platform/src/components/aws/router.ts` - your router changes
- `platform/scripts/build` - Docker/bridge-task step commented out (not needed for npm publish; uncomment if you need to build that image)
