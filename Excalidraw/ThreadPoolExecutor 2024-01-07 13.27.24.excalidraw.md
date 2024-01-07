---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
    private void processWorkerExit(Worker w, boolean completedAbruptly) {
        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted
            decrementWorkerCount();

        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            completedTaskCount += w.completedTasks;
            workers.remove(w);
        } finally {
            mainLock.unlock();
        }

        tryTerminate();

        int c = ctl.get();
        if (runStateLessThan(c, STOP)) {
            if (!completedAbruptly) {
                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                if (min == 0 && ! workQueue.isEmpty())
                    min = 1;
                if (workerCountOf(c) >= min)
                    return; // replacement not needed
            }
            addWorker(null, false);
        }
    }

    final void tryTerminate() {
        for (;;) {
            int c = ctl.get();
            if (isRunning(c) ||
                runStateAtLeast(c, TIDYING) ||
                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))
                return;
            if (workerCountOf(c) != 0) { // Eligible to terminate
                interruptIdleWorkers(ONLY_ONE);
                return;
            }

            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {
                    try {
                        terminated();
                    } finally {
                        ctl.set(ctlOf(TERMINATED, 0));
                        termination.signalAll();
                    }
                    return;
                }
            } finally {
                mainLock.unlock();
            }
            // else retry on failed CAS
        }
    }

 ^vDmZxDyM

意外终止 ^xFRrxMvH

队列中存在任务时，最小线程数为1 ^Fij6yKve

超过最小线程数，可以销毁 ^iFk0LSXg

不足最小线程数，重新生成工作线程，代替本任务继续运行 ^AdIpuA16

running或shutdown状态 ^KskBA1hq

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "vDmZxDyM",
			"type": "text",
			"x": -174.69577026367188,
			"y": -216.93511657714845,
			"width": 721.875,
			"height": 1152,
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
			"seed": 397017633,
			"version": 12,
			"versionNonce": 892405825,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610794523,
			"link": null,
			"locked": false,
			"text": "    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n",
			"rawText": "    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 1148,
			"containerId": null,
			"originalText": "    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n",
			"lineHeight": 1.2
		},
		{
			"id": "AxC2fD6xKpZJZ9nDc6Zhh",
			"type": "rectangle",
			"x": 271.6925354003906,
			"y": -245.8647003173828,
			"width": 80.0880126953125,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1294524737,
			"version": 121,
			"versionNonce": 1541347649,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "xFRrxMvH"
				}
			],
			"updated": 1704610791332,
			"link": null,
			"locked": false
		},
		{
			"id": "xFRrxMvH",
			"type": "text",
			"x": 279.7365417480469,
			"y": -240.8647003173828,
			"width": 64,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 192402799,
			"version": 73,
			"versionNonce": 673896271,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "意外终止",
			"rawText": "意外终止",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "AxC2fD6xKpZJZ9nDc6Zhh",
			"originalText": "意外终止",
			"lineHeight": 1.2
		},
		{
			"id": "-vgd3o5KulgWJqn8qpWUW",
			"type": "rectangle",
			"x": 339.4496154785156,
			"y": 149.48793029785156,
			"width": 139.2623291015625,
			"height": 49,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 368191567,
			"version": 315,
			"versionNonce": 110171503,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Fij6yKve"
				}
			],
			"updated": 1704610791332,
			"link": null,
			"locked": false
		},
		{
			"id": "Fij6yKve",
			"type": "text",
			"x": 345.0807800292969,
			"y": 154.78793029785157,
			"width": 128,
			"height": 38.4,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 988397231,
			"version": 423,
			"versionNonce": 919108865,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "队列中存在任务时\n，最小线程数为1",
			"rawText": "队列中存在任务时，最小线程数为1",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "-vgd3o5KulgWJqn8qpWUW",
			"originalText": "队列中存在任务时，最小线程数为1",
			"lineHeight": 1.2
		},
		{
			"id": "6ngkcdn3BAGFzKqcKn_2l",
			"type": "rectangle",
			"x": 326.6816711425781,
			"y": 202.3399200439453,
			"width": 202.18750000000003,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 2130485089,
			"version": 223,
			"versionNonce": 1439338383,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "iFk0LSXg"
				}
			],
			"updated": 1704610791332,
			"link": null,
			"locked": false
		},
		{
			"id": "iFk0LSXg",
			"type": "text",
			"x": 331.7754211425781,
			"y": 207.3399200439453,
			"width": 192,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1271293697,
			"version": 187,
			"versionNonce": 764622049,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "超过最小线程数，可以销毁",
			"rawText": "超过最小线程数，可以销毁",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "6ngkcdn3BAGFzKqcKn_2l",
			"originalText": "超过最小线程数，可以销毁",
			"lineHeight": 1.2
		},
		{
			"id": "lQbaXtxvtYJPM0s3XDbYL",
			"type": "rectangle",
			"x": 157.88955688476562,
			"y": 238.71107482910156,
			"width": 269.406494140625,
			"height": 49,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1186558351,
			"version": 380,
			"versionNonce": 269469103,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "AdIpuA16"
				}
			],
			"updated": 1704610791332,
			"link": null,
			"locked": false
		},
		{
			"id": "AdIpuA16",
			"type": "text",
			"x": 164.59280395507812,
			"y": 244.01107482910157,
			"width": 256,
			"height": 38.4,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1417671919,
			"version": 540,
			"versionNonce": 234614977,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "不足最小线程数，重新生成工作线程\n，代替本任务继续运行",
			"rawText": "不足最小线程数，重新生成工作线程，代替本任务继续运行",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "lQbaXtxvtYJPM0s3XDbYL",
			"originalText": "不足最小线程数，重新生成工作线程，代替本任务继续运行",
			"lineHeight": 1.2
		},
		{
			"id": "Zj4pIBzbSmDn7uCigcrrR",
			"type": "rectangle",
			"x": 206.86331176757812,
			"y": 87.51502990722656,
			"width": 201.99096679687503,
			"height": 29.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1454433679,
			"version": 143,
			"versionNonce": 1418278863,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "KskBA1hq"
				}
			],
			"updated": 1704610791332,
			"link": null,
			"locked": false
		},
		{
			"id": "KskBA1hq",
			"type": "text",
			"x": 213.54629516601562,
			"y": 92.51502990722656,
			"width": 188.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 504160495,
			"version": 165,
			"versionNonce": 2107781281,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610791333,
			"link": null,
			"locked": false,
			"text": "running或shutdown状态",
			"rawText": "running或shutdown状态",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "Zj4pIBzbSmDn7uCigcrrR",
			"originalText": "running或shutdown状态",
			"lineHeight": 1.2
		},
		{
			"id": "9S51tvL9dj-akB0u_dveJ",
			"type": "arrow",
			"x": -96.94784545898438,
			"y": 45.40235900878906,
			"width": 42.4056396484375,
			"height": 292.855224609375,
			"angle": 0,
			"strokeColor": "#2f9e44",
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
			"seed": 1667129281,
			"version": 64,
			"versionNonce": 556062863,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704610816921,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-42.4056396484375,
					292.855224609375
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "mTM91wNl",
			"type": "text",
			"x": -307.6957702636719,
			"y": -287.13511657714844,
			"width": 10,
			"height": 25,
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
			"seed": 299916271,
			"version": 3,
			"versionNonce": 1590783329,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 20,
			"fontFamily": 1,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 18,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.25
		},
		{
			"id": "t2mzqnFu",
			"type": "text",
			"x": 337.0027160644531,
			"y": -240.48899841308594,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 533709249,
			"version": 3,
			"versionNonce": 1712081185,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704610791332,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "AxC2fD6xKpZJZ9nDc6Zhh",
			"originalText": "",
			"lineHeight": 1.2
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#2f9e44",
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
		"scrollX": 479.2371520996094,
		"scrollY": 309.08221435546875,
		"zoom": {
			"value": 1
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