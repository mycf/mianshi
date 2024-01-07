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

获取HashEntry的下标 ^BJgbzZaB

达到阈值扩容 ^Ss1EFycL

执行插入 ^djOCiHCo

元素不为空 ^IvnjyUcA

是否存在不操作 ^76YNtjq7

        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {
            HashEntry<K,V> first = entryForHash(this, hash);
            HashEntry<K,V> e = first;
            HashEntry<K,V> node = null;
            int retries = -1; // negative while locating node
            while (!tryLock()) {
                HashEntry<K,V> f; // to recheck first below
                if (retries < 0) {
                    if (e == null) {
                        if (node == null) // speculatively create node
                            node = new HashEntry<K,V>(hash, key, value, null);
                        retries = 0;
                    }
                    else if (key.equals(e.key))
                        retries = 0;
                    else
                        e = e.next;
                }
                else if (++retries > MAX_SCAN_RETRIES) {
                    lock();
                    break;
                }
                else if ((retries & 1) == 0 &&
                         (f = entryForHash(this, hash)) != first) {
                    e = first = f; // re-traverse if entry changed
                    retries = -1;
                }
            }
            return node;
        }
 ^1LtLcmqy

非首次说明遍历到尾部 ^o4hYLbmu

当前key存在 ^iyKMDMLr

遍历中，直到找到相同key，或者到尾部 ^3kKZ1Is6

没有相同key，提前生成HashEntry ^F1wzcXYm

一遍遍历，一遍尝试获取锁 ^ntlTE4TI

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
			"version": 100,
			"versionNonce": 625599983,
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
			"updated": 1704635725977,
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
			"version": 238,
			"versionNonce": 421721217,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "xrKUlGfD"
				}
			],
			"updated": 1704635725977,
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
			"version": 199,
			"versionNonce": 907366415,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725977,
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
			"version": 249,
			"versionNonce": 170817071,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "w52cY6sK"
				}
			],
			"updated": 1704635725978,
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
			"version": 356,
			"versionNonce": 1321109569,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 139,
			"versionNonce": 1753077839,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "obqmGVra"
				}
			],
			"updated": 1704635725978,
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
			"version": 67,
			"versionNonce": 1227216929,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 82,
			"versionNonce": 1037577217,
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
			"updated": 1704635725978,
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
			"version": 138,
			"versionNonce": 1276203151,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 88,
			"versionNonce": 886375393,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 143,
			"versionNonce": 170647489,
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
			"updated": 1704635725978,
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
			"version": 161,
			"versionNonce": 1725151439,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 149,
			"versionNonce": 1811693473,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 336,
			"versionNonce": 410362753,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "DlYyXv5u"
				}
			],
			"updated": 1704635725978,
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
			"version": 281,
			"versionNonce": 231280911,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
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
			"id": "0xhVn5yOEOxKlrDdWHic0",
			"type": "rectangle",
			"x": 755.6660130818682,
			"y": 714.239659627278,
			"width": 217.35172526041677,
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
			"seed": 648433871,
			"version": 328,
			"versionNonce": 1540852577,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "BJgbzZaB"
				}
			],
			"updated": 1704635725978,
			"link": null,
			"locked": false
		},
		{
			"id": "BJgbzZaB",
			"type": "text",
			"x": 761.6075007120766,
			"y": 719.239659627278,
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
			"seed": 62984687,
			"version": 329,
			"versionNonce": 398595887,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "获取HashEntry的下标",
			"rawText": "获取HashEntry的下标",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "0xhVn5yOEOxKlrDdWHic0",
			"originalText": "获取HashEntry的下标",
			"lineHeight": 1.2
		},
		{
			"id": "OWYvnoko3xYxZ0OD9CvIT",
			"type": "rectangle",
			"x": 1038.0525690714512,
			"y": 1255.818028767903,
			"width": 130.26627604166674,
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
			"seed": 1164551887,
			"version": 281,
			"versionNonce": 1316058945,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Ss1EFycL"
				}
			],
			"updated": 1704635725978,
			"link": null,
			"locked": false
		},
		{
			"id": "Ss1EFycL",
			"type": "text",
			"x": 1043.1857070922847,
			"y": 1260.818028767903,
			"width": 120,
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
			"seed": 1209685665,
			"version": 170,
			"versionNonce": 620058959,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "达到阈值扩容",
			"rawText": "达到阈值扩容",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "OWYvnoko3xYxZ0OD9CvIT",
			"originalText": "达到阈值扩容",
			"lineHeight": 1.2
		},
		{
			"id": "aYhU97dad0eAYeu55zzQw",
			"type": "rectangle",
			"x": 823.1091283162432,
			"y": 1323.1428985595696,
			"width": 91.26668294270833,
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
			"seed": 1283569423,
			"version": 319,
			"versionNonce": 1386407713,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "djOCiHCo"
				}
			],
			"updated": 1704635725978,
			"link": null,
			"locked": false
		},
		{
			"id": "djOCiHCo",
			"type": "text",
			"x": 828.7424697875973,
			"y": 1328.1428985595696,
			"width": 80,
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
			"seed": 76630401,
			"version": 303,
			"versionNonce": 1253791599,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "执行插入",
			"rawText": "执行插入",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "aYhU97dad0eAYeu55zzQw",
			"originalText": "执行插入",
			"lineHeight": 1.2
		},
		{
			"id": "5uQyUALFVvfFL3HDbB9zm",
			"type": "rectangle",
			"x": 574.6123021443682,
			"y": 794.085444132486,
			"width": 115.21573893229166,
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
			"seed": 715312993,
			"version": 162,
			"versionNonce": 1067078369,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "IvnjyUcA"
				}
			],
			"updated": 1704635725978,
			"link": null,
			"locked": false
		},
		{
			"id": "IvnjyUcA",
			"type": "text",
			"x": 582.2201716105141,
			"y": 799.085444132486,
			"width": 100,
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
			"seed": 151247617,
			"version": 161,
			"versionNonce": 1544652719,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "元素不为空",
			"rawText": "元素不为空",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "5uQyUALFVvfFL3HDbB9zm",
			"originalText": "元素不为空",
			"lineHeight": 1.2
		},
		{
			"id": "pMY1AreKR11MaRwLcirOa",
			"type": "rectangle",
			"x": 713.9635391235349,
			"y": 911.1430613199859,
			"width": 151.42692057291663,
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
			"seed": 1075357185,
			"version": 252,
			"versionNonce": 1299140289,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "76YNtjq7"
				}
			],
			"updated": 1704635725978,
			"link": null,
			"locked": false
		},
		{
			"id": "76YNtjq7",
			"type": "text",
			"x": 719.6769994099932,
			"y": 916.1430613199859,
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
			"seed": 1799050145,
			"version": 267,
			"versionNonce": 2130885071,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "是否存在不操作",
			"rawText": "是否存在不操作",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "pMY1AreKR11MaRwLcirOa",
			"originalText": "是否存在不操作",
			"lineHeight": 1.2
		},
		{
			"id": "1LtLcmqy",
			"type": "text",
			"x": 146.69632735270142,
			"y": 1667.018061319986,
			"width": 914.0625,
			"height": 744,
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
			"seed": 826712527,
			"version": 152,
			"versionNonce": 759736303,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"rawText": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 739,
			"containerId": null,
			"originalText": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"lineHeight": 1.2
		},
		{
			"id": "2bcPOWFHR0xBeWMjCfyPU",
			"type": "rectangle",
			"x": 576.062985738118,
			"y": 1853.443272908528,
			"width": 231.10465494791669,
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
			"seed": 1576884833,
			"version": 92,
			"versionNonce": 1578826401,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "o4hYLbmu"
				}
			],
			"updated": 1704635749547,
			"link": null,
			"locked": false
		},
		{
			"id": "o4hYLbmu",
			"type": "text",
			"x": 591.6153132120763,
			"y": 1858.443272908528,
			"width": 200,
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
			"seed": 773329217,
			"version": 78,
			"versionNonce": 1947719873,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635758294,
			"link": null,
			"locked": false,
			"text": "非首次说明遍历到尾部",
			"rawText": "非首次说明遍历到尾部",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "2bcPOWFHR0xBeWMjCfyPU",
			"originalText": "非首次说明遍历到尾部",
			"lineHeight": 1.2
		},
		{
			"id": "b4uwDQ0qBnYkb_JYSfxbR",
			"type": "rectangle",
			"x": 714.2814102172849,
			"y": 1982.4404246012368,
			"width": 152.33951822916666,
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
			"seed": 103910959,
			"version": 73,
			"versionNonce": 2047838703,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "iyKMDMLr"
				}
			],
			"updated": 1704635858677,
			"link": null,
			"locked": false
		},
		{
			"id": "iyKMDMLr",
			"type": "text",
			"x": 732.8730443318683,
			"y": 1987.4404246012368,
			"width": 115.15625,
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
			"seed": 1918120673,
			"version": 16,
			"versionNonce": 713454607,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635858678,
			"link": null,
			"locked": false,
			"text": "当前key存在",
			"rawText": "当前key存在",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "b4uwDQ0qBnYkb_JYSfxbR",
			"originalText": "当前key存在",
			"lineHeight": 1.2
		},
		{
			"id": "Mw9n7jrUA4vcMpDIJ2JR0",
			"type": "rectangle",
			"x": 566.3121719360348,
			"y": 2049.6363881429033,
			"width": 373.15771484374994,
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
			"seed": 484121135,
			"version": 497,
			"versionNonce": 145899503,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "3kKZ1Is6"
				}
			],
			"updated": 1704635944584,
			"link": null,
			"locked": false
		},
		{
			"id": "3kKZ1Is6",
			"type": "text",
			"x": 575.3129043579098,
			"y": 2054.6363881429033,
			"width": 355.15625,
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
			"seed": 1433090319,
			"version": 440,
			"versionNonce": 1087140431,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635944584,
			"link": null,
			"locked": false,
			"text": "遍历中，直到找到相同key，或者到尾部",
			"rawText": "遍历中，直到找到相同key，或者到尾部",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "Mw9n7jrUA4vcMpDIJ2JR0",
			"originalText": "遍历中，直到找到相同key，或者到尾部",
			"lineHeight": 1.2
		},
		{
			"id": "NW8Nvu7jJ4PbmsAakdflZ",
			"type": "rectangle",
			"x": 1052.2838516235347,
			"y": 1864.8723093668618,
			"width": 155.70198567708348,
			"height": 82,
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
			"seed": 436963809,
			"version": 107,
			"versionNonce": 258655503,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "F1wzcXYm"
				}
			],
			"updated": 1704635996273,
			"link": null,
			"locked": false
		},
		{
			"id": "F1wzcXYm",
			"type": "text",
			"x": 1060.8379694620764,
			"y": 1869.8723093668618,
			"width": 138.59375,
			"height": 72,
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
			"seed": 1721161537,
			"version": 103,
			"versionNonce": 1661815599,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704635996273,
			"link": null,
			"locked": false,
			"text": "没有相同key，\n提前生成HashE\nntry",
			"rawText": "没有相同key，提前生成HashEntry",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 67,
			"containerId": "NW8Nvu7jJ4PbmsAakdflZ",
			"originalText": "没有相同key，提前生成HashEntry",
			"lineHeight": 1.2
		},
		{
			"id": "7So0Z-KRSgE8t3CNxeDpZ",
			"type": "rectangle",
			"x": 527.9932428995764,
			"y": 1785.9540964762368,
			"width": 276.89493815104174,
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
			"seed": 2076805999,
			"version": 100,
			"versionNonce": 39588609,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "ntlTE4TI"
				}
			],
			"updated": 1704636067293,
			"link": null,
			"locked": false
		},
		{
			"id": "ntlTE4TI",
			"type": "text",
			"x": 546.4407119750973,
			"y": 1790.9540964762368,
			"width": 240,
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
			"seed": 1605078543,
			"version": 60,
			"versionNonce": 776211169,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704636067293,
			"link": null,
			"locked": false,
			"text": "一遍遍历，一遍尝试获取锁",
			"rawText": "一遍遍历，一遍尝试获取锁",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 19,
			"containerId": "7So0Z-KRSgE8t3CNxeDpZ",
			"originalText": "一遍遍历，一遍尝试获取锁",
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
			"version": 31,
			"versionNonce": 1555217505,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725977,
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
			"version": 24,
			"versionNonce": 651949679,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 28,
			"versionNonce": 1909876399,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
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
			"version": 27,
			"versionNonce": 279037679,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
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
		},
		{
			"id": "pzpP9nkzWB4YCwgt-LB1_",
			"type": "arrow",
			"x": 459.87166086832656,
			"y": 1101.6246693929027,
			"width": 81.692138671875,
			"height": 25.6796875,
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
			"seed": 2038427521,
			"version": 24,
			"versionNonce": 1390107393,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					81.692138671875,
					25.6796875
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "2IJ898IqdeOfEpaorsJ_8",
			"type": "arrow",
			"x": 584.9024225870766,
			"y": 802.5667266845693,
			"width": 63.62361653645826,
			"height": 27.342447916666742,
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
			"seed": 1123274607,
			"version": 18,
			"versionNonce": 1599902095,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					63.62361653645826,
					27.342447916666742
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": null,
			"startArrowhead": null,
			"endArrowhead": "arrow"
		},
		{
			"id": "Qj5cJXBM",
			"type": "text",
			"x": 188.38639068603473,
			"y": 1741.6847279866527,
			"width": 11.71875,
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
			"seed": 855473953,
			"version": 3,
			"versionNonce": 1928973985,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 19,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "99ktG-B-jOyHj7S13Hp9N",
			"type": "arrow",
			"x": 581.646237691243,
			"y": 1854.836339314778,
			"width": 0,
			"height": 0,
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
			"roundness": {
				"type": 2
			},
			"seed": 451656513,
			"version": 22,
			"versionNonce": 652825217,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704635725978,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					0,
					0
				]
			],
			"lastCommittedPoint": [
				0,
				0
			],
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
		"scrollX": 216.29838307698603,
		"scrollY": -968.4219309488931,
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