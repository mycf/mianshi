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
			"version": 35,
			"versionNonce": 1722047823,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 209,
			"versionNonce": 173417249,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 140,
			"versionNonce": 132766575,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 112,
			"versionNonce": 1487834849,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 112,
			"versionNonce": 980356015,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624175130,
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
			"version": 196,
			"versionNonce": 815532737,
			"isDeleted": false,
			"boundElements": [],
			"updated": 1704624175130,
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
			"version": 257,
			"versionNonce": 1535019151,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624202874,
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
			"version": 437,
			"versionNonce": 503464879,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704624204544,
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
			"version": 29,
			"versionNonce": 684136193,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 40,
			"versionNonce": 2082117007,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704624175129,
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
			"version": 51,
			"versionNonce": 1057996239,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704624175130,
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
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#2f9e44",
		"currentItemBackgroundColor": "transparent",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 1,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 1,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "triangle",
		"scrollX": 396.7697067260742,
		"scrollY": 251.3115005493164,
		"zoom": {
			"value": 1.6
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