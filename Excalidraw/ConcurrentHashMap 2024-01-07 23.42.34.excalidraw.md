---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    /** Implementation for put and putIfAbsent */
    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh; K fk; V fv;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else if (onlyIfAbsent // check first node without acquiring lock
                     && fh == hash
                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))
                     && (fv = f.val) != null)
                return fv;
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) == f) {
                        if (fh >= 0) {
                            binCount = 1;
                            for (Node<K,V> e = f;; ++binCount) {
                                K ek;
                                if (e.hash == hash &&
                                    ((ek = e.key) == key ||
                                     (ek != null && key.equals(ek)))) {
                                    oldVal = e.val;
                                    if (!onlyIfAbsent)
                                        e.val = value;
                                    break;
                                }
                                Node<K,V> pred = e;
                                if ((e = e.next) == null) {
                                    pred.next = new Node<K,V>(hash, key, value);
                                    break;
                                }
                            }
                        }
                        else if (f instanceof TreeBin) {
                            Node<K,V> p;
                            binCount = 2;
                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,
                                                           value)) != null) {
                                oldVal = p.val;
                                if (!onlyIfAbsent)
                                    p.val = value;
                            }
                        }
                        else if (f instanceof ReservationNode)
                            throw new IllegalStateException("Recursive update");
                    }
                }
                if (binCount != 0) {
                    if (binCount >= TREEIFY_THRESHOLD)
                        treeifyBin(tab, i);
                    if (oldVal != null)
                        return oldVal;
                    break;
                }
            }
        }
        addCount(1L, binCount);
        return null;
    }

    private final void addCount(long x, int check) {
        CounterCell[] cs; long b, s;
        if ((cs = counterCells) != null ||
            !U.compareAndSetLong(this, BASECOUNT, b = baseCount, s = b + x)) {
            CounterCell c; long v; int m;
            boolean uncontended = true;
            if (cs == null || (m = cs.length - 1) < 0 ||
                (c = cs[ThreadLocalRandom.getProbe() & m]) == null ||
                !(uncontended =
                  U.compareAndSetLong(c, CELLVALUE, v = c.value, v + x))) {
                fullAddCount(x, uncontended);
                return;
            }
            if (check <= 1)
                return;
            s = sumCount();
        }
        if (check >= 0) {
            Node<K,V>[] tab, nt; int n, sc;
            while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&
                   (n = tab.length) < MAXIMUM_CAPACITY) {
                int rs = resizeStamp(n) << RESIZE_STAMP_SHIFT;
                if (sc < 0) {
                    if (sc == rs + MAX_RESIZERS || sc == rs + 1 ||
                        (nt = nextTable) == null || transferIndex <= 0)
                        break;
                    if (U.compareAndSetInt(this, SIZECTL, sc, sc + 1))
                        transfer(tab, nt);
                }
                else if (U.compareAndSetInt(this, SIZECTL, sc, rs + 2))
                    transfer(tab, null);
                s = sumCount();
            }
        }
    }

 ^jKzRtqql

获取hash ^W32HTwT1

数组未初始化，先初始化 ^HTHJGQWf

hash对应下标位置不存在数据，cas设置 ^KzgloNBK

正在做rehash，帮助迁移 ^shzX0Lxt

当前节点存在，直接返回，（只允许节点不存在时操作） ^URLgwNC8

头节点加锁 ^0AeifAXd

recheck头节点是否变化，防止其他线程并发修改 ^16FCdxP0

遍历链表，找到就覆盖原值（如果允许），没找到就放末尾（尾插法） ^gwUqg7RF

元素在链表什么位置🤔 ^CyilUEhl

树节点 ^Xd6ZclDY

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "jKzRtqql",
			"type": "text",
			"x": -222.95419311523438,
			"y": -275.28196716308594,
			"width": 937.5,
			"height": 2664,
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
			"seed": 579666095,
			"version": 22,
			"versionNonce": 895366735,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704699632517,
			"link": null,
			"locked": false,
			"text": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n\n    private final void addCount(long x, int check) {\n        CounterCell[] cs; long b, s;\n        if ((cs = counterCells) != null ||\n            !U.compareAndSetLong(this, BASECOUNT, b = baseCount, s = b + x)) {\n            CounterCell c; long v; int m;\n            boolean uncontended = true;\n            if (cs == null || (m = cs.length - 1) < 0 ||\n                (c = cs[ThreadLocalRandom.getProbe() & m]) == null ||\n                !(uncontended =\n                  U.compareAndSetLong(c, CELLVALUE, v = c.value, v + x))) {\n                fullAddCount(x, uncontended);\n                return;\n            }\n            if (check <= 1)\n                return;\n            s = sumCount();\n        }\n        if (check >= 0) {\n            Node<K,V>[] tab, nt; int n, sc;\n            while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&\n                   (n = tab.length) < MAXIMUM_CAPACITY) {\n                int rs = resizeStamp(n) << RESIZE_STAMP_SHIFT;\n                if (sc < 0) {\n                    if (sc == rs + MAX_RESIZERS || sc == rs + 1 ||\n                        (nt = nextTable) == null || transferIndex <= 0)\n                        break;\n                    if (U.compareAndSetInt(this, SIZECTL, sc, sc + 1))\n                        transfer(tab, nt);\n                }\n                else if (U.compareAndSetInt(this, SIZECTL, sc, rs + 2))\n                    transfer(tab, null);\n                s = sumCount();\n            }\n        }\n    }\n\n",
			"rawText": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n\n    private final void addCount(long x, int check) {\n        CounterCell[] cs; long b, s;\n        if ((cs = counterCells) != null ||\n            !U.compareAndSetLong(this, BASECOUNT, b = baseCount, s = b + x)) {\n            CounterCell c; long v; int m;\n            boolean uncontended = true;\n            if (cs == null || (m = cs.length - 1) < 0 ||\n                (c = cs[ThreadLocalRandom.getProbe() & m]) == null ||\n                !(uncontended =\n                  U.compareAndSetLong(c, CELLVALUE, v = c.value, v + x))) {\n                fullAddCount(x, uncontended);\n                return;\n            }\n            if (check <= 1)\n                return;\n            s = sumCount();\n        }\n        if (check >= 0) {\n            Node<K,V>[] tab, nt; int n, sc;\n            while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&\n                   (n = tab.length) < MAXIMUM_CAPACITY) {\n                int rs = resizeStamp(n) << RESIZE_STAMP_SHIFT;\n                if (sc < 0) {\n                    if (sc == rs + MAX_RESIZERS || sc == rs + 1 ||\n                        (nt = nextTable) == null || transferIndex <= 0)\n                        break;\n                    if (U.compareAndSetInt(this, SIZECTL, sc, sc + 1))\n                        transfer(tab, nt);\n                }\n                else if (U.compareAndSetInt(this, SIZECTL, sc, rs + 2))\n                    transfer(tab, null);\n                s = sumCount();\n            }\n        }\n    }\n\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 2658,
			"containerId": null,
			"originalText": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n\n    private final void addCount(long x, int check) {\n        CounterCell[] cs; long b, s;\n        if ((cs = counterCells) != null ||\n            !U.compareAndSetLong(this, BASECOUNT, b = baseCount, s = b + x)) {\n            CounterCell c; long v; int m;\n            boolean uncontended = true;\n            if (cs == null || (m = cs.length - 1) < 0 ||\n                (c = cs[ThreadLocalRandom.getProbe() & m]) == null ||\n                !(uncontended =\n                  U.compareAndSetLong(c, CELLVALUE, v = c.value, v + x))) {\n                fullAddCount(x, uncontended);\n                return;\n            }\n            if (check <= 1)\n                return;\n            s = sumCount();\n        }\n        if (check >= 0) {\n            Node<K,V>[] tab, nt; int n, sc;\n            while (s >= (long)(sc = sizeCtl) && (tab = table) != null &&\n                   (n = tab.length) < MAXIMUM_CAPACITY) {\n                int rs = resizeStamp(n) << RESIZE_STAMP_SHIFT;\n                if (sc < 0) {\n                    if (sc == rs + MAX_RESIZERS || sc == rs + 1 ||\n                        (nt = nextTable) == null || transferIndex <= 0)\n                        break;\n                    if (U.compareAndSetInt(this, SIZECTL, sc, sc + 1))\n                        transfer(tab, nt);\n                }\n                else if (U.compareAndSetInt(this, SIZECTL, sc, rs + 2))\n                    transfer(tab, null);\n                s = sumCount();\n            }\n        }\n    }\n\n",
			"lineHeight": 1.2
		},
		{
			"id": "JC5UXtxz0YMpHtR7lqHoF",
			"type": "rectangle",
			"x": 280.2745853392924,
			"y": -108.97579058800989,
			"width": 83.89960819927865,
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
			"seed": 1771378383,
			"version": 168,
			"versionNonce": 1431114785,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "W32HTwT1"
				}
			],
			"updated": 1704701600422,
			"link": null,
			"locked": false
		},
		{
			"id": "W32HTwT1",
			"type": "text",
			"x": 287.4743894389317,
			"y": -103.97579058800989,
			"width": 69.5,
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
			"seed": 1038731759,
			"version": 179,
			"versionNonce": 968353793,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701600422,
			"link": null,
			"locked": false,
			"text": "获取hash",
			"rawText": "获取hash",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "JC5UXtxz0YMpHtR7lqHoF",
			"originalText": "获取hash",
			"lineHeight": 1.2
		},
		{
			"id": "HuJqqbK8ptBKIGSfJAeFB",
			"type": "rectangle",
			"x": 175.90360763299694,
			"y": 6.963086756212192,
			"width": 192.43978196459506,
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
			"seed": 1330125665,
			"version": 303,
			"versionNonce": 1768288321,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "HTHJGQWf"
				}
			],
			"updated": 1704701608374,
			"link": null,
			"locked": false
		},
		{
			"id": "HTHJGQWf",
			"type": "text",
			"x": 184.12349861529447,
			"y": 11.963086756212192,
			"width": 176,
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
			"seed": 950799759,
			"version": 289,
			"versionNonce": 1788887073,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701608374,
			"link": null,
			"locked": false,
			"text": "数组未初始化，先初始化",
			"rawText": "数组未初始化，先初始化",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "HuJqqbK8ptBKIGSfJAeFB",
			"originalText": "数组未初始化，先初始化",
			"lineHeight": 1.2
		},
		{
			"id": "yiaSXNjAFQSEiHjVvrbbF",
			"type": "rectangle",
			"x": -210.77706697950453,
			"y": 61.157155470694875,
			"width": 162.94276551412872,
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
			"seed": 1214283855,
			"version": 212,
			"versionNonce": 29212143,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "KzgloNBK"
				}
			],
			"updated": 1704701423594,
			"link": null,
			"locked": false
		},
		{
			"id": "KzgloNBK",
			"type": "text",
			"x": -204.05568422244016,
			"y": 66.45715547069487,
			"width": 149.5,
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
			"seed": 1984119183,
			"version": 160,
			"versionNonce": 105785359,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701423594,
			"link": null,
			"locked": false,
			"text": "hash对应下标位置不\n存在数据，cas设置",
			"rawText": "hash对应下标位置不存在数据，cas设置",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "yiaSXNjAFQSEiHjVvrbbF",
			"originalText": "hash对应下标位置不存在数据，cas设置",
			"lineHeight": 1.2
		},
		{
			"id": "fq7-Z6VgTEbjVVaSJQcUA",
			"type": "rectangle",
			"x": 312.8185111404315,
			"y": 136.75476472301705,
			"width": 140.70991032089012,
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
			"seed": 1702658081,
			"version": 105,
			"versionNonce": 172239,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "shzX0Lxt"
				}
			],
			"updated": 1704701513653,
			"link": null,
			"locked": false
		},
		{
			"id": "shzX0Lxt",
			"type": "text",
			"x": 323.04846630087656,
			"y": 142.05476472301706,
			"width": 120.25,
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
			"seed": 1130779457,
			"version": 38,
			"versionNonce": 1345626159,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701520675,
			"link": null,
			"locked": false,
			"text": "正在做rehash，\n帮助迁移",
			"rawText": "正在做rehash，帮助迁移",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "fq7-Z6VgTEbjVVaSJQcUA",
			"originalText": "正在做rehash，帮助迁移",
			"lineHeight": 1.2
		},
		{
			"id": "w_HDEWu2YAZu-86Mve4Pr",
			"type": "rectangle",
			"x": 320.1398616665085,
			"y": 254.83739656403634,
			"width": 222.76948541063823,
			"height": 49.453393649481654,
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
			"seed": 2054192815,
			"version": 168,
			"versionNonce": 1700334543,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "URLgwNC8"
				}
			],
			"updated": 1704701722284,
			"link": null,
			"locked": false
		},
		{
			"id": "URLgwNC8",
			"type": "text",
			"x": 327.52460437182765,
			"y": 260.3640933887772,
			"width": 208,
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
			"seed": 239524047,
			"version": 257,
			"versionNonce": 1145326127,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701722284,
			"link": null,
			"locked": false,
			"text": "当前节点存在，直接返回，（\n只允许节点不存在时操作）",
			"rawText": "当前节点存在，直接返回，（只允许节点不存在时操作）",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "w_HDEWu2YAZu-86Mve4Pr",
			"originalText": "当前节点存在，直接返回，（只允许节点不存在时操作）",
			"lineHeight": 1.2
		},
		{
			"id": "Z39o21F7AzP-ZxOqohyVo",
			"type": "rectangle",
			"x": 185.75084095227123,
			"y": 343.852212499305,
			"width": 90.34696056260032,
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
			"seed": 1284295279,
			"version": 113,
			"versionNonce": 1428767073,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "0AeifAXd"
				}
			],
			"updated": 1704701763374,
			"link": null,
			"locked": false
		},
		{
			"id": "0AeifAXd",
			"type": "text",
			"x": 190.9243212335714,
			"y": 348.852212499305,
			"width": 80,
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
			"seed": 725566127,
			"version": 68,
			"versionNonce": 1612475713,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704701763375,
			"link": null,
			"locked": false,
			"text": "头节点加锁",
			"rawText": "头节点加锁",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "Z39o21F7AzP-ZxOqohyVo",
			"originalText": "头节点加锁",
			"lineHeight": 1.2
		},
		{
			"id": "kxWqZsy3jxxrw6O_SwWSZ",
			"type": "rectangle",
			"x": 297.0720712509077,
			"y": 360.52850493577915,
			"width": 194.126092567686,
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
			"seed": 1656803585,
			"version": 206,
			"versionNonce": 1201432015,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "16FCdxP0"
				}
			],
			"updated": 1704702042813,
			"link": null,
			"locked": false
		},
		{
			"id": "16FCdxP0",
			"type": "text",
			"x": 305.3226175347507,
			"y": 365.82850493577916,
			"width": 177.625,
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
			"seed": 635908993,
			"version": 316,
			"versionNonce": 231527407,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702042813,
			"link": null,
			"locked": false,
			"text": "recheck头节点是否变化\n，防止其他线程并发修改",
			"rawText": "recheck头节点是否变化，防止其他线程并发修改",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "kxWqZsy3jxxrw6O_SwWSZ",
			"originalText": "recheck头节点是否变化，防止其他线程并发修改",
			"lineHeight": 1.2
		},
		{
			"id": "kMP55H0TyjSeiV1cy0bib",
			"type": "rectangle",
			"x": 86.86644994405003,
			"y": 443.56352942196617,
			"width": 635.4925652468509,
			"height": 381.5300573046439,
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
			"seed": 159352399,
			"version": 122,
			"versionNonce": 1021547375,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702243877,
			"link": null,
			"locked": false
		},
		{
			"id": "aQMuY5rj9Z8dJIu4U16kV",
			"type": "rectangle",
			"x": 455.37084331181165,
			"y": 580.6750825160207,
			"width": 188.44580708064734,
			"height": 68,
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
			"seed": 1448788911,
			"version": 231,
			"versionNonce": 2096602145,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "gwUqg7RF"
				}
			],
			"updated": 1704702369340,
			"link": null,
			"locked": false
		},
		{
			"id": "gwUqg7RF",
			"type": "text",
			"x": 461.5937468521353,
			"y": 585.8750825160207,
			"width": 176,
			"height": 57.599999999999994,
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
			"seed": 248650031,
			"version": 258,
			"versionNonce": 449933313,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702369340,
			"link": null,
			"locked": false,
			"text": "遍历链表，找到就覆盖原\n值（如果允许），没找到\n就放末尾（尾插法）",
			"rawText": "遍历链表，找到就覆盖原值（如果允许），没找到就放末尾（尾插法）",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 54,
			"containerId": "aQMuY5rj9Z8dJIu4U16kV",
			"originalText": "遍历链表，找到就覆盖原值（如果允许），没找到就放末尾（尾插法）",
			"lineHeight": 1.2
		},
		{
			"id": "GhR7teOuyzsRnbOvu386V",
			"type": "rectangle",
			"x": 526.6802409996707,
			"y": 477.87990101490504,
			"width": 108.30860640643846,
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
			"seed": 614736833,
			"version": 44,
			"versionNonce": 383761473,
			"isDeleted": false,
			"boundElements": [
				{
					"id": "9d0xOvpityzplCHdx1cos",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "CyilUEhl"
				}
			],
			"updated": 1704702532670,
			"link": null,
			"locked": false
		},
		{
			"id": "CyilUEhl",
			"type": "text",
			"x": 532.8345442028899,
			"y": 483.17990101490506,
			"width": 96,
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
			"seed": 1761329103,
			"version": 33,
			"versionNonce": 775708673,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702532670,
			"link": null,
			"locked": false,
			"text": "元素在链表什\n么位置🤔",
			"rawText": "元素在链表什么位置🤔",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 34,
			"containerId": "GhR7teOuyzsRnbOvu386V",
			"originalText": "元素在链表什么位置🤔",
			"lineHeight": 1.2
		},
		{
			"id": "9d0xOvpityzplCHdx1cos",
			"type": "arrow",
			"x": 474.2775874904878,
			"y": 463.5224437361264,
			"width": 50.231850033876526,
			"height": 40.10676608258211,
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
			"seed": 489377473,
			"version": 45,
			"versionNonce": 1593871393,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702532670,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					50.231850033876526,
					40.10676608258211
				]
			],
			"lastCommittedPoint": null,
			"startBinding": null,
			"endBinding": {
				"elementId": "GhR7teOuyzsRnbOvu386V",
				"gap": 2.170803475306343,
				"focus": -0.6823457263147499
			},
			"startArrowhead": null,
			"endArrowhead": "triangle"
		},
		{
			"id": "F9h7KbmI8K1LepAFRNE3n",
			"type": "rectangle",
			"x": 442.20368160163434,
			"y": 856.2318087302881,
			"width": 94.01719078947588,
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
			"seed": 773002177,
			"version": 28,
			"versionNonce": 2129954351,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "Xd6ZclDY"
				}
			],
			"updated": 1704702555191,
			"link": null,
			"locked": false
		},
		{
			"id": "Xd6ZclDY",
			"type": "text",
			"x": 465.2122769963723,
			"y": 861.2318087302881,
			"width": 48,
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
			"seed": 1488170081,
			"version": 10,
			"versionNonce": 36530031,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702559910,
			"link": null,
			"locked": false,
			"text": "树节点",
			"rawText": "树节点",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "F9h7KbmI8K1LepAFRNE3n",
			"originalText": "树节点",
			"lineHeight": 1.2
		},
		{
			"id": "HW_6O3kTTdhHJVI__0n-l",
			"type": "rectangle",
			"x": 425.10957026854385,
			"y": 1215.5939283901166,
			"width": 131.99704792893465,
			"height": 37.53035229658826,
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
			"seed": 1245681583,
			"version": 56,
			"versionNonce": 1607428239,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704702696530,
			"link": null,
			"locked": false
		},
		{
			"id": "Uq0EikE9",
			"type": "text",
			"x": -256.6957702636719,
			"y": -310.13511657714844,
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
			"seed": 1720290881,
			"version": 2,
			"versionNonce": 1116803183,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704642161180,
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
			"id": "yIdLiDpt",
			"type": "text",
			"x": 324.6766253950148,
			"y": -101.57579058800988,
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
			"seed": 1456716623,
			"version": 4,
			"versionNonce": 913805487,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704701596127,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "center",
			"verticalAlign": "middle",
			"baseline": 15,
			"containerId": "JC5UXtxz0YMpHtR7lqHoF",
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
		"currentItemEndArrowhead": "triangle",
		"scrollX": 275.81967301772977,
		"scrollY": -185.41635129731733,
		"zoom": {
			"value": 0.8687402057647708
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