---
layout: post
title: "[TIL] Using pm2 cluster"
date: "2020-10-02"
categories:
  - TIL
excerpt: |
  How to implement pm2 cluster
feature_text: |
  ## Scale-out your app with multi-core CPU by implementing pm2 cluster
  How to implement cluster in pm2
feature_image: "https://images.unsplash.com/photo-1579620586506-fb462d7443fa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2294&q=800"
image: "https://images.unsplash.com/photo-1579620586506-fb462d7443fa?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2294&q=80"
---

Most modern server instances are composed of multi-core CPU.
Since, Node.js runs application based on single-thread, Node.js provides API called **[Cluster](https://nodejs.org/api/cluster.html)**.

Simmilar, pm2, a process manager for Node.js, offers **[Cluster Mode](https://pm2.keymetrics.io/docs/usage/cluster-mode/)**.
By running Node.js application with `cluster mode`, pm2 generates multiple processes based on configurations.

## Basics
`cluster mode` uses Node.js [cluster module](https://nodejs.org/api/cluster.html).

According to the pm2 documentation:
> This greatly increases the performance and reliability of your applications, depending on the number of CPUs available. 

The configuration could be made either manually with simple command, or piece by piece with `yaml` or `config.js` file.

## Implementation
### Simple Configuration
Running app with max number of processes, just command:
```
pm2 start app.js -i max
```

pm2 will automatically detect the number of available CPUs and generates processes accordingly.

### Using Ecosystem File
By using `ecosystem` command, pm2 offers you to create ecosystem file(ecosystem.config.js) easily, which helps you to configure in code-basis.

Just run:
```
pm2 ecosystem
```

Then pm2 automatically generates `ecosystem.config.js` in the working directory.

Then, write configuration.

My app is based on Next.js, so my app should be run by command `npm start`. I added scripts and args to start my app.
```javascript
module.exports = {
  apps: [{
    script: 'npm',
    args: 'start',
    instances: "max",
    exec_mode: "cluster"
  }],
};
```

If you are using `ecosystem.config.js` to enable cluster mode, be sure to include `exec_mode: "cluster"`.

According to the [documentation](https://pm2.keymetrics.io/docs/usage/cluster-mode/),
> 0/max to spread the app across all CPUs
> -1 to spread the app across all CPUs - 1
> number to spread the app across number CPUs


## FYI
`reload` vs. `restart`:
`reload` achieves 0-second-downtime reload whereas `restart` kills and restarts app which eventually creates downtime.

## Possible Bug
![cpu-usage-0](https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/pm2-1.png?raw=true)

Generally `pm2 status` command offers CPU usage, yet in cluster mode, it returns '0%'.
I made some search and found out that CPU usage computed by pm2 is not that accurate, since it only take first process in the computation([Ref: pm2 ls memory data is not accurate #3319](https://github.com/Unitech/pm2/issues/3319)).

![log-not-available](https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/pm2-2.png?raw=true)
Also, in cluster mode, `pm2 monit` does not generates npm logs.

