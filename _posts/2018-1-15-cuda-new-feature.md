---
layout:     post
title:      CUDA-new-feature
keywords:   new-feature
category:   CUDA
tags:		[CUDA编程]
---

介绍整理一些cuda的新的特性：如fp16， 块间同步等；


# New Features in CUDA 7.5 about 半精度（fp16）

https://devblogs.nvidia.com/parallelforall/new-features-cuda-7-5/

https://zhuanlan.zhihu.com/madeye/20125242




## fp16的指令

从xue的lstm的inference的代码中，整理出来：为什么这些函数，我都没用搜到呢？在哪个文档中？？

查看/usr/local/cuda-8.0/include/cuda_fp16.h中的定义：

    half2 h_tmp1_local = __float2half2_rn(0.0); 
    h_tmp1_local = __hfma2(h_value, T[output_index * hiddenSize / 2 + i], h_tmp1_local);

    half2 h_value1 = __float2half2_rn(0.0);
    h_value1 = __hadd2(h_value1, h_tmp1[blockDim.y * i + thread_index_inBlock]);

    half2 input_gate = h2rcp(__hadd2(__float2half2_rn(1), h2exp(__hneg2(g1))));

    half2 cell_value = __hmul2(__hsub2(h2exp(g4), h2exp(__hneg2(g4))), h2rcp(__hadd2(h2exp(g4), h2exp(__hneg2(g4)))));

    half2 cell_output = __hadd2(__hmul2(forget_gate, cell_now[thread_index_inBlock]), __hmul2(input_gate, cell_value));



# 块间同步in cuda 9
