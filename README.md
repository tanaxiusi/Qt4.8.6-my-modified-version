# Qt4.8.6-my-modified-version

自用修改版Qt4.8.6



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
