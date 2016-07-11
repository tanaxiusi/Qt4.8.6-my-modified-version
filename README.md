# Qt4.8.6-my-modified-version

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

    ConnectAtEnd = 0, // default
    
    ConnectAtBegin // the connection will be inserted at first, and be called firstly
    
};



For example:

original:

QObject::connect(sender, SIGNAL(xxx), receiver, SLOT(xxx), Qt::AutoConnection);

new:

QObject::connect(sender, SIGNAL(xxx), receiver, SLOT(xxx), Qt::AutoConnection, Qt::ConnectAtBegin);



The new dll(or .so in linux) is binary compatible with program which compile with original Qt.

But, program compiling with new version CAN NOT run with the original library.

