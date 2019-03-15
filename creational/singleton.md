# singleton_pattern:
* overview:
    restricts the instantiation of a class to a single object.
* advantages:
    - 
* drawbacks:
    - 
* application:
    - db connection manager
    - configuration manager
    - logger
    - cache
* implementation:
    - classic (lazy initialization):
        ```java
        class Singleton {
            private static Singleton obj;
            private Singleton() {}
            public static Singleton getInstance() {
                if (obj == null) {
                    obj = new Singleton();
                }
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: not thread-safe
    - eager instantiation:
        ```java
        class Singleton {
            private static final Singelton obj = new Singelton();
            private Singleton() {}
            public static Singleton getInstance() {
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: only useful when the class needs to be instantiated early to be used throughout the program.
    - thread-safe using method-level synchronization:
        ```java
        class Singleton {
            private static Singleton obj;
            private Singleton() {}
            public static synchronized Singleton getInstance() {
                if (obj == null) {
                    obj = new Singleton();
                }
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: every call to getInstance() requires taking a lock even after the initialization has been done, leading to performance issues.
    - thread-safe using class-level synchronization:
        ```java
        class Singleton {
            private volatile static Singleton obj;
            private Singleton() {}
            public static Singleton getInstance() {
                if (obj == null) {
                    synchronized(Singleton.class) {
                        if (obj == null) {
                            obj = new Singleton();
                        }
                    }
                }
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: some method outside can also acquire a class-level lock on Singleton.class leading to unexpected results.
    - thread-safe using object-level synchronization:
        ```java
        class Singleton {
            private volatile static Singleton obj;
            private static final Object lock = new Object();
            private Singleton() {}
            public static Singleton getInstance() {
                if (obj == null) {
                    synchronized(lock) {
                        if (obj == null) {
                            obj = new Singleton();
                        }
                    }
                }
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: ???
    - bill-pugh method (using inner class):
        ```java
        class Singleton {
            private Singleton() {}
            private static class SingletonHolder {
                public static final Singleton INSTANCE = new Singleton();
            }
            public static Singleton getInstance() {
                return SingletonHolder.INSTANCE;
            }
        }
        ```
        advantage: ???
        drawback: ???
    - enum-based:
        ```java
        enum Singleton {
            INSTANCE;
            private Singleton() {}
        }
        ```
        advantage: prevents reflection from breaking the pattern
        drawback: ???
    - with-serializable:
        ```java
        class Singleton implements Serializable {
            private static Singleton obj = new Singleton();
            private Singleton() {}
            public static getInstance() {
                return obj;
            }
            @Override
            protected Object readResolve() {
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: ???
    - with-cloneable:
        ```java
        class Parent implements Cloneable {
            @Override
            protected Object clone() throws CloneNotSupportedException {
                return super.clone();
            }
        }

        class Singleton extends Parent {
            private static Singleton obj = new Singleton();
            private Singleton() {}
            public static Singleton getInstance() {
                return obj;
            }
            @Override
            protected Object clone() throws CloneNotSupportedException {
                // option 1: throw exception
                throw new CloneNotSupportedException();

                // option 2: return the same instance
                return obj;
            }
        }
        ```
        advantage: ???
        drawback: ???