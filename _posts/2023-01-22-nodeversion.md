---
title: Check Required Node Version
date: 2023-01-21 +03:00
categories: [Node Version, Node Sass]
tags: [nodeversion,vuejs,packagejson,nodesass]
---
# Error/node_modules/node-sass: Command failed

### Step 1
- Check your node version based on node-sass version in package.json

NodeJs | Supported node-sass version | Node Module
------------ | ------------- | -------------
Node 16 | 6.0+ | 93
Node 15 | 5.0+ | 88
Node 14 | 4.14+ | 83
Node 13 | 4.13+, <5.0 | 79
Node 12 | 4.12+ | 72
Node 11 | 4.10+, <5.0 | 67
Node 10 | 4.9+, <6.0 | 64
Node 8 | 4.5.3+, <5.0 | 57
Node < 8 | <5.0 | <57

### Step 2
- check your Node version with:
```
 node --version
 ```

### Step 3
- install the Nodejs version according with your node-sass, if your are using NVM, run:
 ```
 nvm use DESIRED_VERSION
 ```

### Step 4
- clean old node_modules deleting the folder or type this on terminal:
```
rm -rf node_modules
```

### Step 5
- run:
```
npm install
```
