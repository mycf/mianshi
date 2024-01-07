---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
    public ConcurrentHashMap() {
        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR, DEFAULT_CONCURRENCY_LEVEL);
    }

    public ConcurrentHashMap(int initialCapacity,
                             float loadFactor, int concurrencyLevel) {
        if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)
            throw new IllegalArgumentException();
        if (concurrencyLevel > MAX_SEGMENTS)
            concurrencyLevel = MAX_SEGMENTS;
        // Find power-of-two sizes best matching arguments
        int sshift = 0;
        int ssize = 1;
        while (ssize < concurrencyLevel) {
            ++sshift;
            ssize <<= 1;
        }
        this.segmentShift = 32 - sshift;
        this.segmentMask = ssize - 1;
        if (initialCapacity > MAXIMUM_CAPACITY)
            initialCapacity = MAXIMUM_CAPACITY;
        int c = initialCapacity / ssize;
        if (c * ssize < initialCapacity)
            ++c;
        int cap = MIN_SEGMENT_TABLE_CAPACITY;
        while (cap < c)
            cap <<= 1;
        // create segments and segments[0]
        Segment<K,V> s0 =
            new Segment<K,V>(loadFactor, (int)(cap * loadFactor),
                             (HashEntry<K,V>[])new HashEntry[cap]);
        Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];
        UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]
        this.segments = ss;
    }
 ^Dj7sTXPf

segment的长度 ^7sQfTwf4

找到大于等于并发级别2的次幂 ^8C6dpZ93

因为segment的下标
也是&hash得到的，所以需要2的次幂 ^URdbFWep

不能超过最大值 ^Rm4xsPJ2

假设初始cap为17，ssize为16，
每个segment里面要存放两个 ^zEDgNGbU

获取每个segment
存放的数量 ^SQ9SgDHc

这里为了往上取整等同于
Math.ceil(initialCapacity%ssize) ^H0iMCPFK

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "Dj7sTXPf",
			"type": "text",
			"x": -319.6957702636719,
			"y": -325.3351165771484,
			"width": 815.625,
			"height": 691.1999999999999,
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
			"seed": 1908817551,
			"version": 54,
			"versionNonce": 1905593039,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "    public ConcurrentHashMap() {\n        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR, DEFAULT_CONCURRENCY_LEVEL);\n    }\n\n    public ConcurrentHashMap(int initialCapacity,\n                             float loadFactor, int concurrencyLevel) {\n        if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)\n            throw new IllegalArgumentException();\n        if (concurrencyLevel > MAX_SEGMENTS)\n            concurrencyLevel = MAX_SEGMENTS;\n        // Find power-of-two sizes best matching arguments\n        int sshift = 0;\n        int ssize = 1;\n        while (ssize < concurrencyLevel) {\n            ++sshift;\n            ssize <<= 1;\n        }\n        this.segmentShift = 32 - sshift;\n        this.segmentMask = ssize - 1;\n        if (initialCapacity > MAXIMUM_CAPACITY)\n            initialCapacity = MAXIMUM_CAPACITY;\n        int c = initialCapacity / ssize;\n        if (c * ssize < initialCapacity)\n            ++c;\n        int cap = MIN_SEGMENT_TABLE_CAPACITY;\n        while (cap < c)\n            cap <<= 1;\n        // create segments and segments[0]\n        Segment<K,V> s0 =\n            new Segment<K,V>(loadFactor, (int)(cap * loadFactor),\n                             (HashEntry<K,V>[])new HashEntry[cap]);\n        Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];\n        UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]\n        this.segments = ss;\n    }\n",
			"rawText": "    public ConcurrentHashMap() {\n        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR, DEFAULT_CONCURRENCY_LEVEL);\n    }\n\n    public ConcurrentHashMap(int initialCapacity,\n                             float loadFactor, int concurrencyLevel) {\n        if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)\n            throw new IllegalArgumentException();\n        if (concurrencyLevel > MAX_SEGMENTS)\n            concurrencyLevel = MAX_SEGMENTS;\n        // Find power-of-two sizes best matching arguments\n        int sshift = 0;\n        int ssize = 1;\n        while (ssize < concurrencyLevel) {\n            ++sshift;\n            ssize <<= 1;\n        }\n        this.segmentShift = 32 - sshift;\n        this.segmentMask = ssize - 1;\n        if (initialCapacity > MAXIMUM_CAPACITY)\n            initialCapacity = MAXIMUM_CAPACITY;\n        int c = initialCapacity / ssize;\n        if (c * ssize < initialCapacity)\n            ++c;\n        int cap = MIN_SEGMENT_TABLE_CAPACITY;\n        while (cap < c)\n            cap <<= 1;\n        // create segments and segments[0]\n        Segment<K,V> s0 =\n            new Segment<K,V>(loadFactor, (int)(cap * loadFactor),\n                             (HashEntry<K,V>[])new HashEntry[cap]);\n        Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];\n        UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]\n        this.segments = ss;\n    }\n",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 687,
			"containerId": null,
			"originalText": "    public ConcurrentHashMap() {\n        this(DEFAULT_INITIAL_CAPACITY, DEFAULT_LOAD_FACTOR, DEFAULT_CONCURRENCY_LEVEL);\n    }\n\n    public ConcurrentHashMap(int initialCapacity,\n                             float loadFactor, int concurrencyLevel) {\n        if (!(loadFactor > 0) || initialCapacity < 0 || concurrencyLevel <= 0)\n            throw new IllegalArgumentException();\n        if (concurrencyLevel > MAX_SEGMENTS)\n            concurrencyLevel = MAX_SEGMENTS;\n        // Find power-of-two sizes best matching arguments\n        int sshift = 0;\n        int ssize = 1;\n        while (ssize < concurrencyLevel) {\n            ++sshift;\n            ssize <<= 1;\n        }\n        this.segmentShift = 32 - sshift;\n        this.segmentMask = ssize - 1;\n        if (initialCapacity > MAXIMUM_CAPACITY)\n            initialCapacity = MAXIMUM_CAPACITY;\n        int c = initialCapacity / ssize;\n        if (c * ssize < initialCapacity)\n            ++c;\n        int cap = MIN_SEGMENT_TABLE_CAPACITY;\n        while (cap < c)\n            cap <<= 1;\n        // create segments and segments[0]\n        Segment<K,V> s0 =\n            new Segment<K,V>(loadFactor, (int)(cap * loadFactor),\n                             (HashEntry<K,V>[])new HashEntry[cap]);\n        Segment<K,V>[] ss = (Segment<K,V>[])new Segment[ssize];\n        UNSAFE.putOrderedObject(ss, SBASE, s0); // ordered write of segments[0]\n        this.segments = ss;\n    }\n",
			"lineHeight": 1.2
		},
		{
			"id": "WkJUqlEh363c346WeP1jG",
			"type": "arrow",
			"x": 255.54451751708984,
			"y": 281.4114017486572,
			"width": 347.4748229980469,
			"height": 306.6098403930664,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1070260961,
			"version": 211,
			"versionNonce": 1714490785,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-33.336029052734375,
					-250.0777816772461
				],
				[
					-347.4748229980469,
					-306.6098403930664
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "G7kV5THU_PKLlNNf_l2Dx",
			"type": "arrow",
			"x": -96.43309783935547,
			"y": -30.640581130981445,
			"width": 40.81840515136719,
			"height": 55.02246856689453,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1229959425,
			"version": 142,
			"versionNonce": 459856111,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					20.584793090820312,
					-49.40591812133789
				],
				[
					-20.233612060546875,
					-55.02246856689453
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "7sQfTwf4",
			"type": "text",
			"x": -73.25606536865234,
			"y": -93.44522285461426,
			"width": 113.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1996549857,
			"version": 114,
			"versionNonce": 1840306529,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "segment的长度",
			"rawText": "segment的长度",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "segment的长度",
			"lineHeight": 1.2
		},
		{
			"id": "77qSwD1KpMjvoh2BVT6yz",
			"type": "rectangle",
			"x": 92.63809967041016,
			"y": 272.31540870666504,
			"width": 183.14476013183594,
			"height": 16.890792846679688,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1786801103,
			"version": 114,
			"versionNonce": 627878191,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false
		},
		{
			"id": "AyUoC2fyVcmNDSOkaVEBz",
			"type": "arrow",
			"x": 80.86006927490234,
			"y": -85.11447715759277,
			"width": 1.102294921875,
			"height": 75.28579711914062,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 152802017,
			"version": 198,
			"versionNonce": 871433537,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					1.102294921875,
					75.28579711914062
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "8C6dpZ93",
			"type": "text",
			"x": -38.439857482910156,
			"y": -54.68118476867676,
			"width": 157.44680867095812,
			"height": 13.90674514770508,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1104193263,
			"version": 259,
			"versionNonce": 1023456545,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "找到大于等于并发级别2的次幂",
			"rawText": "找到大于等于并发级别2的次幂",
			"fontSize": 11.588954289754252,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 10.999999999999977,
			"containerId": null,
			"originalText": "找到大于等于并发级别2的次幂",
			"lineHeight": 1.2
		},
		{
			"id": "URdbFWep",
			"type": "text",
			"x": 122.3954849243164,
			"y": -62.235544204711914,
			"width": 202.31640625,
			"height": 29.423111562214096,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 294726529,
			"version": 439,
			"versionNonce": 409561455,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "因为segment的下标\n也是&hash得到的，所以需要2的次幂",
			"rawText": "因为segment的下标\n也是&hash得到的，所以需要2的次幂",
			"fontSize": 12.259629817589207,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 27,
			"containerId": null,
			"originalText": "因为segment的下标\n也是&hash得到的，所以需要2的次幂",
			"lineHeight": 1.2
		},
		{
			"id": "Rm4xsPJ2",
			"type": "text",
			"x": 123.0653073252621,
			"y": 40.36542846032813,
			"width": 88.8621303778732,
			"height": 15.233508064778267,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 703359553,
			"version": 143,
			"versionNonce": 206530817,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "不能超过最大值",
			"rawText": "不能超过最大值",
			"fontSize": 12.69459005398188,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 12.000000000000009,
			"containerId": null,
			"originalText": "不能超过最大值",
			"lineHeight": 1.2
		},
		{
			"id": "zEDgNGbU",
			"type": "text",
			"x": 175.97800008842972,
			"y": 91.40991236663308,
			"width": 144.9243369787348,
			"height": 23.140531796997315,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 766999727,
			"version": 709,
			"versionNonce": 2061279983,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704625446640,
			"link": null,
			"locked": false,
			"text": "假设初始cap为17，ssize为16，\n每个segment里面要存放两个",
			"rawText": "假设初始cap为17，ssize为16，\n每个segment里面要存放两个",
			"fontSize": 9.641888248748877,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 21.00000000000001,
			"containerId": null,
			"originalText": "假设初始cap为17，ssize为16，\n每个segment里面要存放两个",
			"lineHeight": 1.2
		},
		{
			"id": "SQ9SgDHc",
			"type": "text",
			"x": 58.45331307720335,
			"y": 76.83378683995659,
			"width": 79.07122802734375,
			"height": 23.431640582785715,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 662441729,
			"version": 353,
			"versionNonce": 1203231937,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "获取每个segment\n存放的数量",
			"rawText": "获取每个segment\n存放的数量",
			"fontSize": 9.763183576160715,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 21,
			"containerId": null,
			"originalText": "获取每个segment\n存放的数量",
			"lineHeight": 1.2
		},
		{
			"id": "H0iMCPFK",
			"type": "text",
			"x": 59.60035836779468,
			"y": 103.60523789196299,
			"width": 186.37451171875,
			"height": 23.871949911925444,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1517687535,
			"version": 256,
			"versionNonce": 366087119,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704625442895,
			"link": null,
			"locked": false,
			"text": "这里为了往上取整等同于\nMath.ceil(initialCapacity%ssize)",
			"rawText": "这里为了往上取整等同于\nMath.ceil(initialCapacity%ssize)",
			"fontSize": 9.946645796635602,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 21,
			"containerId": null,
			"originalText": "这里为了往上取整等同于\nMath.ceil(initialCapacity%ssize)",
			"lineHeight": 1.2
		},
		{
			"id": "S6kJ75R0ct6c5kCFbre93",
			"type": "arrow",
			"x": -71.93946075439453,
			"y": -93.12152671813965,
			"width": 22.215347290039062,
			"height": 18.183517456054688,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1754066063,
			"version": 31,
			"versionNonce": 1323348353,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					22.215347290039062,
					18.183517456054688
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "Oq8-r7noY8sjqqVJfkqiA",
			"type": "rectangle",
			"x": -68.39423370361328,
			"y": -91.56368064880371,
			"width": 41.013336181640625,
			"height": 19.916839599609375,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 462036065,
			"version": 42,
			"versionNonce": 299754255,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false
		},
		{
			"id": "N63grWFp",
			"type": "text",
			"x": 76.72371673583984,
			"y": -57.07157859802246,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 197238977,
			"version": 53,
			"versionNonce": 58567503,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "AyUoC2fyVcmNDSOkaVEBz",
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "CyKD-3z8-Platl1woEQiO",
			"type": "rectangle",
			"x": 67.0107992790297,
			"y": 83.17667983298367,
			"width": 60.93496259633474,
			"height": 44.26944501031551,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"seed": 1593352335,
			"version": 43,
			"versionNonce": 829973391,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false
		},
		{
			"id": "Xu0dHR4vOzQwbzoFRzDRO",
			"type": "arrow",
			"x": 180.03639345515006,
			"y": 97.21251995170405,
			"width": 36.03775724438509,
			"height": 12.876667719274906,
			"angle": 0,
			"strokeColor": "#2f9e44",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 2101082561,
			"version": 701,
			"versionNonce": 1591941551,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704625442894,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-36.03775724438509,
					12.876667719274906
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "zEDgNGbU",
				"focus": 0.9154812370312059,
				"gap": 1.1974472016459003
			},
			"endBinding": {
				"elementId": "H0iMCPFK",
				"focus": 0.8153024835041033,
				"gap": 4.825111469885485
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#2f9e44",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "dashed",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 333.12119449402064,
		"scrollY": 243.87020414627898,
		"zoom": {
			"value": 1.4810151942914025
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