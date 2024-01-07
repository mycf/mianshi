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

自旋 ^obqmGVra

本线程设置成功 ^cMqKzBWU

其他线程设置成功 ^Xeq6F3YO

从s0中获取初始化信息 ^DlYyXv5u

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
			"version": 78,
			"versionNonce": 913693455,
			"isDeleted": false,
			"id": "hMZHCo9O",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 146.69632720947266,
			"y": -236.49050394694018,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 937.5,
			"height": 1872,
			"seed": 290355311,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704633552390,
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
			"version": 234,
			"versionNonce": 2025087329,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "xrKUlGfD"
				}
			],
			"updated": 1704633552390,
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
			"version": 195,
			"versionNonce": 1682284847,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552390,
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
			"version": 245,
			"versionNonce": 1819779919,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "w52cY6sK"
				}
			],
			"updated": 1704633552390,
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
			"version": 352,
			"versionNonce": 1925332257,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552390,
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
			"id": "5PHalYde7P3Gry29LiSo-",
			"type": "rectangle",
			"x": 532.6286595662434,
			"y": 360.8478953043612,
			"width": 50.89778645833337,
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
			"seed": 1902515343,
			"version": 135,
			"versionNonce": 1063543151,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "obqmGVra"
				}
			],
			"updated": 1704633552390,
			"link": null,
			"locked": false
		},
		{
			"id": "obqmGVra",
			"type": "text",
			"x": 538.0775527954102,
			"y": 365.8478953043612,
			"width": 40,
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
			"seed": 1840883279,
			"version": 63,
			"versionNonce": 547352833,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552390,
			"link": null,
			"locked": false,
			"text": "自旋",
			"rawText": "自旋",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "5PHalYde7P3Gry29LiSo-",
			"originalText": "自旋",
			"lineHeight": 1.2
		},
		{
			"id": "sbKO5lxg_c9054MkduPn6",
			"type": "rectangle",
			"x": 501.6145807902018,
			"y": 412.44571431477783,
			"width": 168.352783203125,
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
			"seed": 1020904449,
			"version": 78,
			"versionNonce": 1415325921,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "cMqKzBWU"
				},
				{
					"id": "_NciPePEOvSDQdnhGqK_I",
					"type": "arrow"
				}
			],
			"updated": 1704633552390,
			"link": null,
			"locked": false
		},
		{
			"id": "cMqKzBWU",
			"type": "text",
			"x": 515.7909723917643,
			"y": 417.44571431477783,
			"width": 140,
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
			"seed": 1783416001,
			"version": 134,
			"versionNonce": 1300238767,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"text": "本线程设置成功",
			"rawText": "本线程设置成功",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "sbKO5lxg_c9054MkduPn6",
			"originalText": "本线程设置成功",
			"lineHeight": 1.2
		},
		{
			"id": "_NciPePEOvSDQdnhGqK_I",
			"type": "arrow",
			"x": 609.4641431562471,
			"y": 447.6675567626945,
			"width": 235.35249205354535,
			"height": 81.26749674479163,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1709817487,
			"version": 84,
			"versionNonce": 1468425409,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-235.35249205354535,
					81.26749674479163
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "sbKO5lxg_c9054MkduPn6",
				"gap": 1.2218424479166288,
				"focus": -0.5730060580007055
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "zI81PA6Zq3nkNzmeASr04",
			"type": "rectangle",
			"x": 180.04304758707679,
			"y": 382.27033996581946,
			"width": 199.01944986979163,
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
			"seed": 786353455,
			"version": 139,
			"versionNonce": 364782753,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Xeq6F3YO"
				},
				{
					"id": "BNPjwwX0kbpIXNk790uxh",
					"type": "arrow"
				}
			],
			"updated": 1704633552391,
			"link": null,
			"locked": false
		},
		{
			"id": "Xeq6F3YO",
			"type": "text",
			"x": 199.5527725219726,
			"y": 387.27033996581946,
			"width": 160,
			"height": 24,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 406651823,
			"version": 157,
			"versionNonce": 1209840111,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"text": "其他线程设置成功",
			"rawText": "其他线程设置成功",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "zI81PA6Zq3nkNzmeASr04",
			"originalText": "其他线程设置成功",
			"lineHeight": 1.2
		},
		{
			"id": "BNPjwwX0kbpIXNk790uxh",
			"type": "arrow",
			"x": 180.02286529541013,
			"y": 399.3730417887361,
			"width": 172.62052408854166,
			"height": 54.58072916666663,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 664100961,
			"version": 145,
			"versionNonce": 1521674369,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					-11.46337890625,
					-54.58072916666663
				],
				[
					161.15714518229166,
					-50.42252604166663
				]
			],
			"lastCommittedPoint": null,
			"startBinding": {
				"elementId": "zI81PA6Zq3nkNzmeASr04",
				"focus": -0.9657675015700949,
				"gap": 1
			},
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "oO-tX3CitvVqyER1IguET",
			"type": "rectangle",
			"x": 641.2969538370767,
			"y": 173.58186340331946,
			"width": 213.50366210937506,
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
			"seed": 1472086575,
			"version": 332,
			"versionNonce": 402656815,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "DlYyXv5u"
				}
			],
			"updated": 1704633579927,
			"link": null,
			"locked": false
		},
		{
			"id": "DlYyXv5u",
			"type": "text",
			"x": 646.3300348917642,
			"y": 178.58186340331946,
			"width": 203.4375,
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
			"seed": 915246191,
			"version": 277,
			"versionNonce": 925493391,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704633579927,
			"link": null,
			"locked": false,
			"text": "从s0中获取初始化信息",
			"rawText": "从s0中获取初始化信息",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "oO-tX3CitvVqyER1IguET",
			"originalText": "从s0中获取初始化信息",
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
			"version": 27,
			"versionNonce": 625184065,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704633552390,
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
		},
		{
			"id": "aIYpeu5AZE6EhcO36YL5R",
			"type": "arrow",
			"x": 503.81095123291016,
			"y": 411.74649556477783,
			"width": 67.58837890625,
			"height": 12.939697265625,
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
			"seed": 728443585,
			"version": 20,
			"versionNonce": 777280399,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704633552390,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					67.58837890625,
					12.939697265625
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "QNuBJvWliEbRNTK9tU24l",
			"type": "arrow",
			"x": 215.71060943603513,
			"y": 384.61555480956946,
			"width": 93.14575195312503,
			"height": 29.15299479166663,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"seed": 1007485153,
			"version": 24,
			"versionNonce": 370148303,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					93.14575195312503,
					29.15299479166663
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "wvzeIH0pGonrrvZ6rixBx",
			"type": "arrow",
			"x": 644.7340469360352,
			"y": 173.26325988769446,
			"width": 54.20556640625,
			"height": 33.97281901041663,
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
			"seed": 157032527,
			"version": 23,
			"versionNonce": 212769807,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704633552391,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					54.20556640625,
					33.97281901041663
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