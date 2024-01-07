---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
        private void rehash(HashEntry<K,V> node) {
            HashEntry<K,V>[] oldTable = table;
            int oldCapacity = oldTable.length;
            int newCapacity = oldCapacity << 1;
            threshold = (int)(newCapacity * loadFactor);
            HashEntry<K,V>[] newTable =
                (HashEntry<K,V>[]) new HashEntry[newCapacity];
            int sizeMask = newCapacity - 1;
            for (int i = 0; i < oldCapacity ; i++) {
                HashEntry<K,V> e = oldTable[i];
                if (e != null) {
                    HashEntry<K,V> next = e.next;
                    int idx = e.hash & sizeMask;
                    if (next == null)   //  Single node on list
                        newTable[idx] = e;
                    else { // Reuse consecutive sequence at same slot
                        HashEntry<K,V> lastRun = e;
                        int lastIdx = idx;
                        for (HashEntry<K,V> last = next;
                             last != null;
                             last = last.next) {
                            int k = last.hash & sizeMask;
                            if (k != lastIdx) {
                                lastIdx = k;
                                lastRun = last;
                            }
                        }
                        newTable[lastIdx] = lastRun;
                        // Clone remaining nodes
                        for (HashEntry<K,V> p = e; p != lastRun; p = p.next) {
                            V v = p.value;
                            int h = p.hash;
                            int k = h & sizeMask;
                            HashEntry<K,V> n = newTable[k];
                            newTable[k] = new HashEntry<K,V>(h, p.key, v, n);
                        }
                    }
                }
            }
            int nodeIndex = node.hash & sizeMask; // add the new node
            node.setNext(newTable[nodeIndex]);
            newTable[nodeIndex] = node;
            table = newTable;
        }
 ^zbmDvqGP

找到最后的相同下标连续串
比如下标为：
1010111001 
lastRun=1
1010111000
lastRun=000
 ^bhBTVWOX

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"id": "zbmDvqGP",
			"type": "text",
			"x": -166.8270263671875,
			"y": -310.13511657714844,
			"width": 914.0625,
			"height": 1080,
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
			"seed": 790045153,
			"version": 15,
			"versionNonce": 1980096289,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704638641220,
			"link": null,
			"locked": false,
			"text": "        private void rehash(HashEntry<K,V> node) {\n            HashEntry<K,V>[] oldTable = table;\n            int oldCapacity = oldTable.length;\n            int newCapacity = oldCapacity << 1;\n            threshold = (int)(newCapacity * loadFactor);\n            HashEntry<K,V>[] newTable =\n                (HashEntry<K,V>[]) new HashEntry[newCapacity];\n            int sizeMask = newCapacity - 1;\n            for (int i = 0; i < oldCapacity ; i++) {\n                HashEntry<K,V> e = oldTable[i];\n                if (e != null) {\n                    HashEntry<K,V> next = e.next;\n                    int idx = e.hash & sizeMask;\n                    if (next == null)   //  Single node on list\n                        newTable[idx] = e;\n                    else { // Reuse consecutive sequence at same slot\n                        HashEntry<K,V> lastRun = e;\n                        int lastIdx = idx;\n                        for (HashEntry<K,V> last = next;\n                             last != null;\n                             last = last.next) {\n                            int k = last.hash & sizeMask;\n                            if (k != lastIdx) {\n                                lastIdx = k;\n                                lastRun = last;\n                            }\n                        }\n                        newTable[lastIdx] = lastRun;\n                        // Clone remaining nodes\n                        for (HashEntry<K,V> p = e; p != lastRun; p = p.next) {\n                            V v = p.value;\n                            int h = p.hash;\n                            int k = h & sizeMask;\n                            HashEntry<K,V> n = newTable[k];\n                            newTable[k] = new HashEntry<K,V>(h, p.key, v, n);\n                        }\n                    }\n                }\n            }\n            int nodeIndex = node.hash & sizeMask; // add the new node\n            node.setNext(newTable[nodeIndex]);\n            newTable[nodeIndex] = node;\n            table = newTable;\n        }\n",
			"rawText": "        private void rehash(HashEntry<K,V> node) {\n            HashEntry<K,V>[] oldTable = table;\n            int oldCapacity = oldTable.length;\n            int newCapacity = oldCapacity << 1;\n            threshold = (int)(newCapacity * loadFactor);\n            HashEntry<K,V>[] newTable =\n                (HashEntry<K,V>[]) new HashEntry[newCapacity];\n            int sizeMask = newCapacity - 1;\n            for (int i = 0; i < oldCapacity ; i++) {\n                HashEntry<K,V> e = oldTable[i];\n                if (e != null) {\n                    HashEntry<K,V> next = e.next;\n                    int idx = e.hash & sizeMask;\n                    if (next == null)   //  Single node on list\n                        newTable[idx] = e;\n                    else { // Reuse consecutive sequence at same slot\n                        HashEntry<K,V> lastRun = e;\n                        int lastIdx = idx;\n                        for (HashEntry<K,V> last = next;\n                             last != null;\n                             last = last.next) {\n                            int k = last.hash & sizeMask;\n                            if (k != lastIdx) {\n                                lastIdx = k;\n                                lastRun = last;\n                            }\n                        }\n                        newTable[lastIdx] = lastRun;\n                        // Clone remaining nodes\n                        for (HashEntry<K,V> p = e; p != lastRun; p = p.next) {\n                            V v = p.value;\n                            int h = p.hash;\n                            int k = h & sizeMask;\n                            HashEntry<K,V> n = newTable[k];\n                            newTable[k] = new HashEntry<K,V>(h, p.key, v, n);\n                        }\n                    }\n                }\n            }\n            int nodeIndex = node.hash & sizeMask; // add the new node\n            node.setNext(newTable[nodeIndex]);\n            newTable[nodeIndex] = node;\n            table = newTable;\n        }\n",
			"fontSize": 20,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 1075,
			"containerId": null,
			"originalText": "        private void rehash(HashEntry<K,V> node) {\n            HashEntry<K,V>[] oldTable = table;\n            int oldCapacity = oldTable.length;\n            int newCapacity = oldCapacity << 1;\n            threshold = (int)(newCapacity * loadFactor);\n            HashEntry<K,V>[] newTable =\n                (HashEntry<K,V>[]) new HashEntry[newCapacity];\n            int sizeMask = newCapacity - 1;\n            for (int i = 0; i < oldCapacity ; i++) {\n                HashEntry<K,V> e = oldTable[i];\n                if (e != null) {\n                    HashEntry<K,V> next = e.next;\n                    int idx = e.hash & sizeMask;\n                    if (next == null)   //  Single node on list\n                        newTable[idx] = e;\n                    else { // Reuse consecutive sequence at same slot\n                        HashEntry<K,V> lastRun = e;\n                        int lastIdx = idx;\n                        for (HashEntry<K,V> last = next;\n                             last != null;\n                             last = last.next) {\n                            int k = last.hash & sizeMask;\n                            if (k != lastIdx) {\n                                lastIdx = k;\n                                lastRun = last;\n                            }\n                        }\n                        newTable[lastIdx] = lastRun;\n                        // Clone remaining nodes\n                        for (HashEntry<K,V> p = e; p != lastRun; p = p.next) {\n                            V v = p.value;\n                            int h = p.hash;\n                            int k = h & sizeMask;\n                            HashEntry<K,V> n = newTable[k];\n                            newTable[k] = new HashEntry<K,V>(h, p.key, v, n);\n                        }\n                    }\n                }\n            }\n            int nodeIndex = node.hash & sizeMask; // add the new node\n            node.setNext(newTable[nodeIndex]);\n            newTable[nodeIndex] = node;\n            table = newTable;\n        }\n",
			"lineHeight": 1.2
		},
		{
			"id": "fUGIwPxAkUGib8aalf2Ye",
			"type": "rectangle",
			"x": 501.0886569552954,
			"y": 81.87892489963122,
			"width": 184.8095703125002,
			"height": 186.96854654947927,
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
			"seed": 57940367,
			"version": 109,
			"versionNonce": 1051671951,
			"isDeleted": false,
			"boundElements": [
				{
					"type": "text",
					"id": "bhBTVWOX"
				}
			],
			"updated": 1704638772868,
			"link": null,
			"locked": false
		},
		{
			"id": "bhBTVWOX",
			"type": "text",
			"x": 506.0886569552954,
			"y": 98.56319817437085,
			"width": 160,
			"height": 153.6,
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
			"seed": 374269761,
			"version": 169,
			"versionNonce": 338604975,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704638772868,
			"link": null,
			"locked": false,
			"text": "找到最后的相同下标连\n续串\n比如下标为：\n1010111001 \nlastRun=1\n1010111000\nlastRun=000\n",
			"rawText": "找到最后的相同下标连续串\n比如下标为：\n1010111001 \nlastRun=1\n1010111000\nlastRun=000\n",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "middle",
			"baseline": 149,
			"containerId": "fUGIwPxAkUGib8aalf2Ye",
			"originalText": "找到最后的相同下标连续串\n比如下标为：\n1010111001 \nlastRun=1\n1010111000\nlastRun=000\n",
			"lineHeight": 1.2
		},
		{
			"id": "XTzW-tYZodvIwblN2gGkC",
			"type": "arrow",
			"x": 511.57476806640653,
			"y": 82.94381883409301,
			"width": 26.959635416666686,
			"height": 206.2218899197049,
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
			"seed": 774425327,
			"version": 36,
			"versionNonce": 1025761135,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704638641220,
			"link": null,
			"locked": false,
			"points": [
				[
					0,
					0
				],
				[
					26.959635416666686,
					206.2218899197049
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
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 185.90800645616292,
		"scrollY": 322.4486880832247,
		"zoom": {
			"value": 0.9
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