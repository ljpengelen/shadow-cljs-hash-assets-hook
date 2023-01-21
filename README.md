# shadow-cljs-hash-assets-hook

A [build hook](https://shadow-cljs.github.io/docs/UsersGuide.html#build-hooks) for [shadow-cljs](https://github.com/thheller/shadow-cljs) that
1. adds md5 hashes to the filenames of your static assets and
1. uses these filenames in your `index.html`.

This allows you to permit caching of your static assets, without the risk of users ever seeing outdated versions due to caching.
Do not allow caching of `index.html` itself, however.

## Usage

First, include `[com.github.ljpengelen/shadow-cljs-hash-assets-hook "x.y.z"]` as a dependency in the `shadow-cljs.edn` file of your project.
The latest version is shown in the badge below.

[![Clojars Project](https://img.shields.io/clojars/v/com.github.ljpengelen/shadow-cljs-hash-assets-hook.svg)](https://clojars.org/com.github.ljpengelen/shadow-cljs-hash-assets-hook)

Second, configure the build hook in your `shadow-cljs.edn` as follows:

```
{...
 :builds {:app {:target :browser
                ...
                :build-hooks [(shadow-cljs-hash-assets-hook/hash-assets! {:source-root "public"
                                                                          :target-root "dist"
                                                                          :index "index.html"
                                                                          :files ["css/site.css" "js/app.js"]
                                                                          :release-mode-only? true})]
                ...}}}
```

Then, build your app for production:

    $ npx shadow-cljs release app

For the given configuration, this will calculate the md5 hashes of `public/css/site.css` and `public/js/app.js`, and place copies of these files with the corresponding hashes in their filename into `dist/css/site-<hash>.css` and `dist/js/app-<hash>.js`.
Additionally, it will copy `public/index.html` into `dist/index.html`, with all references to the original files replaced by their renamed counterparts.

Because the configuration option `:release-mode-only?` is set to `true`, the above only happens when creating release builds.
Set its value to `false` if you always want to hash assets.

## License

MIT
