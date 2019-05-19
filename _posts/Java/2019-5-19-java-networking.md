
layout: post
title: Java Network Programing
categories: Java
description: Java 网络
keywords: Java



UDP编程

client 端
1. 使用DatagramSocket  指定端口 创建发送端
2. 将基本类型  转成字节数组
3. 封装成DatagramPacket 包裹，需要指定目的地
4. 发送包裹send​(DatagramPacket p)  
5. 释放资源

server端
1. 使用DatagramSocket  指定端口 创建接收端
2. 准备容器 封装成DatagramPacket 包裹
3. 阻塞式接收包裹receive​(DatagramPacket p)
4. 分析数据    将字节数组还原为对应的类型
    *   byte[]  getData​()
    *   getLength​()
5. 释放资源

TCP编程

server端
1. 指定端口 使用ServerSocket创建服务器
2. 阻塞式等待连接 accept
3. 操作: 输入输出流操作
4. 释放资源 

client端
1. 建立连接: 使用Socket创建客户端 +服务的地址和端口
2. 操作: 输入输出流操作
3. 释放资源 

