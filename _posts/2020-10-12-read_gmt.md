---
layout: post
title: "gmt格式文件的读取方式"
date: 2020-10-12
description: "生信学习"
tags: 生信学习
--- 
```
kegg_geneset1 <- qusage::read.gmt("c2.cp.kegg.v7.0.symbols.gmt") #返回的是list

kegg_geneset2 <- clusterProfiler::read.gmt("c2.cp.kegg.v7.0.symbols.gmt") #返回的是表格

kegg_geneset3 <- GSA::GSA.read.gmt("c2.cp.kegg.v7.0.symbols.gmt") #"GSA.genesets", 返回的是list

kegg_geneset4 <- GSEABase::getGmt("c2.cp.kegg.v7.0.symbols.gmt") #list中的list, "GeneSetCollection"

kegg_geneset5 <- cogena::gmt2list("c2.cp.kegg.v7.0.symbols.gmt") #list

```