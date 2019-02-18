# Production
* [Overview and tools used](#production)
* [Production DNS](#production--dns)
* [Production Platform](#production--platform)
* [Production Multicore](#production--multicore)

## <a name="production">Production overview and tools used</a>

**The support team is normally responsible for the initial launch of websites,** including provisioning of production servers. We always offer managed hosting services to the client.

We use [mechanic](https://npmjs.org/package/mechanic) and [Stagecoach](https://github.com/punkave/stagecoach).

`mechanic` manages nginx, which we use as a reverse proxy, so that Node.js is not responsible for static files, load balancing, HTTPS or starting as root to capture port 80. 

We use `stagecoach` to handle deployments in a way that minimizes downtime, rules out any issues with cached assets and permits rollback.

## <a name="production--dns">Production DNS</a>

We always add a DNS entry that looks exactly like this:

`MYSHORTNAME-prod.punkave.net`

So that the server can be immediately referred to before production DNS changes are made. We continue to use this name when configuring backups, `deployment/settings.production` files, etc. so that we do not confuse caching CDN services like CloudFlare with our actual production server.

We ask our clients to make their official DNS changes for production launches via the control panel of their domain registrar. Consolidating DNS and domain registration with one company reduces the number of moving parts for them.

Our Stagecoach recipes use `forever` to keep Node processes running.

## <a name="production--platform">Production Platform</a>

Our current recommended platform is CentOS 7, with EPEL 7. We use our `new-node-server` script, which is provided with `pktools`. We install Node 8.x (soon to be Node 10.x) from the nodesource repository, and MongoDB 3.6.x from the MongoDB repository. All of this is taken care of for you by `new-node-server`, as well as automatic configuration of nightly updates. Please don't bypass this script.

## <a name="production--multicore">Multicore / multi-process</a>

We always run at least two Node.js server processes per production site to provide levels of reliability akin to that which comes standard with "all in one" solutions like Apache's PHP module. See the [multicore HOW-TO](http://apostrophecms.org/docs/tutorials/howtos/multicore.html).

