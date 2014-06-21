
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
