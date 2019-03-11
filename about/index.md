---
layout: post
title: About
skip_related: true
---

Hi. I'm Guodong Jin.

## BIO

I am a second year Ph.D. candidate in the <a href="http://iir.ruc.edu.cn">DBIIR Lab</a> at <a href="http://www.ruc.edu.cn">Renmin University of China (RUC)</a> working on database systems. I am supervised by <a href="http://iir.ruc.edu.cn/~ygchen/"> Professor Yueguo Chen</a>. I earned my bachelor's degree from <a href="http://www.scu.edu.cn">Sichuan University</a> in the year of 2015, then I joined the successive master-doctor program of RUC, and spent two years as a master student in computer science. During these two years, I investigated <a href="http://hadoop.apache.org">Hadoop</a> and popular SQL-on-Hadoop systems, such as <a href="http://hive.apache.org">Hive</a>, <a href="http://spark.apache.org">Spark SQL</a> and <a href="http://prestodb.io">Presto</a>, and I built <a href="http://github.com/dbiir/paraflow">Paraflow</a>.

My current work investigates how to build an adaptive column store for big tables, including adaptive physical layout optimization and columnar caching. In the future, I plan to continue my research with a focus on investigating cross-model storage and computation integrating big relational tables and large dynamic graphs.

Besides daily researches, I enjoy coding and playing with open source projects.

Moreover, I'm the founder and co-organizer of <a href="https://dbiir.github.io/meetup">DBIIR Weekly Meetup</a>.

<hr/>

## RESEARCH AGENDA

The challenge of Big Data has shifted the design of data analytical systems from single machines to large-scale distributed systems.
My research focuses on key techniques of big data analytics to improve the performance of distributed analytical systems over big data.

Nowadays, many big data analysis systems share HDFS (Hadoop Distributed File System) as their common underlying storage, and relational tables are stored as columnar files to speed up query executions. The physical layouts of columnar files play a fundamental and critical role in system I/O performance, which is critical to the performance of existing analytical systems on HDFS. My current work investigates how to optimize physical layouts of columnar files adaptive to various workloads and storage devices.

In the future, I plan to continue my research in the field of big data analytics, with a foucus on building analytical systems to support cross-model storage and computation integrating big relational tables and large dynamic graphs, and exploit potential benefits of emerging new hardwares (such as NVM, GPU and FPGA). In my dissertation work, I hope to build an open source analytical system which is optimized efficiently and ready to be used.

<hr/>

## PUBLICATIONS

<li>Towards Real-Time Analysis of ID-Associated Data. <i>International Conference on Conceptual Modeling (Demonstration Track), Oct 2018.</i></li>
<small><b>Guodong JIN</b>, Yixuan Wang, Xiongpai QIN, Yueguo CHEN, Xiaoyong DU.</small>
<small><a href="./paper/paraflow-er-2018.pdf">[paper] </a><a href="./paper/paraflow-er-poster.pdf">[poster]</a></small>

<li>Rainbow: Adaptive Layout Optimization for Wide Tables. <i>IICDE, International Conference on Data Engineering (Demonstrantion Track), Apr 2018.</i></li>
<small>Haoqiong BIAN, Youxian TAO, <b>Guodong JIN</b>, Yueguo CHEN, Xiongpai QIN, Xiaoyong DU.</small>
<small><a href="./paper/rainbow-icde-2018.pdf">[paper] </a><a href="./paper/rainbow-icde-poster.pdf">[poster]</a> <a href="http://github.com/dbiir/rainbow">[code]</a></small>

<li>Entity Fiber based Partitioning, No Loss Staging and Fast Loading of Log Data. <i>PDCAT, Parallel and Distributed Computing, Applications and Technologies, Dec 2016.</i></li>
<small>Xiongpai QIN, Yueguo CHEN, <b>Guodong JIN</b>, Yang LIU, Yiming CONG, Xiaoyong DU.</small>
<small><a href="./paper/paraflow-pdcat-2016.pdf">[paper]</a> <a href="http://github.com/dbiir/paraflow">[code]</a></small>

<li>No Loss Staging and Fast Loading of Log Data (Written in Chinese).  <i>NDBC'16 Demo</i></li>
<small>Xiongpai QIN, <b>Guodong JIN</b>, Yang LIU, Yiming CONG, Xiaoyong DU.</small>
<small><a href="http://github.com/dbiir/paraflow">[code]</a></small>

<hr/>

## PROJECTS

<li><a href="#">Pixels</a>. A flexible column storage format with adaptive optimization techniques embedded.</li>
<li><a href="https://github.com/dbiir/rainbow">Rainbow</a>. A data layout optimization framework for wide tables stored on HDFS.</li>
<li><a href="https://github.com/dbiir/paraflow">Paraflow</a>. A real-time analytical system for ID-associated data.</li>
<li><a href="https://github.com/dbiir/pard">Pard</a>. A parallel database running like a leopard. This is a course project of <a href="https://github.com/fanju1984/ddb/tree/master/Fall%202017">Distributed Database Systems</a>.</li>
<li><a href="#">Claims</a>. A distributed in-memory database system, which I was involved during my internship at InfoSys Bangalore.</li>

<hr/>

## SKILLS

<li>Good understandings of Java as a system developing language.</li>
<li>Familiar with source code of <i>Facebook Presto</i> and <i>Apache ORC</i></li>
<li>Good communication skills both in English (TOEFL 103) and Mandarin Chinese.</li>
<li>Good at cooking, still improving.</li>
<li>Good team work spirit and considerable system design experiences.</li>

<hr/>

## TA

<li>Principles and Design of Database System (for graduate students). 2017.09 - 2018.01</li>
<small>A hard-core course on the principles of database systems for graduate students. During the course, each group of students is requried to implement a toy DBMS.</small>
<li>The Practice of Programming (for undergraduate students). 2016.09 - 2017.01</li>
<small>A startup course for undergraduate students to learn about programming languages and practice them! Javascript and PHP are covered.</small>

<hr/>

## NEWS

<li>Nov 2018: Great talk by <a href="http://iir.ruc.edu.cn/~zhangf/">Feng Zhang</a> at <a href="https://dbiir.github.io/meetup/meetup/2018/11/22/weekly-meetup.html">Weekly Meetup 3</a>!</li>
<li>Nov 2018: <a href="https://dbiir.github.io/meetup/meetup/2018/11/15/weekly-meetup.html">Weekly Meetup 2</a> is ON! <a href="http://iir.ruc.edu.cn/~yangjr/">Jingru Yang</a> gave an excellent talk on her newly accepted VLDB paper!</li>
<li>Nov 2018: <a href="https://dbiir.github.io/meetup/meetup/2018/11/08/weekly-meetup.html">Weekly Meetup 1</a> is ON! Great talk by <a href="http://iir.ruc.edu.cn/~chenj/index.html">Jun Chen</a>, sharing lots of researching experience with us.</li>
<li>Nov 2018: Our first <a href="https://dbiir.github.io/meetup/meetup/2018/11/01/weekly-meetup.html">meetup</a> is organized successfully. Thanks to our organizing commitee members!</li>
<li>Oct 2018: I'm demonstrating <a href="./paper/paraflow-er-2018.pdf">Towards Real-Time Analysis of ID-Associated Data</a> at ER 2018 (Xi'an, China).</li>
<li>Oct 2018: Excited to attend <a href="http://ndbc2018.dlmu.edu.cn">NDBC'2018</a> at Dalian.</li>
<li>Apr 2018: I'm demonstrating <a href="./paper/rainbow-icde-2018.pdf">Rainbow: Adaptive Layout Optimization for Wide Tables</a> at ICDE 2018 (Paris France).</li>
<li>Sep 2017: I'm TA'ing the <i>Principles and Design of Database System</i> (for graduate students). Working hard!</li>
<li>Jul 2017: We started a new project called <i>Pixels</i>.</li>
<li>Jul 2017: Attending <a href="https://strata.oreilly.com.cn/strata-cn?locale=en">Strata Beijing 2017</a>. Happy to meet new friends!</li>
<li>Dec 2016: I'm joining <a href="https://www.infosys.com/instep/default.asp">InfoStep</a> (at Bangalore, India), an internship program hosted by <i>InfoSys</i>, to develop the distributed in-memory database system (called Claims). Three months in India!</li>
<li>Aug 2016: We got a seventeen position in the second round of<a href="https://tianchi.shuju.aliyun.com/programming/introduction.htm?spm=5176.100066.333.3.Qtkjcf&raceId=231533"> Midleware Development Performance Challenge</a> hosted by<i>  Alibaba Group</i>.</li>
<small>The contest requires to develop a system from scratch (without any other library dependencies except for JDK) to load and support queries over 100GB relational dataset as efficiently as possible on a single cheap server with only 4GB memroy. And Java is the only choice as the programming language.</small>
<li>Aug 2016: I'm attending <a href="https://strata.oreilly.com.cn/hadoop-big-data-cn?locale=en">Strata+Hadoop World Beijing</a>. Excited to meet Doug Cutting, and learn about excellent open source projects.</li>

This blog is [open source][os] - and forked from [swanson][sw], whom I should give my thanks!

You can reach me at [`guod.jin@gmail.com`][email] or [`@ray6080`][twitter].

[email]: mailto:guod.jin@gmail.com
[twitter]: https://twitter.com/ray6080
[os]: https://github.com/ray6080/ray6080.github.io
[sw]: https://github.com/swanson/swanson.github.com
[rim]: http://store.steampowered.com/app/294100/
[civ]: http://store.steampowered.com/app/8930/