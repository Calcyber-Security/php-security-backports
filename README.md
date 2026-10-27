# PHP Security Backports

Source backports for **CVE-2026-109618** and **CVE-2026-109619**, published by [CalCyber](https://calcyber.net) for review and version-specific evaluation of legacy PHP installations.

**Prefer an upgrade to a supported PHP release or a maintained vendor package.** These backports do not restore upstream support to an end-of-life PHP version and do not address every vulnerability in that version. Consult [PHP's current support policy](https://www.php.net/supported-versions.php).

Read the [CalCyber PHP2Shell explainer](https://calcyber.net/php2shell.html) for background and deployment considerations.

## What is here

This repository preserves the branch and tag history imported from [zach115th/php-src](https://github.com/zach115th/php-src). At the 27 October 2026 import, it contained **1,050 source branches and 1,535 tags**, including **425 branches with CVE-specific names**. Those names indicate intended patch coverage, not a tested compatibility or support guarantee.

The `main` branch is this landing page. It is **not a patched PHP source tree**. Historical branches, upstream development branches and tags are also present; selecting generic `master` or an ordinary version tag does not select a security backport.

This project distributes source history. The import did not create release packages or binaries, and no blanket build, regression-test or production-readiness claim is made for the imported branches.

## Choose an exact version

Backport branches use `PHP-<version>-CVE-2026-109618-109619`. Verified examples include:

| PHP version | Backport branch |
| --- | --- |
| 5.0.0 | [PHP-5.0.0-CVE-2026-109618-109619](https://github.com/Calcyber-Security/php-security-backports/tree/PHP-5.0.0-CVE-2026-109618-109619) |
| 5.2.17 | [PHP-5.2.17-CVE-2026-109618-109619](https://github.com/Calcyber-Security/php-security-backports/tree/PHP-5.2.17-CVE-2026-109618-109619) |
| 7.4.33 | [PHP-7.4.33-CVE-2026-109618-109619](https://github.com/Calcyber-Security/php-security-backports/tree/PHP-7.4.33-CVE-2026-109618-109619) |
| 8.0.30 | [PHP-8.0.30-CVE-2026-109618-109619](https://github.com/Calcyber-Security/php-security-backports/tree/PHP-8.0.30-CVE-2026-109618-109619) |
| 8.1.33 | [PHP-8.1.33-CVE-2026-109618-109619](https://github.com/Calcyber-Security/php-security-backports/tree/PHP-8.1.33-CVE-2026-109618-109619) |

Match the exact runtime version, vendor modifications, extensions and SAPI before evaluating a branch. For example, to obtain the PHP 7.4.33 backport source without building or installing it:

```sh
git clone --single-branch --branch PHP-7.4.33-CVE-2026-109618-109619 https://github.com/Calcyber-Security/php-security-backports.git php-backport
cd php-backport
git rev-parse HEAD
```

Record and review the full commit ID, then pin the reviewed revision with `git checkout --detach <reviewed-full-commit-id>`. Branch tips can change. Inspect that branch's own installation documentation and test harness; legacy PHP versions require different toolchains and dependencies, so there is no universal build command here.

Validate in an isolated staging environment with the actual application, extensions, request handling and process manager. Review error paths and worker restarts, compare regression results with the unpatched baseline, and prepare a tested rollback before deployment. A successful checkout is not evidence of a successful or safe build.

## Mitigation behavior and limitations

The upstream mitigation addresses state corruption around bailout and request shutdown. It can intentionally terminate a worker rather than permit execution with unsafe state. In the PHP 7.4 example, session upload-progress handling is disabled by the patch. These changes can affect application behavior and worker lifecycle; review the exact branch diff and test operational impact. Older version-specific adaptations can differ from the 7.4 implementation.

Official advisories:

- [CVE-2026-109618 / GHSA-w8hw-w9fx-j79w: RCE via OOM corruption](https://github.com/php/php-src/security/advisories/GHSA-w8hw-w9fx-j79w)
- [CVE-2026-109619 / GHSA-6wmf-4rv5-p3rp: RCE via stale global state](https://github.com/php/php-src/security/advisories/GHSA-6wmf-4rv5-p3rp)

Use the advisories and current PHP guidance to assess affected versions and supported upgrade options. A mitigation for these two issues is not comprehensive hardening for an obsolete runtime.

## Provenance and credits

The minimal EOL mitigation originates with PHP contributors **Arnaud Le Blanc and Ilija Tovilo**. Upstream PHP 7.4, 8.0 and 8.1 series mitigation work is distinct from the older version-specific adaptations carried in the imported source repository. CalCyber's publication preserves that provenance rather than claiming authorship of those patches.

Useful preserved history includes:

- [16c7f82e9254eb50bf8fbc5412af5a41856db99b](https://github.com/Calcyber-Security/php-security-backports/commit/16c7f82e9254eb50bf8fbc5412af5a41856db99b): minimal EOL mitigation credited to Arnaud Le Blanc and Ilija Tovilo, cherry-picked from upstream commit `712e60f9edfd03f0960fc4fc3c2c375f42b90aa2`.
- [d779943aa1bbc32f53662aeb3415c54e20fbb95d](https://github.com/Calcyber-Security/php-security-backports/commit/d779943aa1bbc32f53662aeb3415c54e20fbb95d): PHP 5.0.0 adaptation, with version-specific differences and contributor credits recorded in its commit message.
- [b99a3c1391a4763ed5b4b89b08940fe1609e1341](https://github.com/Calcyber-Security/php-security-backports/commit/b99a3c1391a4763ed5b4b89b08940fe1609e1341): follow-up review fixes for an older backport. Its historical test report applies to the described revision and environment, not all 425 CVE-named branches.

Consult individual commits for the complete attribution, including AI-assisted adaptation credits recorded by the source project. Existing source-file notices and each branch's license files remain authoritative; this landing page does not replace them or introduce a new license for PHP or bundled components.
