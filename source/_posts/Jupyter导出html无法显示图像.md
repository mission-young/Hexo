title: Jupyter导出html无法显示图像
author: 远方
date: 2020-04-08 13:41:20
tags:
---
此问题是由于root版本变更引起的。
在`root v620`[更新日志](https://root.cern/doc/v620/release-notes.html#release-6.2004)中可以看到关于网络部分做了诸多变更。

# Core Libraries
- Speed-up startup, in particular in case of no or poor network accesibility, by avoiding a network access that was used as input to generate a globally unique ID for the current process.
- This network access is replaced by a passive scan of the network interface. This reduces somewhat the uniqueness of the unique ID as the IP address is no longer guaranteed by the DNS server to be unique. Note that this was already the case when the network access (used to look up the hostname and its IP address) failed.
# Language Bindings
## Jupyter Notebook Integration
- When starting Jupyter server with `root --notebook` arg1 arg2 ..., extra arguments can be provided. All these arguments delivered as is to jupyter executable and can be used for configuration. Like server binding to specific host `root --notebook --ip=hostname`
- Remove `c.NotebookApp.ip = '*' from default jupyter config`. One has to provide ip address for server binding using `root --notebook --ip=<hostaddr>` arguments
- Now Jupyter Notebooks will use JSROOT provided with ROOT installation. This allows to use notebooks without internet connection (offline).
# JavaScript ROOT
- Provide monitoring capabilities for TGeoManager object. Now geomtry with some tracks can be displayed and updated in web browser, using THttpServer monitoring capability like histogram objects.
- JSROOT graphics are now supported in the JupyterLab interface. They are activated in the same way as in the classic Jupyter, i.e. by typing at the beginning of a notebook cell:
```bash
%jsroon on
```
从上述变更中可以解释最近版本为什么突然jupyter无法正常显示图像了。
root官方为了解决弱网络连接下jupyter显示问题，把jupyter的js运行库放在了新版本root安装包中，运行jupyter时会自动调用这个js库。 但是需要留意的是 <font color='red'>仅使用root --notebook这种方式启动jupyter才会生效！使用jupyter-notebook的方式启动并不会生效！！图依旧无法正常显示。</font> 而前面的博客[Jupyter离线配置](/2020/03/25/Jupyter离线配置/)中已经提过，为了解决这个问题，在jupyter的配置文件中可以新增`static`路径来解决，解决了在jupyter运行时的绘图问题。
但这个问题也带来了其他的问题。其一是，无法自由地分享`.ipynb`和对应导出的`.html`。分享的文件在nbviewer中无法显示图像。猜测是root更新之后把原来的链接进行了替换。
为了做验证，新建了一个测试linux环境，编译安装了`root v61902`，发现问题和`v620`一致，下载`v61802`并安装jupyter环境，无需任何配置，输入`jupyter-notebook`即可正常显示图像和导出有图的html文件，`nbviewer`也可正常显示上传到github的`.ipynb`文件。显然，这个版本和最初用的版本一致，在弱网条件下无法正常显示。
把`v620`和`v618`导出的`html`文件做对比，我们把`v620`和`v618`分别定义为`offline`和`online`。两者对应的测试文件为[`
offline.html
`](/attachments/offline.html)和[`online.html`](/attachments/online.html). 点击链接即可查看.
~~<font color='red'>
需要留意，在本机localhost打开hexo时，online.html也无法正常画图，这是由于html挂在了localhost:4000服务上，js判断根据服务判断跳过了绘图。把该文件下载下来，用浏览器打开可以正常显示。
</font> 此问题已修复，Hexo跳过渲染html文件之后正常运行~~
  
用文件比对工具对比，发现两者的差异
#### offline
```html
<script>
if (typeof require !== 'undefined') {
    // All requirements met (we are in jupyter notebooks or we loaded requirejs before).
    display_root_plot_1586256162911();
} else {
    // We are in jupyterlab, we need to insert requirejs and configure it.
    // Jupyterlab might be installed in a different base_url so we need to know it.
    try {
        var base_url = JSON.parse(document.getElementById('jupyter-config-data').innerHTML).baseUrl;
    } catch(_) {
        var base_url = '/';
    }
    // Try loading a local version of requirejs and fallback to cdn if not possible.
    requirejs_load(base_url + 'static/components/requirejs/require.js', requirejs_success(base_url), function(){
        requirejs_load('https://cdnjs.cloudflare.com/ajax/libs/require.js/2.2.0/require.min.js', requirejs_success(base_url), function(){
            document.getElementById("root_plot_1586256162911").innerHTML = "Failed to load requireJs";
        });
    });
}
function requirejs_load(src, on_load, on_error) {
    var script = document.createElement('script');
    script.src = src;
    script.onload = on_load;
    script.onerror = on_error;
    document.head.appendChild(script);
}
function requirejs_success(base_url) {
    return function() {
        require.config({
            baseUrl: base_url + 'static/'
        });
        display_root_plot_1586256162911();
    }
}
function display_root_plot_1586256162911() {
    require(['scripts/JSRootCore'],
        function(Core) {
            var obj = Core.JSONR_unref({"_typename":"TCanvas","fUniqueID":0,"fBits":3342344,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fLeftMargin":0.1,"fRightMargin":0.1,"fBottomMargin":0.1,"fTopMargin":0.1,"fXfile":2,"fYfile":2,"fAfile":1,"fXstat":0.99,"fYstat":0.99,"fAstat":2,"fFrameFillColor":0,"fFrameLineColor":1,"fFrameFillStyle":1001,"fFrameLineStyle":1,"fFrameLineWidth":1,"fFrameBorderSize":1,"fFrameBorderMode":0,"fX1":-91.2875068014492,"fY1":-136.475010235236,"fX2":821.587506801449,"fY2":1237.27501023524,"fXtoAbsPixelk":69.6000541484835,"fXtoPixelk":69.6000541484835,"fXtoPixel":0.762426388748505,"fYtoAbsPixelk":425.109273751662,"fYtoPixelk":425.109273751662,"fYtoPixel":-0.343585072223222,"fUtoAbsPixelk":5e-5,"fUtoPixelk":5e-5,"fUtoPixel":696,"fVtoAbsPixelk":472.00005,"fVtoPixelk":472,"fVtoPixel":-472,"fAbsPixeltoXk":-91.2875068014492,"fPixeltoXk":-91.2875068014492,"fPixeltoX":1.31160203103865,"fAbsPixeltoYk":1237.27501023524,"fPixeltoYk":-136.475010235236,"fPixeltoY":-2.91048733150524,"fXlowNDC":0,"fYlowNDC":0,"fXUpNDC":1,"fYUpNDC":1,"fWNDC":1,"fHNDC":1,"fAbsXlowNDC":0,"fAbsYlowNDC":0,"fAbsWNDC":1,"fAbsHNDC":1,"fUxmin":0,"fUymin":0.9,"fUxmax":730.3,"fUymax":1099.9,"fTheta":30,"fPhi":30,"fAspectRatio":0,"fNumber":0,"fTickx":0,"fTicky":0,"fLogx":0,"fLogy":0,"fLogz":0,"fPadPaint":0,"fCrosshair":0,"fCrosshairPos":0,"fBorderSize":2,"fBorderMode":0,"fModified":false,"fGridx":false,"fGridy":false,"fAbsCoord":false,"fEditable":true,"fFixedAspectRatio":false,"fPrimitives":{"_typename":"TList","name":"TList","arr":[{"_typename":"TFrame","fUniqueID":0,"fBits":8,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fX1":0,"fY1":0.9,"fX2":730.3,"fY2":1099.9,"fBorderSize":1,"fBorderMode":0},{"_typename":"TGraph","fUniqueID":0,"fBits":1032,"fName":"","fTitle":"","fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1000,"fMarkerColor":1,"fMarkerStyle":3,"fMarkerSize":1,"fNpoints":664,"fX":[1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89,90,91,92,93,94,95,96,97,98,99,100,101,102,103,104,105,106,107,108,109,110,111,112,113,114,115,116,117,118,119,120,121,122,123,124,125,126,127,128,129,130,131,132,133,134,135,136,137,138,139,140,141,142,143,144,145,146,147,148,149,150,151,152,153,154,155,156,157,158,159,160,161,162,163,164,165,166,167,168,169,170,171,172,173,174,175,176,177,178,179,180,181,182,183,184,185,186,187,188,189,190,191,192,193,194,195,196,197,198,199,200,201,202,203,204,205,206,207,208,209,210,211,212,213,214,215,216,217,218,219,220,221,222,223,224,225,226,227,228,229,230,231,232,233,234,235,236,237,238,239,240,241,242,243,244,245,246,247,248,249,250,251,252,253,254,255,256,257,258,259,260,261,262,263,264,265,266,267,268,269,270,271,272,273,274,275,276,277,278,279,280,281,282,283,284,285,286,287,288,289,290,291,292,293,294,295,296,297,298,299,300,301,302,303,304,305,306,307,308,309,310,311,312,313,314,315,316,317,318,319,320,321,322,323,324,325,326,327,328,329,330,331,332,333,334,335,336,337,338,339,340,341,342,343,344,345,346,347,348,349,350,351,352,353,354,355,356,357,358,359,360,361,362,363,364,365,366,367,368,369,370,371,372,373,374,375,376,377,378,379,380,381,382,383,384,385,386,387,388,389,390,391,392,393,394,395,396,397,398,399,400,401,402,403,404,405,406,407,408,409,410,411,412,413,414,415,416,417,418,419,420,421,422,423,424,425,426,427,428,429,430,431,432,433,434,435,436,437,438,439,440,441,442,443,444,445,446,447,448,449,450,451,452,453,454,455,456,457,458,459,460,461,462,463,464,465,466,467,468,469,470,471,472,473,474,475,476,477,478,479,480,481,482,483,484,485,486,487,488,489,490,491,492,493,494,495,496,497,498,499,500,501,502,503,504,505,506,507,508,509,510,511,512,513,514,515,516,517,518,519,520,521,522,523,524,525,526,527,528,529,530,531,532,533,534,535,536,537,538,539,540,541,542,543,544,545,546,547,548,549,550,551,552,553,554,555,556,557,558,559,560,561,562,563,564,565,566,567,568,569,570,571,572,573,574,575,576,577,578,579,580,581,582,583,584,585,586,587,588,589,590,591,592,593,594,595,596,597,598,599,600,601,602,603,604,605,606,607,608,609,610,611,612,613,614,615,616,617,618,619,620,621,622,623,624,625,626,627,628,629,630,631,632,633,634,635,636,637,638,639,640,641,642,643,644,645,646,647,648,649,650,651,652,653,654,655,656,657,658,659,660,661,662,663,664],"fY":[1,3,5,6,8,9,10,12,14,15,18,20,21,22,23,24,25,26,27,29,30,31,36,37,38,42,44,45,48,49,52,53,54,55,58,60,62,63,64,66,70,71,72,73,74,76,79,80,82,85,87,88,90,91,93,94,95,96,97,98,100,102,104,105,107,109,110,111,112,113,114,115,116,118,119,120,121,122,123,124,125,127,132,133,134,137,139,140,142,143,144,147,149,150,151,153,154,155,156,157,159,160,161,162,164,166,167,169,170,172,175,177,179,183,186,187,190,191,192,193,194,197,198,200,201,204,205,206,207,208,209,211,212,213,214,215,216,218,220,221,224,225,228,230,231,233,235,237,240,241,242,243,246,247,248,250,251,254,255,256,257,258,261,263,264,265,268,269,272,274,275,276,277,278,279,280,281,282,283,285,286,288,291,292,294,296,298,300,301,303,304,305,306,307,312,314,315,316,317,318,319,320,321,322,323,325,326,327,329,330,331,333,335,337,338,339,340,341,342,343,344,346,348,349,351,353,354,355,356,359,360,362,364,365,366,367,368,369,372,373,377,378,379,380,381,383,386,387,388,389,390,391,392,393,395,396,397,398,401,402,403,406,407,408,411,413,415,416,417,420,421,424,427,429,431,433,434,435,437,440,443,447,448,449,452,453,455,456,457,458,460,462,463,465,467,468,471,472,475,477,479,480,482,484,486,487,489,490,491,492,495,496,498,499,500,502,503,504,505,507,508,509,510,512,513,514,515,517,518,519,520,521,523,524,526,527,529,530,532,533,535,536,537,538,540,542,544,546,548,552,553,555,556,558,559,560,562,563,564,565,566,567,568,569,570,571,572,573,576,578,579,580,581,582,583,584,585,586,588,589,590,592,594,595,596,597,598,600,601,603,605,606,607,608,609,611,612,614,617,618,619,621,622,624,625,626,627,628,629,631,632,633,635,636,639,641,643,644,646,648,651,653,654,655,656,657,659,660,662,664,665,669,670,671,672,673,674,675,677,679,680,682,683,685,686,687,688,689,690,691,692,693,694,695,696,697,698,699,701,706,707,708,709,710,711,713,714,715,718,719,720,722,725,726,727,728,729,730,731,732,734,735,736,737,738,739,740,744,746,747,748,749,751,753,754,755,757,758,759,760,763,764,765,766,767,768,770,771,772,774,776,778,779,780,781,782,783,785,788,789,791,793,795,796,797,798,799,801,803,804,806,807,808,809,812,815,817,818,819,821,824,826,827,829,830,832,833,834,835,836,837,838,839,841,842,843,844,845,846,848,850,854,855,856,858,859,861,862,864,865,866,867,868,869,871,873,874,875,876,877,878,879,880,881,883,885,886,887,888,889,890,892,893,894,895,896,897,898,900,902,905,906,907,908,909,910,912,914,916,917,918,920,922,924,925,928,931,932,933,934,935,936,937,938,940,941,943,944,947,949,950,951,952,953,957,959,960,961,962,963,964,966,967,969,970,972,974,975,976,978,979,983,984,986,987,988,989,990,991,992,994,997,999,1000],"fFunctions":{"_typename":"TList","name":"TList","arr":[],"opt":[]},"fHistogram":{"_typename":"TH1F","fUniqueID":0,"fBits":520,"fName":"Graph","fTitle":"","fLineColor":602,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fMarkerColor":1,"fMarkerStyle":1,"fMarkerSize":1,"fNcells":666,"fXaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":0,"fName":"xaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":1,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":664,"fXmin":0,"fXmax":730.3,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fYaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":0,"fName":"yaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":0,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":1,"fXmin":0.9,"fXmax":1099.9,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fZaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":0,"fName":"zaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":1,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":1,"fXmin":0,"fXmax":1,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fBarOffset":0,"fBarWidth":1000,"fEntries":0,"fTsumw":0,"fTsumw2":0,"fTsumwx":0,"fTsumwx2":0,"fMaximum":1099.9,"fMinimum":0.9,"fNormFactor":0,"fContour":[],"fSumw2":[],"fOption":"","fFunctions":{"_typename":"TList","name":"TList","arr":[],"opt":[]},"fBufferSize":0,"fBuffer":[],"fBinStatErrOpt":0,"fStatOverflows":2,"fArray":[0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]},"fMinimum":-1111,"fMaximum":-1111}],"opt":["","ap"]},"fExecs":null,"fName":"c1","fTitle":"c1","fNumPaletteColor":0,"fNextPaletteColor":0,"fDISPLAY":"$DISPLAY","fDoubleBuffer":0,"fRetained":true,"fXsizeUser":0,"fYsizeUser":0,"fXsizeReal":20,"fYsizeReal":14.28571,"fWindowTopX":0,"fWindowTopY":0,"fWindowWidth":0,"fWindowHeight":0,"fCw":696,"fCh":472,"fCatt":{"_typename":"TAttCanvas","fXBetween":2,"fYBetween":2,"fTitleFromTop":1.2,"fXdate":0.2,"fYdate":0.3,"fAdate":1},"kMoveOpaque":true,"kResizeOpaque":true,"fHighLightColor":2,"fBatch":true,"kShowEventStatus":false,"kAutoExec":true,"kMenuBar":true});
            Core.draw("root_plot_1586256162911", obj, "");
        }
    );
}
</script>
```
#### online
```html
<script>
 requirejs.config({
     paths: {
       'JSRootCore' : 'https://root.cern.ch/js/notebook//scripts/JSRootCore',
     }
   });
 require(['JSRootCore'],
     function(Core) {
       var obj = Core.JSONR_unref({"_typename":"TCanvas","fUniqueID":0,"fBits":36896776,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fLeftMargin":0.1,"fRightMargin":0.1,"fBottomMargin":0.1,"fTopMargin":0.1,"fXfile":2,"fYfile":2,"fAfile":1,"fXstat":0.99,"fYstat":0.99,"fAstat":2,"fFrameFillColor":0,"fFrameLineColor":1,"fFrameFillStyle":1001,"fFrameLineStyle":1,"fFrameLineWidth":1,"fFrameBorderSize":1,"fFrameBorderMode":0,"fX1":-1.25000009313226,"fY1":-0.131250009778888,"fX2":11.2500000931323,"fY2":1.18125000977889,"fXtoAbsPixelk":69.6000541484835,"fXtoPixelk":69.6000541484835,"fXtoPixel":55.6799991703033,"fYtoAbsPixelk":424.800047186661,"fYtoPixelk":424.800047186661,"fYtoPixel":-359.619042260306,"fUtoAbsPixelk":5e-5,"fUtoPixelk":5e-5,"fUtoPixel":696,"fVtoAbsPixelk":472.00005,"fVtoPixelk":472,"fVtoPixel":-472,"fAbsPixeltoXk":-1.25000009313226,"fPixeltoXk":-1.25000009313226,"fPixeltoX":0.017959770382564,"fAbsPixeltoYk":1.18125000977889,"fPixeltoYk":-0.131250009778888,"fPixeltoY":-0.00278072038041902,"fXlowNDC":0,"fYlowNDC":0,"fXUpNDC":0,"fYUpNDC":0,"fWNDC":1,"fHNDC":1,"fAbsXlowNDC":0,"fAbsYlowNDC":0,"fAbsWNDC":1,"fAbsHNDC":1,"fUxmin":0,"fUymin":0,"fUxmax":10,"fUymax":1.05,"fTheta":30,"fPhi":30,"fAspectRatio":0,"fNumber":0,"fTickx":0,"fTicky":0,"fLogx":0,"fLogy":0,"fLogz":0,"fPadPaint":0,"fCrosshair":0,"fCrosshairPos":0,"fBorderSize":2,"fBorderMode":0,"fModified":false,"fGridx":false,"fGridy":false,"fAbsCoord":false,"fEditable":true,"fFixedAspectRatio":false,"fPrimitives":{"_typename":"TList","name":"TList","arr":[{"_typename":"TFrame","fUniqueID":0,"fBits":50331656,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fX1":0,"fY1":0,"fX2":10,"fY2":1.05,"fBorderSize":1,"fBorderMode":0},{"_typename":"TH1F","fUniqueID":0,"fBits":50331656,"fName":"h","fTitle":"h","fLineColor":602,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fMarkerColor":1,"fMarkerStyle":1,"fMarkerSize":1,"fNcells":12,"fXaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":50331648,"fName":"xaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":1,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":10,"fXmin":0,"fXmax":10,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fYaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":50331648,"fName":"yaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":0,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":1,"fXmin":0,"fXmax":1,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fZaxis":{"_typename":"TAxis","fUniqueID":0,"fBits":50331648,"fName":"zaxis","fTitle":"","fNdivisions":510,"fAxisColor":1,"fLabelColor":1,"fLabelFont":42,"fLabelOffset":0.005,"fLabelSize":0.035,"fTickLength":0.03,"fTitleOffset":1,"fTitleSize":0.035,"fTitleColor":1,"fTitleFont":42,"fNbins":1,"fXmin":0,"fXmax":1,"fXbins":[],"fFirst":0,"fLast":0,"fBits2":0,"fTimeDisplay":false,"fTimeFormat":"","fLabels":null,"fModLabs":null},"fBarOffset":0,"fBarWidth":1000,"fEntries":1,"fTsumw":1,"fTsumw2":1,"fTsumwx":3,"fTsumwx2":9,"fMaximum":-1111,"fMinimum":-1111,"fNormFactor":0,"fContour":[],"fSumw2":[],"fOption":"","fFunctions":{"_typename":"TList","name":"TList","arr":[{"_typename":"TPaveStats","fUniqueID":0,"fBits":50331657,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":1001,"fX1":8.50000025331975,"fY1":0.885937513201498,"fX2":11.0000003278256,"fY2":1.09593751163688,"fX1NDC":0.780000016093254,"fY1NDC":0.775000005960464,"fX2NDC":0.980000019073486,"fY2NDC":0.935000002384186,"fBorderSize":1,"fInit":1,"fShadowColor":1,"fCornerRadius":0,"fOption":"brNDC","fName":"stats","fTextAngle":0,"fTextSize":0,"fTextAlign":12,"fTextColor":1,"fTextFont":42,"fLabel":"","fLongest":18,"fMargin":0.05,"fLines":{"_typename":"TList","name":"TList","arr":[{"_typename":"TLatex","fUniqueID":0,"fBits":50331648,"fName":"","fTitle":"h","fTextAngle":0,"fTextSize":0.0368,"fTextAlign":0,"fTextColor":0,"fTextFont":0,"fX":0,"fY":0,"fLineColor":1,"fLineStyle":1,"fLineWidth":2,"fLimitFactorSize":3,"fOriginSize":0.0368000008165836},{"_typename":"TLatex","fUniqueID":0,"fBits":50331648,"fName":"","fTitle":"Entries = 1      ","fTextAngle":0,"fTextSize":0,"fTextAlign":0,"fTextColor":0,"fTextFont":0,"fX":0,"fY":0,"fLineColor":1,"fLineStyle":1,"fLineWidth":2,"fLimitFactorSize":3,"fOriginSize":0.04},{"_typename":"TLatex","fUniqueID":0,"fBits":50331648,"fName":"","fTitle":"Mean  =      3","fTextAngle":0,"fTextSize":0,"fTextAlign":0,"fTextColor":0,"fTextFont":0,"fX":0,"fY":0,"fLineColor":1,"fLineStyle":1,"fLineWidth":2,"fLimitFactorSize":3,"fOriginSize":0.04},{"_typename":"TLatex","fUniqueID":0,"fBits":50331648,"fName":"","fTitle":"Std Dev   =      0","fTextAngle":0,"fTextSize":0,"fTextAlign":0,"fTextColor":0,"fTextFont":0,"fX":0,"fY":0,"fLineColor":1,"fLineStyle":1,"fLineWidth":2,"fLimitFactorSize":3,"fOriginSize":0.04}],"opt":["","","",""]},"fOptFit":0,"fOptStat":1111,"fFitFormat":"5.4g","fStatFormat":"6.4g","fParent":{"$ref":3}}],"opt":["brNDC"]},"fBufferSize":0,"fBuffer":[],"fBinStatErrOpt":0,"fStatOverflows":2,"fArray":[0,0,0,0,1,0,0,0,0,0,0,0]},{"_typename":"TPaveText","fUniqueID":0,"fBits":50331657,"fLineColor":1,"fLineStyle":1,"fLineWidth":1,"fFillColor":0,"fFillStyle":0,"fX1":4.74928160545941,"fY1":1.10250001378823,"fX2":5.25071839454059,"fY2":1.17468751593959,"fX1NDC":0.479942528735632,"fY1NDC":0.940000003948808,"fX2NDC":0.520057471264368,"fY2NDC":0.995000004768372,"fBorderSize":0,"fInit":1,"fShadowColor":1,"fCornerRadius":0,"fOption":"blNDC","fName":"title","fTextAngle":0,"fTextSize":0,"fTextAlign":22,"fTextColor":1,"fTextFont":42,"fLabel":"","fLongest":1,"fMargin":0.05,"fLines":{"_typename":"TList","name":"TList","arr":[{"_typename":"TLatex","fUniqueID":0,"fBits":50331648,"fName":"","fTitle":"h","fTextAngle":0,"fTextSize":0,"fTextAlign":0,"fTextColor":0,"fTextFont":0,"fX":0,"fY":0,"fLineColor":1,"fLineStyle":1,"fLineWidth":2,"fLimitFactorSize":3,"fOriginSize":0.0467500016093254}],"opt":[""]}}],"opt":["","","blNDC"]},"fExecs":null,"fName":"c1","fTitle":"c1","fNumPaletteColor":0,"fNextPaletteColor":0,"fDISPLAY":"$DISPLAY","fDoubleBuffer":0,"fRetained":true,"fXsizeUser":0,"fYsizeUser":0,"fXsizeReal":20,"fYsizeReal":14.28571,"fWindowTopX":0,"fWindowTopY":0,"fWindowWidth":0,"fWindowHeight":0,"fCw":696,"fCh":472,"fCatt":{"_typename":"TAttCanvas","fXBetween":2,"fYBetween":2,"fTitleFromTop":1.2,"fXdate":0.2,"fYdate":0.3,"fAdate":1},"kMoveOpaque":true,"kResizeOpaque":true,"fHighLightColor":2,"fBatch":true,"kShowEventStatus":false,"kAutoExec":true,"kMenuBar":true});
       Core.draw("root_plot_1", obj, "");
     }
 );
</script>
```
可以发现最显著的差异在这一部分
```js
 requirejs.config({
     paths: {
       'JSRootCore' : 'https://root.cern.ch/js/notebook//scripts/JSRootCore',
     }
   });
```
在`online`部分，调用了root官网的JSRootCore库，而`offline`则试图从本地服务器寻找。在
```js
if (typeof require !== 'undefined') {
    // All requirements met (we are in jupyter notebooks or we loaded requirejs before).
    display_root_plot_1586256162911();
} 
```
中添加`alert`函数。`alert`会执行。意味着会直接执行`require(['scripts/JSRootCore'],...)`函数，相比于`online`，缺失的正是上面提到的从root官网加载js这一部分。
遵从最少修改原则，为了将导出的html正常显示图，只许将`require(['scripts/JSRootCore'],`更改为
```js
requirejs.config({
     paths: {
       'JSRootCore' : 'https://root.cern.ch/js/notebook//scripts/JSRootCore',
     }
   });
 require(['JSRootCore'],
```
即可.
在`bash`中执行
```bash
sed -i "s/require(['scripts\/JSRootCore'\],/requirejs.config({paths:{'JSRootCore':'https:\/\/root.cern.ch\/js\/notebook\/\/scripts\/JSRootCore',}});require(\['JSRootCore'\],/g" output.html
```
`output.html`为v620导出的html. 由此即可实现导出的html正常绘图.
如果需要发布或共享ipynb文件,则需要额外删除其他部分。
为了方便起见，在bashrc中构造函数
```shell
py2html()
{
sed -i "s/require(['scripts\/JSRootCore'\],/requirejs.config({paths:{'JSRootCore':'https:\/\/root.cern.ch\/js\/notebook\/\/scripts\/JSRootCore',}});require(\['JSRootCore'\],/g" $1
sed -i "s/\"if (typeof/bbbegin/g" $1
sed -i "s/\"    display_root_plot/eeend\"    display_root_plot/g" $1
sed -i "s/\"} else {/bbbegin/g" $1
sed -i "s/\"function display_root_plot/eeend\"function display_root_plot/g" $1
sed -e '/bbbegin/!b;:a;/eeend/bb;$!{N;ba};:b;s/bbbegin.*.*eeend//' -i $1
}
```
在`bash`中执行`py2html *.ipynb`或`py2html *.html`即可将其转化为有图的文件。
由于转义符号的问题，部分文件可能需要重命名方能正常转化.
此部分也可在`py2html`函数中用中间变量解决，这点回头再修正。