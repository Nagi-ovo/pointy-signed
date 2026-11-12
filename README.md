# pointy-signed

Signs and notarizes the macOS app in [Pointy](https://github.com/pointylab/pointy)'s plugin package, with the upstream maintainer's agreement.

You don't install from here. Install Pointy in DSH as usual: upstream publishes the signed package to npm as `@pointylab/dsh`.

## How a release happens

1. Upstream tags a release in `pointylab/pointy`.
2. Every 15 minutes, [the workflow](.github/workflows/release.yml) here checks for an upstream release it has not signed yet. When it finds one, it pins the tag's commit and runs upstream's own `tools/pack-plugin.ts` on it. Pointy.app is signed with a Developer ID and notarized by Apple; the signature, notarization ticket and version are checked before anything is published.
3. The package is published as a GitHub release here, with a build-provenance attestation.
4. Upstream picks the release up, verifies the attestation, and publishes the package to npm.

Nothing in Pointy's code is changed. The Windows executables in the package are built the same way as upstream's.

To sign a particular tag, run the workflow by hand and enter the tag.

## Verify a build

```sh
gh attestation verify pointylab-dsh-<version>.tgz --repo Nagi-ovo/pointy-signed
```

This ties the package to the workflow run that built it; the release notes name the upstream commit. After installing, `spctl --assess --type execute -vv <plugin>/pet/Pointy.app` should report `source=Notarized Developer ID`.

## License

MIT. Pointy itself is MIT-licensed as well; see [upstream](https://github.com/pointylab/pointy/blob/main/LICENSE).
