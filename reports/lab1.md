# 实现的功能：
    1. 完善sys_trace追踪当前任务系统调用的历史信息（/os/syscall/process.rs）
    2. 在syscall函数中增加更新系统调用计数器(/os/syscall/mod.rs)
    3. 扩充TaskControlBlock结构，增加系统调用次数（/os/task/task.rs）
    4. 完善sys_trace函数需要调用的函数：获取当前任务的系统调用次数、增加当前任务的系统调用次数（/os/task/mod.rs）
    5. 定义常量最大系统调用数及以上四个文件间的命名空间调用解决
# 简答作业
## 第1题
    U态是用户态，权限最低，不能执行S态的特权指令。
    ch2b_bad_address.rs：PageFault in application, kernel killed it.
    ch2b_bad_instructions.rs：IllegalInstruction in application, kernel killed it.
    ch2b_bad_register.rs：IllegalInstruction in application, kernel killed it.
    RustSBI版本：version 0.3.0-alpha.4
## 第2.1题
    sp 的值：sp 指向内核栈中保存的 Trap 上下文（TrapContext），该上下文包含了用户态寄存器和 CSR 的保存值。
    使用情景：
        处理完中断/异常后，从内核态返回用户态。
        任务切换时，恢复目标任务的上下文（例如进程调度）。
## 第2.2题
    处理的寄存器：sstatus、sepc、sscratch。
    意义：
        sstatus：控制处理器状态（如中断使能、权限模式），恢复后确保返回到用户态（SPP 位设置为用户模式）。
        sepc：保存异常返回地址，sret 指令会跳转到此地址继续执行用户程序。
        sscratch：用于内核与用户栈切换，恢复后保证下次 Trap 能正确切换栈。
## 第2.3题
    x2 (sp)：需最后通过 csrrw 交换恢复，避免提前破坏栈指针
    x4 (tp)：线程指针通常由内核固定设置，用户态无需修改，因此无需恢复。
## 第2.4题
    csrrw sp, sscratch, sp：交换后，sp 指向用户栈，sscratch 保存内核栈指针，为下次 Trap 做准备。
## 第2.5题
    切换指令：sret。原因：sret 根据 sstatus.SPP 切换特权级到用户态，并跳转到 sepc
## 第2.6题
    Trap 入口，sp 切到内核栈，sscratch 保存用户栈指针
## 第2.7题
    从U态进入S态是通过执行ecall指令，触发环境调用异常，进入S态处理程序

# 荣誉准则
在完成本次实验的过程（含此前学习的过程）中，我曾分别与 以下各位 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：
    https://opencamp.cn/os2edu/camp/2025spring/stage/2
    2025春夏季OS训练营专业阶段1群、2025春夏季OS训练营1群

此外，我也参考了 以下资料 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

    https://rcore-os.cn/rCore-Tutorial-Book-v3/
    https://learningos.cn/rCore-Tutorial-Guide-2025S/
    https://www.csdn.net/
    https://chat.deepseek.com/ 

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。

# 你对本次实验设计及难度/工作量的看法
    一阶段跳到二阶段难度陡增，具体表现如下
    1. 环境搭建复杂，不同的版面缺失的依赖不同。 建议统一搭建环境，指定具体的依赖版本号，不能高也不能低
    2. 完成的任务趋向复杂，需要考虑多个文件的修改。需要修改那些文件要深入理解整个项目的各个文件关系。   建议第一章手把手指定版本号，完成一个完整的实验，对新手更友好。
