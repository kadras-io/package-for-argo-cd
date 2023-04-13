# Verifying the Package Release

This package is published as an OCI artifact, signed with Sigstore [Cosign](https://docs.sigstore.dev/cosign/overview), and associated with a [SLSA Provenance](https://slsa.dev/provenance) attestation.

Using `cosign`, you can display the supply chain security related artifacts for the `ghcr.io/kadras-io/package-for-argo-cd` images. Use the specific digest you'd like to verify.

```shell
cosign tree ghcr.io/kadras-io/package-for-argo-cd
```

The result:

```shell
📦 Supply Chain Security Related artifacts for an image: ghcr.io/kadras-io/package-for-argo-cd
└── 🔐 Signatures for an image tag: ghcr.io/kadras-io/package-for-argo-cd:sha256-6d6ff476644c0a40323c1a8b73cbe77a1523d00bfb2eaf02c890bee69de9011f.sig
   └── 🍒 sha256:7e1f50e35b01d27442265e0ed6fdc124c0dfcedfde03236fe5b6d20494da2a21
└── 💾 Attestations for an image tag: ghcr.io/kadras-io/package-for-argo-cd:sha256-6d6ff476644c0a40323c1a8b73cbe77a1523d00bfb2eaf02c890bee69de9011f.att
   └── 🍒 sha256:529d43895a07daf2c9b49af3e5f4557735b1e25a62e5cba3d6c5621fda1c171c
```

You can verify the signature and its claims:

```shell
cosign verify \
   --certificate-identity-regexp https://github.com/kadras-io \
   --certificate-oidc-issuer https://token.actions.githubusercontent.com \
   ghcr.io/kadras-io/package-for-argo-cd | jq
```

You can also verify the SLSA Provenance attestation associated with the image.

```shell
cosign verify-attestation --type slsaprovenance \
   --certificate-identity-regexp https://github.com/slsa-framework \
   --certificate-oidc-issuer https://token.actions.githubusercontent.com \
   ghcr.io/kadras-io/package-for-argo-cd | jq .payload -r | base64 --decode | jq
```
