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

除了第一次firstTask，后面都getTask了 ^FhiVBt3t

未获取到task，线程就回收了 ^Hrw6YL2a

不用回收的线程，会使用blockqueue阻塞在这 ^GQT0GLdJ

1、至少stop的状态+当前线程非中断
2、清除当前线程中断成功并确保状态至少stop状态+非中断 ^gYHVFgBW

重复判断了至少stop状态防止清除状态的时候调用了shutdowNow进入了stop状态 ^2Q8jSgLD

确保stop状态下是中断状态 ^dVQ2PDTs

shutdownNow会触发中断 ^aRBtUR7S

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
			"x": -240.818257539807,
			"y": -399.09220912884774,
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
			"version": 254,
			"versionNonce": 293829871,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704514804450,
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
			"version": 142,
			"versionNonce": 946067777,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "9h9DVq4lCWG7_Vl8fUIBF",
					"type": "arrow"
				}
			],
			"updated": 1704468568364,
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
			"version": 41,
			"versionNonce": 1397888847,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "_dXLwFeSmFSkhBEvZ1Tj_",
					"type": "arrow"
				}
			],
			"updated": 1704468568364,
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
			"version": 60,
			"versionNonce": 1408829729,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
			"link": null,
			"locked": false
		},
		{
			"id": "f5jJ4bcpTXWh5kcfJr1Xw",
			"type": "arrow",
			"x": 321.6104285830546,
			"y": -201.518769037156,
			"width": 713.3103143601184,
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
			"version": 229,
			"versionNonce": 231843681,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Hrw6YL2a"
				}
			],
			"updated": 1704512354332,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					435.22553943452317,
					222.5315929594468
				],
				[
					-278.0847749255952,
					739.9711390904017
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": "arrow",
			"endArrowhead": "arrow"
		},
		{
			"id": "Hrw6YL2a",
			"type": "text",
			"x": 650.0859680175778,
			"y": 11.412823922290807,
			"width": 213.5,
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
			"seed": 686576449,
			"version": 36,
			"versionNonce": 289105569,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468655167,
			"link": null,
			"locked": false,
			"text": "未获取到task，线程就回收了",
			"rawText": "未获取到task，线程就回收了",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "f5jJ4bcpTXWh5kcfJr1Xw",
			"originalText": "未获取到task，线程就回收了",
			"lineHeight": 1.2
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
			"version": 113,
			"versionNonce": 1226652929,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "1Dle7lSO"
				}
			],
			"updated": 1704468568364,
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
			"version": 41,
			"versionNonce": 1344393103,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 508,
			"versionNonce": 391116001,
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
			"updated": 1704468568364,
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
			"version": 643,
			"versionNonce": 1031734703,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 598,
			"versionNonce": 572567745,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 94,
			"versionNonce": 303069135,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "SPYEAGem"
				}
			],
			"updated": 1704468568364,
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
			"version": 143,
			"versionNonce": 791605409,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 25,
			"versionNonce": 1408148975,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 176,
			"versionNonce": 606745729,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "OvNtRpCU"
				}
			],
			"updated": 1704468568364,
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
			"version": 138,
			"versionNonce": 841093135,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 71,
			"versionNonce": 80870497,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 228,
			"versionNonce": 1721972271,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Gn8kPbfq"
				}
			],
			"updated": 1704468568364,
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
			"version": 62,
			"versionNonce": 821413953,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 58,
			"versionNonce": 1651841057,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 81,
			"versionNonce": 2099295855,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 508,
			"versionNonce": 1799318529,
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
			"updated": 1704468568364,
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
			"version": 1017,
			"versionNonce": 2076053647,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 97,
			"versionNonce": 861160417,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568364,
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
			"version": 215,
			"versionNonce": 233514671,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "UpMR0sSl"
				}
			],
			"updated": 1704468568364,
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
			"version": 237,
			"versionNonce": 1992499137,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568365,
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
			"version": 80,
			"versionNonce": 575699151,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "DnF7lZC2"
				}
			],
			"updated": 1704468568365,
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
			"version": 45,
			"versionNonce": 1135312801,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468568365,
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
			"id": "rg5v3LXQOY8IWTrTRJvbp",
			"type": "rectangle",
			"x": -13.798332577661313,
			"y": 276.0039302280952,
			"width": 161.45484561011904,
			"height": 23.042747860863066,
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
			"seed": 555077903,
			"version": 105,
			"versionNonce": 1786047873,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "9T5J_Rpem9QWpIaawKaLO",
					"type": "arrow"
				}
			],
			"updated": 1704468590662,
			"link": null,
			"locked": false
		},
		{
			"id": "9T5J_Rpem9QWpIaawKaLO",
			"type": "arrow",
			"x": -14.209070114863664,
			"y": 290.66498166038684,
			"width": 192.01561337425596,
			"height": 508.1532796223958,
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
			"seed": 1812661295,
			"version": 107,
			"versionNonce": 708413423,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "FhiVBt3t"
				}
			],
			"updated": 1704468624898,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-192.01561337425596,
					-253.8555036272321
				],
				[
					-87.3440406436012,
					-508.1532796223958
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "rg5v3LXQOY8IWTrTRJvbp",
				"focus": -0.9337096179660197,
				"gap": 1
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "FhiVBt3t",
			"type": "text",
			"x": -288.4121834891196,
			"y": 17.609478033154733,
			"width": 164.375,
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
			"seed": 236907407,
			"version": 49,
			"versionNonce": 9411023,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468623930,
			"link": null,
			"locked": false,
			"text": "除了第一次firstTask\n，后面都getTask了",
			"rawText": "除了第一次firstTask，后面都getTask了",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "9T5J_Rpem9QWpIaawKaLO",
			"originalText": "除了第一次firstTask，后面都getTask了",
			"lineHeight": 1.2
		},
		{
			"id": "3qxgcFYpSNZYS8bcqslo4",
			"type": "rectangle",
			"x": 266.8575919015057,
			"y": -309.020890735447,
			"width": 154.22828311011904,
			"height": 68,
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
			"seed": 143757487,
			"version": 136,
			"versionNonce": 1094041711,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "GQT0GLdJ"
				}
			],
			"updated": 1704468736513,
			"link": null,
			"locked": false
		},
		{
			"id": "GQT0GLdJ",
			"type": "text",
			"x": 271.97173345656523,
			"y": -303.820890735447,
			"width": 144,
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
			"seed": 1221817825,
			"version": 113,
			"versionNonce": 1285675663,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704468736513,
			"link": null,
			"locked": false,
			"text": "不用回收的线程，会\n使用blockqueue阻\n塞在这",
			"rawText": "不用回收的线程，会使用blockqueue阻塞在这",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 54,
			"containerId": "3qxgcFYpSNZYS8bcqslo4",
			"originalText": "不用回收的线程，会使用blockqueue阻塞在这",
			"lineHeight": 1.2
		},
		{
			"id": "4SpYSMDe9lTZl-MV6p1C4",
			"type": "rectangle",
			"x": 463.52263096400577,
			"y": -76.40361604236625,
			"width": 239.93524460565482,
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
			"seed": 810140367,
			"version": 51,
			"versionNonce": 979040335,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "gYHVFgBW"
				}
			],
			"updated": 1704512235690,
			"link": null,
			"locked": false
		},
		{
			"id": "gYHVFgBW",
			"type": "text",
			"x": 468.52263096400577,
			"y": -71.30361604236626,
			"width": 217.375,
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
			"seed": 1584322337,
			"version": 156,
			"versionNonce": 984474657,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704513087063,
			"link": null,
			"locked": false,
			"text": "1、至少stop的状态+当前线程\n非中断\n2、清除当前线程中断成功并确\n保状态至少stop状态+非中断",
			"rawText": "1、至少stop的状态+当前线程非中断\n2、清除当前线程中断成功并确保状态至少stop状态+非中断",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 73,
			"containerId": "4SpYSMDe9lTZl-MV6p1C4",
			"originalText": "1、至少stop的状态+当前线程非中断\n2、清除当前线程中断成功并确保状态至少stop状态+非中断",
			"lineHeight": 1.2
		},
		{
			"id": "pL_KibyXWDNHFT2Ob0knR",
			"type": "rectangle",
			"x": 681.2403955373828,
			"y": -128.4142809095493,
			"width": 176.08596075148807,
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
			"seed": 199654735,
			"version": 68,
			"versionNonce": 1565855375,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "2Q8jSgLD"
				}
			],
			"updated": 1704513325841,
			"link": null,
			"locked": false
		},
		{
			"id": "2Q8jSgLD",
			"type": "text",
			"x": 686.5333759131269,
			"y": -123.3142809095493,
			"width": 165.5,
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
			"seed": 1743643137,
			"version": 113,
			"versionNonce": 1609721007,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704513325841,
			"link": null,
			"locked": false,
			"text": "重复判断了至少stop状\n态防止清除状态的时候\n调用了shutdowNow进\n入了stop状态",
			"rawText": "重复判断了至少stop状态防止清除状态的时候调用了shutdowNow进入了stop状态",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 73,
			"containerId": "pL_KibyXWDNHFT2Ob0knR",
			"originalText": "重复判断了至少stop状态防止清除状态的时候调用了shutdowNow进入了stop状态",
			"lineHeight": 1.2
		},
		{
			"id": "dkM04gF89L8hF8_szKMiH",
			"type": "rectangle",
			"x": -174.48447728640105,
			"y": -90.45762041643002,
			"width": 124.2606811966574,
			"height": 51.52993019686966,
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
			"seed": 1247167727,
			"version": 116,
			"versionNonce": 601893185,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "dVQ2PDTs"
				}
			],
			"updated": 1704514717933,
			"link": null,
			"locked": false
		},
		{
			"id": "dVQ2PDTs",
			"type": "text",
			"x": -169.48447728640105,
			"y": -83.89265531799519,
			"width": 101.5,
			"height": 38.4,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 734995617,
			"version": 103,
			"versionNonce": 931714433,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704514717869,
			"link": null,
			"locked": false,
			"text": "确保stop状态\n下是中断状态",
			"rawText": "确保stop状态下是中断状态",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "dkM04gF89L8hF8_szKMiH",
			"originalText": "确保stop状态下是中断状态",
			"lineHeight": 1.2
		},
		{
			"id": "aYGklveSOi1pUSz4V9STM",
			"type": "rectangle",
			"x": 355.9916917927753,
			"y": 1297.0252311958143,
			"width": 236.1920270725022,
			"height": 33.55381408671565,
			"angle": 0,
			"strokeColor": "#1971c2",
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
			"seed": 1767752001,
			"version": 69,
			"versionNonce": 1330604673,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "aRBtUR7S"
				}
			],
			"updated": 1704514573218,
			"link": null,
			"locked": false
		},
		{
			"id": "aRBtUR7S",
			"type": "text",
			"x": 382.52520532902645,
			"y": 1304.2021382391722,
			"width": 183.125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#1971c2",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 310340495,
			"version": 75,
			"versionNonce": 1105228385,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704514573218,
			"link": null,
			"locked": false,
			"text": "shutdownNow会触发中断",
			"rawText": "shutdownNow会触发中断",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "aYGklveSOi1pUSz4V9STM",
			"originalText": "shutdownNow会触发中断",
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
			"version": 18,
			"versionNonce": 1482332239,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704468568364,
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
		},
		{
			"id": "2x0pzcbmS3AvmNpXQBoUG",
			"type": "arrow",
			"x": -33.89575631278035,
			"y": -222.95242672875008,
			"width": 18.860212053571445,
			"height": 524.0482584635416,
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
			"seed": 1773644385,
			"version": 106,
			"versionNonce": 1049003759,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704468568365,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					18.860212053571445,
					524.0482584635416
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
		"currentItemStrokeColor": "#1971c2",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": "arrow",
		"currentItemEndArrowhead": "arrow",
		"scrollX": 486.4971168107537,
		"scrollY": 421.7717407797357,
		"zoom": {
			"value": 0.8939533477795967
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