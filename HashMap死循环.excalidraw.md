---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==

# Text Elements
0 ^mpL20mcV

1 ^nEo8hgx1

B ^rtvo2CB8

新HashMap ^bgE6D20t

0 ^EAlLNyop

1 ^U8Tjuz35

A ^AhKik6NV

B ^y8tSDjie

旧HashMap ^z3KzLYKs

T2 ^QMuXSG3z

T2 ^OvUrGHS3

T1.next ^w0UMfDnQ

T2.next ^YGshXhPZ

T1 ^2KRqemJy

T2.next ^mf5B9GoO

0 ^5ZcApj2D

1 ^Xk3peiqC

B ^D0LC4NCQ

A ^SPJdexqy

新HashMap ^IpF78fyr

T2 ^xiuTsmpr

T2.next ^jspySMHs

A ^lMCKtQ64

e.next = (Entry<K,V>)newTable[i]; ^ObOD1zPR

第一步：线程启动，有线程T1和线程T2都准备对HashMap进行扩容操作，
此时T1和T2指向的都是链表的头节点A，而T1和T2的下一个节点分别是
T1.next和T2.next，它们都指向B节点。 ^xchAwSDe

第二步：开始扩容，这时候，假设线程T2的时间片用完，进入了休眠状态，
而线程T1开始执行扩容操作，一直到线程T1扩容完成后，线程T2才被唤醒。 ^g5h8y1z2

当线程T1执行完成之后，线程T2恢复执行时，死循环就发生了。 ^JcTbnruU

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"type": "rectangle",
			"version": 486,
			"versionNonce": 2113732769,
			"isDeleted": false,
			"id": "A2xukiwcxAXWd5SiYNH5y",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -109.8125,
			"y": 119.51017761230469,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 20060870,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "EAlLNyop"
				}
			],
			"updated": 1704520933055,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 403,
			"versionNonce": 663316431,
			"isDeleted": false,
			"id": "EAlLNyop",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -94.41167449951172,
			"y": 127.49949645996094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.759994506835938,
			"height": 25,
			"seed": 1928253958,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704520933055,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "0",
			"rawText": "0",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "A2xukiwcxAXWd5SiYNH5y",
			"originalText": "0",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 793,
			"versionNonce": 246826113,
			"isDeleted": false,
			"id": "t6zzmx7MMCto_j522rzII",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -65.23846435546875,
			"y": 119.51017761230469,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1385341254,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "U8Tjuz35"
				}
			],
			"updated": 1704520933056,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 403,
			"versionNonce": 751408623,
			"isDeleted": false,
			"id": "U8Tjuz35",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -45.667640686035156,
			"y": 127.49949645996094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 5.4199981689453125,
			"height": 25,
			"seed": 58361990,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704520933056,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "1",
			"rawText": "1",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "t6zzmx7MMCto_j522rzII",
			"originalText": "1",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 583,
			"versionNonce": 1666976865,
			"isDeleted": false,
			"id": "8IyPVG0aawEVrox4QJS4G",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -17.49322509765625,
			"y": 119.51017761230469,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1969457094,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "e-npUITNZVz-teseyYlUL",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "AhKik6NV"
				}
			],
			"updated": 1704520933056,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 413,
			"versionNonce": 304021519,
			"isDeleted": false,
			"id": "AhKik6NV",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -1.77239990234375,
			"y": 127.49949645996094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.1199951171875,
			"height": 25,
			"seed": 409640710,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704520933056,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "A",
			"rawText": "A",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "8IyPVG0aawEVrox4QJS4G",
			"originalText": "A",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 640,
			"versionNonce": 890666310,
			"isDeleted": false,
			"id": "h53A9Xgho_DPxFSdJqVtX",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -15.54998779296875,
			"y": 195.1725616455078,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1359793734,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "L5E1JW3FIql1iTLcDoPZY",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "y8tSDjie"
				}
			],
			"updated": 1702997748319,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 472,
			"versionNonce": 730817050,
			"isDeleted": false,
			"id": "y8tSDjie",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -0.5391616821289062,
			"y": 203.16188049316406,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 14.539993286132812,
			"height": 25,
			"seed": 1814500742,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748319,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "B",
			"rawText": "B",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "h53A9Xgho_DPxFSdJqVtX",
			"originalText": "B",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 1185,
			"versionNonce": 1451950415,
			"isDeleted": false,
			"id": "nyfeAIxweyHi_skpKrUEF",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 12.059706444528214,
			"y": 165.39842224121094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 2.6532960103445618,
			"height": 28.774139404296875,
			"seed": 780696390,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923018,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "8IyPVG0aawEVrox4QJS4G",
				"gap": 4.293853759765625,
				"focus": -0.3954210822913199
			},
			"endBinding": {
				"elementId": "h53A9Xgho_DPxFSdJqVtX",
				"gap": 1,
				"focus": 0.028713953469260072
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-2.6532960103445618,
					28.774139404296875
				]
			]
		},
		{
			"type": "text",
			"version": 116,
			"versionNonce": 946446426,
			"isDeleted": false,
			"id": "z3KzLYKs",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -105.86273193359375,
			"y": 82.23388671875,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 102.03125,
			"height": 24,
			"seed": 1563091398,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "旧HashMap",
			"rawText": "旧HashMap",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "旧HashMap",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "rectangle",
			"version": 628,
			"versionNonce": 348301894,
			"isDeleted": false,
			"id": "C7OnQ2npThxpINuOgcB8u",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 82.5726318359375,
			"y": 81.96669006347656,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1920201306,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "2KRqemJy"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 461,
			"versionNonce": 231245082,
			"isDeleted": false,
			"id": "2KRqemJy",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 94.10346221923828,
			"y": 89.95600891113281,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 21.499984741210938,
			"height": 25,
			"seed": 1991808794,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T1",
			"rawText": "T1",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "C7OnQ2npThxpINuOgcB8u",
			"originalText": "T1",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 632,
			"versionNonce": 1958834566,
			"isDeleted": false,
			"id": "7WNA9uUUyg1ZclX6DMN15",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 82.68670654296875,
			"y": 157.1182403564453,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 288665478,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "e-npUITNZVz-teseyYlUL",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "OvUrGHS3"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 466,
			"versionNonce": 1198313946,
			"isDeleted": false,
			"id": "OvUrGHS3",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 89.80754089355469,
			"y": 165.10755920410156,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 30.319976806640625,
			"height": 25,
			"seed": 926994118,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2",
			"rawText": "T2",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "7WNA9uUUyg1ZclX6DMN15",
			"originalText": "T2",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 348,
			"versionNonce": 75784655,
			"isDeleted": false,
			"id": "r7yXt0iFyzJeQ_RK6Y1ro",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 73.602783203125,
			"y": 108.01638364434358,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 45.38653564453125,
			"height": 33.1221498109626,
			"seed": 1560741382,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923021,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "C7OnQ2npThxpINuOgcB8u",
				"gap": 8.9698486328125,
				"focus": 0.469395907881753
			},
			"endBinding": {
				"elementId": "8IyPVG0aawEVrox4QJS4G",
				"gap": 1.1478271484375,
				"focus": 0.4794915104395872
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-45.38653564453125,
					33.1221498109626
				]
			]
		},
		{
			"type": "arrow",
			"version": 314,
			"versionNonce": 337869327,
			"isDeleted": false,
			"id": "e-npUITNZVz-teseyYlUL",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 80.15167236328125,
			"y": 177.26776123046878,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 51.1175537109375,
			"height": 32.16552734375003,
			"seed": 864310598,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923023,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "7WNA9uUUyg1ZclX6DMN15",
				"gap": 2.5350341796875,
				"focus": -0.4426465665551028
			},
			"endBinding": {
				"elementId": "8IyPVG0aawEVrox4QJS4G",
				"gap": 1.9656982421875,
				"focus": -0.3120898706298264
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-51.1175537109375,
					-32.16552734375003
				]
			]
		},
		{
			"type": "rectangle",
			"version": 846,
			"versionNonce": 835405830,
			"isDeleted": false,
			"id": "Gg_z63XzthT85lpGPR1X0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -190.85760498046875,
			"y": 189.84339904785156,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 99.91644287109375,
			"height": 35,
			"seed": 1142275206,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "KEYeghz8E26ZBufrjqTCL",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "w0UMfDnQ"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 911,
			"versionNonce": 426877786,
			"isDeleted": false,
			"id": "w0UMfDnQ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -175.79935455322266,
			"y": 194.84339904785156,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 69.79994201660156,
			"height": 25,
			"seed": 1685601222,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T1.next",
			"rawText": "T1.next",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Gg_z63XzthT85lpGPR1X0",
			"originalText": "T1.next",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 888,
			"versionNonce": 712005446,
			"isDeleted": false,
			"id": "O_5dh8-2Z2BiTPEgt6blR",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -193.876708984375,
			"y": 253.0416717529297,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 99.91644287109375,
			"height": 35,
			"seed": 1191999302,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "L5E1JW3FIql1iTLcDoPZY",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "mf5B9GoO"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 955,
			"versionNonce": 886581274,
			"isDeleted": false,
			"id": "mf5B9GoO",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -183.22845458984375,
			"y": 258.0416717529297,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 78.61993408203125,
			"height": 25,
			"seed": 910794374,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2.next",
			"rawText": "T2.next",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "O_5dh8-2Z2BiTPEgt6blR",
			"originalText": "T2.next",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 184,
			"versionNonce": 417816623,
			"isDeleted": false,
			"id": "KEYeghz8E26ZBufrjqTCL",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -86.69079589843749,
			"y": 207.64538574218753,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 71.49963378906249,
			"height": 9.331024169921847,
			"seed": 2072529286,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923026,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Gg_z63XzthT85lpGPR1X0",
				"gap": 4.2503662109375,
				"focus": -0.2819542937874159
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
					71.49963378906249,
					9.331024169921847
				]
			]
		},
		{
			"type": "arrow",
			"version": 274,
			"versionNonce": 400711791,
			"isDeleted": false,
			"id": "L5E1JW3FIql1iTLcDoPZY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -92.96026611328125,
			"y": 271.3054264727815,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 74.6112060546875,
			"height": 49.085483624197195,
			"seed": 135342278,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923030,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "O_5dh8-2Z2BiTPEgt6blR",
				"gap": 1,
				"focus": 0.6802429910018469
			},
			"endBinding": {
				"elementId": "h53A9Xgho_DPxFSdJqVtX",
				"gap": 2.799072265625,
				"focus": 0.28285329115624214
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					74.6112060546875,
					-49.085483624197195
				]
			]
		},
		{
			"type": "rectangle",
			"version": 533,
			"versionNonce": 1842068934,
			"isDeleted": false,
			"id": "J0m6_bKcb5szm9iytSj_R",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -102.3936767578125,
			"y": 391.6623077392578,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 664282138,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "5ZcApj2D"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 450,
			"versionNonce": 1949067674,
			"isDeleted": false,
			"id": "5ZcApj2D",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -86.99285125732422,
			"y": 399.65162658691406,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.759994506835938,
			"height": 25,
			"seed": 1676710106,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "0",
			"rawText": "0",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "J0m6_bKcb5szm9iytSj_R",
			"originalText": "0",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 840,
			"versionNonce": 39589126,
			"isDeleted": false,
			"id": "LJkEBR9CXZSt0yKB3za_z",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -57.81964111328125,
			"y": 391.50953674316406,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 2082301338,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "Xk3peiqC"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 450,
			"versionNonce": 1893738074,
			"isDeleted": false,
			"id": "Xk3peiqC",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -38.248817443847656,
			"y": 399.4988555908203,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 5.4199981689453125,
			"height": 25,
			"seed": 1230086746,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "1",
			"rawText": "1",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "LJkEBR9CXZSt0yKB3za_z",
			"originalText": "1",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 744,
			"versionNonce": 1421020038,
			"isDeleted": false,
			"id": "CizphIFFZb_q9mNwMo2QI",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -13.68585205078125,
			"y": 391.5983428955078,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1803077786,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "HgFTxlqUMtwxGXdQYugFM",
					"type": "arrow"
				},
				{
					"id": "Dz_MiNVN_z-2156iKXEmV",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "D0LC4NCQ"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 574,
			"versionNonce": 1283053530,
			"isDeleted": false,
			"id": "D0LC4NCQ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1.3249740600585938,
			"y": 399.58766174316406,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 14.539993286132812,
			"height": 25,
			"seed": 491300186,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "B",
			"rawText": "B",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "CizphIFFZb_q9mNwMo2QI",
			"originalText": "B",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 806,
			"versionNonce": 848538310,
			"isDeleted": false,
			"id": "FA6vxS-VBp9Y5ECVSY18e",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -13.97283935546875,
			"y": 463.9324188232422,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1786702362,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "HgFTxlqUMtwxGXdQYugFM",
					"type": "arrow"
				},
				{
					"id": "PX27ozN6Aie-ScgAsMkd3",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "SPJdexqy"
				}
			],
			"updated": 1702997748320,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 642,
			"versionNonce": 837143706,
			"isDeleted": false,
			"id": "SPJdexqy",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 1.74798583984375,
			"y": 471.92173767089844,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.1199951171875,
			"height": 25,
			"seed": 1779341018,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "A",
			"rawText": "A",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "FA6vxS-VBp9Y5ECVSY18e",
			"originalText": "A",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 1480,
			"versionNonce": 259064111,
			"isDeleted": false,
			"id": "HgFTxlqUMtwxGXdQYugFM",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 8.3236083984375,
			"y": 435.7106170654297,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 1.413340742112001,
			"height": 25.832672119140625,
			"seed": 2015766618,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923038,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "CizphIFFZb_q9mNwMo2QI",
				"gap": 3.133636474609375,
				"focus": -0.04363257889239089
			},
			"endBinding": {
				"elementId": "FA6vxS-VBp9Y5ECVSY18e",
				"gap": 2.389129638671875,
				"focus": -0.11321455876131244
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-1.413340742112001,
					25.832672119140625
				]
			]
		},
		{
			"type": "text",
			"version": 250,
			"versionNonce": 617623878,
			"isDeleted": false,
			"id": "IpF78fyr",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -197.150390625,
			"y": 349.157470703125,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 102.03125,
			"height": 24,
			"seed": 884215066,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748320,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "新HashMap",
			"rawText": "新HashMap",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "新HashMap",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 252,
			"versionNonce": 859836911,
			"isDeleted": false,
			"id": "Dz_MiNVN_z-2156iKXEmV",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 113.17938232421875,
			"y": 413.10003662109375,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 74.66290283203125,
			"height": 4.06866455078125,
			"seed": 396843482,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923042,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "fHNr3RBZJOG9JxKh9Vvdi",
				"gap": 5.86383056640625,
				"focus": -0.2068840206300012
			},
			"endBinding": {
				"elementId": "CizphIFFZb_q9mNwMo2QI",
				"gap": 7.64068603515625,
				"focus": -0.21594808485292602
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-74.66290283203125,
					-4.06866455078125
				]
			]
		},
		{
			"type": "arrow",
			"version": 308,
			"versionNonce": 1476096431,
			"isDeleted": false,
			"id": "PX27ozN6Aie-ScgAsMkd3",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 119.2806396484375,
			"y": 486.67474365234375,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 79.65203857421875,
			"height": 1.3792724609375,
			"seed": 1248922,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923040,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cIehuLeBgcOhclCh9mmz9",
				"gap": 10.0782470703125,
				"focus": 0.002502617025689575
			},
			"endBinding": {
				"elementId": "FA6vxS-VBp9Y5ECVSY18e",
				"gap": 9.039794921875,
				"focus": 0.015874324389093964
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-79.65203857421875,
					-1.3792724609375
				]
			]
		},
		{
			"type": "rectangle",
			"version": 812,
			"versionNonce": 1427287898,
			"isDeleted": false,
			"id": "cIehuLeBgcOhclCh9mmz9",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 129.35888671875,
			"y": 466.7980041503906,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1923754842,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "PX27ozN6Aie-ScgAsMkd3",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "xiuTsmpr"
				}
			],
			"updated": 1702997748321,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 646,
			"versionNonce": 1043729222,
			"isDeleted": false,
			"id": "xiuTsmpr",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 136.47972106933594,
			"y": 474.7873229980469,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 30.319976806640625,
			"height": 25,
			"seed": 1707061274,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748321,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2",
			"rawText": "T2",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "cIehuLeBgcOhclCh9mmz9",
			"originalText": "T2",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 1017,
			"versionNonce": 173940954,
			"isDeleted": false,
			"id": "fHNr3RBZJOG9JxKh9Vvdi",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 119.043212890625,
			"y": 394.4582977294922,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 99.91644287109375,
			"height": 35,
			"seed": 222731482,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "L5E1JW3FIql1iTLcDoPZY",
					"type": "arrow"
				},
				{
					"id": "Dz_MiNVN_z-2156iKXEmV",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "jspySMHs"
				}
			],
			"updated": 1702997748321,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1083,
			"versionNonce": 1790210502,
			"isDeleted": false,
			"id": "jspySMHs",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 129.69146728515625,
			"y": 399.4582977294922,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 78.61993408203125,
			"height": 25,
			"seed": 651293082,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997748321,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2.next",
			"rawText": "T2.next",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "fHNr3RBZJOG9JxKh9Vvdi",
			"originalText": "T2.next",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 858,
			"versionNonce": 477999578,
			"isDeleted": false,
			"id": "qWRHyiaV0NTBzjSmqvntB",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -60.13739013671875,
			"y": 693.6263580322266,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1813543706,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "mpL20mcV"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 776,
			"versionNonce": 1214146202,
			"isDeleted": false,
			"id": "mpL20mcV",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -44.73656463623047,
			"y": 701.6156768798828,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.759994506835938,
			"height": 25,
			"seed": 312911366,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "0",
			"rawText": "0",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "qWRHyiaV0NTBzjSmqvntB",
			"originalText": "0",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 1165,
			"versionNonce": 1204454234,
			"isDeleted": false,
			"id": "tGWUUDb4JNOvNC5LWUM9M",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -15.5633544921875,
			"y": 693.4735870361328,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 72222682,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "nEo8hgx1"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 776,
			"versionNonce": 1695877146,
			"isDeleted": false,
			"id": "nEo8hgx1",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 4.007469177246094,
			"y": 701.4629058837891,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 5.4199981689453125,
			"height": 25,
			"seed": 2137006662,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "1",
			"rawText": "1",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "tGWUUDb4JNOvNC5LWUM9M",
			"originalText": "1",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 1088,
			"versionNonce": 674297050,
			"isDeleted": false,
			"id": "HYqwufvn9j1IbZab5Utzr",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 30.53900146484375,
			"y": 695.1234283447266,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 2068321158,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "rtvo2CB8"
				},
				{
					"id": "H91220yMoggCREBP3yCdS",
					"type": "arrow"
				},
				{
					"id": "TXrHP4T9QamhmwKkWYFRp",
					"type": "arrow"
				},
				{
					"id": "tlXKQ34s9JQz_GUqdmFew",
					"type": "arrow"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 919,
			"versionNonce": 582044058,
			"isDeleted": false,
			"id": "rtvo2CB8",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 45.549827575683594,
			"y": 703.1127471923828,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 14.539993286132812,
			"height": 25,
			"seed": 1363653318,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "B",
			"rawText": "B",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "HYqwufvn9j1IbZab5Utzr",
			"originalText": "B",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 1155,
			"versionNonce": 117091482,
			"isDeleted": false,
			"id": "gaQHwKQzT7HlwzjbeeTod",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 30.25201416015625,
			"y": 767.4575042724609,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 1827620998,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "H91220yMoggCREBP3yCdS",
					"type": "arrow"
				},
				{
					"id": "oSKcosiay55Mov3mCnRs0",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "lMCKtQ64"
				},
				{
					"id": "tlXKQ34s9JQz_GUqdmFew",
					"type": "arrow"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 989,
			"versionNonce": 921569626,
			"isDeleted": false,
			"id": "lMCKtQ64",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 45.97283935546875,
			"y": 775.4468231201172,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 13.1199951171875,
			"height": 25,
			"seed": 1379944390,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "A",
			"rawText": "A",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "gaQHwKQzT7HlwzjbeeTod",
			"originalText": "A",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "arrow",
			"version": 2536,
			"versionNonce": 464741583,
			"isDeleted": false,
			"id": "H91220yMoggCREBP3yCdS",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 52.5484619140625,
			"y": 739.2357025146484,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 1.4126671083781304,
			"height": 22.169281005859375,
			"seed": 1545997254,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923049,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "HYqwufvn9j1IbZab5Utzr",
				"gap": 3.133636474609375,
				"focus": -0.052315466388622105
			},
			"endBinding": {
				"elementId": "gaQHwKQzT7HlwzjbeeTod",
				"gap": 6.052520751953125,
				"focus": -0.1309369576269448
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-1.4126671083781304,
					22.169281005859375
				]
			]
		},
		{
			"type": "text",
			"version": 577,
			"versionNonce": 645591322,
			"isDeleted": false,
			"id": "bgE6D20t",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -154.89410400390625,
			"y": 651.1215209960938,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 102.03125,
			"height": 24,
			"seed": 1163871066,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "新HashMap",
			"rawText": "新HashMap",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "新HashMap",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 1302,
			"versionNonce": 1703588783,
			"isDeleted": false,
			"id": "TXrHP4T9QamhmwKkWYFRp",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 157.40423583984375,
			"y": 716.6251220703125,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 74.66290283203125,
			"height": 4.06866455078125,
			"seed": 1997842970,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923054,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "dfKjdsrB1qWxzzrs2HqV4",
				"gap": 5.86383056640625,
				"focus": -0.2068840206300012
			},
			"endBinding": {
				"elementId": "HYqwufvn9j1IbZab5Utzr",
				"gap": 7.64068603515625,
				"focus": -0.21594808485292602
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-74.66290283203125,
					-4.06866455078125
				]
			]
		},
		{
			"type": "arrow",
			"version": 1362,
			"versionNonce": 575636335,
			"isDeleted": false,
			"id": "oSKcosiay55Mov3mCnRs0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 163.5054931640625,
			"y": 790.1998291015625,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 79.83660888671875,
			"height": 4.002997183646585,
			"seed": 348181446,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923052,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Xn5AS20Vjjq21n0jy5lT2",
				"gap": 10.0782470703125,
				"focus": -0.04674062475089477
			},
			"endBinding": {
				"elementId": "gaQHwKQzT7HlwzjbeeTod",
				"gap": 8.855224609375,
				"focus": -0.15324796625336323
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-79.83660888671875,
					-4.002997183646585
				]
			]
		},
		{
			"type": "rectangle",
			"version": 1155,
			"versionNonce": 1168871258,
			"isDeleted": false,
			"id": "Xn5AS20Vjjq21n0jy5lT2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 173.583740234375,
			"y": 770.3230895996094,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 44.5616455078125,
			"height": 40.9786376953125,
			"seed": 37140742,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "QMuXSG3z"
				},
				{
					"id": "oSKcosiay55Mov3mCnRs0",
					"type": "arrow"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 990,
			"versionNonce": 452236314,
			"isDeleted": false,
			"id": "QMuXSG3z",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 180.70457458496094,
			"y": 778.3124084472656,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 30.319976806640625,
			"height": 25,
			"seed": 507881542,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2",
			"rawText": "T2",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Xn5AS20Vjjq21n0jy5lT2",
			"originalText": "T2",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 1360,
			"versionNonce": 924702106,
			"isDeleted": false,
			"id": "dfKjdsrB1qWxzzrs2HqV4",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 163.26806640625,
			"y": 697.9833831787109,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 99.91644287109375,
			"height": 35,
			"seed": 717804294,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"id": "nyfeAIxweyHi_skpKrUEF",
					"type": "arrow"
				},
				{
					"id": "r7yXt0iFyzJeQ_RK6Y1ro",
					"type": "arrow"
				},
				{
					"id": "L5E1JW3FIql1iTLcDoPZY",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "YGshXhPZ"
				},
				{
					"id": "TXrHP4T9QamhmwKkWYFRp",
					"type": "arrow"
				}
			],
			"updated": 1702997969522,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1427,
			"versionNonce": 717492826,
			"isDeleted": false,
			"id": "YGshXhPZ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 173.91632080078125,
			"y": 702.9833831787109,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 78.61993408203125,
			"height": 25,
			"seed": 1636074054,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997969522,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "T2.next",
			"rawText": "T2.next",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "dfKjdsrB1qWxzzrs2HqV4",
			"originalText": "T2.next",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "text",
			"version": 381,
			"versionNonce": 759288838,
			"isDeleted": false,
			"id": "ObOD1zPR",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -315.09564208984375,
			"y": 598.52001953125,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 386.71875,
			"height": 24,
			"seed": 165250458,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997988770,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "e.next = (Entry<K,V>)newTable[i];",
			"rawText": "e.next = (Entry<K,V>)newTable[i];",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "e.next = (Entry<K,V>)newTable[i];",
			"lineHeight": 1.2,
			"baseline": 19
		},
		{
			"type": "arrow",
			"version": 1162,
			"versionNonce": 2016494383,
			"isDeleted": false,
			"id": "tlXKQ34s9JQz_GUqdmFew",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 27.535873341257034,
			"y": 797.0270798120832,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 160.5446824652492,
			"height": 206.14405007825457,
			"seed": 869536966,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704520923049,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "gaQHwKQzT7HlwzjbeeTod",
				"gap": 2.7161408188992198,
				"focus": -0.8652761879286792
			},
			"endBinding": {
				"elementId": "HYqwufvn9j1IbZab5Utzr",
				"gap": 1.433640411151373,
				"focus": 0.6118173949904561
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-136.28819740806878,
					-206.14405007825457
				],
				[
					24.256485057180416,
					-103.33729187850804
				]
			]
		},
		{
			"type": "text",
			"version": 78,
			"versionNonce": 1042450586,
			"isDeleted": false,
			"id": "xchAwSDe",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -381.531005859375,
			"y": 6.13958740234375,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 648.90625,
			"height": 72,
			"seed": 627408518,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997905177,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "第一步：线程启动，有线程T1和线程T2都准备对HashMap进行扩容操作，\n此时T1和T2指向的都是链表的头节点A，而T1和T2的下一个节点分别是\nT1.next和T2.next，它们都指向B节点。",
			"rawText": "第一步：线程启动，有线程T1和线程T2都准备对HashMap进行扩容操作，\n此时T1和T2指向的都是链表的头节点A，而T1和T2的下一个节点分别是\nT1.next和T2.next，它们都指向B节点。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "第一步：线程启动，有线程T1和线程T2都准备对HashMap进行扩容操作，\n此时T1和T2指向的都是链表的头节点A，而T1和T2的下一个节点分别是\nT1.next和T2.next，它们都指向B节点。",
			"lineHeight": 1.2,
			"baseline": 67
		},
		{
			"type": "text",
			"version": 101,
			"versionNonce": 1994072710,
			"isDeleted": false,
			"id": "g5h8y1z2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -374.26947021484375,
			"y": 302.595703125,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 670.3125,
			"height": 48,
			"seed": 1729240198,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997957133,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "第二步：开始扩容，这时候，假设线程T2的时间片用完，进入了休眠状态，\n而线程T1开始执行扩容操作，一直到线程T1扩容完成后，线程T2才被唤醒。",
			"rawText": "第二步：开始扩容，这时候，假设线程T2的时间片用完，进入了休眠状态，\n而线程T1开始执行扩容操作，一直到线程T1扩容完成后，线程T2才被唤醒。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "第二步：开始扩容，这时候，假设线程T2的时间片用完，进入了休眠状态，\n而线程T1开始执行扩容操作，一直到线程T1扩容完成后，线程T2才被唤醒。",
			"lineHeight": 1.2,
			"baseline": 43
		},
		{
			"type": "text",
			"version": 37,
			"versionNonce": 1017913670,
			"isDeleted": false,
			"id": "JcTbnruU",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "dashed",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -371.9083251953125,
			"y": 539.4377746582031,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 566.875,
			"height": 24,
			"seed": 254346886,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1702997985003,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "当线程T1执行完成之后，线程T2恢复执行时，死循环就发生了。",
			"rawText": "当线程T1执行完成之后，线程T2恢复执行时，死循环就发生了。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "当线程T1执行完成之后，线程T2恢复执行时，死循环就发生了。",
			"lineHeight": 1.2,
			"baseline": 19
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
		"scrollX": 491.3846618652343,
		"scrollY": 209.74511413574217,
		"zoom": {
			"value": 1.25
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