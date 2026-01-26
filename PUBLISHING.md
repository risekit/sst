# Publishing @risekit/sst

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
# 1. Tag (use SST's current version)
git tag v3.17.37
git push origin v3.17.37

# 2. Buildnpm login --scope=@risekit"
rm -rf platform/dist/sst_* dist/ sdk/js/dist/ sdk/js/tmp/
cd platform && bun run build && cd ..
goreleaser build --clean --skip validate
cd sdk/js && bun run build

# 3. Publish
bun run release
```

## Custom Files

- `sdk/js/package.json` - name: `@risekit/sst`
- `sdk/js/bin/sst.mjs` - reads package name dynamically  
- `platform/src/components/aws/router.ts` - your router changes
