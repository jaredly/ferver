
# Fear-Driven Versioning

Fe(a)rVer - a versioning scheme for those of us who only care about breaking changes.
This is similar to [semver](http://semver.org) and uses the same operators like
`~` and `^`, but has different as well as stricter semantics.

Large projects like `jQuery` and `Go` effectively use this type of versioning,
except without the "development channel".
Breaking changes are an inevitable in software development,
and we shouldn't make it any more difficult on ourselves.
Ferver is more practical and is more inline with our common sense.

## Semantics

Like semver, ferver uses the same 3-number versioning scheme:

```
<major>.<minor>.<patch>
```

However, the semantics for each number is different:

| Number        | semver                  | ferver                  |
|---------------|-------------------------|-------------------------|
| Major         | Breaking changes        | Major breaking changes  |
| Minor         | New features            | Minor breaking changes  |
| Patch         | Non-breaking bug fixes  | Non-breaking changes    |

Thus, for ferver, `patch` isn't really a "patch", just a "change",
but we'll continue calling it `patch` because I can't think of a better word.

### Affixes

Ferver does not allow affixes. There's no `1.0.0-beta1` or `2.0.0-1`.
In other words, only "strict" versions are allowed.
Read about the dev channel for prereleases.

### Major breaking changes

Major breaking changes are changes in which fundamentally changes the library.
For example:

- You've broken a "majority" or the "primary" API.
- You're combining many "minor" breaking changes.
- You've removed some features.
- You've decided to change the character, purpose, or philosophy of the library.

Developers __should__ bump the major version on any breaking changes,
but __may__ instead opt for bumping the `minor` version.
Either way, the versioning scheme the developer chooses
__should__ be public.

### Minor breaking changes

Minor breaking changes are minor changes in the library.

- You've changed how one of many APIs works.
- You've updated an external dependency,
   which is not a significant aspect of your public API,
   that introduces some breaking changes.
- You've fixed a bug that people could have been relying on.
- You've refactored a significant portion of your code
  and could have possibly introduced regressions.

Minor versions should be bumped __liberally__.
If there's a `> 0%` change a change will break someone's app,
you __must__ bump the minor version, even if it's a "patch" according to semver.

The idea is to allow developers to use `~x.y.z` ranges without worrying
about their app breaking. With semver, even `patch`s have a possibility
of breaking something.

### Non-breaking changes

Non-breaking changes are changes that have __absolutely no chance of breaking anything__.
When in doubt, bump a minor version.

- Adding a completely new feature.
- Fixing an obvious bug that no body could have relied on.

### 0.y.z

`0.y.z` versions are development versions.
Consumers __should not__ use libraries with only `0.y.z` versions published.
Authors __should__ release a `1.x.y` version as soon as possible.

In the development channel, you __should never bump the major__,
even if you did, in fact, introduce a major breaking change.
Versions `>= 1.0.0` are considered stable releases, not dev releases or prereleases.
Instead, you continuously bump the `minor` version on __any breaking change__.

### >= 1.0.0

Versions `>= 1.0.0` are considered stable, at least within a major version.
