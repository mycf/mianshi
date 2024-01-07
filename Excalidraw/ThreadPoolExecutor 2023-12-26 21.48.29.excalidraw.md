---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements

    private boolean addWorker(Runnable firstTask, boolean core) {
        retry:
        for (int c = ctl.get();;) {
            // Check if queue empty only if necessary.
            if (runStateAtLeast(c, SHUTDOWN)
                && (runStateAtLeast(c, STOP)
                    || firstTask != null
                    || workQueue.isEmpty()))
                return false;

            for (;;) {
                if (workerCountOf(c)
                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                    return false;
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                c = ctl.get();  // Re-read ctl
                if (runStateAtLeast(c, SHUTDOWN))
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            w = new Worker(firstTask);
            final Thread t = w.thread;
            if (t != null) {
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int c = ctl.get();

                    if (isRunning(c) ||
                        (runStateLessThan(c, STOP) && firstTask == null)) {
                        if (t.getState() != Thread.State.NEW)
                            throw new IllegalThreadStateException();
                        workers.add(w);
                        workerAdded = true;
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }

   

  
 ^NPcqCzcP

满足return false条件的情况有：
1、stop、tidying、terminated状态
2、shutdown状态+firstTask不为null
3、shutdown状态+工作队列为空
 ^3Yhb8X9L

防止数值超出COUNT_MASK ^QDnp1mor

尝试自增 ^BnthpH9M

cas失败，重新循环 ^M4Poncut

状态变化 ^rMWwuyIW

线程是否启动成功 ^NyTyJsmO

线程是否添加成功 ^rxsDpxRS

添加到工作线程集合 ^MSNhqP1k

线程启动 ^P0mIVURa

线程启动失败 ^mGrdNTJH

    public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.
         */
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }
 ^uEgW2oxe

线程数小于核心线程数 ^agqkj0jG

队列未满 ^ItWAXEVb

队列已满，尝试使用非核心线程 ^rILL5G1r

如果核心线程数为0，直接开启一个工作线程 ^tk7Hguty

运行状态，尝试放入队列 ^MOo5wHHN

放入队列后，重新检查运行状态，否则移除并拒绝 ^Y0I9J28I

因为ctl前三位是状态，所以COUNT_MAK是线程最大值
 ^OxVDl7XY

占位成功 ^bwdvew9g

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"type": "text",
			"version": 924,
			"versionNonce": 577579631,
			"isDeleted": false,
			"id": "NPcqCzcP",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -203.31660082225875,
			"y": 9131.65791908211,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 731.25,
			"height": 1305.6,
			"seed": 1591035480,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189744,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n   \n\n  \n",
			"rawText": "\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n   \n\n  \n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n   \n\n  \n",
			"lineHeight": 1.2,
			"baseline": 1301
		},
		{
			"type": "text",
			"version": 1195,
			"versionNonce": 632122561,
			"isDeleted": false,
			"id": "3Yhb8X9L",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 214.93920405170184,
			"y": 9226.209154990676,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 295.625,
			"height": 96,
			"seed": 1380964225,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "PVllQbFBx98HcNm1HMl11",
					"type": "arrow"
				}
			],
			"updated": 1704599195388,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "满足return false条件的情况有：\n1、stop、tidying、terminated状态\n2、shutdown状态+firstTask不为null\n3、shutdown状态+工作队列为空\n",
			"rawText": "满足return false条件的情况有：\n1、stop、tidying、terminated状态\n2、shutdown状态+firstTask不为null\n3、shutdown状态+工作队列为空\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "满足return false条件的情况有：\n1、stop、tidying、terminated状态\n2、shutdown状态+firstTask不为null\n3、shutdown状态+工作队列为空\n",
			"lineHeight": 1.2,
			"baseline": 92
		},
		{
			"type": "text",
			"version": 835,
			"versionNonce": 660341903,
			"isDeleted": false,
			"id": "QDnp1mor",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 535.223507723577,
			"y": 9378.622441621497,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 189.75,
			"height": 19.2,
			"seed": 1145782113,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "防止数值超出COUNT_MASK",
			"rawText": "防止数值超出COUNT_MASK",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "防止数值超出COUNT_MASK",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 739,
			"versionNonce": 264212449,
			"isDeleted": false,
			"id": "BnthpH9M",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 311.55398298604905,
			"y": 9418.700793642376,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 64,
			"height": 19.2,
			"seed": 1447996225,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "尝试自增",
			"rawText": "尝试自增",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "尝试自增",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 759,
			"versionNonce": 2041223855,
			"isDeleted": false,
			"id": "M4Poncut",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 540.4600787974024,
			"y": 9513.302054311236,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 140.125,
			"height": 19.2,
			"seed": 1354341153,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "cas失败，重新循环",
			"rawText": "cas失败，重新循环",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "cas失败，重新循环",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 864,
			"versionNonce": 1071967169,
			"isDeleted": false,
			"id": "CT31Rn4KSTnW4XvIcaPMm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 500.5228490850542,
			"y": 9517.212732067754,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 368.23494480852673,
			"height": 141.92377753466644,
			"seed": 1140559617,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-99.66661811738527,
					-93.24361376656816
				],
				[
					-368.23494480852673,
					-141.92377753466644
				]
			]
		},
		{
			"type": "text",
			"version": 783,
			"versionNonce": 1308716239,
			"isDeleted": false,
			"id": "rMWwuyIW",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 264.8755311169497,
			"y": 9476.548068214783,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 64,
			"height": 19.2,
			"seed": 110364385,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "状态变化",
			"rawText": "状态变化",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "状态变化",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 875,
			"versionNonce": 515099553,
			"isDeleted": false,
			"id": "NyTyJsmO",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 156.5985054995259,
			"y": 9594.571847228208,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 128,
			"height": 19.2,
			"seed": 1930592961,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "线程是否启动成功",
			"rawText": "线程是否启动成功",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "线程是否启动成功",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 918,
			"versionNonce": 804846319,
			"isDeleted": false,
			"id": "rxsDpxRS",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 144.16772646997288,
			"y": 9615.123634000283,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 128,
			"height": 19.2,
			"seed": 1712751265,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "线程是否添加成功",
			"rawText": "线程是否添加成功",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "线程是否添加成功",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 895,
			"versionNonce": 609988481,
			"isDeleted": false,
			"id": "MSNhqP1k",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 165.14102358023774,
			"y": 9959.316867154606,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 144,
			"height": 19.2,
			"seed": 2085586561,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "添加到工作线程集合",
			"rawText": "添加到工作线程集合",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "添加到工作线程集合",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 804,
			"versionNonce": 605668623,
			"isDeleted": false,
			"id": "P0mIVURa",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 79.07465222480471,
			"y": 10148.561507378228,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 64,
			"height": 19.2,
			"seed": 276631137,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "线程启动",
			"rawText": "线程启动",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "线程启动",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 778,
			"versionNonce": 387716961,
			"isDeleted": false,
			"id": "mGrdNTJH",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 105.11380304823456,
			"y": 10246.074405564423,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 96,
			"height": 19.2,
			"seed": 2037291585,
			"groupIds": [
				"QljCsLP8fwO3KCnwyp-X7"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "线程启动失败",
			"rawText": "线程启动失败",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "线程启动失败",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"id": "uEgW2oxe",
			"type": "text",
			"x": -203.31660130780378,
			"y": 8373.332145970126,
			"width": 656.25,
			"height": 768,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1758843727,
			"version": 272,
			"versionNonce": 1229226817,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"rawText": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 764,
			"containerId": null,
			"originalText": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"lineHeight": 1.2
		},
		{
			"id": "-C3j5jfWAoxz5SZzIyRtK",
			"type": "rectangle",
			"x": 229.57251920385897,
			"y": 8829.31135866239,
			"width": 180.36643534986433,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1551922913,
			"version": 197,
			"versionNonce": 1034489167,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "agqkj0jG"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "agqkj0jG",
			"type": "text",
			"x": 239.75573687879114,
			"y": 8834.31135866239,
			"width": 160,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 661876353,
			"version": 392,
			"versionNonce": 389313313,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "线程数小于核心线程数",
			"rawText": "线程数小于核心线程数",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "-C3j5jfWAoxz5SZzIyRtK",
			"originalText": "线程数小于核心线程数",
			"lineHeight": 1.2
		},
		{
			"id": "lJwzhkOrx22ppjaTQTEKu",
			"type": "rectangle",
			"x": -174.5521289319775,
			"y": 8978.451611787248,
			"width": 77.28541820820135,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1609602305,
			"version": 254,
			"versionNonce": 2061011823,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "ItWAXEVb"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "ItWAXEVb",
			"type": "text",
			"x": -167.90941982787683,
			"y": 8983.451611787248,
			"width": 64,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2081053167,
			"version": 187,
			"versionNonce": 626747137,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "队列未满",
			"rawText": "队列未满",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "lJwzhkOrx22ppjaTQTEKu",
			"originalText": "队列未满",
			"lineHeight": 1.2
		},
		{
			"id": "R07j44TcMx88vijDHo1SZ",
			"type": "arrow",
			"x": -133.22508673534531,
			"y": 8939.367472453087,
			"width": 1.0843701100270664,
			"height": 118.35340001917939,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 975670305,
			"version": 63,
			"versionNonce": 2108900751,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-1.0843701100270664,
					118.35340001917939
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "u9l5avXYvXBs195ZOAMfN",
			"type": "rectangle",
			"x": 214.51950541732504,
			"y": 9060.259736477376,
			"width": 236.97541391181426,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1141601889,
			"version": 494,
			"versionNonce": 1580785377,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "rILL5G1r"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "rILL5G1r",
			"type": "text",
			"x": 221.00721237323216,
			"y": 9065.259736477376,
			"width": 224,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 155560673,
			"version": 470,
			"versionNonce": 1701952431,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "队列已满，尝试使用非核心线程",
			"rawText": "队列已满，尝试使用非核心线程",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "u9l5avXYvXBs195ZOAMfN",
			"originalText": "队列已满，尝试使用非核心线程",
			"lineHeight": 1.2
		},
		{
			"id": "KXM6VuiRur7K_thpxEJZ5",
			"type": "rectangle",
			"x": 267.37727908941997,
			"y": 9011.810421391778,
			"width": 335.2662925728889,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffc9c9",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1369465281,
			"version": 556,
			"versionNonce": 581332687,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "tk7Hguty"
				}
			],
			"updated": 1704599189746,
			"link": null,
			"locked": false
		},
		{
			"id": "tk7Hguty",
			"type": "text",
			"x": 278.32292537586443,
			"y": 9016.810421391778,
			"width": 313.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2131517135,
			"version": 648,
			"versionNonce": 1446800847,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "如果核心线程数为0，直接开启一个工作线程",
			"rawText": "如果核心线程数为0，直接开启一个工作线程",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "KXM6VuiRur7K_thpxEJZ5",
			"originalText": "如果核心线程数为0，直接开启一个工作线程",
			"lineHeight": 1.2
		},
		{
			"id": "j_c6jZrZsJ8IDJhZ1grqM",
			"type": "rectangle",
			"x": 314.31622386667505,
			"y": 8924.347879458819,
			"width": 190.06575336520254,
			"height": 29.847522221971303,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 40235585,
			"version": 151,
			"versionNonce": 207968929,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "MOo5wHHN"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "MOo5wHHN",
			"type": "text",
			"x": 321.34910054927633,
			"y": 8929.671640569804,
			"width": 176,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1306498383,
			"version": 246,
			"versionNonce": 1529601007,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "运行状态，尝试放入队列",
			"rawText": "运行状态，尝试放入队列",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "j_c6jZrZsJ8IDJhZ1grqM",
			"originalText": "运行状态，尝试放入队列",
			"lineHeight": 1.2
		},
		{
			"id": "Rjbnxjn56y020NAZrmU09",
			"type": "rectangle",
			"x": 324.6404253417239,
			"y": 8967.132895515164,
			"width": 373.23717152465525,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1612062671,
			"version": 612,
			"versionNonce": 587423361,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Y0I9J28I"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "Y0I9J28I",
			"type": "text",
			"x": 335.2590111040515,
			"y": 8972.132895515164,
			"width": 352,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 723555617,
			"version": 465,
			"versionNonce": 138970639,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"rawText": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "Rjbnxjn56y020NAZrmU09",
			"originalText": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"lineHeight": 1.2
		},
		{
			"id": "3gimwo_5ZtpnIiwnr9xtI",
			"type": "rectangle",
			"x": 393.1403117181318,
			"y": 9375.521222577103,
			"width": 344.9241793112404,
			"height": 25.214576160005915,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 690602639,
			"version": 317,
			"versionNonce": 1552608303,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false
		},
		{
			"id": "OxVDl7XY",
			"type": "text",
			"x": 388.6475460639631,
			"y": 9350.093416509002,
			"width": 372.36274333253596,
			"height": 37.197086079679494,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1839231279,
			"version": 179,
			"versionNonce": 1675333199,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"rawText": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"fontSize": 15.498785866533126,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "top",
			"baseline": 32.99999999999999,
			"containerId": null,
			"originalText": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"lineHeight": 1.2
		},
		{
			"id": "v4U7MtJ_5coL_d4thSVZp",
			"type": "arrow",
			"x": -13.137416796933053,
			"y": 9504.6026312876,
			"width": 177.05639805124605,
			"height": 325.4871323502357,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1427317377,
			"version": 430,
			"versionNonce": 69632545,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-171.9059847420948,
					-2.5087918579502
				],
				[
					-177.05639805124605,
					-325.4871323502357
				],
				[
					-120.15910770285188,
					-325.0696679142966
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "PrkgU9XaGSvxVk4cM9Ufp",
			"type": "arrow",
			"x": -12.004327182114025,
			"y": 9449.303760918528,
			"width": 118.22659478437623,
			"height": 152.68909478859314,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 103422945,
			"version": 253,
			"versionNonce": 1731433985,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "bwdvew9g"
				}
			],
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-118.22659478437623,
					-0.8328277611617523
				],
				[
					-117.1820636233071,
					151.8562670274314
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "bwdvew9g",
			"type": "text",
			"x": -180.16856379079564,
			"y": 9438.910788601313,
			"width": 64,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 222010369,
			"version": 59,
			"versionNonce": 133793423,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704599189746,
			"link": null,
			"locked": false,
			"text": "占位成功",
			"rawText": "占位成功",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "PrkgU9XaGSvxVk4cM9Ufp",
			"originalText": "占位成功",
			"lineHeight": 1.2
		},
		{
			"id": "PVllQbFBx98HcNm1HMl11",
			"type": "arrow",
			"x": 511.4037579346631,
			"y": 9274.5863258854,
			"width": 540.2398204461872,
			"height": 236.6101817751478,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1823325423,
			"version": 313,
			"versionNonce": 934945217,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704599213587,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					183.60647939766318,
					-184.0333003422711
				],
				[
					-356.633341048524,
					-236.6101817751478
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "3Yhb8X9L",
				"focus": 0.7615097333945998,
				"gap": 1
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "ZbQJvlol",
			"type": "text",
			"x": -166.4607412921789,
			"y": 8706.944430312573,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 599483809,
			"version": 7,
			"versionNonce": 165432111,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "cuflgxEY",
			"type": "text",
			"x": 559.7576694616046,
			"y": 9374.817678784857,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1233837569,
			"version": 7,
			"versionNonce": 1660282465,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "B2OMMOCg",
			"type": "text",
			"x": 531.7509593192258,
			"y": 9385.05598864837,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 319549039,
			"version": 7,
			"versionNonce": 923562561,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "3gimwo_5ZtpnIiwnr9xtI",
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "cGs8p3gy",
			"type": "text",
			"x": -134.35205764322143,
			"y": 9373.35889188279,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2112197377,
			"version": 7,
			"versionNonce": 420670575,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189745,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "v4U7MtJ_5coL_d4thSVZp",
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "KdvDN19H5YQAlY1KVd8MU",
			"type": "arrow",
			"x": -65.05993356032502,
			"y": 9525.36922171563,
			"width": 8.010418955688237,
			"height": 15.253998158726972,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2037705327,
			"version": 59,
			"versionNonce": 858992097,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189746,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					8.010418955688237,
					-15.253998158726972
				]
			],
			"lastCommittedPoint": [
				8.010418955688237,
				-15.253998158726972
			],
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "BZMgd8nbNMQQ-Kg75-uWV",
			"type": "arrow",
			"x": -66.29939190528296,
			"y": 9521.64421505005,
			"width": 3.2895514691101653,
			"height": 2.4708405455894535,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 945360271,
			"version": 54,
			"versionNonce": 793238703,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189746,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					3.2895514691101653,
					-2.4708405455894535
				]
			],
			"lastCommittedPoint": [
				3.2895514691101653,
				-2.4708405455894535
			],
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "X2_ayZ1i-mHyUrQ3m98hk",
			"type": "arrow",
			"x": 157.07540100436495,
			"y": 9034.836296779857,
			"width": 461.17863538205563,
			"height": 3.8578362588323216,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 978711567,
			"version": 81,
			"versionNonce": 1725277807,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599182576,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					461.17863538205563,
					3.8578362588323216
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "KXM6VuiRur7K_thpxEJZ5",
				"focus": -0.6718323114924379,
				"gap": 15.610464724111694
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "LOdiRxt2y0udxEM3I5i73",
			"type": "rectangle",
			"x": 512.1164284265936,
			"y": 9249.507468345888,
			"width": 124.6732127627588,
			"height": 24.957682544778436,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1793348577,
			"version": 31,
			"versionNonce": 791988673,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599189746,
			"link": null,
			"locked": false
		},
		{
			"id": "NUHV2VLb",
			"type": "text",
			"x": 328.39958741040107,
			"y": 9146.681234997826,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2069763087,
			"version": 2,
			"versionNonce": 1695492097,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704599199103,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "PVllQbFBx98HcNm1HMl11",
			"originalText": "",
			"lineHeight": 1.2
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "dashed",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "center",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 717.1883892649058,
		"scrollY": -8805.792953374817,
		"zoom": {
			"value": 0.9295678626217359
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"gridColor": {
			"Bold": "#C9C9C9FF",
			"Regular": "#EDEDEDFF"
		},
		"currentStrokeOptions": null,
		"previousGridSize": null,
		"frameRendering": {
			"enabled": true,
			"clip": true,
			"name": true,
			"outline": true
		}
	},
	"files": {}
}
```
%%