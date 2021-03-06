# Reproducing error

```
$ java -version                                                                                                                                                     master!+
openjdk version "11.0.6" 2020-01-14
OpenJDK Runtime Environment GraalVM CE 19.3.1 (build 11.0.6+9-jvmci-19.3-b07)
OpenJDK 64-Bit Server VM GraalVM CE 19.3.1 (build 11.0.6+9-jvmci-19.3-b07, mixed mode, sharing)
```

## Creating demo project
```
mn create-app -l kotlin -f=graal-native-image micronaut-graal-demo
```

+ implementing Controller + Declarative Client 

## Build native image within container and run
```
./gradlew assemble
./docker-build.sh

docker run -p 8080:8080 micronaut-graal-demo
curl http://localhost:8080/test
```

## Resulting Stacktrace
```
13:19:45.344 [main] INFO  io.micronaut.runtime.Micronaut - Startup completed in 25ms. Server Running: http://d24ae46f658f:8080
13:19:55.380 [pool-2-thread-3] ERROR i.m.r.intercept.RecoveryInterceptor - Type [micronaut.graal.demo.TodoClient$Intercepted] executed with error: Connect Error: jsonplaceholder.typicode.com: System error
io.micronaut.http.client.exceptions.HttpClientException: Connect Error: jsonplaceholder.typicode.com: System error
	at io.micronaut.http.client.DefaultHttpClient.lambda$null$27(DefaultHttpClient.java:1042)
	at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:577)
	at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:551)
	at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:490)
	at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:615)
	at io.netty.util.concurrent.DefaultPromise.setFailure0(DefaultPromise.java:608)
	at io.netty.util.concurrent.DefaultPromise.setFailure(DefaultPromise.java:109)
	at io.netty.channel.DefaultChannelPromise.setFailure(DefaultChannelPromise.java:89)
	at io.netty.bootstrap.Bootstrap.doResolveAndConnect0(Bootstrap.java:208)
	at io.netty.bootstrap.Bootstrap.access$000(Bootstrap.java:46)
	at io.netty.bootstrap.Bootstrap$1.operationComplete(Bootstrap.java:180)
	at io.netty.bootstrap.Bootstrap$1.operationComplete(Bootstrap.java:166)
	at io.netty.util.concurrent.DefaultPromise.notifyListener0(DefaultPromise.java:577)
	at io.netty.util.concurrent.DefaultPromise.notifyListenersNow(DefaultPromise.java:551)
	at io.netty.util.concurrent.DefaultPromise.notifyListeners(DefaultPromise.java:490)
	at io.netty.util.concurrent.DefaultPromise.setValue0(DefaultPromise.java:615)
	at io.netty.util.concurrent.DefaultPromise.setSuccess0(DefaultPromise.java:604)
	at io.netty.util.concurrent.DefaultPromise.trySuccess(DefaultPromise.java:104)
	at io.netty.channel.DefaultChannelPromise.trySuccess(DefaultChannelPromise.java:84)
	at io.netty.channel.AbstractChannel$AbstractUnsafe.safeSetSuccess(AbstractChannel.java:984)
	at io.netty.channel.AbstractChannel$AbstractUnsafe.register0(AbstractChannel.java:504)
	at io.netty.channel.AbstractChannel$AbstractUnsafe.access$200(AbstractChannel.java:417)
	at io.netty.channel.AbstractChannel$AbstractUnsafe$1.run(AbstractChannel.java:474)
	at io.netty.util.concurrent.AbstractEventExecutor.safeExecute(AbstractEventExecutor.java:164)
	at io.netty.util.concurrent.SingleThreadEventExecutor.runAllTasks(SingleThreadEventExecutor.java:472)
	at io.netty.channel.nio.NioEventLoop.run(NioEventLoop.java:500)
	at io.netty.util.concurrent.SingleThreadEventExecutor$4.run(SingleThreadEventExecutor.java:989)
	at io.netty.util.internal.ThreadExecutorMap$2.run(ThreadExecutorMap.java:74)
	at io.micronaut.scheduling.instrument.InvocationInstrumenterWrappedRunnable.run(InvocationInstrumenterWrappedRunnable.java:48)
	at io.netty.util.concurrent.FastThreadLocalRunnable.run(FastThreadLocalRunnable.java:30)
	at java.lang.Thread.run(Thread.java:834)
	at com.oracle.svm.core.thread.JavaThreads.threadStartRoutine(JavaThreads.java:497)
	at com.oracle.svm.core.posix.thread.PosixJavaThreads.pthreadStartRoutine(PosixJavaThreads.java:193)
Caused by: java.net.UnknownHostException: jsonplaceholder.typicode.com: System error
	at com.oracle.svm.jni.JNIJavaCallWrappers.jniInvoke_VA_LIST:Ljava_net_UnknownHostException_2_0002e_0003cinit_0003e_00028Ljava_lang_String_2_00029V(JNIJavaCallWrappers.java:0)
	at java.net.Inet4AddressImpl.lookupAllHostAddr(Inet4AddressImpl.java)
	at java.net.InetAddress$PlatformNameService.lookupAllHostAddr(InetAddress.java:929)
	at java.net.InetAddress.getAddressesFromNameService(InetAddress.java:1515)
	at java.net.InetAddress$NameServiceAddresses.get(InetAddress.java:848)
	at java.net.InetAddress.getAllByName0(InetAddress.java:1505)
	at java.net.InetAddress.getAllByName(InetAddress.java:1364)
	at java.net.InetAddress.getAllByName(InetAddress.java:1298)
	at java.net.InetAddress.getByName(InetAddress.java:1248)
	at io.netty.util.internal.SocketUtils$8.run(SocketUtils.java:148)
	at io.netty.util.internal.SocketUtils$8.run(SocketUtils.java:145)
	at java.security.AccessController.doPrivileged(AccessController.java:117)
	at io.netty.util.internal.SocketUtils.addressByName(SocketUtils.java:145)
	at io.netty.resolver.DefaultNameResolver.doResolve(DefaultNameResolver.java:43)
	at io.netty.resolver.SimpleNameResolver.resolve(SimpleNameResolver.java:63)
	at io.netty.resolver.SimpleNameResolver.resolve(SimpleNameResolver.java:55)
	at io.netty.resolver.InetSocketAddressResolver.doResolve(InetSocketAddressResolver.java:57)
	at io.netty.resolver.InetSocketAddressResolver.doResolve(InetSocketAddressResolver.java:32)
	at io.netty.resolver.AbstractAddressResolver.resolve(AbstractAddressResolver.java:108)
	at io.netty.bootstrap.Bootstrap.doResolveAndConnect0(Bootstrap.java:200)
	... 24 common frames omitted

```
