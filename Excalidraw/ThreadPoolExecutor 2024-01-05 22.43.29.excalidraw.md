---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
    final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        Runnable task = w.firstTask;
        w.firstTask = null;
        w.unlock(); // allow interrupts
        boolean completedAbruptly = true;
        try {
            while (task != null || (task = getTask()) != null) {
                w.lock();
                // If pool is stopping, ensure thread is interrupted;
                // if not, ensure thread is not interrupted.  This
                // requires a recheck in second case to deal with
                // shutdownNow race while clearing interrupt
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                    beforeExecute(wt, task);
                    try {
                        task.run();
                        afterExecute(task, null);
                    } catch (Throwable ex) {
                        afterExecute(task, ex);
                        throw ex;
                    }
                } finally {
                    task = null;
                    w.completedTasks++;
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            processWorkerExit(w, completedAbruptly);
        }
    }

    private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        for (;;) {
            int c = ctl.get();

            // Check if queue empty only if necessary.
            if (runStateAtLeast(c, SHUTDOWN)
                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }

            int wc = workerCountOf(c);

            // Are workers subject to culling?
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;

            if ((wc > maximumPoolSize || (timed && timedOut))
                && (wc > 1 || workQueue.isEmpty())) {
                if (compareAndDecrementWorkerCount(c))
                    return null;
                continue;
            }

            try {
                Runnable r = timed ?
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }

 ^r7BTOJwb

线程数 ^1Dle7lSO

是否允许过期
1、核心线程允许回收
2、超过核心线程数 ^S4X3xbjr

什么时候会出现线程数大于最大线程数？
我认为只有动态调整了最大线程数会出现这个问题 ^SPYEAGem

未获取到任务会一直阻塞 ^OvNtRpCU

这里会因为超时返回null ^Gn8kPbfq

思考🤔 ^iwTvfHhA

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "r7BTOJwb",
			"type": "text",
			"x": -238.18077305385054,
			"y": -397.2820194789341,
			"width": 855.46875,
			"height": 1872,
			"angle": 0,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 925744097,
			"version": 189,
			"versionNonce": 1936736399,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466846689,
			"link": null,
			"locked": false,
			"text": "    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n",
			"rawText": "    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 1866,
			"containerId": null,
			"originalText": "    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n",
			"lineHeight": 1.2
		},
		{
			"id": "eZjB9pwCM6n-TEszVclUg",
			"type": "rectangle",
			"x": -52.74411010742162,
			"y": 963.5912976946152,
			"width": 254.67215401785703,
			"height": 33.322579520089214,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1221732449,
			"version": 140,
			"versionNonce": 1945873775,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "9h9DVq4lCWG7_Vl8fUIBF",
					"type": "arrow"
				}
			],
			"updated": 1704466980261,
			"link": null,
			"locked": false
		},
		{
			"id": "h9WpijYMQd0SZk3CEezC-",
			"type": "rectangle",
			"x": -6.612808954147965,
			"y": 919.739717755999,
			"width": 66.95021856398807,
			"height": 30.651884533110206,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1293283375,
			"version": 39,
			"versionNonce": 2111146433,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "_dXLwFeSmFSkhBEvZ1Tj_",
					"type": "arrow"
				}
			],
			"updated": 1704466869266,
			"link": null,
			"locked": false
		},
		{
			"id": "utEtbES-y4udhnaGLwbV-",
			"type": "rectangle",
			"x": 263.4942641485308,
			"y": -236.43233163016225,
			"width": 125.61569940476193,
			"height": 36.13089425223214,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 648511535,
			"version": 58,
			"versionNonce": 394209615,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466298930,
			"link": null,
			"locked": false
		},
		{
			"id": "f5jJ4bcpTXWh5kcfJr1Xw",
			"type": "arrow",
			"x": 321.6104285830546,
			"y": -201.518769037156,
			"width": 278.0847749255952,
			"height": 739.9711390904017,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1974624655,
			"version": 92,
			"versionNonce": 1093936033,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466347886,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-278.0847749255952,
					739.9711390904017
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "O44hfkXphfKxj7zSK8AwF",
			"type": "rectangle",
			"x": 210.3434549967451,
			"y": 844.9607522147041,
			"width": 78.4168061755953,
			"height": 34,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1956953857,
			"version": 111,
			"versionNonce": 58189423,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "1Dle7lSO"
				}
			],
			"updated": 1704466430923,
			"link": null,
			"locked": false
		},
		{
			"id": "1Dle7lSO",
			"type": "text",
			"x": 219.55185808454274,
			"y": 849.9607522147041,
			"width": 60,
			"height": 24,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1506086721,
			"version": 39,
			"versionNonce": 263143567,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466430923,
			"link": null,
			"locked": false,
			"text": "线程数",
			"rawText": "线程数",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "O44hfkXphfKxj7zSK8AwF",
			"originalText": "线程数",
			"lineHeight": 1.2
		},
		{
			"id": "hXDkZY68xdp5JYVquwaT3",
			"type": "rectangle",
			"x": 396.21352568126895,
			"y": 837.0125972202846,
			"width": 219.0619187127978,
			"height": 82,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1683034177,
			"version": 505,
			"versionNonce": 1841701089,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "S4X3xbjr"
				},
				{
					"id": "_dXLwFeSmFSkhBEvZ1Tj_",
					"type": "arrow"
				}
			],
			"updated": 1704466681432,
			"link": null,
			"locked": false
		},
		{
			"id": "S4X3xbjr",
			"type": "text",
			"x": 401.21352568126895,
			"y": 842.0125972202846,
			"width": 191.71875,
			"height": 72,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 29019649,
			"version": 596,
			"versionNonce": 264233089,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466681432,
			"link": null,
			"locked": false,
			"text": "是否允许过期\n1、核心线程允许回收\n2、超过核心线程数",
			"rawText": "是否允许过期\n1、核心线程允许回收\n2、超过核心线程数",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 67,
			"containerId": "hXDkZY68xdp5JYVquwaT3",
			"originalText": "是否允许过期\n1、核心线程允许回收\n2、超过核心线程数",
			"lineHeight": 1.2
		},
		{
			"id": "_dXLwFeSmFSkhBEvZ1Tj_",
			"type": "arrow",
			"x": 61.33740960984011,
			"y": 921.7711688301299,
			"width": 332.1558082217262,
			"height": 51.44682298962039,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#fcfcfc",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1027802497,
			"version": 590,
			"versionNonce": 1728715937,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466681432,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					332.1558082217262,
					-51.44682298962039
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "h9WpijYMQd0SZk3CEezC-",
				"gap": 1,
				"focus": -0.3863409931026124
			},
			"endBinding": {
				"elementId": "hXDkZY68xdp5JYVquwaT3",
				"gap": 2.7203078497026922,
				"focus": 0.432581515890206
			},
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "UfgN-yPUMh-9HkEtkxhdH",
			"type": "rectangle",
			"x": -284.62265595935617,
			"y": 999.7271910167879,
			"width": 214.5726376488095,
			"height": 87,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 705756399,
			"version": 92,
			"versionNonce": 1236022337,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "SPYEAGem"
				}
			],
			"updated": 1704466972755,
			"link": null,
			"locked": false
		},
		{
			"id": "SPYEAGem",
			"type": "text",
			"x": -279.62265595935617,
			"y": 1004.8271910167879,
			"width": 192,
			"height": 76.8,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1457829999,
			"version": 141,
			"versionNonce": 1904084001,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466972756,
			"link": null,
			"locked": false,
			"text": "什么时候会出现线程数大于\n最大线程数？\n我认为只有动态调整了最大\n线程数会出现这个问题",
			"rawText": "什么时候会出现线程数大于最大线程数？\n我认为只有动态调整了最大线程数会出现这个问题",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 73,
			"containerId": "UfgN-yPUMh-9HkEtkxhdH",
			"originalText": "什么时候会出现线程数大于最大线程数？\n我认为只有动态调整了最大线程数会出现这个问题",
			"lineHeight": 1.2
		},
		{
			"id": "9h9DVq4lCWG7_Vl8fUIBF",
			"type": "arrow",
			"x": -45.256549653553634,
			"y": 997.109364100864,
			"width": 33.51876395089283,
			"height": 14.711739676339448,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 772943841,
			"version": 23,
			"versionNonce": 1426616143,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466980261,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-33.51876395089283,
					14.711739676339448
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "eZjB9pwCM6n-TEszVclUg",
				"focus": 0.4927055428072657,
				"gap": 1
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "nY_tjoEEoSVTAECZnteqW",
			"type": "rectangle",
			"x": 205.0771092006141,
			"y": 1210.6945110502697,
			"width": 194.16120256696428,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1150081217,
			"version": 174,
			"versionNonce": 701536079,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "OvNtRpCU"
				}
			],
			"updated": 1704467097654,
			"link": null,
			"locked": false
		},
		{
			"id": "OvNtRpCU",
			"type": "text",
			"x": 214.15771048409624,
			"y": 1215.6945110502697,
			"width": 176,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2070491873,
			"version": 136,
			"versionNonce": 1300157807,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467097654,
			"link": null,
			"locked": false,
			"text": "未获取到任务会一直阻塞",
			"rawText": "未获取到任务会一直阻塞",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "nY_tjoEEoSVTAECZnteqW",
			"originalText": "未获取到任务会一直阻塞",
			"lineHeight": 1.2
		},
		{
			"id": "Oh8GzOyqvZu-dyMjjy9xW",
			"type": "rectangle",
			"x": -11.43784150623111,
			"y": 1187.8827667236326,
			"width": 179.37825520833337,
			"height": 27.572719029017883,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1183679919,
			"version": 69,
			"versionNonce": 495108161,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467120069,
			"link": null,
			"locked": false
		},
		{
			"id": "YFsD-vBxTQeeVnig4zjMr",
			"type": "arrow",
			"x": -11.066922142391832,
			"y": 1204.5098477318174,
			"width": 165.72980608258925,
			"height": 90.20937965029748,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 418266735,
			"version": 226,
			"versionNonce": 1698003887,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Gn8kPbfq"
				}
			],
			"updated": 1704467188593,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-165.72980608258925,
					25.498163132440368
				],
				[
					-47.32404436383925,
					90.20937965029748
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "Gn8kPbfq",
			"type": "text",
			"x": -262.85922822498105,
			"y": 1210.8080108642578,
			"width": 172.125,
			"height": 38.4,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 232080239,
			"version": 60,
			"versionNonce": 221611279,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467177577,
			"link": null,
			"locked": false,
			"text": "这里会因为超时返回nul\nl",
			"rawText": "这里会因为超时返回null",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "YFsD-vBxTQeeVnig4zjMr",
			"originalText": "这里会因为超时返回null",
			"lineHeight": 1.2
		},
		{
			"id": "hB-lo38wATbadByAR7Quw",
			"type": "rectangle",
			"x": -55.3110162644156,
			"y": 1285.5998193650018,
			"width": 192.6045735677083,
			"height": 23.100295293898625,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1948728911,
			"version": 56,
			"versionNonce": 371655471,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467145221,
			"link": null,
			"locked": false
		},
		{
			"id": "CbDmvPQl-9kH3195bVLHQ",
			"type": "rectangle",
			"x": 240.16220964704263,
			"y": 965.6085968017575,
			"width": 240.46677362351193,
			"height": 31.47693452380952,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 381000353,
			"version": 79,
			"versionNonce": 1250768097,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467267252,
			"link": null,
			"locked": false
		},
		{
			"id": "EwpSFe_JXDWRYznQeDvLo",
			"type": "rectangle",
			"x": 551.5526486351375,
			"y": 987.0018259684251,
			"width": 201.16117931547637,
			"height": 149.8784528459821,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 2075521217,
			"version": 50,
			"versionNonce": 1465811809,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "iwTvfHhA"
				}
			],
			"updated": 1704467343386,
			"link": null,
			"locked": false
		},
		{
			"id": "iwTvfHhA",
			"type": "text",
			"x": 556.5526486351375,
			"y": 1052.3410523914163,
			"width": 52,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 256949295,
			"version": 24,
			"versionNonce": 1740496751,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704467345119,
			"link": null,
			"locked": false,
			"text": "思考🤔",
			"rawText": "思考🤔",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "EwpSFe_JXDWRYznQeDvLo",
			"originalText": "思考🤔",
			"lineHeight": 1.2
		},
		{
			"id": "Iip2O6hr00N2ezZhgEnsr",
			"type": "arrow",
			"x": -53.00324794224301,
			"y": 1281.6724686395555,
			"width": 81.8388439360119,
			"height": 27.624569847469957,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1026208801,
			"version": 16,
			"versionNonce": 2022480943,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704467140323,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					81.8388439360119,
					27.624569847469957
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 237.87588791620163,
		"scrollY": -487.8818221319295,
		"zoom": {
			"value": 1.05
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