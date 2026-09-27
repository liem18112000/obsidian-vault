---
title: "Java (and ecosystem) deep dive - Sprint [79, 80]"
type: source
status: reference
source: https://axonivy.atlassian.net/wiki/spaces/TS/pages/47212725725/Java+and+ecosystem+deep+dive+-+Sprint+79+80
space: "TS"
topic: programming
relevance: 0.947
depth: 3
updated: 2022-12-03
attachments: 0
tags:
  - confluence
  - programming
  - space/ts
---

# Java (and ecosystem) deep dive - Sprint [79, 80]

> [!info] Imported from Confluence
> Space **TS** · updated 2022-12-03 · [open original](https://axonivy.atlassian.net/wiki/spaces/TS/pages/47212725725/Java+and+ecosystem+deep+dive+-+Sprint+79+80)
> Relevance 0.947 · topic `programming`

1 - 2 3 5 8 - 6 11

# I. Generic

- When? Why?

- Wildcard, Type Parameters, Bound

- Combination with interface and design pattern

# II. Stream API

- Stream under the hood

- Stream of primitive (IntStream, LongStream, DoubleStream)

- flatMap

- Reduction (accumulator, combiner)

- Parallel Stream

# III. IO

InputStream/OutputStream

BufferedReader

- POSIX file operation (how IO works under the hood)

- Buffer - Why, when

- RandomAccessFile

- FileDescriptor

- Channel, Socket

- NIO vs IO

# IV. Serialization/Deserialization

- java.io.Serializable and why shouldn’t use it

- Customize serialization behavior

- Provider/JAX-RS

- Other options beside JSON: MessagePack, gRPC, XML

# V. MultiThreading/Concurrency/Async

Multi-Threading

- Executor Service

- Runnable and Callable

- Request model: Thread per request, Thread per connection,…

---

Concurrency

- Race condition, Deadlock, Starvation,…

- Mutex: synchronize, Lock, Reentrance Look, StampedeLock,…

- Semaphore

- CountDownLatch

---

Async

- Future/CompletableFuture

- CompletionStage

- Reactive: Uni and Multi

- Event-driven, non-blocking IO

# VI. Encoding

- Encoder/Decoder

- Formatter

- Codec

Only one thing u need to know tbh: UTF-8

# VII. Time

- Java 8 time API:

ZonedDateTime

Period

Duration

Calendar

DateTimeFormatter

Instance

Clock

- TemporalAdjuster

- Temporal

# VIII. Transaction

- Commit, Auto-commit

- Attach, Detach entities

- Auto-flush

- Rollback

- TransactionType

# IX. Immutable

- Functional programming

- Varv

- Value/Data class

- Thread Safety

# X. AOP - Aspect-Oriented Programming

- Runtime modification

- Code generation

# XI. Memory

- Memory model

- GC cycle

- Heap, Stack, ThreadLocal

- Memory Leak

- JVM tunning

# XII. Exception

- Exception handling

- To throw or not to throw

# XIII. Reflection

- Basic reflection

- Internal state

- MethodHandler, CallSite, invokedynamic

- Please don’t use this in your code

# XIV. DI/IOC

- Concept

- DI framework (Spring, Guice, CDI,…)

- Provider, Producer, InjectionPoint

# XV. Others

Collections framework and beyond? Streams API? Java EE specs (CDI, Transactions, Security, Filter, Interceptor, Event, …), Java NIO, Async within Java (Future, Completable Future, Completion Stage,…)
