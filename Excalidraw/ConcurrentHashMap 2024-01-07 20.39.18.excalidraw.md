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

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "hMZHCo9O",
			"type": "text",
			"x": 142.45584869384766,
			"y": -234.13511657714844,
			"width": 937.5,
			"height": 1320,
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
			"seed": 290355311,
			"version": 8,
			"versionNonce": 238290785,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704631205464,
			"link": null,
			"locked": false,
			"text": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"rawText": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 1315,
			"containerId": null,
			"originalText": "    public V put(K key, V value) {\n        Segment<K,V> s;\n        if (value == null)\n            throw new NullPointerException();\n        int hash = hash(key);\n        int j = (hash >>> segmentShift) & segmentMask;\n        if ((s = (Segment<K,V>)UNSAFE.getObject          // nonvolatile; recheck\n             (segments, (j << SSHIFT) + SBASE)) == null) //  in ensureSegment\n            s = ensureSegment(j);\n        return s.put(key, hash, value, false);\n    }\n        final V put(K key, int hash, V value, boolean onlyIfAbsent) {\n            HashEntry<K,V> node = tryLock() ? null :\n                scanAndLockForPut(key, hash, value);\n            V oldValue;\n            try {\n                HashEntry<K,V>[] tab = table;\n                int index = (tab.length - 1) & hash;\n                HashEntry<K,V> first = entryAt(tab, index);\n                for (HashEntry<K,V> e = first;;) {\n                    if (e != null) {\n                        K k;\n                        if ((k = e.key) == key ||\n                            (e.hash == hash && key.equals(k))) {\n                            oldValue = e.value;\n                            if (!onlyIfAbsent) {\n                                e.value = value;\n                                ++modCount;\n                            }\n                            break;\n                        }\n                        e = e.next;\n                    }\n                    else {\n                        if (node != null)\n                            node.setNext(first);\n                        else\n                            node = new HashEntry<K,V>(hash, key, value, first);\n                        int c = count + 1;\n                        if (c > threshold && tab.length < MAXIMUM_CAPACITY)\n                            rehash(node);\n                        else\n                            setEntryAt(tab, index, node);\n                        ++modCount;\n                        count = c;\n                        oldValue = null;\n                        break;\n                    }\n                }\n            } finally {\n                unlock();\n            }\n            return oldValue;\n        }\n",
			"lineHeight": 1.2
		},
		{
			"id": "IVuFiXLg",
			"type": "text",
			"x": 0.45584869384765625,
			"y": -305.13511657714844,
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
			"seed": 558928513,
			"version": 3,
			"versionNonce": 770147393,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704631201444,
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
			"id": "DWhXZpHS",
			"type": "text",
			"x": 198.45584869384766,
			"y": 64.86488342285156,
			"width": 11.71875,
			"height": 24,
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
			"seed": 564466831,
			"version": 2,
			"versionNonce": 1056365505,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704631203197,
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
		"scrollX": -212.16678619384766,
		"scrollY": 595.0822143554688,
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