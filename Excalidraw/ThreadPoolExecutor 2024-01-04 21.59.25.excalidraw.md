---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements

 
     
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    private static final int COUNT_BITS = Integer.SIZE - 3;
    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;

    // runState is stored in the high-order bits
    private static final int RUNNING    = -1 << COUNT_BITS;
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    private static final int STOP       =  1 << COUNT_BITS;
    private static final int TIDYING    =  2 << COUNT_BITS;
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }
    private static int workerCountOf(int c)  { return c & COUNT_MASK; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }


    
   

  
 ^NPcqCzcP

Integer.SIZE=32
COUNT_BITS=29 ^r8Jy8cRC

0000 0000 0000 0000 0000 0000 0000 0001 ^9kmxIQru

1<< COUNT_BITS ^ofOIy6n7

0010 0000 0000 0000 0000 0000 0000 0000

-1

0001 1111 1111 1111 1111 1111 1111 1111

=536870911 ^7h9LiOwn

0 ^1OPjqjHo

-536870912 ^XCFyHt02

536870912 ^3wqx3EmY

1073741824 ^qYpWM769

1610612736 ^l1Md3CGB

or 操作，操作符“|” ^C1v7WYVX

定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。 ^PI9c484y

1110 0000 0000 0000 0000 0000 0000 0000 ^hdo43w4V

1000 0000 0000 0000 0000 0000 0000 0000 ^sqVkHS59

and 操作，操作符“&” ^QNnuUB83

定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。 ^daZJgN0g

100 0000 0000 0000 0000 0000 0000 0000 ^Jn4zVZDH

10 0000 0000 0000 0000 0000 0000 0000 ^orrySlph

not操作，操作符“~” ^61pEFIUS

定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。 ^1J0eQdto

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
			"version": 562,
			"versionNonce": 1187001295,
			"isDeleted": false,
			"id": "hzEQ0ow4Zc4vtwJxATPeM",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 405.26022538604906,
			"y": 13116.600094416484,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 196.01311957523475,
			"height": 44.90566677787278,
			"seed": 1064450392,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "r8Jy8cRC"
				}
			],
			"updated": 1704378253530,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 805,
			"versionNonce": 1397154287,
			"isDeleted": false,
			"id": "r8Jy8cRC",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 439.9614408009982,
			"y": 13121.763604439604,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 126.61068874533646,
			"height": 34.578646731634265,
			"seed": 1129824088,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "Integer.SIZE=32\nCOUNT_BITS=29",
			"rawText": "Integer.SIZE=32\nCOUNT_BITS=29",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "hzEQ0ow4Zc4vtwJxATPeM",
			"originalText": "Integer.SIZE=32\nCOUNT_BITS=29",
			"lineHeight": 1.2,
			"baseline": 30
		},
		{
			"type": "rectangle",
			"version": 325,
			"versionNonce": 319998991,
			"isDeleted": false,
			"id": "cO2ShusooszR-hj-UXi7k",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 38.74424924949983,
			"y": 13155.16792556504,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 307.2670417833954,
			"height": 22.48912232533016,
			"seed": 588102488,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "arrow",
					"id": "YHCkOjSZ"
				}
			],
			"updated": 1704378253530,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1062,
			"versionNonce": 135245889,
			"isDeleted": false,
			"id": "NPcqCzcP",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13071.082354963983,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 633.1539318536547,
			"height": 432.2330841454283,
			"seed": 1591035480,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378210036,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514275,
			"fontFamily": 3,
			"text": "\n \n     \n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n\n    \n   \n\n  \n",
			"rawText": "\n \n     \n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n\n    \n   \n\n  \n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "\n \n     \n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n\n    \n   \n\n  \n",
			"lineHeight": 1.2,
			"baseline": 428.00000000000006
		},
		{
			"type": "rectangle",
			"version": 700,
			"versionNonce": 155997263,
			"isDeleted": false,
			"id": "O8q29OUubiYYe-bntwt9_",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 750.2964255531929,
			"y": 13036.979514449427,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 413.40227538765663,
			"height": 223.44422016793882,
			"seed": 1773817128,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "arrow",
					"id": "YHCkOjSZ"
				}
			],
			"updated": 1704378253530,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 563,
			"versionNonce": 544928367,
			"isDeleted": false,
			"id": "9kmxIQru",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 774.0493648482968,
			"y": 13065.237186730503,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 339.52879595652234,
			"height": 17.834650872495548,
			"seed": 1103613784,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.86220906041296,
			"fontFamily": 3,
			"text": "0000 0000 0000 0000 0000 0000 0000 0001",
			"rawText": "0000 0000 0000 0000 0000 0000 0000 0001",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0000 0000 0000 0000 0000 0000 0000 0001",
			"lineHeight": 1.2,
			"baseline": 13.999999999999996
		},
		{
			"type": "text",
			"version": 563,
			"versionNonce": 812698767,
			"isDeleted": false,
			"id": "ofOIy6n7",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 774.0493648482968,
			"y": 13092.36071826576,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 121.88213188182851,
			"height": 17.834650872495548,
			"seed": 2061595176,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.86220906041296,
			"fontFamily": 3,
			"text": "1<< COUNT_BITS",
			"rawText": "1<< COUNT_BITS",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1<< COUNT_BITS",
			"lineHeight": 1.2,
			"baseline": 13.999999999999996
		},
		{
			"type": "text",
			"version": 642,
			"versionNonce": 350642863,
			"isDeleted": false,
			"id": "7h9LiOwn",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 775.3482870094233,
			"y": 13123.74454643629,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 339.52879595652234,
			"height": 124.84255610746884,
			"seed": 1281202472,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "YHCkOjSZ",
					"type": "arrow"
				}
			],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.86220906041296,
			"fontFamily": 3,
			"text": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"rawText": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"lineHeight": 1.2,
			"baseline": 120.99999999999996
		},
		{
			"type": "arrow",
			"version": 1305,
			"versionNonce": 406875489,
			"isDeleted": false,
			"id": "YHCkOjSZ",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 350.738930109788,
			"y": 13174.529419188771,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 395.9555530755264,
			"height": 4.300994082666412,
			"seed": 29075,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378254744,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cO2ShusooszR-hj-UXi7k",
				"focus": -0.13919716060582105,
				"gap": 4.7276390768927286
			},
			"endBinding": {
				"elementId": "7h9LiOwn",
				"focus": 0.16452060091211568,
				"gap": 28.65380382410899
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
					60.84728571371365,
					4.300994082666412
				],
				[
					395.9555530755264,
					2.3562301421730396
				]
			]
		},
		{
			"type": "text",
			"version": 411,
			"versionNonce": 948604175,
			"isDeleted": false,
			"id": "1OPjqjHo",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 306.9892341931905,
			"y": 13230.50162718233,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 8.442052424715396,
			"height": 17.28932336581713,
			"seed": 1763383640,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "0",
			"rawText": "0",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 366,
			"versionNonce": 1693717295,
			"isDeleted": false,
			"id": "XCFyHt02",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 298.30328819077545,
			"y": 13209.35159159599,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 84.42052424715396,
			"height": 17.28932336581713,
			"seed": 376300120,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "-536870912",
			"rawText": "-536870912",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "-536870912",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 388,
			"versionNonce": 1921504591,
			"isDeleted": false,
			"id": "3wqx3EmY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 296.62274032906214,
			"y": 13246.589971022919,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 75.97847182243856,
			"height": 17.28932336581713,
			"seed": 266906152,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "536870912",
			"rawText": "536870912",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "536870912",
			"lineHeight": 1.2,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 422,
			"versionNonce": 949900143,
			"isDeleted": false,
			"id": "qYpWM769",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 297.1696245722377,
			"y": 13262.437859698752,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 84.42052424715396,
			"height": 17.28932336581713,
			"seed": 1742792792,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "1073741824",
			"rawText": "1073741824",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1073741824",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 372,
			"versionNonce": 260050319,
			"isDeleted": false,
			"id": "l1Md3CGB",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 297.0240564345063,
			"y": 13278.15078152861,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 84.42052424715396,
			"height": 17.28932336581713,
			"seed": 1801215784,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "1610612736",
			"rawText": "1610612736",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1610612736",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 459,
			"versionNonce": 606521921,
			"isDeleted": false,
			"id": "C1v7WYVX",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13479.69443328525,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 137.09893137737802,
			"height": 17.28932336581713,
			"seed": 403328600,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378293316,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514273,
			"fontFamily": 3,
			"text": "or 操作，操作符“|”",
			"rawText": "or 操作，操作符“|”",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "or 操作，操作符“|”",
			"lineHeight": 1.2,
			"baseline": 13.000000000000004
		},
		{
			"type": "text",
			"version": 499,
			"versionNonce": 1471250607,
			"isDeleted": false,
			"id": "PI9c484y",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -95.05525378871027,
			"y": 13504.764841165741,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 823.7191952542302,
			"height": 17.28932336581713,
			"seed": 1149996840,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378279362,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"rawText": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 486,
			"versionNonce": 1137550319,
			"isDeleted": false,
			"id": "hdo43w4V",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 391.6569997458932,
			"y": 13209.24839781944,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 329.2400445639005,
			"height": 17.28932336581713,
			"seed": 163512152,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "1110 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "1110 0000 0000 0000 0000 0000 0000 0000",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1110 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 549,
			"versionNonce": 158420495,
			"isDeleted": false,
			"id": "orrySlph",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 409.42344085162284,
			"y": 13248.091982831851,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 312.3559397144696,
			"height": 17.28932336581713,
			"seed": 1705953064,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514275,
			"fontFamily": 3,
			"text": "10 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "10 0000 0000 0000 0000 0000 0000 0000",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "10 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 403,
			"versionNonce": 775075407,
			"isDeleted": false,
			"id": "QNnuUB83",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13563.744320987837,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 145.54098380209345,
			"height": 17.28932336581713,
			"seed": 1177002328,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378293316,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514278,
			"fontFamily": 3,
			"text": "and 操作，操作符“&”",
			"rawText": "and 操作，操作符“&”",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "and 操作，操作符“&”",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 467,
			"versionNonce": 204855841,
			"isDeleted": false,
			"id": "daZJgN0g",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13595.297504702889,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 780.4958868396874,
			"height": 17.28932336581713,
			"seed": 964676184,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378293316,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"rawText": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"lineHeight": 1.2,
			"baseline": 13
		},
		{
			"type": "text",
			"version": 593,
			"versionNonce": 1586541679,
			"isDeleted": false,
			"id": "Jn4zVZDH",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 410.38042603747346,
			"y": 13264.942966704371,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 320.7979921391851,
			"height": 17.28932336581713,
			"seed": 1029211736,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "100 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "100 0000 0000 0000 0000 0000 0000 0000",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "100 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 628,
			"versionNonce": 1786662543,
			"isDeleted": false,
			"id": "sqVkHS59",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 409.547640902213,
			"y": 13282.083686727045,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 329.2400445639005,
			"height": 17.28932336581713,
			"seed": 179640104,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378253530,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "1000 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "1000 0000 0000 0000 0000 0000 0000 0000",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1000 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 402,
			"versionNonce": 1581585519,
			"isDeleted": false,
			"id": "61pEFIUS",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13673.160727484425,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 137.09893137737802,
			"height": 17.28932336581713,
			"seed": 233231448,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378293316,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514273,
			"fontFamily": 3,
			"text": "not操作，操作符“~”",
			"rawText": "not操作，操作符“~”",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "not操作，操作符“~”",
			"lineHeight": 1.2,
			"baseline": 13.000000000000004
		},
		{
			"type": "text",
			"version": 503,
			"versionNonce": 1670901249,
			"isDeleted": false,
			"id": "1J0eQdto",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -198.59921469410045,
			"y": 13703.473599501893,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 561.9030093890567,
			"height": 17.28932336581713,
			"seed": 1619912792,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378293316,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"rawText": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"lineHeight": 1.2,
			"baseline": 12.999999999999996
		},
		{
			"type": "text",
			"version": 150,
			"versionNonce": 1586419841,
			"isDeleted": true,
			"id": "43qID4cW",
			"fillStyle": "hachure",
			"strokeWidth": 1,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -27.034542846473556,
			"y": 43159.151877545206,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 8.442052424715396,
			"height": 34.57864673163426,
			"seed": 3910,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704378222398,
			"link": null,
			"locked": false,
			"fontSize": 14.407769471514277,
			"fontFamily": 3,
			"text": "\n",
			"rawText": "\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "\n",
			"lineHeight": 1.2,
			"baseline": 29.999999999999993
		},
		{
			"id": "rilO0RVe",
			"type": "text",
			"x": 966.4776948378967,
			"y": 10580.840327792976,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 437516033,
			"version": 2,
			"versionNonce": 1304177583,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704378173312,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "G3QgDPaW",
			"type": "text",
			"x": -3.5223051621032937,
			"y": 10830.840327792976,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 650755553,
			"version": 2,
			"versionNonce": 1622335183,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704378189086,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "yVKohsUA",
			"type": "text",
			"x": -23.522305162103294,
			"y": 43150.840327792976,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1644433519,
			"version": 2,
			"versionNonce": 764594657,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704378220743,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "#ffffff",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 387.9559425086431,
		"scrollY": -12844.209004651873,
		"zoom": {
			"value": 0.7000000000000001
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