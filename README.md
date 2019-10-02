# dev-nginx-demo

A quick demonstration of [dev-nginx](https://github.com/guardian/dev-nginx).

There is a companion presentation available [here](https://docs.google.com/presentation/d/1pqqucMJaTGfBi5dVQ82psEg2RtMWmj-7oF7UrSKmIzc/edit?usp=sharing).

These instructions assume you have [dev-nginx](https://github.com/guardian/dev-nginx) installed.

In this repository, there are two simple webpages, served by two different servers on two different ports.

Server1 uses Python's `SimpleHTTPServer`. Server2 uses the npm module `http-server`.

We can see these running on `localhost` when we:
- Start server1 with `./script/start-server1`
- Open browser at `http://localhost:1234`
- Start server2 with `./script/start-server2`
- Open browser at `http://localhost:9001`


## Instructions
We want to access these over https on subdomains of `my.proxy.server`, that is `https://server1.my.proxy.server` and `https://server2.my.proxy.server`.

🔍 Hmm, it looks like `my.proxy.server` isn't a registered domain (`dig my.proxy.server`). We don't want to buy it, so let's update our `hosts` file to pretend.

```sh
dev-nginx add-to-hosts-file server1.my.proxy.server
dev-nginx add-to-hosts-file server2.my.proxy.server
```

Our `hosts` file now includes:
```console
127.0.0.1		server1.my.proxy.server
127.0.0.1		server2.my.proxy.server
```

🔐 In order to serve our sites over https, we need some TLS certificates. We will use `dev-nginx` to generate them. We could do this manually:

```sh
dev-nginx setup-cert server1.my.proxy.server
dev-nginx setup-cert server2.my.proxy.server
```

However, we also need some nginx configuration, so we'll use the `dev-nginx setup-app` command instead, allowing us to [define our servers in a yaml file](./dev-nginx.yaml).

```sh
dev-nginx setup-app dev-nginx.yaml
```

✅ We can now browse to our wonderful sites with https!
- Open browser at `https://server1.my.proxy.server`
- Open browser at `https://server2.my.proxy.server`


## No Look Instructions
The `setup` script can do all the hard work for you:
- Run `./script/setup`
- Run `./script/start-server1`
- Open browser at `https://server1.my.proxy.server`
- Run `./script/start-server2`
- Open browser at `https://server2.my.proxy.server`