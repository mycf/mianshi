---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
    public V put(K key, V value) {
        Segment<K,V> s;
        if (value == null)
            throw new NullPointerException();
        int hash = hash(key);
        int j = (hash >>> segmentShift) & segmentMask;
        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck
             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment
            s = ensureSegment(j);
        return s.put(key, hash, value, false);
    }
    private Segment<K,V> ensureSegment(int k) {
        final Segment<K,V>[] ss = this.segments;
        long u = (k << SSHIFT) + SBASE; // raw offset
        Segment<K,V> seg;
        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {
            Segment<K,V> proto = ss[0]; // use segment 0 as prototype
            int cap = proto.table.length;
            float lf = proto.loadFactor;
            int threshold = (int)(cap * lf);
            HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];
            if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))
                == null) { // recheck
                Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);
                while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))
                       == null) {
                    if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))
                        break;
                }
            }
        }
        return seg;
    }

        final V put(K key, int hash, V value, boolean onlyIfAbsent) {
            HashEntry<K,V> node = tryLock() ? null :
                scanAndLockForPut(key, hash, value);
            V oldValue;
            try {
                HashEntry<K,V>[] tab = table;
                int index = (tab.length - 1) & hash;
                HashEntry<K,V> first = entryAt(tab, index);
                for (HashEntry<K,V> e = first;;) {
                    if (e != null) {
                        K k;
                        if ((k = e.key) == key ||
                            (e.hash == hash && key.equals(k))) {
                            oldValue = e.value;
                            if (!onlyIfAbsent) {
                                e.value = value;
                                ++modCount;
                            }
                            break;
                        }
                        e = e.next;
                    }
                    else {
                        if (node != null)
                            node.setNext(first);
                        else
                            node = new HashEntry<K,V>(hash, key, value, first);
                        int c = count + 1;
                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)
                            rehash(node);
                        else
                            setEntryAt(tab, index, node);
                        ++modCount;
                        count = c;
                        oldValue = null;
                        break;
                    }
                }
            } finally {
                unlock();
            }
            return oldValue;
        }
 ^hMZHCo9O

获取segment[]的下标 ^xrKUlGfD

指定下标为null，创建一个 ^w52cY6sK

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
			"version": 11,
			"versionNonce": 1165204559,
			"isDeleted": false,
			"id": "hMZHCo9O",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 142.45584869384766,
			"y": -234.13511657714844,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 937.5,
			"height": 1872,
			"seed": 290355311,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704633331503,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n    private Segment<K,V> ensureSegment(int k) {\n        final Segment<K,V>[] ss = this.segments;\n        long u = (k << SSHIFT) + SBASE; // raw offset\n        Segment<K,V> seg;\n        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {\n            Segment<K,V> proto = ss[0]; // use segment 0 as prototype\n            int cap = proto.table.length;\n            float lf = proto.loadFactor;\n            int threshold = (int)(cap * lf);\n            HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];\n            if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                == null) { // recheck\n                Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);\n                while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                       == null) {\n                    if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))\n                        break;\n                }\n            }\n        }\n        return seg;\n    }\n\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"rawText": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n    private Segment<K,V> ensureSegment(int k) {\n        final Segment<K,V>[] ss = this.segments;\n        long u = (k << SSHIFT) + SBASE; // raw offset\n        Segment<K,V> seg;\n        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {\n            Segment<K,V> proto = ss[0]; // use segment 0 as prototype\n            int cap = proto.table.length;\n            float lf = proto.loadFactor;\n            int threshold = (int)(cap * lf);\n            HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];\n            if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                == null) { // recheck\n                Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);\n                while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                       == null) {\n                    if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))\n                        break;\n                }\n            }\n        }\n        return seg;\n    }\n\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n    private Segment<K,V> ensureSegment(int k) {\n        final Segment<K,V>[] ss = this.segments;\n        long u = (k << SSHIFT) + SBASE; // raw offset\n        Segment<K,V> seg;\n        if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u)) == null) {\n            Segment<K,V> proto = ss[0]; // use segment 0 as prototype\n            int cap = proto.table.length;\n            float lf = proto.loadFactor;\n            int threshold = (int)(cap * lf);\n            HashEntry<K,V>[] tab = (HashEntry<K,V>[])new HashEntry[cap];\n            if ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                == null) { // recheck\n                Segment<K,V> s = new Segment<K,V>(lf, threshold, tab);\n                while ((seg = (Segment<K,V>)UNSAFE.getObjectVolatile(ss, u))\n                       == null) {\n                    if (UNSAFE.compareAndSwapObject(ss, u, null, seg = s))\n                        break;\n                }\n            }\n        }\n        return seg;\n    }\n\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"lineHeight": 1.2,
			"baseline": 1866
		},
		{
			"id": "UTaxNtWABUlequn9l4E6M",
			"type": "rectangle",
			"x": 777.5578587849935,
			"y": -126.3481903076181,
			"width": 221.44986979166654,
			"height": 34,
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
			"seed": 1260500097,
			"version": 231,
			"versionNonce": 2103136335,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "xrKUlGfD"
				}
			],
			"updated": 1704632654458,
			"link": null,
			"locked": false
		},
		{
			"id": "xrKUlGfD",
			"type": "text",
			"x": 785.5484186808268,
			"y": -121.3481903076181,
			"width": 205.46875,
			"height": 24,
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
			"seed": 1977187713,
			"version": 192,
			"versionNonce": 352051233,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704632654458,
			"link": null,
			"locked": false,
			"text": "获取segment[]的下标",
			"rawText": "获取segment[]的下标",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "UTaxNtWABUlequn9l4E6M",
			"originalText": "获取segment[]的下标",
			"lineHeight": 1.2
		},
		{
			"id": "gfxqhQrKCo2vBbAF9BSX-",
			"type": "rectangle",
			"x": 531.8693008422853,
			"y": -45.793055216472396,
			"width": 257.22200520833326,
			"height": 34,
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
			"seed": 1911605391,
			"version": 242,
			"versionNonce": 1459518159,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "w52cY6sK"
				}
			],
			"updated": 1704632693249,
			"link": null,
			"locked": false
		},
		{
			"id": "w52cY6sK",
			"type": "text",
			"x": 537.0428034464519,
			"y": -40.793055216472396,
			"width": 246.875,
			"height": 24,
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
			"seed": 1806569231,
			"version": 349,
			"versionNonce": 1914980591,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704632693250,
			"link": null,
			"locked": false,
			"text": "指定下标为null，创建一个",
			"rawText": "指定下标为null，创建一个",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "gfxqhQrKCo2vBbAF9BSX-",
			"originalText": "指定下标为null，创建一个",
			"lineHeight": 1.2
		},
		{
			"id": "u019i49IJq-Ms5B1LJexZ",
			"type": "arrow",
			"x": 537.2454401652019,
			"y": -44.008265177409896,
			"width": 182.67171223958326,
			"height": 25.51220703125,
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
			"seed": 2029440335,
			"version": 24,
			"versionNonce": 826961519,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704632654458,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					182.67171223958326,
					25.51220703125
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
		"currentItemFontSize": 20,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 240.2983830769857,
		"scrollY": 482.2447357177743,
		"zoom": {
			"value": 0.75
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