1. static类成员变量只有在static int时可以直接在类内声明同时定义，其他的都得在类外初始化。e.g.

    memfs::inode* memfs::root = new inode{"/",{},nullptr};


