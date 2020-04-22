### JVM(Java 虚拟机)

#### Java 内存结构：

- **Java堆：**堆内存是JVM中最大的一块，由老年代和年轻代组成(年轻代分为：Eden空间，From Survivor空间，To Survivor空间(8：1：1))
- **方法区：**存储类信息，常量，静态常量等数据，线程共享
- **java虚拟机栈：**java虚拟机栈 + 本地方法栈，主要存放方法内的局部变量
- **程序计数器：** 每条线程都需要有一个程序计数器，计数器记录的是正在执行的指令地址，如果正在执行的是Natvie 方法，这个计数器值为空（Undefined）
- 本地方法区



- Java 内存结构

  

- 

#### JMM (Java 内存模型)

| study_learningreport        | 学习页_查看学习报告  |
| --------------------------- | -------------------- |
| study_predictedscore        | 学习页_预估分        |
| study_quickgrading          | 学习页_快速提分      |
| study_clockinrecord         | 学习页_打卡日历      |
| study_filesdownload         | 学习页_资料下载      |
| study_homework              | 学习页_课后作业      |
| study_materials             | 学习页_资料          |
| study_course                | 学习页_听课          |
| study_coursecalendar        | 学习页_查看课程日历  |
| study_course_completereplay | 学习页_听课_完整重播 |
| study_course_keypoints      | 学习页_听课_重点串讲 |
| study_course_exercises      | 学习页_听课_练习题   |
| study_course_shortvideos    | 学习页_听课_碎片视频 |