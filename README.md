# Welcome, Athenians!

I'm so glad you're here. Let's talk about [Project Athens](https://github.com/gomods/athens) today!

This repository has demos and code that I used in a talk about Athens at
the September 20, 2018 PDX Go meetup.

# The Demo

There's a lot going on in the background to make this demo simple as pie :pie:!

We're going to build a web application in here, but the actual web app
isn't the exciting part here -- it'll be super simple, actually.

Even the tools that we use to build the application won't be exciting --
we'll be using the standard `go get` toolchain and the
[echo](https://github.com/labstack/echo) web framework.

It's what's going on under the covers that's really, really cool :)

So, let's get started!

# The App

The application code is in this repository, in [main.go](./main.go).
If you look closely enough, you'll notice that I copy-pasted it right from the 
[Echo README](https://github.com/labstack/echo/tree/a2d4cb9c7a629e2ee21861501690741d2374de10#example).

Like I said, nothing remarkable, right?

But, notice what's not in here:

1. There's no `vendor` directory
2. There's no `glide.yaml` or `Gopkg.yaml` files, `Godeps` directory, or 
anything else that our dependency management tools use
3. There's a _new_ file called `go.mod` in it!

# The Dependencies

But our app depends on packages, and we're missing them, our editor even 
told us so with red squiggly lines!

![missing deps](/images/missing-imports.png)

That's right, we still have two dependencies:

- `github.com/labstack/echo`
- `github.com/labstack/echo/middleware`

We'd be crazy to try and build another HTTP router, right?

So, how do we pull them into our app so we can use them?

# Go Modules + Athens to the Rescue!

Let's use the power of [Go Modules](https://cda.ms/FN) and 
[Athens](https://cda.ms/FW) to get these dependencies, but do it _without_

1. Other dependency management tools
2. A `vendor/` directory

It's simple! So let's get to it.

First, let's get the code. Clone this repository to somewhere on your computer,
OUTSIDE your `GOPATH`:

```console
cd my/dir
git clone https://github.com/aarons-demo-code/2018-09-20-PDX-Go.git
cd 2018-09-20-PDX-Go
```

>That's right - we're going to build this app outside the GOPATH. We can do that now!

Next, let's set up our environment to turn on the Go Modules flag. In Go
1.11, the older GOPATH behavior is turned on by default. Let's turn on Go
Modules by setting an environment variable:

```console
export GO111MODULE=on
```

Next, let's point Go modules to use Athens to fetch code from:

```console
export GOPROXY=https://microsoftgoproxy.azurewebsites.net
```

>This `GOPROXY` environment variable is telling Go modules to download all our code from Athens, and _not_ GitHub!

Almost done - let's make sure that our local Go modules cache is empty. This
cache is located in your `GOPATH` under `${GOPATH}/src/mod` and is used
for all Go module based builds on our machine.

Let's delete it to be sure that all our code is coming from Athens:

```console
rm -rf ${GOPATH}/src/mod
```

And finally, let's go do our build:

```
go build
```
