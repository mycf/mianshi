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

遍历中间不断尝试获取锁 ^ntlTE4TI

不相等代表其他线程修改了当前链表 ^Cv6VZstd

当前链表被修改需要重新遍历 ^FOJwNb4V

偶数 ^hHAtrors

遍历到最后或者first为null ^TkScBEwn

其他线程修改了当前链表，如果新链表包含此key，这里提前生成的node就无效了 ^8rRT3cgz

主方法会重新处理
key相同的问题 ^TZyBBeBc

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
			"version": 101,
			"versionNonce": 2002498671,
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
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
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
			"type": "rectangle",
			"version": 239,
			"versionNonce": 79384065,
			"isDeleted": false,
			"id": "UTaxNtWABUlequn9l4E6M",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 777.5578587849935,
			"y": -126.3481903076181,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 221.44986979166654,
			"height": 34,
			"seed": 1260500097,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "xrKUlGfD"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 200,
			"versionNonce": 837373583,
			"isDeleted": false,
			"id": "xrKUlGfD",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 785.5484186808268,
			"y": -121.3481903076181,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 205.46875,
			"height": 24,
			"seed": 1977187713,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "获取segment[]的下标",
			"rawText": "获取segment[]的下标",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "UTaxNtWABUlequn9l4E6M",
			"originalText": "获取segment[]的下标",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 250,
			"versionNonce": 923002337,
			"isDeleted": false,
			"id": "gfxqhQrKCo2vBbAF9BSX-",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 531.8693008422853,
			"y": -45.793055216472396,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 257.22200520833326,
			"height": 34,
			"seed": 1911605391,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "w52cY6sK"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 357,
			"versionNonce": 402564271,
			"isDeleted": false,
			"id": "w52cY6sK",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 537.0428034464519,
			"y": -40.793055216472396,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 246.875,
			"height": 24,
			"seed": 1806569231,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "指定下标为null，创建一个",
			"rawText": "指定下标为null，创建一个",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "gfxqhQrKCo2vBbAF9BSX-",
			"originalText": "指定下标为null，创建一个",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 140,
			"versionNonce": 1747878337,
			"isDeleted": false,
			"id": "5PHalYde7P3Gry29LiSo-",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 532.6286595662434,
			"y": 360.8478953043612,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 50.89778645833337,
			"height": 34,
			"seed": 1902515343,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "obqmGVra"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 68,
			"versionNonce": 1497642703,
			"isDeleted": false,
			"id": "obqmGVra",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 538.0775527954102,
			"y": 365.8478953043612,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 40,
			"height": 24,
			"seed": 1840883279,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "自旋",
			"rawText": "自旋",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "5PHalYde7P3Gry29LiSo-",
			"originalText": "自旋",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 83,
			"versionNonce": 1968873889,
			"isDeleted": false,
			"id": "sbKO5lxg_c9054MkduPn6",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 501.6145807902018,
			"y": 412.44571431477783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 168.352783203125,
			"height": 34,
			"seed": 1020904449,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
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
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 139,
			"versionNonce": 475170031,
			"isDeleted": false,
			"id": "cMqKzBWU",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 515.7909723917643,
			"y": 417.44571431477783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 140,
			"height": 24,
			"seed": 1783416001,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "本线程设置成功",
			"rawText": "本线程设置成功",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "sbKO5lxg_c9054MkduPn6",
			"originalText": "本线程设置成功",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 90,
			"versionNonce": 1473110561,
			"isDeleted": false,
			"id": "_NciPePEOvSDQdnhGqK_I",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 609.4641431562471,
			"y": 447.6675567626945,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 235.35249205354535,
			"height": 81.26749674479163,
			"seed": 1709817487,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704639978122,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "sbKO5lxg_c9054MkduPn6",
				"gap": 1.2218424479166288,
				"focus": -0.5730060580007055
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-235.35249205354535,
					81.26749674479163
				]
			]
		},
		{
			"type": "rectangle",
			"version": 144,
			"versionNonce": 548540175,
			"isDeleted": false,
			"id": "zI81PA6Zq3nkNzmeASr04",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 180.04304758707679,
			"y": 382.27033996581946,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 199.01944986979163,
			"height": 34,
			"seed": 786353455,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
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
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 162,
			"versionNonce": 109407585,
			"isDeleted": false,
			"id": "Xeq6F3YO",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 199.5527725219726,
			"y": 387.27033996581946,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 160,
			"height": 24,
			"seed": 406651823,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "其他线程设置成功",
			"rawText": "其他线程设置成功",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "zI81PA6Zq3nkNzmeASr04",
			"originalText": "其他线程设置成功",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 151,
			"versionNonce": 250541569,
			"isDeleted": false,
			"id": "BNPjwwX0kbpIXNk790uxh",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 179.04304758707676,
			"y": 394.70782349589155,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 172.62052408854166,
			"height": 49.915510873822086,
			"seed": 664100961,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704639978125,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "zI81PA6Zq3nkNzmeASr04",
				"gap": 1,
				"focus": -0.9657675015700949
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-10.483561197916629,
					-49.915510873822086
				],
				[
					162.13696289062503,
					-45.757307748822086
				]
			]
		},
		{
			"type": "rectangle",
			"version": 337,
			"versionNonce": 1829968193,
			"isDeleted": false,
			"id": "oO-tX3CitvVqyER1IguET",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 641.2969538370767,
			"y": 173.58186340331946,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 213.50366210937506,
			"height": 34,
			"seed": 1472086575,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "DlYyXv5u"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 282,
			"versionNonce": 1949751119,
			"isDeleted": false,
			"id": "DlYyXv5u",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 646.3300348917642,
			"y": 178.58186340331946,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 203.4375,
			"height": 24,
			"seed": 915246191,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "从s0中获取初始化信息",
			"rawText": "从s0中获取初始化信息",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "oO-tX3CitvVqyER1IguET",
			"originalText": "从s0中获取初始化信息",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 329,
			"versionNonce": 817908001,
			"isDeleted": false,
			"id": "0xhVn5yOEOxKlrDdWHic0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 755.6660130818682,
			"y": 714.239659627278,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 217.35172526041677,
			"height": 34,
			"seed": 648433871,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "BJgbzZaB"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 330,
			"versionNonce": 1897731439,
			"isDeleted": false,
			"id": "BJgbzZaB",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 761.6075007120766,
			"y": 719.239659627278,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 205.46875,
			"height": 24,
			"seed": 62984687,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "获取HashEntry的下标",
			"rawText": "获取HashEntry的下标",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "0xhVn5yOEOxKlrDdWHic0",
			"originalText": "获取HashEntry的下标",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 282,
			"versionNonce": 184319233,
			"isDeleted": false,
			"id": "OWYvnoko3xYxZ0OD9CvIT",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1038.0525690714512,
			"y": 1255.818028767903,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 130.26627604166674,
			"height": 34,
			"seed": 1164551887,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Ss1EFycL"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 171,
			"versionNonce": 248942479,
			"isDeleted": false,
			"id": "Ss1EFycL",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1043.1857070922847,
			"y": 1260.818028767903,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 120,
			"height": 24,
			"seed": 1209685665,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "达到阈值扩容",
			"rawText": "达到阈值扩容",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "OWYvnoko3xYxZ0OD9CvIT",
			"originalText": "达到阈值扩容",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 320,
			"versionNonce": 1531393249,
			"isDeleted": false,
			"id": "aYhU97dad0eAYeu55zzQw",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 823.1091283162432,
			"y": 1323.1428985595696,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 91.26668294270833,
			"height": 34,
			"seed": 1283569423,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "djOCiHCo"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 304,
			"versionNonce": 1201391023,
			"isDeleted": false,
			"id": "djOCiHCo",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 828.7424697875973,
			"y": 1328.1428985595696,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 80,
			"height": 24,
			"seed": 76630401,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "执行插入",
			"rawText": "执行插入",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "aYhU97dad0eAYeu55zzQw",
			"originalText": "执行插入",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 163,
			"versionNonce": 1697640641,
			"isDeleted": false,
			"id": "5uQyUALFVvfFL3HDbB9zm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 574.6123021443682,
			"y": 794.085444132486,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 115.21573893229166,
			"height": 34,
			"seed": 715312993,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "IvnjyUcA"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 162,
			"versionNonce": 982857679,
			"isDeleted": false,
			"id": "IvnjyUcA",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 582.2201716105141,
			"y": 799.085444132486,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 100,
			"height": 24,
			"seed": 151247617,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "元素不为空",
			"rawText": "元素不为空",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "5uQyUALFVvfFL3HDbB9zm",
			"originalText": "元素不为空",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 253,
			"versionNonce": 1314271393,
			"isDeleted": false,
			"id": "pMY1AreKR11MaRwLcirOa",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 713.9635391235349,
			"y": 911.1430613199859,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 151.42692057291663,
			"height": 34,
			"seed": 1075357185,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "76YNtjq7"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 268,
			"versionNonce": 232482287,
			"isDeleted": false,
			"id": "76YNtjq7",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 719.6769994099932,
			"y": 916.1430613199859,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 140,
			"height": 24,
			"seed": 1799050145,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "是否存在不操作",
			"rawText": "是否存在不操作",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "pMY1AreKR11MaRwLcirOa",
			"originalText": "是否存在不操作",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "text",
			"version": 153,
			"versionNonce": 1550414977,
			"isDeleted": false,
			"id": "1LtLcmqy",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 146.69632735270142,
			"y": 1667.018061319986,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 914.0625,
			"height": 744,
			"seed": 826712527,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"rawText": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "        private HashEntry<K,V> scanAndLockForPut(K key, int hash, V value) {\n            HashEntry<K,V> first = entryForHash(this, hash);\n            HashEntry<K,V> e = first;\n            HashEntry<K,V> node = null;\n            int retries = -1; // negative while locating node\n            while (!tryLock()) {\n                HashEntry<K,V> f; // to recheck first below\n                if (retries < 0) {\n                    if (e == null) {\n                        if (node == null) // speculatively create node\n                            node = new HashEntry<K,V>(hash, key, value, null);\n                        retries = 0;\n                    }\n                    else if (key.equals(e.key))\n                        retries = 0;\n                    else\n                        e = e.next;\n                }\n                else if (++retries > MAX_SCAN_RETRIES) {\n                    lock();\n                    break;\n                }\n                else if ((retries & 1) == 0 &&\n                         (f = entryForHash(this, hash)) != first) {\n                    e = first = f; // re-traverse if entry changed\n                    retries = -1;\n                }\n            }\n            return node;\n        }\n",
			"lineHeight": 1.2,
			"baseline": 739
		},
		{
			"type": "rectangle",
			"version": 93,
			"versionNonce": 276221967,
			"isDeleted": false,
			"id": "2bcPOWFHR0xBeWMjCfyPU",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 576.062985738118,
			"y": 1853.443272908528,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 231.10465494791669,
			"height": 34,
			"seed": 1576884833,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "o4hYLbmu"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 79,
			"versionNonce": 1932082273,
			"isDeleted": false,
			"id": "o4hYLbmu",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 591.6153132120763,
			"y": 1858.443272908528,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 200,
			"height": 24,
			"seed": 773329217,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "非首次说明遍历到尾部",
			"rawText": "非首次说明遍历到尾部",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "2bcPOWFHR0xBeWMjCfyPU",
			"originalText": "非首次说明遍历到尾部",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 74,
			"versionNonce": 476050991,
			"isDeleted": false,
			"id": "b4uwDQ0qBnYkb_JYSfxbR",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 714.2814102172849,
			"y": 1982.4404246012368,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 152.33951822916666,
			"height": 34,
			"seed": 103910959,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "iyKMDMLr"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 17,
			"versionNonce": 1382352961,
			"isDeleted": false,
			"id": "iyKMDMLr",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 732.8730443318683,
			"y": 1987.4404246012368,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 115.15625,
			"height": 24,
			"seed": 1918120673,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "当前key存在",
			"rawText": "当前key存在",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "b4uwDQ0qBnYkb_JYSfxbR",
			"originalText": "当前key存在",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 499,
			"versionNonce": 209882593,
			"isDeleted": false,
			"id": "Mw9n7jrUA4vcMpDIJ2JR0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 566.3121719360347,
			"y": 2049.6363881429033,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 373.15771484374994,
			"height": 34,
			"seed": 484121135,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "3kKZ1Is6"
				}
			],
			"updated": 1704639978151,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 442,
			"versionNonce": 1738025409,
			"isDeleted": false,
			"id": "3kKZ1Is6",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 575.3129043579097,
			"y": 2054.6363881429033,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 355.15625,
			"height": 24,
			"seed": 1433090319,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704639978151,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "遍历中，直到找到相同key，或者到尾部",
			"rawText": "遍历中，直到找到相同key，或者到尾部",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Mw9n7jrUA4vcMpDIJ2JR0",
			"originalText": "遍历中，直到找到相同key，或者到尾部",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 109,
			"versionNonce": 2058188399,
			"isDeleted": false,
			"id": "NW8Nvu7jJ4PbmsAakdflZ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1052.2838516235347,
			"y": 1864.8723093668618,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 155.70198567708348,
			"height": 82,
			"seed": 436963809,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "F1wzcXYm"
				},
				{
					"id": "3IxUfn0F6xbbhr5IPjFN6",
					"type": "arrow"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 104,
			"versionNonce": 1776200705,
			"isDeleted": false,
			"id": "F1wzcXYm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1060.8379694620764,
			"y": 1869.8723093668618,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 138.59375,
			"height": 72,
			"seed": 1721161537,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "没有相同key，\n提前生成HashE\nntry",
			"rawText": "没有相同key，提前生成HashEntry",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "NW8Nvu7jJ4PbmsAakdflZ",
			"originalText": "没有相同key，提前生成HashEntry",
			"lineHeight": 1.2,
			"baseline": 67
		},
		{
			"type": "rectangle",
			"version": 101,
			"versionNonce": 699584655,
			"isDeleted": false,
			"id": "7So0Z-KRSgE8t3CNxeDpZ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 527.9932428995764,
			"y": 1785.9540964762368,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 276.89493815104174,
			"height": 34,
			"seed": 2076805999,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "ntlTE4TI"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 76,
			"versionNonce": 470516705,
			"isDeleted": false,
			"id": "ntlTE4TI",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 556.4407119750973,
			"y": 1790.9540964762368,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 220,
			"height": 24,
			"seed": 1605078543,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "遍历中间不断尝试获取锁",
			"rawText": "遍历中间不断尝试获取锁",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "7So0Z-KRSgE8t3CNxeDpZ",
			"originalText": "遍历中间不断尝试获取锁",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 65,
			"versionNonce": 524805807,
			"isDeleted": false,
			"id": "L23dNp9uXapy3VpQcZ4P-",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 941.1583633422847,
			"y": 2215.3509877522783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 198.6827799479164,
			"height": 58,
			"seed": 1106739663,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Cv6VZstd"
				},
				{
					"id": "3IxUfn0F6xbbhr5IPjFN6",
					"type": "arrow"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 58,
			"versionNonce": 1183997889,
			"isDeleted": false,
			"id": "Cv6VZstd",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 950.499753316243,
			"y": 2220.3509877522783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 180,
			"height": 48,
			"seed": 1080942863,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "不相等代表其他线程\n修改了当前链表",
			"rawText": "不相等代表其他线程修改了当前链表",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "L23dNp9uXapy3VpQcZ4P-",
			"originalText": "不相等代表其他线程修改了当前链表",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "rectangle",
			"version": 104,
			"versionNonce": 1779775695,
			"isDeleted": false,
			"id": "SDvMeOfZNg7eu16IDPA2F",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 543.8318659464514,
			"y": 2267.566157023112,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 204.3817545572917,
			"height": 58,
			"seed": 1799703425,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "FOJwNb4V"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 160,
			"versionNonce": 1469235105,
			"isDeleted": false,
			"id": "FOJwNb4V",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 556.0227432250973,
			"y": 2272.566157023112,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 180,
			"height": 48,
			"seed": 1016640193,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "当前链表被修改需要\n重新遍历",
			"rawText": "当前链表被修改需要重新遍历",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "SDvMeOfZNg7eu16IDPA2F",
			"originalText": "当前链表被修改需要重新遍历",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 111,
			"versionNonce": 800278255,
			"isDeleted": false,
			"id": "oBJxuXJw_7sW7GRUdqkjv",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 381.54378000895144,
			"y": 2282.2357533772783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 217.8211263020834,
			"height": 439.245849609375,
			"seed": 2000282209,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-217.8211263020834,
					-223.76708984375
				],
				[
					-43.225585937500114,
					-439.245849609375
				]
			]
		},
		{
			"type": "rectangle",
			"version": 69,
			"versionNonce": 1295786881,
			"isDeleted": false,
			"id": "bwPz9H-TO9F9RyFH4L3uG",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 697.6297175089514,
			"y": 2193.5337677001953,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 72.69921875,
			"height": 34,
			"seed": 734386831,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "hHAtrors"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 7,
			"versionNonce": 1319801103,
			"isDeleted": false,
			"id": "hHAtrors",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 713.9793268839514,
			"y": 2198.5337677001953,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 40,
			"height": 24,
			"seed": 1861477007,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "偶数",
			"rawText": "偶数",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "bwPz9H-TO9F9RyFH4L3uG",
			"originalText": "偶数",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 121,
			"versionNonce": 869141345,
			"isDeleted": false,
			"id": "tAojXUZ88jvfstWHdg7Z1",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 470.71752675374296,
			"y": 1099.3495229085283,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 330.53841145833326,
			"height": 34.882486979166735,
			"seed": 1496076161,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "TkScBEwn"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 114,
			"versionNonce": 1081317167,
			"isDeleted": false,
			"id": "TkScBEwn",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 503.2523574829096,
			"y": 1104.7907663981116,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 265.46875,
			"height": 24,
			"seed": 748429569,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "遍历到最后或者first为null",
			"rawText": "遍历到最后或者first为null",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "tAojXUZ88jvfstWHdg7Z1",
			"originalText": "遍历到最后或者first为null",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 78,
			"versionNonce": 912834881,
			"isDeleted": false,
			"id": "3IxUfn0F6xbbhr5IPjFN6",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1063.996242347316,
			"y": 2214.3509877522783,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 52.413911359552,
			"height": 266.30135091145826,
			"seed": 204622191,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "8rRT3cgz"
				}
			],
			"updated": 1704639978159,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "L23dNp9uXapy3VpQcZ4P-",
				"gap": 1,
				"focus": 0.16746278220728206
			},
			"endBinding": {
				"elementId": "NW8Nvu7jJ4PbmsAakdflZ",
				"gap": 1.1773274739582575,
				"focus": 0.06311952840221603
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					52.413911359552,
					-266.30135091145826
				]
			]
		},
		{
			"type": "text",
			"version": 151,
			"versionNonce": 997644623,
			"isDeleted": false,
			"id": "8rRT3cgz",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 980.899411519368,
			"y": 2033.2354278564453,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 218.59375,
			"height": 96,
			"seed": 118039215,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "其他线程修改了当前链\n表，如果新链表包含此k\ney，这里提前生成的nod\ne就无效了",
			"rawText": "其他线程修改了当前链表，如果新链表包含此key，这里提前生成的node就无效了",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "3IxUfn0F6xbbhr5IPjFN6",
			"originalText": "其他线程修改了当前链表，如果新链表包含此key，这里提前生成的node就无效了",
			"lineHeight": 1.2,
			"baseline": 91
		},
		{
			"type": "arrow",
			"version": 246,
			"versionNonce": 2130274081,
			"isDeleted": false,
			"id": "Nd-npSqRA3L0tsd41q2vH",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1194.1188125610347,
			"y": 2071.315424601236,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 392.48046875,
			"height": 1195.4996948242188,
			"seed": 675886575,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "TZyBBeBc"
				}
			],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					69.52864583333326,
					-193.01920572916606
				],
				[
					98.05240885416652,
					-394.72770182291606
				],
				[
					61.455729166666515,
					-791.7152913411458
				],
				[
					-294.4280598958335,
					-1195.4996948242188
				]
			]
		},
		{
			"type": "text",
			"version": 49,
			"versionNonce": 353163119,
			"isDeleted": false,
			"id": "TZyBBeBc",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1212.1712214152012,
			"y": 1652.5877227783199,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 160,
			"height": 48,
			"seed": 1347239503,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "主方法会重新处理\nkey相同的问题",
			"rawText": "主方法会重新处理\nkey相同的问题",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Nd-npSqRA3L0tsd41q2vH",
			"originalText": "主方法会重新处理\nkey相同的问题",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "arrow",
			"version": 51,
			"versionNonce": 811719425,
			"isDeleted": false,
			"id": "FNHaH9kY1zbRdgyNdhOZT",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1027.890622456868,
			"y": 1003.8186798095694,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 477.37565104166674,
			"height": 23.861002604166515,
			"seed": 1752389601,
			"groupIds": [
				"ENfpyAf1UFfvZuGvRg4EM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704637465050,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					-477.37565104166674,
					23.861002604166515
				]
			]
		},
		{
			"id": "CSQQHApQ",
			"type": "text",
			"x": 1300.2519723801383,
			"y": 1306.8777313232417,
			"width": 11.71875,
			"height": 24,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"fillStyle": "solid",
			"strokeWidth": 1,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1115352431,
			"version": 2,
			"versionNonce": 1235082465,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704639985864,
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
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
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
		"scrollX": 127.33136545416454,
		"scrollY": 115.88858896166707,
		"zoom": {
			"value": 0.8661242079734806
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