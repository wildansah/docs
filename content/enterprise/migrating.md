<!--
title: Migrating from Old Servers
-->

# migrating from pre-docker npm enterprise installations

1. install npm Enterprise on a new server that you've provisioned,
   using either the [Quick Start](https://docs.npmjs.com/enterprise/intro) guide, or
   one of our [other tutorials](http://blog.npmjs.org/post/142409778875/run-npm-enterprise-on-aws-with-just-a-few-clicks).
2. configure and test the new npm Enterprise instance: setting up the
   [authentication](https://docs.npmjs.com/enterprise/github) that you would like to use,
   configuring the `Full URL of npm Enterprise registry`.
3. stop both the old npm Enterprise server and the new instance.
4. copy both the CouchDB configuration files and the package folders from the
   old Enterprise instance to the new server, moving:
     * `/etc/npme/couchdb/registry.db` to `/usr/local/lib/npme/couchdb`, and
     * `/etc/npme/packages` to `/usr/local/lib/npme/packages`.

_the next steps vary depending on whether or not the new server will have the same DNS._

## if you intend to use the same DNS

1. start the new server.
2. ensure that the `Full URL of npm Enterprise registry` is set to the appropriate
   full-URL of the registry (should match the old server).
3. switch your DNS entries so that they are pointing at the new Enterprise server.
4. smoke test the new server and retire the old server.

## if you would like to use a new DNS name

1. start the new server.
2. you will need to run [this migration tool](https://www.npmjs.com/package/rewrite-url-follower).
  * the command you run will look like this: `rewrite-url-follower rewrite --rewrite=http://my-new-server-host:8080 --host=172.17.0.1 --port=5984`.
    `172.17.0.1` is the address of the docker device, there's a chance that this may
    vary run `ifconfig` to check.
  * this tool may take a while to run, you will know that it's complete when the
    current sequence is equal to the maximum sequence in the logs.
3. smoke test the new server and retire the old server.
