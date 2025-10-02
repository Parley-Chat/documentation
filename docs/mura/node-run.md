---
title: Node Run
parent: Mura (Frontend)
nav_order: 1
---

# Node.js Run

How to run Mura frontend locally using node.js

## Node.js Installation

If alredy installed this section can be ignored, elsewise follow the instructions in this section.

1. Go to [https://nodejs.org/en/download](https://nodejs.org/en/download)
2. Download and setup node.js

{: .warning }
In some systems, specially Linux, using a terminal based installation is prefered

{: .note }
If npm is on the path but not node or node is several versions behind: install n globally `npm install -g n` and upgrade to node latest `n latest`

## Cloning the repository

Go to a folder where you want the frontend folder to be and run:
```bash
git clone https://github.com/parley-chat/mura.git
```

## Running locally

Go to the frontend repo and run
```bash
npm i express open
node quickrun.js
```

This will automatically open the browser on localhost with the frontend running.