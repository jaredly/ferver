
# Fear-Driven Versioning

Fe(a)rVer - a versioning scheme for those of us who only care about breaking changes.
This is similar to [semver](http://semver.org) and uses the same operators like
`~` and `^`, but has slightly different as well as stricter semantics.

The primary differences are:

- No `patch` version bumps are allowed for any changes that
  have a possibility of being a breaking change.
- "minor" breaking changes are allowed in `minor` version bumps.

Large projects like `jQuery` effectively use this type of versioning
as introducing breaking changes only in `major` versions
would mean very high version numbers like Chrome and Firefox.
Breaking changes are inevitable in software development,
and we shouldn't make it any more difficult on ourselves.
Ferver is more practical and is more inline with our common sense.

The primary purpose of this versioning scheme is to allow
consumers of libraries to use `~x.y.z` versioning
without worrying about breaking changes as well as receiving updates.
Semver's definition of a `patch` is simply too vague and idealistic.

## Semantics

### x.y.z

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

### 0.y.z.

`0.y.z` versions are development versions.
Consumers __should not__ use libraries with only `0.y.z` versions published.
Authors __should__ release a `1.0.0` version as soon as possible to
declare a "stable" library.

The semantics for `0.y.z` versions are very similar to `x.y.z` semantics.
Unlike semver, there are defined semantics for versions `< 1.0.0`.

| Number        | semver                  | ferver                  |
|---------------|-------------------------|-------------------------|
| Major         | 0                       | 0                       |
| Minor         | Undefined               | Breaking Changes        |
| Patch         | Undefined               | Non-breaking changes    |

Thus, bump `minor` only if you introduce a breaking change in development
(no matter how major or minor), and bump `patch` otherwise.

### Affixes

Ferver does not allow affixes. There's no `1.0.0-beta1` or `2.0.0-1`.
In other words, only "strict" versions are allowed.
Development versions should be published in a different channel.

## Consuming ferver packages

Consumers of packages using fear-driven versioning __should not__ use `^x.y.z`
version ranges and instead __should__ use `~x.y.z`, whether or not `x > 0`.

## Types of Changes

### Major breaking changes

Major breaking changes are changes which fundamentally change the library.

- You've broken a "majority" or the "primary" API.
- You're combining many "minor" breaking changes.
- You've completely removed some features.
- You've decided to change the character, purpose, or philosophy of the library.
- You've significantly refactored the library.

Developers __should__ bump the major version on any breaking changes,
but __may__ instead opt for bumping the `minor` version if appropriate.

When you're not sure whether a breaking change is minor or major,
it's a major breaking change.

### Minor breaking changes

Minor breaking changes are relatively minor breaking changes in the library
or changes that __may__ introduce a breaking change.

- You've changed how one of many APIs works.
- You've deprecated any feature.
- You've fixed a bug that people could have been using as a feature.
- You've refactored a non-trivial portion of your code.
- You've updated an external dependency that may introduce some breaking changes.
- You've changed one of your dependencies.

Minor versions should be bumped __liberally__.
If there's a `> 0%` change a change will break someone's app,
you __must__ bump the minor version, even if it's a "patch" according to semver.

### Non-breaking changes

Non-breaking changes are changes that have __absolutely no chance of breaking anything__.
When in doubt, bump a minor version.

- Adding a completely new feature.
- Fixing an obvious bug that no body could have relied on.
- Documentation updates.
- Updating package metadata.

## Semver Compatibility

People may not expect this type of versioning scheme,
so here are a few ways to improve semver compatibility.

### Bump `minor` on new features

With ferver, new features shouldn't bump `minor`, only `patch`.
This is more ideal as people who use `~x.y.z` will then be able to receive the
new feature. But this isn't really important since if the consumer needed
this new feature in the first place, s/he wouldn't have used your library.

However, you're free to bump `minor` when you release a new feature
since versioning isn't just about what has changed, but marketing as well.
As long as you don't introducing any breaking changes on `patch`, you're good.

### Bump `major` instead of `minor` whenever possible

Try to use the semver logic of `major === breaking changes`.
This is impossible with larger projects like `jQuery` because
breaking changes happen all the time to one of their many APIs.
But if you have a very small library with very few APIs,
it's easier to just bump `major` and not confuse any people.
