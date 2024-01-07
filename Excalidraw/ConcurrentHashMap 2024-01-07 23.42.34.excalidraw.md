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
 ^jKzRtqql

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
			"height": 1776,
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
			"version": 19,
			"versionNonce": 2087055201,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704642170639,
			"link": null,
			"locked": false,
			"text": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n",
			"rawText": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 1770,
			"containerId": null,
			"originalText": "    public V put(K key, V value) {\n        return putVal(key, value, false);\n    }\n\n    /** Implementation for put and putIfAbsent */\n    final V putVal(K key, V value, boolean onlyIfAbsent) {\n        if (key == null || value == null) throw new NullPointerException();\n        int hash = spread(key.hashCode());\n        int binCount = 0;\n        for (Node<K,V>[] tab = table;;) {\n            Node<K,V> f; int n, i, fh; K fk; V fv;\n            if (tab == null || (n = tab.length) == 0)\n                tab = initTable();\n            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {\n                if (casTabAt(tab, i, null, new Node<K,V>(hash, key, value)))\n                    break;                   // no lock when adding to empty bin\n            }\n            else if ((fh = f.hash) == MOVED)\n                tab = helpTransfer(tab, f);\n            else if (onlyIfAbsent // check first node without acquiring lock\n                     && fh == hash\n                     && ((fk = f.key) == key || (fk != null && key.equals(fk)))\n                     && (fv = f.val) != null)\n                return fv;\n            else {\n                V oldVal = null;\n                synchronized (f) {\n                    if (tabAt(tab, i) == f) {\n                        if (fh >= 0) {\n                            binCount = 1;\n                            for (Node<K,V> e = f;; ++binCount) {\n                                K ek;\n                                if (e.hash == hash &&\n                                    ((ek = e.key) == key ||\n                                     (ek != null && key.equals(ek)))) {\n                                    oldVal = e.val;\n                                    if (!onlyIfAbsent)\n                                        e.val = value;\n                                    break;\n                                }\n                                Node<K,V> pred = e;\n                                if ((e = e.next) == null) {\n                                    pred.next = new Node<K,V>(hash, key, value);\n                                    break;\n                                }\n                            }\n                        }\n                        else if (f instanceof TreeBin) {\n                            Node<K,V> p;\n                            binCount = 2;\n                            if ((p = ((TreeBin<K,V>)f).putTreeVal(hash, key,\n                                                           value)) != null) {\n                                oldVal = p.val;\n                                if (!onlyIfAbsent)\n                                    p.val = value;\n                            }\n                        }\n                        else if (f instanceof ReservationNode)\n                            throw new IllegalStateException(\"Recursive update\");\n                    }\n                }\n                if (binCount != 0) {\n                    if (binCount >= TREEIFY_THRESHOLD)\n                        treeifyBin(tab, i);\n                    if (oldVal != null)\n                        return oldVal;\n                    break;\n                }\n            }\n        }\n        addCount(1L, binCount);\n        return null;\n    }\n",
			"lineHeight": 1.2
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
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#1e1e1e",
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
		"scrollX": 39.237152099609375,
		"scrollY": 137.08221435546875,
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