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

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "h9WpijYMQd0SZk3CEezC-",
			"type": "rectangle",
			"x": -6.612808954147965,
			"y": 919.739717755999,
			"width": 66.95021856398807,
			"height": 30.651884533110206,
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
			"seed": 1293283375,
			"version": 38,
			"versionNonce": 2074089103,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "_dXLwFeSmFSkhBEvZ1Tj_",
					"type": "arrow"
				}
			],
			"updated": 1704466598956,
			"link": null,
			"locked": false
		},
		{
			"id": "r7BTOJwb",
			"type": "text",
			"x": -238.32702491396958,
			"y": -398.14168512253536,
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
			"version": 65,
			"versionNonce": 339180673,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704466436356,
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
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "#fcfcfc",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 329.30445934477297,
		"scrollY": -516.4532507033576,
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