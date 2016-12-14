# Qt4.8.6-my-modified-version

自用修改版Qt4.8.6


#QtCore

主要对QObject::connect进行修改

    enum ConnectionType {
        AutoConnection,    
        DirectConnection,    
        QueuedConnection,    
        AutoCompatConnection,    
        BlockingQueuedConnection,    
        ParallelBlockingQueuedConnection, //like BlockingQueuedConnection, but the receivers will begin together    
        UniqueConnection =  0x80    
    };

    enum ConnectionPosition {
        ConnectAtEnd = 0,
        ConnectAtBegin
    };


原版Qt没有ConnectionPosition，信号触发时槽函数的调用顺序由QObject::connect的连接顺序决定。

现在可以在连接时指定ConnectAtBegin，让该槽函数获得优先调用。

ParallelBlockingQueuedConnection是一种新的连接类型，类似于BlockingQueuedConnection，但不同的是，槽函数会同时被调用。


#QtGui
由于Qt4对高dpi支持得不好，所以我删除了Windows系统下SetProcessDPIAware的调用，修改版的QtGui被视为不支持高dpi的旧程序，在高dpi环境下，界面被放大，会模糊，比例不会失调。

#版本兼容性
####修改版的Qt模块，仅QtCore与原版不同(多了几个导出符号)，其他模块(QtGui、QtWidgets等)理论上与原版完全相同，可以随意混用。
####修改版在代码级、二进制级向下兼容原版。
####反过来，如果代码中没有用到修改版的新特性，就可以在原版上编译（必须的）；`但是，无论是否用到新特性，使用修改版编译的程序都无法在原版上运行`。
    
    编译所用的版本           程序可以在原版上运行      程序可以在修改版上运行
    原版                    √                       √
    修改版                  ×                       √
    
    如果试图用原版dll/so上运行使用修改版编译的程序，会因为找不到专有的导出符号而出错。
    Windows会弹出错误提示框，而Linux会在第一次运行到QObject::connect时崩溃。
