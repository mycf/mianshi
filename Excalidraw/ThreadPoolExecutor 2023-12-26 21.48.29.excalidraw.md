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

线程是否添加成功，基本可以认为添加成功就能启动成功 ^rxsDpxRS

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

如果核心线程数为0，直接开启一个工作线程正常应该在队列未满之前只有一个线程，如果并发多个线程同时到这个方法，会产生多个线程 ^tk7Hguty

运行状态，尝试放入队列 ^MOo5wHHN

放入队列后，重新检查运行状态，否则移除并拒绝 ^Y0I9J28I

因为ctl前三位是状态，所以COUNT_MAK是线程最大值
 ^OxVDl7XY

占位成功 ^bwdvew9g

正常提交的firstTask，不应该为空
只有这里，核心线程数为空的时候 ^5M0yN7LH

firstTask为null，队列为空，应该也只有上面的情况，原task放入后，队列中的task都被其他线程poll了 ^RNgtVFuR

可能会有把其他线程提交任务执行的情况，队列就为空了 ^TUnUNpdN

添加核心线程 ^lGroBr57

添加失败：1、状态不对，2、线程数满了（核心线程或最大线程） ^wucn5LbN

核心线程数为0的情况 ^xCeLv8rH

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
			"version": 929,
			"versionNonce": 14676399,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001458,
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
			"version": 1200,
			"versionNonce": 1922335937,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "PVllQbFBx98HcNm1HMl11",
					"type": "arrow"
				}
			],
			"updated": 1704604001458,
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
			"version": 840,
			"versionNonce": 309584847,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001458,
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
			"version": 744,
			"versionNonce": 1778926753,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001458,
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
			"version": 764,
			"versionNonce": 1292145135,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001458,
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
			"version": 870,
			"versionNonce": 1622739073,
			"isDeleted": false,
			"id": "CT31Rn4KSTnW4XvIcaPMm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704604001458,
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
			"version": 788,
			"versionNonce": 1771731983,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
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
			"version": 880,
			"versionNonce": 1885753441,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
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
			"version": 988,
			"versionNonce": 1814962735,
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
			"width": 400,
			"height": 19.2,
			"seed": 1712751265,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "线程是否添加成功，基本可以认为添加成功就能启动成功",
			"rawText": "线程是否添加成功，基本可以认为添加成功就能启动成功",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "线程是否添加成功，基本可以认为添加成功就能启动成功",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 901,
			"versionNonce": 1323277377,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "cVq_sJ9i6giQtWwisZ1JO",
					"type": "arrow"
				}
			],
			"updated": 1704604001459,
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
			"version": 810,
			"versionNonce": 1168639055,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "cVq_sJ9i6giQtWwisZ1JO",
					"type": "arrow"
				}
			],
			"updated": 1704604001459,
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
			"version": 783,
			"versionNonce": 1275358241,
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
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
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
			"type": "text",
			"version": 276,
			"versionNonce": 903914095,
			"isDeleted": false,
			"id": "uEgW2oxe",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -203.31660130780378,
			"y": 8373.332145970126,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 656.25,
			"height": 768,
			"seed": 1758843727,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"rawText": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n",
			"lineHeight": 1.2,
			"baseline": 764
		},
		{
			"type": "rectangle",
			"version": 245,
			"versionNonce": 264519681,
			"isDeleted": false,
			"id": "-C3j5jfWAoxz5SZzIyRtK",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 229.57251920385897,
			"y": 8829.31135866239,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 158.90008739796826,
			"height": 27,
			"seed": 1551922913,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "agqkj0jG"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 460,
			"versionNonce": 1839234191,
			"isDeleted": false,
			"id": "agqkj0jG",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 238.57258121338998,
			"y": 8834.353904613017,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 140.89996337890625,
			"height": 16.914908098743915,
			"seed": 661876353,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 14.095756748953262,
			"fontFamily": 3,
			"text": "线程数小于核心线程数",
			"rawText": "线程数小于核心线程数",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "-C3j5jfWAoxz5SZzIyRtK",
			"originalText": "线程数小于核心线程数",
			"lineHeight": 1.2,
			"baseline": 13
		},
		{
			"type": "rectangle",
			"version": 258,
			"versionNonce": 404658145,
			"isDeleted": false,
			"id": "lJwzhkOrx22ppjaTQTEKu",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -174.5521289319775,
			"y": 8978.451611787248,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 77.28541820820135,
			"height": 29.2,
			"seed": 1609602305,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "ItWAXEVb"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 191,
			"versionNonce": 1889538735,
			"isDeleted": false,
			"id": "ItWAXEVb",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -167.90941982787683,
			"y": 8983.451611787248,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 64,
			"height": 19.2,
			"seed": 2081053167,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "队列未满",
			"rawText": "队列未满",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "lJwzhkOrx22ppjaTQTEKu",
			"originalText": "队列未满",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 67,
			"versionNonce": 1998779329,
			"isDeleted": false,
			"id": "R07j44TcMx88vijDHo1SZ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -133.22508673534531,
			"y": 8939.367472453087,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 1.0843701100270664,
			"height": 118.35340001917939,
			"seed": 975670305,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-1.0843701100270664,
					118.35340001917939
				]
			]
		},
		{
			"type": "rectangle",
			"version": 498,
			"versionNonce": 931652815,
			"isDeleted": false,
			"id": "u9l5avXYvXBs195ZOAMfN",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 214.51950541732504,
			"y": 9060.259736477376,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 236.97541391181426,
			"height": 29.2,
			"seed": 1141601889,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "rILL5G1r"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 474,
			"versionNonce": 1357960097,
			"isDeleted": false,
			"id": "rILL5G1r",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 221.00721237323216,
			"y": 9065.259736477376,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 224,
			"height": 19.2,
			"seed": 155560673,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "队列已满，尝试使用非核心线程",
			"rawText": "队列已满，尝试使用非核心线程",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "u9l5avXYvXBs195ZOAMfN",
			"originalText": "队列已满，尝试使用非核心线程",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 725,
			"versionNonce": 300970735,
			"isDeleted": false,
			"id": "KXM6VuiRur7K_thpxEJZ5",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 267.3772790894199,
			"y": 9011.810421391778,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffc9c9",
			"width": 676.5975068952836,
			"height": 49,
			"seed": 1369465281,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "tk7Hguty"
				},
				{
					"id": "tNF9R-U8jlwUTz34plAo9",
					"type": "arrow"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 897,
			"versionNonce": 431470465,
			"isDeleted": false,
			"id": "tk7Hguty",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 272.9885325370617,
			"y": 9017.110421391777,
			"strokeColor": "#1971c2",
			"backgroundColor": "#ffffff",
			"width": 665.375,
			"height": 38.4,
			"seed": 2131517135,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "如果核心线程数为0，直接开启一个工作线程正常应该在队列未满之前只有一个线程，如果并发\n多个线程同时到这个方法，会产生多个线程",
			"rawText": "如果核心线程数为0，直接开启一个工作线程正常应该在队列未满之前只有一个线程，如果并发多个线程同时到这个方法，会产生多个线程",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "KXM6VuiRur7K_thpxEJZ5",
			"originalText": "如果核心线程数为0，直接开启一个工作线程正常应该在队列未满之前只有一个线程，如果并发多个线程同时到这个方法，会产生多个线程",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "rectangle",
			"version": 155,
			"versionNonce": 1214397711,
			"isDeleted": false,
			"id": "j_c6jZrZsJ8IDJhZ1grqM",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 314.31622386667505,
			"y": 8924.347879458819,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 190.06575336520254,
			"height": 29.847522221971303,
			"seed": 40235585,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "MOo5wHHN"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 250,
			"versionNonce": 330768225,
			"isDeleted": false,
			"id": "MOo5wHHN",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 321.34910054927633,
			"y": 8929.671640569804,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 176,
			"height": 19.2,
			"seed": 1306498383,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "运行状态，尝试放入队列",
			"rawText": "运行状态，尝试放入队列",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "j_c6jZrZsJ8IDJhZ1grqM",
			"originalText": "运行状态，尝试放入队列",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 616,
			"versionNonce": 2119549743,
			"isDeleted": false,
			"id": "Rjbnxjn56y020NAZrmU09",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 324.6404253417239,
			"y": 8967.132895515164,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 373.23717152465525,
			"height": 29.2,
			"seed": 1612062671,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Y0I9J28I"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 469,
			"versionNonce": 222202689,
			"isDeleted": false,
			"id": "Y0I9J28I",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 335.2590111040515,
			"y": 8972.132895515164,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 352,
			"height": 19.2,
			"seed": 723555617,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"rawText": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Rjbnxjn56y020NAZrmU09",
			"originalText": "放入队列后，重新检查运行状态，否则移除并拒绝",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "rectangle",
			"version": 321,
			"versionNonce": 1412662607,
			"isDeleted": false,
			"id": "3gimwo_5ZtpnIiwnr9xtI",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 393.1403117181318,
			"y": 9375.521222577103,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 344.9241793112404,
			"height": 25.214576160005915,
			"seed": 690602639,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 183,
			"versionNonce": 1588427553,
			"isDeleted": false,
			"id": "OxVDl7XY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 388.6475460639631,
			"y": 9350.093416509002,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 372.243896484375,
			"height": 37.1970860796795,
			"seed": 1839231279,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 15.498785866533126,
			"fontFamily": 3,
			"text": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"rawText": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"textAlign": "center",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "因为ctl前三位是状态，所以COUNT_MAK是线程最大值\n",
			"lineHeight": 1.2,
			"baseline": 33
		},
		{
			"type": "arrow",
			"version": 435,
			"versionNonce": 1230242671,
			"isDeleted": false,
			"id": "v4U7MtJ_5coL_d4thSVZp",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -13.137416796933053,
			"y": 9504.6026312876,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 177.05639805124605,
			"height": 325.4871323502357,
			"seed": 1427317377,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
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
			]
		},
		{
			"type": "arrow",
			"version": 258,
			"versionNonce": 678653697,
			"isDeleted": false,
			"id": "PrkgU9XaGSvxVk4cM9Ufp",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -12.004327182114025,
			"y": 9449.303760918528,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 118.22659478437623,
			"height": 152.68909478859314,
			"seed": 103422945,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "bwdvew9g"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
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
			]
		},
		{
			"type": "text",
			"version": 64,
			"versionNonce": 1577041295,
			"isDeleted": false,
			"id": "bwdvew9g",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -180.16856379079564,
			"y": 9438.910788601313,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 64,
			"height": 19.2,
			"seed": 222010369,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "占位成功",
			"rawText": "占位成功",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "PrkgU9XaGSvxVk4cM9Ufp",
			"originalText": "占位成功",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "arrow",
			"version": 320,
			"versionNonce": 1359996641,
			"isDeleted": false,
			"id": "PVllQbFBx98HcNm1HMl11",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 511.4037579346631,
			"y": 9274.5863258854,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 540.2398204461872,
			"height": 236.6101817751478,
			"seed": 1823325423,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "5M0yN7LH"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "3Yhb8X9L",
				"focus": 0.7615097333945998,
				"gap": 1
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
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
			]
		},
		{
			"type": "text",
			"version": 104,
			"versionNonce": 1970221999,
			"isDeleted": false,
			"id": "5M0yN7LH",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 564.8227373323263,
			"y": 9071.353025543129,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 260.375,
			"height": 38.4,
			"seed": 1155229057,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "正常提交的firstTask，不应该为空\n只有这里，核心线程数为空的时候",
			"rawText": "正常提交的firstTask，不应该为空\n只有这里，核心线程数为空的时候",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "PVllQbFBx98HcNm1HMl11",
			"originalText": "正常提交的firstTask，不应该为空\n只有这里，核心线程数为空的时候",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 653,
			"versionNonce": 1125278401,
			"isDeleted": false,
			"id": "pZQY8QwPFmd9AEOcbsT_S",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 454.92642709118775,
			"y": 9295.513749772985,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 233.70641393192227,
			"height": 146.40115983419855,
			"seed": 1390175791,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "RNgtVFuR"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					224.94845917831344,
					-12.04698094504056
				],
				[
					233.70641393192227,
					-146.40115983419855
				]
			]
		},
		{
			"type": "text",
			"version": 234,
			"versionNonce": 2009785807,
			"isDeleted": false,
			"id": "RNgtVFuR",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 594.9373862695012,
			"y": 9235.466768827944,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 217.875,
			"height": 76.8,
			"seed": 514729025,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "firstTask为null，队列为空\n，应该也只有上面的情况，原t\nask放入后，队列中的task都\n被其他线程poll了",
			"rawText": "firstTask为null，队列为空，应该也只有上面的情况，原task放入后，队列中的task都被其他线程poll了",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "pZQY8QwPFmd9AEOcbsT_S",
			"originalText": "firstTask为null，队列为空，应该也只有上面的情况，原task放入后，队列中的task都被其他线程poll了",
			"lineHeight": 1.2,
			"baseline": 73
		},
		{
			"type": "arrow",
			"version": 108,
			"versionNonce": 1796891297,
			"isDeleted": false,
			"id": "tNF9R-U8jlwUTz34plAo9",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 916.4783379243798,
			"y": 9065.140124229052,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 145.5619302513188,
			"height": 240.91443840201646,
			"seed": 929424623,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "TUnUNpdN"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "KXM6VuiRur7K_thpxEJZ5",
				"gap": 4.3297028372744535,
				"focus": -0.9194712632186743
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-7.412718615901781,
					184.17066045551292
				],
				[
					-145.5619302513188,
					240.91443840201646
				]
			]
		},
		{
			"type": "text",
			"version": 79,
			"versionNonce": 875254767,
			"isDeleted": false,
			"id": "TUnUNpdN",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 829.0656193084781,
			"y": 9220.510784684566,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 208,
			"height": 38.4,
			"seed": 1095826703,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "可能会有把其他线程提交任务\n执行的情况，队列就为空了",
			"rawText": "可能会有把其他线程提交任务执行的情况，队列就为空了",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "tNF9R-U8jlwUTz34plAo9",
			"originalText": "可能会有把其他线程提交任务执行的情况，队列就为空了",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "rectangle",
			"version": 232,
			"versionNonce": 1977050753,
			"isDeleted": false,
			"id": "32-nCxCtoC_j0wkFstsFm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 188.99035082689085,
			"y": 8858.459117651693,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 96.3636722233283,
			"height": 27,
			"seed": 1127290209,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "lGroBr57"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 293,
			"versionNonce": 43580943,
			"isDeleted": false,
			"id": "lGroBr57",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 197.18222722175813,
			"y": 8863.955887767213,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"width": 79.97991943359375,
			"height": 16.006459768958653,
			"seed": 1067942497,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"fontSize": 13.33871647413221,
			"fontFamily": 3,
			"text": "添加核心线程",
			"rawText": "添加核心线程",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "32-nCxCtoC_j0wkFstsFm",
			"originalText": "添加核心线程",
			"lineHeight": 1.2,
			"baseline": 12
		},
		{
			"id": "SaZdBhsWWCsct4O9NKMG1",
			"type": "arrow",
			"x": 65.36639793409279,
			"y": 10318.25356381244,
			"width": 221.525495700404,
			"height": 138.6667778431456,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1412418735,
			"version": 104,
			"versionNonce": 1309208161,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					221.525495700404,
					-56.32138649421904
				],
				[
					118.63685284094504,
					-138.6667778431456
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": "triangle",
			"endArrowhead": null
		},
		{
			"id": "cVq_sJ9i6giQtWwisZ1JO",
			"type": "arrow",
			"x": 118.15861063940258,
			"y": 10139.942612466046,
			"width": 237.2892086242066,
			"height": 151.42934348745257,
			"angle": 0,
			"strokeColor": "#f08c00",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1468744463,
			"version": 251,
			"versionNonce": 638346287,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					237.2892086242066,
					-94.6376804880656
				],
				[
					82.35156781996818,
					-151.42934348745257
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "P0mIVURa",
				"focus": -0.6883668834765602,
				"gap": 8.61889491218244
			},
			"endBinding": {
				"elementId": "MSNhqP1k",
				"focus": 0.9175356646519862,
				"gap": 9.996401823986162
			},
			"startArrowhead": "triangle",
			"endArrowhead": null
		},
		{
			"id": "YzvRTOAIR0clU8033bpDm",
			"type": "rectangle",
			"x": -156.0763639366977,
			"y": 9121.493616991893,
			"width": 507.74245351047443,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1682642049,
			"version": 191,
			"versionNonce": 1518238273,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "wucn5LbN"
				},
				{
					"id": "GOoZotPe3AeH2dKNDSUgY",
					"type": "arrow"
				}
			],
			"updated": 1704604001459,
			"link": null,
			"locked": false
		},
		{
			"id": "wucn5LbN",
			"type": "text",
			"x": -135.5801371814605,
			"y": 9126.493616991893,
			"width": 466.75,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": null,
			"seed": 1953724353,
			"version": 234,
			"versionNonce": 1852704335,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"text": "添加失败：1、状态不对，2、线程数满了（核心线程或最大线程）",
			"rawText": "添加失败：1、状态不对，2、线程数满了（核心线程或最大线程）",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "YzvRTOAIR0clU8033bpDm",
			"originalText": "添加失败：1、状态不对，2、线程数满了（核心线程或最大线程）",
			"lineHeight": 1.2
		},
		{
			"type": "arrow",
			"version": 93,
			"versionNonce": 2081917473,
			"isDeleted": false,
			"id": "GOoZotPe3AeH2dKNDSUgY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -88.70283104158466,
			"y": 8864.295109844967,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 151.96218764427852,
			"height": 271.77405233633,
			"seed": 918398351,
			"groupIds": [
				"h32QGEiwMP9DG9wOPrMIQ"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "YzvRTOAIR0clU8033bpDm",
				"gap": 6.962149041334214,
				"focus": -1.0021353116795906
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-151.96218764427852,
					94.56912165949325
				],
				[
					-74.33568193644729,
					271.77405233633
				]
			]
		},
		{
			"id": "54LwCMSSYEJF_RHHRuWem",
			"type": "rectangle",
			"x": 506.43486471044446,
			"y": 9899.244187779837,
			"width": 98.59733123947558,
			"height": 49,
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
				"type": 3
			},
			"seed": 1347245231,
			"version": 50,
			"versionNonce": 1703561665,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "xCeLv8rH"
				}
			],
			"updated": 1704604011824,
			"link": null,
			"locked": false
		},
		{
			"id": "xCeLv8rH",
			"type": "text",
			"x": 515.7335303301822,
			"y": 9904.544187779837,
			"width": 80,
			"height": 38.4,
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
			"seed": 1277143663,
			"version": 25,
			"versionNonce": 2133539969,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704604017871,
			"link": null,
			"locked": false,
			"text": "核心线程数\n为0的情况",
			"rawText": "核心线程数为0的情况",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "54LwCMSSYEJF_RHHRuWem",
			"originalText": "核心线程数为0的情况",
			"lineHeight": 1.2
		},
		{
			"type": "arrow",
			"version": 120,
			"versionNonce": 1051609313,
			"isDeleted": true,
			"id": "w7wLFDjlDgPRn0AHBBqR0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -47.96477147454459,
			"y": 9524.275527926653,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"width": 34.38165656638864,
			"height": 149.90932530763166,
			"seed": 954942081,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704604001458,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-34.38165656638864,
					-71.91182754488909
				],
				[
					-8.516261361237639,
					-149.90932530763166
				]
			]
		},
		{
			"id": "cRmLjcOj4BSYr2Yg2mABW",
			"type": "arrow",
			"x": 511.28576966926096,
			"y": 9901.808759912741,
			"width": 65.91377790074012,
			"height": 15.791594158064981,
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
			"seed": 978844175,
			"version": 13,
			"versionNonce": 446063727,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704604001459,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					65.91377790074012,
					15.791594158064981
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
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
		"scrollX": 507.8284943027139,
		"scrollY": -9278.283780882093,
		"zoom": {
			"value": 1.008188171386719
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