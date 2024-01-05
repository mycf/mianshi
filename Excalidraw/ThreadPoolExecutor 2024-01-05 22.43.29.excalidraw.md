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
1、核心线程允许回收=》所有线程都要超时回收
2、超过核心线程数 ^S4X3xbjr

什么时候会出现线程数大于最大线程数？
我认为只有动态调整了最大线程数会出现这个问题 ^SPYEAGem

未获取到任务会一直阻塞 ^OvNtRpCU

这里会因为超时返回null ^Gn8kPbfq

思考🤔：既然是只有timed为true，timedOut才可能为true这里为什么还要&&，直接timedOut不就好了吗
因为上一次获取task超时了，尝试执行清除线程的时候timed可能发生变化
1、allowCoreThreadTimeOut可能变化
2、其他线程可能被回收了，线程数可能不到核心线程数了 ^iwTvfHhA

1、状态为shutdown并且队列为空
2、状态为stop及以上（不再处理任务了）
这里写的有点绕。。。感觉可以换一种写法 ^UpMR0sSl

处理firstTask ^DnF7lZC2

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
			"version": 193,
			"versionNonce": 2113064481,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505554,
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
			"version": 141,
			"versionNonce": 704733295,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "9h9DVq4lCWG7_Vl8fUIBF",
					"type": "arrow"
				}
			],
			"updated": 1704468505554,
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
			"version": 40,
			"versionNonce": 2039898625,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "_dXLwFeSmFSkhBEvZ1Tj_",
					"type": "arrow"
				}
			],
			"updated": 1704468505554,
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
			"version": 59,
			"versionNonce": 1526299279,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505554,
			"link": null,
			"locked": false
		},
		{
			"id": "f5jJ4bcpTXWh5kcfJr1Xw",
			"type": "arrow",
			"x": 321.6104285830546,
			"y": -201.518769037156,
			"width": 810.7149832589279,
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
			"version": 201,
			"versionNonce": 123501025,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					532.6302083333327,
					218.48879859560753
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
			"version": 112,
			"versionNonce": 103370927,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "1Dle7lSO"
				}
			],
			"updated": 1704468505555,
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
			"version": 40,
			"versionNonce": 1213502913,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"height": 87,
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
			"version": 507,
			"versionNonce": 87646927,
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
			"updated": 1704468505555,
			"link": null,
			"locked": false
		},
		{
			"id": "S4X3xbjr",
			"type": "text",
			"x": 401.21352568126895,
			"y": 842.1125972202847,
			"width": 194.75,
			"height": 76.8,
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
			"version": 642,
			"versionNonce": 262026657,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false,
			"text": "是否允许过期\n1、核心线程允许回收=》所\n有线程都要超时回收\n2、超过核心线程数",
			"rawText": "是否允许过期\n1、核心线程允许回收=》所有线程都要超时回收\n2、超过核心线程数",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 73,
			"containerId": "hXDkZY68xdp5JYVquwaT3",
			"originalText": "是否允许过期\n1、核心线程允许回收=》所有线程都要超时回收\n2、超过核心线程数",
			"lineHeight": 1.2
		},
		{
			"id": "_dXLwFeSmFSkhBEvZ1Tj_",
			"type": "arrow",
			"x": 61.33740960984011,
			"y": 921.9466904500455,
			"width": 332.15580822172615,
			"height": 50.40689656117877,
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
			"version": 597,
			"versionNonce": 1757556975,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					332.15580822172615,
					-50.40689656117877
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
			"version": 93,
			"versionNonce": 543742337,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "SPYEAGem"
				}
			],
			"updated": 1704468505555,
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
			"version": 142,
			"versionNonce": 441203471,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 24,
			"versionNonce": 737411425,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 175,
			"versionNonce": 1045430575,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "OvNtRpCU"
				}
			],
			"updated": 1704468505555,
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
			"version": 137,
			"versionNonce": 261041473,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 70,
			"versionNonce": 1446380367,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 227,
			"versionNonce": 1385815329,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Gn8kPbfq"
				}
			],
			"updated": 1704468505555,
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
			"version": 61,
			"versionNonce": 576110959,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 57,
			"versionNonce": 1140505487,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
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
			"version": 80,
			"versionNonce": 449562849,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false
		},
		{
			"id": "EwpSFe_JXDWRYznQeDvLo",
			"type": "rectangle",
			"x": 426.53986031668524,
			"y": 1010.0080108642586,
			"width": 394.8096865699408,
			"height": 164.87102399553558,
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
			"seed": 2075521217,
			"version": 507,
			"versionNonce": 526021039,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "iwTvfHhA"
				},
				{
					"id": "zFrBqh0kBjnkDvXm1RnXw",
					"type": "arrow"
				}
			],
			"updated": 1704468505555,
			"link": null,
			"locked": false
		},
		{
			"id": "iwTvfHhA",
			"type": "text",
			"x": 431.53986031668524,
			"y": 1015.0080108642586,
			"width": 377.375,
			"height": 153.6,
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
			"version": 1016,
			"versionNonce": 1862403265,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false,
			"text": "思考🤔：既然是只有timed为true，timedOut才可\n能为true这里为什么还要&&，直接timedOut不就好\n了吗\n因为上一次获取task超时了，尝试执行清除线程的时\n候timed可能发生变化\n1、allowCoreThreadTimeOut可能变化\n2、其他线程可能被回收了，线程数可能不到核心线程\n数了",
			"rawText": "思考🤔：既然是只有timed为true，timedOut才可能为true这里为什么还要&&，直接timedOut不就好了吗\n因为上一次获取task超时了，尝试执行清除线程的时候timed可能发生变化\n1、allowCoreThreadTimeOut可能变化\n2、其他线程可能被回收了，线程数可能不到核心线程数了",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 149,
			"containerId": "EwpSFe_JXDWRYznQeDvLo",
			"originalText": "思考🤔：既然是只有timed为true，timedOut才可能为true这里为什么还要&&，直接timedOut不就好了吗\n因为上一次获取task超时了，尝试执行清除线程的时候timed可能发生变化\n1、allowCoreThreadTimeOut可能变化\n2、其他线程可能被回收了，线程数可能不到核心线程数了",
			"lineHeight": 1.2
		},
		{
			"id": "zFrBqh0kBjnkDvXm1RnXw",
			"type": "arrow",
			"x": 572.5421375697752,
			"y": 982.7063751220708,
			"width": 11.538340311125694,
			"height": 26.361839657738074,
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
			"seed": 1249597807,
			"version": 96,
			"versionNonce": 2107532239,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505555,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-11.538340311125694,
					26.361839657738074
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "EwpSFe_JXDWRYznQeDvLo",
				"gap": 27.30163574218784,
				"focus": -0.4258648301559145
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "GG1v9Ez-kBfD96Mn-al3C",
			"type": "rectangle",
			"x": 403.5373956589472,
			"y": 652.9144032796224,
			"width": 321.90917968749994,
			"height": 67.80245535714278,
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
			"seed": 260923279,
			"version": 214,
			"versionNonce": 2093335713,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "UpMR0sSl"
				}
			],
			"updated": 1704468505555,
			"link": null,
			"locked": false
		},
		{
			"id": "UpMR0sSl",
			"type": "text",
			"x": 408.5373956589472,
			"y": 657.9144032796224,
			"width": 304,
			"height": 57.599999999999994,
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
			"seed": 807777103,
			"version": 236,
			"versionNonce": 1860112879,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468505556,
			"link": null,
			"locked": false,
			"text": "1、状态为shutdown并且队列为空\n2、状态为stop及以上（不再处理任务了）\n这里写的有点绕。。。感觉可以换一种写法",
			"rawText": "1、状态为shutdown并且队列为空\n2、状态为stop及以上（不再处理任务了）\n这里写的有点绕。。。感觉可以换一种写法",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 54,
			"containerId": "GG1v9Ez-kBfD96Mn-al3C",
			"originalText": "1、状态为shutdown并且队列为空\n2、状态为stop及以上（不再处理任务了）\n这里写的有点绕。。。感觉可以换一种写法",
			"lineHeight": 1.2
		},
		{
			"id": "26skqW70UMstnFLxsRvah",
			"type": "rectangle",
			"x": -32.77096557617324,
			"y": -253.22382972354217,
			"width": 162.22621372767856,
			"height": 45.09105864025304,
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
			"seed": 1974276257,
			"version": 79,
			"versionNonce": 127042639,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "DnF7lZC2"
				}
			],
			"updated": 1704468510800,
			"link": null,
			"locked": false
		},
		{
			"id": "DnF7lZC2",
			"type": "text",
			"x": -27.77096557617324,
			"y": -248.22382972354217,
			"width": 116.375,
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
			"seed": 1754078273,
			"version": 44,
			"versionNonce": 1376263841,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468540001,
			"link": null,
			"locked": false,
			"text": "处理firstTask",
			"rawText": "处理firstTask",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": "26skqW70UMstnFLxsRvah",
			"originalText": "处理firstTask",
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
			"version": 17,
			"versionNonce": 1686343937,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704468505555,
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
		"scrollX": 378.8282688685839,
		"scrollY": 493.0705588204539,
		"zoom": {
			"value": 1.05
		},
		"currentItemRoundness": "sharp",
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