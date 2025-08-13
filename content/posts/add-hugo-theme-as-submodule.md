---
title: "Add a Theme as a Hugo Module"
date: 2023-03-30T17:21:35+01:00
categories:
  - Hugo
draft: false
authorbox: true
---

Although Hugo modules are supposed to make life simple, this took me an embarrassingly long time to fathom. 

In this tutorial, I'll show you how to add a theme as a module of your Hugo site.

I'm assuming you already have a Hugo site and have chosen a theme that's available as a module.

If you want to learn more about modules, [take a look at the Hugo documentation](https://gohugo.io/hugo-modules/use-modules/).

## Step 1 - Ensure you have installed Go

To use Hugo modules, you'll need Go. Run `go version` to check whether it's already installed. 

:down_arrow: If you don't have Go, you can [download a version](https://go.dev/doc/install) for your operating system.

## Step 2 - Initialize your site as a module

Before you can use modules, your website itself needs to be a module.

Make sure you're at the top level of your website directory. Then run:

```shell
hugo mod init website-name
```

Replace `website-name` with any unique name.

You'll see a message to say that Go is creating your new module.

:eyes: To check what's happening, run `cat go.mod`. You'll see the module you just created.

## Step 3 - Add theme module as a dependency

Make sure you have the full repo link for your theme. Most of them are on GitHub. Then run:

```shell
hugo mod get url-of-theme
```

Replace `url-of-theme` with the GitHub URL (without the https). For example, the command for the Ananke theme would be:

```shell
hugo mod get github.com/theNewDynamic/gohugo-theme-ananke
```

Note this time that the command is `hugo mod get`, rather than `hugo mod init`. That's what tripped me up.

Hugo tells you the theme has been added. 

:eyes: Run `cat go.mod` again for confirmation.

## Step 4 - Edit your config file

Open your config file and add the repo URL as the value for `theme`. In YAML, it would look like this:

```yaml
theme: github.com/theNewDynamic/gohugo-theme-ananke
```

In TOML, it's:

```toml
theme = ["github.com/theNewDynamic/gohugo-theme-ananke"]
```

You need the full URL (without https), rather than just the name.

Save the file and run `hugo serve`. You should see your new theme :tada:


## Conclusion

It's certainly easy to add submodules, but only if you know exactly the right commands. I hope it worked for you, and in a fraction of the time it took me :tired_face:

You can also watch a video of this tutorial:

{{< youtube PLaI3ZJUVTU >}}
