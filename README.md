
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

## Development Channel

> The "development channel" is completely optional.
> If you follow the above semantics, you're following ferver.
> However, following the development channel philosophy is recommended.

In ferver, `0.y.z` versions do not jump to `1.0.0`.
Instead, you continuously develop on `0.y.z` indefinitely.
`0.y.z` is the development channel,
and `>= 1.0.0` is the "release" channel.

All versions `x.y.z` where `x > 0` must correlate to a development version `0.y.z`.
For example, `1.15.2` corresponds to the development version `0.15.2`.
Thus, you develop on the `0.y.z` branch and release your development version
as `x.y.z` when it's "stable".

### Publishing vs. Releasing

You "publish" _any_ version. You only "release" a stable version `>= 1.0.0`.

### Publishing History

Thus, a library's publish history would look something like this if they use
a development channel:

- `0.0.0` - initial commit
- `0.0.1` - added a feature
- `0.1.0` - refactored a little bit
- `0.1.1` - added another feature
- `1.1.1` - initial release
- `0.2.0` - refactor again
- `0.2.1` - new feature
- `1.2.1` - new release
- `0.3.0` - change the API a lot
- `2.3.0` - new release

Thus, release versions do not increment 1 at a time.
They'll skip a lot of versions.
This might be weird for some people as they would expect major changes
between large `minor` version differences,
but there's no practical issues with this.

Of course, you don't have to match `0.y.z` with `x.y.z`.
However, it's generally easier than publishing some mapping between dev
and release channels.

### Prereleases

As noted above, consumers __should not__ use `0.y.z` versions.
However, consumers who want to test the dev versions of a library __may__
use the `0.y.z` versions, even if there are versions `>= 1.0.0`.

Any `0.y.z` version that is not published as `x.y.z` is considered a "prerelease"
and is available only to those who wish to use the development version.

### Version Ranges

Consumers __should__ use `~x.y.z` ranges, regardless of `x`.
Consumers __should not__ use `^x.y.z`.

### Changes in 0.y.z semantics with a release channel

When a release channel exists (i.e. a version `>= 1.0.0`),
the development channel __may__ introduce breaking changes without incrementing
the `minor` version and instead increment the `patch` __as long as it is not released__.

For example, suppose `1.4.5` has a bug.
All consequential `~0.4.5` versions could be potential bug fixes that introduce some sort of breaking change.
However, once that bug is fixed and any breaking change was introduced,
you __must__ publish it as `0.5.0` and `1.5.0`.

You do not need to develop `0.y` versions in ascending order.
However, once you decide to release a `0.y` version,
it __must__ be the largest `y` value.

For example, a new `0.5.0` version could correspond to a new "branch" of your library.
But then the "master" branch introduced a minor breaking change,
so you'll skip any `0.5.0` version and publish this change as `0.6.0` and `1.6.0`.
Then, once your `0.5.x` branch is complete, you'll skip `0.5.0`
all together and publish it as `0.7.0` and `1.7.0`.

## CLI

I'm going to eventually create `ferver(1)` to make using
a development channel easier.
Eventually, I hope to incorporate the [changelog](https://github.com/defunctzombie/changelog)
philosophy.

This is why I'm squatting the name on `npm`.
