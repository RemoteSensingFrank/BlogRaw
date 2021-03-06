---
title: Cesium搭建自己的GIS服务器
date: 2018-10-28 09:14:15
tags: 开发
categories: 学习
---
&nbsp;&nbsp;&nbsp;&nbsp;虽然是用GIS的路线，但是最近接触了很多其他的GIS研发公司，然后找同学也了解了一下目前行业内的研发情况；另外也有大神向我推荐了一些开源库，目前来说主要就是Cesium这个前端的GIS开源框架。所以也去了解了一下，对于我来说使用开源框架最大的问题在于学习成本，但是从另一个方面来说，由于对国产软件的支持力度日益提高，使用ESRI的平台也可能会面临各种各样的问题，因此我觉得对于各种平台的研究与支持我们是有备无患，在这样的背景之下我去了解了一下Cesium的前端GIS框架，下面主要谈谈基于这个GIS框架搭建一个GIS系统的思路以及一些尝试。  
1. 首先是数据的支持，对于一个平台来说最大问题在于其对数据的支持，如果能够对各种数据都进行很好的支持那么这个平台就是具有一定潜力的，当然如果连常见的数据格式都不能支持，那么这个平台的生命力一定不会太强，因此我们第一个考虑的就是数据支持的问题，为了了解对于各种数据格式的支持我们查看了其示例代码，同时也针对一些格式进行了测试，其示例代码的[网站](https://cesiumjs.org/Cesium/Build/Apps/Sandcastle/)上有对各种数据格式的支持情况，从网站上我们可以看到，实际上Cesium对于各种数据都是能够比较好的支持的，但是需要转换为它所定义的**3DTiles**的标准，关于**3DTiles**以后如果有时间再细聊下面展示一下我们自己的测试情况：  
<img src="https://blogimage-1251632003.cos.ap-guangzhou.myqcloud.com/cesium%E7%82%B9%E4%BA%91.JPG"/>  
上面展示的是分类点云的情况，实际上还有三位模型的加载情况以及KML的加载情况由于截图比较麻烦就不截图进行展示了。
2. 除了对于所支持的数据格式有要求之外，在实际应用中对于所支持的数据量的大小也是有要求的，如果只能支持少量的数据那么在实际应用中只能作为一个展示的平台，不具有太大的意义，所以能加载的数据量的大小作为一个极其重要的性能指标也需要被考虑。目前我并没有对可加载的数据量的大小进行测试，了解其官网的Demo可以粗略的了解到通过Cesium加载10亿级别的点云是没有问题的，但是这个数据量的加载需要进行测试，另外在加载的过程中一般来说会混合加载多种格式的数据，相比于加载单一格式的数据，多种格式数据同时加载对于平台的压力要远远大于加载同一个格式的数据，因此在测试过程中也需要对平台进行进一步的测试。
3. 平台工具的使用，实际了解到Cesium是通过一种叫做3DTiles的数据格式来加载数据的，那么对于我们常见的如模型，点云以及倾斜等数据格式，都需要转换为3DTiles的格式，现在面临的最主要的问题是没有现成的工具对各种格式的转换进行很好的支持。通过3DTiles格式的说明我们可以自己编写代码进行转换，但是对于模型和倾斜数据，由于其格式较多且数据格式较为复杂，自己写的转换方式不一定能够适用于所有模型，当然目前也有一些开源的转换代码，但是对于这些转换代码没有进行测试，对其性能以及转换后的数据是否能够成功加载也存在一些疑问，需要进行进一步的测试。第3点问题是开源平台存在的主要问题，相比于成熟的商业平台，开源平台存在着生态不足，且由于开源数据格式多样造成的格式统一的困难。由于这些困难的存在导致开源平台的学习成本很高。
4. 平台API功能的稳定性以及功能是否完善，关于这个问题我并没有深入的了解，但是做一个球并进行简单的操作是没有问题的，至于更加深入的功能可能需要更进一步的摸索。 
    
&nbsp;&nbsp;&nbsp;&nbsp;其实了解完了以上几个问题我们对于这个平台就有一定的了解了，实际上cesium作为一个开源的三位GIS平台是具有很大的潜力的，如果我们自己需要搭建一个功能不复杂的系统可以考虑使用。那么用一个开源的前端球我们需要考虑的是服务端用什么，GIS后台服务可选的有很多开源的如GeoServer商业的如SuperMap以及ESRI等，但是考虑到成本以及既然用了开源就用到底的精神我觉得可以考虑GeoServer，至于三维数据的后台实际上只有一个比较重要的要求那就是服务器，实际上Cesium可以接受通过服务器发布数据服务然后进行展示，所以如果是纯数据则可以直接通过Nginx或者IIS发布静态数据，但是作为一个服务器可能更重要的是上传数据后进行数据的管理以及自动的转换，从这个角度来看就需要编写后台数据格式转换和后台服务的代码，这个应该是最主要的问题，不过实际上也不算太复杂，如果能够了解到各个格式转换为3DTiles的方法，自己编写代码搭建后台服务器也是可以考虑的。
&nbsp;&nbsp;&nbsp;&nbsp;最后一个问题就是关于数据量的问题，实际上一个服务器所能承载的数据是有限的，但是通过Nginx等负载均衡的工具也很容易就搭建出一个负载均衡的服务器，所以数据量的问题也不需要过度担心，总的来说在时间允许的条件下，如果对于功能的要求不是特别高的情况下可以考虑通过Cesium+GeoServer+Nginx的方案搭建一套自己的服务器。
