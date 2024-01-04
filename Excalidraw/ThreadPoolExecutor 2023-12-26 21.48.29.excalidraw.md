---

excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠==


# Text Elements
提交任务 ^4n420RxD

拒绝策略 ^HPDOSMbb

放入队列 ^kOE4EpYm

创建线程执行任务 ^vryEFD3S

创建线程执行任务 ^NsXRwofq

已到
最大线程数 ^wJT1xs79

否 ^XQfTqQvv

否 ^G5aan4TS

否 ^YqCmve2i

是 ^nSVnUTBr

是 ^aH6hUxEf

是 ^ZKfTL8XU

到达核心线程数 ^doY0TAMK

阻塞队列
已满 ^LFRrU0lX

/*
 * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.
 *
 * This code is free software; you can redistribute it and/or modify it
 * under the terms of the GNU General Public License version 2 only, as
 * published by the Free Software Foundation.  Oracle designates this
 * particular file as subject to the "Classpath" exception as provided
 * by Oracle in the LICENSE file that accompanied this code.
 *
 * This code is distributed in the hope that it will be useful, but WITHOUT
 * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
 * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License
 * version 2 for more details (a copy is included in the LICENSE file that
 * accompanied this code).
 *
 * You should have received a copy of the GNU General Public License version
 * 2 along with this work; if not, write to the Free Software Foundation,
 * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.
 *
 * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA
 * or visit www.oracle.com if you need additional information or have any
 * questions.
 */

/*
 * This file is available under and governed by the GNU General Public
 * License version 2 only, as published by the Free Software Foundation.
 * However, the following notice accompanied the original version of this
 * file:
 *
 * Written by Doug Lea with assistance from members of JCP JSR-166
 * Expert Group and released to the public domain, as explained at
 * http://creativecommons.org/publicdomain/zero/1.0/
 */

package java.util.concurrent;

import java.util.ArrayList;
import java.util.ConcurrentModificationException;
import java.util.HashSet;
import java.util.Iterator;
import java.util.List;
import java.util.concurrent.atomic.AtomicInteger;
import java.util.concurrent.locks.AbstractQueuedSynchronizer;
import java.util.concurrent.locks.Condition;
import java.util.concurrent.locks.ReentrantLock;

/**
 * An {@link ExecutorService} that executes each submitted task using
 * one of possibly several pooled threads, normally configured
 * using {@link Executors} factory methods.
 *
 * <p>Thread pools address two different problems: they usually
 * provide improved performance when executing large numbers of
 * asynchronous tasks, due to reduced per-task invocation overhead,
 * and they provide a means of bounding and managing the resources,
 * including threads, consumed when executing a collection of tasks.
 * Each {@code ThreadPoolExecutor} also maintains some basic
 * statistics, such as the number of completed tasks.
 *
 * <p>To be useful across a wide range of contexts, this class
 * provides many adjustable parameters and extensibility
 * hooks. However, programmers are urged to use the more convenient
 * {@link Executors} factory methods {@link
 * Executors#newCachedThreadPool} (unbounded thread pool, with
 * automatic thread reclamation), {@link Executors#newFixedThreadPool}
 * (fixed size thread pool) and {@link
 * Executors#newSingleThreadExecutor} (single background thread), that
 * preconfigure settings for the most common usage
 * scenarios. Otherwise, use the following guide when manually
 * configuring and tuning this class:
 *
 * <dl>
 *
 * <dt>Core and maximum pool sizes</dt>
 *
 * <dd>A {@code ThreadPoolExecutor} will automatically adjust the
 * pool size (see {@link #getPoolSize})
 * according to the bounds set by
 * corePoolSize (see {@link #getCorePoolSize}) and
 * maximumPoolSize (see {@link #getMaximumPoolSize}).
 *
 * When a new task is submitted in method {@link #execute(Runnable)},
 * if fewer than corePoolSize threads are running, a new thread is
 * created to handle the request, even if other worker threads are
 * idle.  Else if fewer than maximumPoolSize threads are running, a
 * new thread will be created to handle the request only if the queue
 * is full.  By setting corePoolSize and maximumPoolSize the same, you
 * create a fixed-size thread pool. By setting maximumPoolSize to an
 * essentially unbounded value such as {@code Integer.MAX_VALUE}, you
 * allow the pool to accommodate an arbitrary number of concurrent
 * tasks. Most typically, core and maximum pool sizes are set only
 * upon construction, but they may also be changed dynamically using
 * {@link #setCorePoolSize} and {@link #setMaximumPoolSize}. </dd>
 *
 * <dt>On-demand construction</dt>
 *
 * <dd>By default, even core threads are initially created and
 * started only when new tasks arrive, but this can be overridden
 * dynamically using method {@link #prestartCoreThread} or {@link
 * #prestartAllCoreThreads}.  You probably want to prestart threads if
 * you construct the pool with a non-empty queue. </dd>
 *
 * <dt>Creating new threads</dt>
 *
 * <dd>New threads are created using a {@link ThreadFactory}.  If not
 * otherwise specified, a {@link Executors#defaultThreadFactory} is
 * used, that creates threads to all be in the same {@link
 * ThreadGroup} and with the same {@code NORM_PRIORITY} priority and
 * non-daemon status. By supplying a different ThreadFactory, you can
 * alter the thread's name, thread group, priority, daemon status,
 * etc. If a {@code ThreadFactory} fails to create a thread when asked
 * by returning null from {@code newThread}, the executor will
 * continue, but might not be able to execute any tasks. Threads
 * should possess the "modifyThread" {@code RuntimePermission}. If
 * worker threads or other threads using the pool do not possess this
 * permission, service may be degraded: configuration changes may not
 * take effect in a timely manner, and a shutdown pool may remain in a
 * state in which termination is possible but not completed.</dd>
 *
 * <dt>Keep-alive times</dt>
 *
 * <dd>If the pool currently has more than corePoolSize threads,
 * excess threads will be terminated if they have been idle for more
 * than the keepAliveTime (see {@link #getKeepAliveTime(TimeUnit)}).
 * This provides a means of reducing resource consumption when the
 * pool is not being actively used. If the pool becomes more active
 * later, new threads will be constructed. This parameter can also be
 * changed dynamically using method {@link #setKeepAliveTime(long,
 * TimeUnit)}.  Using a value of {@code Long.MAX_VALUE} {@link
 * TimeUnit#NANOSECONDS} effectively disables idle threads from ever
 * terminating prior to shut down. By default, the keep-alive policy
 * applies only when there are more than corePoolSize threads, but
 * method {@link #allowCoreThreadTimeOut(boolean)} can be used to
 * apply this time-out policy to core threads as well, so long as the
 * keepAliveTime value is non-zero. </dd>
 *
 * <dt>Queuing</dt>
 *
 * <dd>Any {@link BlockingQueue} may be used to transfer and hold
 * submitted tasks.  The use of this queue interacts with pool sizing:
 *
 * <ul>
 *
 * <li>If fewer than corePoolSize threads are running, the Executor
 * always prefers adding a new thread
 * rather than queuing.
 *
 * <li>If corePoolSize or more threads are running, the Executor
 * always prefers queuing a request rather than adding a new
 * thread.
 *
 * <li>If a request cannot be queued, a new thread is created unless
 * this would exceed maximumPoolSize, in which case, the task will be
 * rejected.
 *
 * </ul>
 *
 * There are three general strategies for queuing:
 * <ol>
 *
 * <li><em> Direct handoffs.</em> A good default choice for a work
 * queue is a {@link SynchronousQueue} that hands off tasks to threads
 * without otherwise holding them. Here, an attempt to queue a task
 * will fail if no threads are immediately available to run it, so a
 * new thread will be constructed. This policy avoids lockups when
 * handling sets of requests that might have internal dependencies.
 * Direct handoffs generally require unbounded maximumPoolSizes to
 * avoid rejection of new submitted tasks. This in turn admits the
 * possibility of unbounded thread growth when commands continue to
 * arrive on average faster than they can be processed.
 *
 * <li><em> Unbounded queues.</em> Using an unbounded queue (for
 * example a {@link LinkedBlockingQueue} without a predefined
 * capacity) will cause new tasks to wait in the queue when all
 * corePoolSize threads are busy. Thus, no more than corePoolSize
 * threads will ever be created. (And the value of the maximumPoolSize
 * therefore doesn't have any effect.)  This may be appropriate when
 * each task is completely independent of others, so tasks cannot
 * affect each others execution; for example, in a web page server.
 * While this style of queuing can be useful in smoothing out
 * transient bursts of requests, it admits the possibility of
 * unbounded work queue growth when commands continue to arrive on
 * average faster than they can be processed.
 *
 * <li><em>Bounded queues.</em> A bounded queue (for example, an
 * {@link ArrayBlockingQueue}) helps prevent resource exhaustion when
 * used with finite maximumPoolSizes, but can be more difficult to
 * tune and control.  Queue sizes and maximum pool sizes may be traded
 * off for each other: Using large queues and small pools minimizes
 * CPU usage, OS resources, and context-switching overhead, but can
 * lead to artificially low throughput.  If tasks frequently block (for
 * example if they are I/O bound), a system may be able to schedule
 * time for more threads than you otherwise allow. Use of small queues
 * generally requires larger pool sizes, which keeps CPUs busier but
 * may encounter unacceptable scheduling overhead, which also
 * decreases throughput.
 *
 * </ol>
 *
 * </dd>
 *
 * <dt>Rejected tasks</dt>
 *
 * <dd>New tasks submitted in method {@link #execute(Runnable)} will be
 * <em>rejected</em> when the Executor has been shut down, and also when
 * the Executor uses finite bounds for both maximum threads and work queue
 * capacity, and is saturated.  In either case, the {@code execute} method
 * invokes the {@link
 * RejectedExecutionHandler#rejectedExecution(Runnable, ThreadPoolExecutor)}
 * method of its {@link RejectedExecutionHandler}.  Four predefined handler
 * policies are provided:
 *
 * <ol>
 *
 * <li>In the default {@link ThreadPoolExecutor.AbortPolicy}, the handler
 * throws a runtime {@link RejectedExecutionException} upon rejection.
 *
 * <li>In {@link ThreadPoolExecutor.CallerRunsPolicy}, the thread
 * that invokes {@code execute} itself runs the task. This provides a
 * simple feedback control mechanism that will slow down the rate that
 * new tasks are submitted.
 *
 * <li>In {@link ThreadPoolExecutor.DiscardPolicy}, a task that cannot
 * be executed is simply dropped. This policy is designed only for
 * those rare cases in which task completion is never relied upon.
 *
 * <li>In {@link ThreadPoolExecutor.DiscardOldestPolicy}, if the
 * executor is not shut down, the task at the head of the work queue
 * is dropped, and then execution is retried (which can fail again,
 * causing this to be repeated.) This policy is rarely acceptable.  In
 * nearly all cases, you should also cancel the task to cause an
 * exception in any component waiting for its completion, and/or log
 * the failure, as illustrated in {@link
 * ThreadPoolExecutor.DiscardOldestPolicy} documentation.
 *
 * </ol>
 *
 * It is possible to define and use other kinds of {@link
 * RejectedExecutionHandler} classes. Doing so requires some care
 * especially when policies are designed to work only under particular
 * capacity or queuing policies. </dd>
 *
 * <dt>Hook methods</dt>
 *
 * <dd>This class provides {@code protected} overridable
 * {@link #beforeExecute(Thread, Runnable)} and
 * {@link #afterExecute(Runnable, Throwable)} methods that are called
 * before and after execution of each task.  These can be used to
 * manipulate the execution environment; for example, reinitializing
 * ThreadLocals, gathering statistics, or adding log entries.
 * Additionally, method {@link #terminated} can be overridden to perform
 * any special processing that needs to be done once the Executor has
 * fully terminated.
 *
 * <p>If hook, callback, or BlockingQueue methods throw exceptions,
 * internal worker threads may in turn fail, abruptly terminate, and
 * possibly be replaced.</dd>
 *
 * <dt>Queue maintenance</dt>
 *
 * <dd>Method {@link #getQueue()} allows access to the work queue
 * for purposes of monitoring and debugging.  Use of this method for
 * any other purpose is strongly discouraged.  Two supplied methods,
 * {@link #remove(Runnable)} and {@link #purge} are available to
 * assist in storage reclamation when large numbers of queued tasks
 * become cancelled.</dd>
 *
 * <dt>Reclamation</dt>
 *
 * <dd>A pool that is no longer referenced in a program <em>AND</em>
 * has no remaining threads may be reclaimed (garbage collected)
 * without being explicitly shutdown. You can configure a pool to
 * allow all unused threads to eventually die by setting appropriate
 * keep-alive times, using a lower bound of zero core threads and/or
 * setting {@link #allowCoreThreadTimeOut(boolean)}.  </dd>
 *
 * </dl>
 *
 * <p><b>Extension example.</b> Most extensions of this class
 * override one or more of the protected hook methods. For example,
 * here is a subclass that adds a simple pause/resume feature:
 *
 * <pre> {@code
 * class PausableThreadPoolExecutor extends ThreadPoolExecutor {
 *   private boolean isPaused;
 *   private ReentrantLock pauseLock = new ReentrantLock();
 *   private Condition unpaused = pauseLock.newCondition();
 *
 *   public PausableThreadPoolExecutor(...) { super(...); }
 *
 *   protected void beforeExecute(Thread t, Runnable r) {
 *     super.beforeExecute(t, r);
 *     pauseLock.lock();
 *     try {
 *       while (isPaused) unpaused.await();
 *     } catch (InterruptedException ie) {
 *       t.interrupt();
 *     } finally {
 *       pauseLock.unlock();
 *     }
 *   }
 *
 *   public void pause() {
 *     pauseLock.lock();
 *     try {
 *       isPaused = true;
 *     } finally {
 *       pauseLock.unlock();
 *     }
 *   }
 *
 *   public void resume() {
 *     pauseLock.lock();
 *     try {
 *       isPaused = false;
 *       unpaused.signalAll();
 *     } finally {
 *       pauseLock.unlock();
 *     }
 *   }
 * }}</pre>
 *
 * @since 1.5
 * @author Doug Lea
 */
public class ThreadPoolExecutor extends AbstractExecutorService {
    /**
     * The main pool control state, ctl, is an atomic integer packing
     * two conceptual fields
     *   workerCount, indicating the effective number of threads
     *   runState,    indicating whether running, shutting down etc
     *
     * In order to pack them into one int, we limit workerCount to
     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2
     * billion) otherwise representable. If this is ever an issue in
     * the future, the variable can be changed to be an AtomicLong,
     * and the shift/mask constants below adjusted. But until the need
     * arises, this code is a bit faster and simpler using an int.
     *
     * The workerCount is the number of workers that have been
     * permitted to start and not permitted to stop.  The value may be
     * transiently different from the actual number of live threads,
     * for example when a ThreadFactory fails to create a thread when
     * asked, and when exiting threads are still performing
     * bookkeeping before terminating. The user-visible pool size is
     * reported as the current size of the workers set.
     *
     * The runState provides the main lifecycle control, taking on values:
     *
     *   RUNNING:  Accept new tasks and process queued tasks
     *   SHUTDOWN: Don't accept new tasks, but process queued tasks
     *   STOP:     Don't accept new tasks, don't process queued tasks,
     *             and interrupt in-progress tasks
     *   TIDYING:  All tasks have terminated, workerCount is zero,
     *             the thread transitioning to state TIDYING
     *             will run the terminated() hook method
     *   TERMINATED: terminated() has completed
     *
     * The numerical order among these values matters, to allow
     * ordered comparisons. The runState monotonically increases over
     * time, but need not hit each state. The transitions are:
     *
     * RUNNING -> SHUTDOWN
     *    On invocation of shutdown()
     * (RUNNING or SHUTDOWN) -> STOP
     *    On invocation of shutdownNow()
     * SHUTDOWN -> TIDYING
     *    When both queue and pool are empty
     * STOP -> TIDYING
     *    When pool is empty
     * TIDYING -> TERMINATED
     *    When the terminated() hook method has completed
     *
     * Threads waiting in awaitTermination() will return when the
     * state reaches TERMINATED.
     *
     * Detecting the transition from SHUTDOWN to TIDYING is less
     * straightforward than you'd like because the queue may become222
     * empty after non-empty and vice versa during SHUTDOWN state, but
     * we can only terminate if, after seeing that it is empty, we see
     * that workerCount is 0 (which sometimes entails a recheck -- see
     * below).
     */
    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));
    private static final int COUNT_BITS = Integer.SIZE - 3;
    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;

    // runState is stored in the high-order bits
    private static final int RUNNING    = -1 << COUNT_BITS;
    private static final int SHUTDOWN   =  0 << COUNT_BITS;
    private static final int STOP       =  1 << COUNT_BITS;
    private static final int TIDYING    =  2 << COUNT_BITS;
    private static final int TERMINATED =  3 << COUNT_BITS;

    // Packing and unpacking ctl
    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }
    private static int workerCountOf(int c)  { return c & COUNT_MASK; }
    private static int ctlOf(int rs, int wc) { return rs | wc; }

    /*
     * Bit field accessors that don't require unpacking ctl.
     * These depend on the bit layout and on workerCount being never negative.
     */

    private static boolean runStateLessThan(int c, int s) {
        return c < s;
    }

    private static boolean runStateAtLeast(int c, int s) {
        return c >= s;
    }

    private static boolean isRunning(int c) {
        return c < SHUTDOWN;
    }

    /**
     * Attempts to CAS-increment the workerCount field of ctl.
     */
    private boolean compareAndIncrementWorkerCount(int expect) {
        return ctl.compareAndSet(expect, expect + 1);
    }

    /**
     * Attempts to CAS-decrement the workerCount field of ctl.
     */
    private boolean compareAndDecrementWorkerCount(int expect) {
        return ctl.compareAndSet(expect, expect - 1);
    }

    /**
     * Decrements the workerCount field of ctl. This is called only on
     * abrupt termination of a thread (see processWorkerExit). Other
     * decrements are performed within getTask.
     */
    private void decrementWorkerCount() {
        ctl.addAndGet(-1);
    }

    /**
     * The queue used for holding tasks and handing off to worker
     * threads.  We do not require that workQueue.poll() returning
     * null necessarily means that workQueue.isEmpty(), so rely
     * solely on isEmpty to see if the queue is empty (which we must
     * do for example when deciding whether to transition from
     * SHUTDOWN to TIDYING).  This accommodates special-purpose
     * queues such as DelayQueues for which poll() is allowed to
     * return null even if it may later return non-null when delays
     * expire.
     */
    private final BlockingQueue<Runnable> workQueue;

    /**
     * Lock held on access to workers set and related bookkeeping.
     * While we could use a concurrent set of some sort, it turns out
     * to be generally preferable to use a lock. Among the reasons is
     * that this serializes interruptIdleWorkers, which avoids
     * unnecessary interrupt storms, especially during shutdown.
     * Otherwise exiting threads would concurrently interrupt those
     * that have not yet interrupted. It also simplifies some of the
     * associated statistics bookkeeping of largestPoolSize etc. We
     * also hold mainLock on shutdown and shutdownNow, for the sake of
     * ensuring workers set is stable while separately checking
     * permission to interrupt and actually interrupting.
     */
    private final ReentrantLock mainLock = new ReentrantLock();

    /**
     * Set containing all worker threads in pool. Accessed only when
     * holding mainLock.
     */
    private final HashSet<Worker> workers = new HashSet<>();

    /**
     * Wait condition to support awaitTermination.
     */
    private final Condition termination = mainLock.newCondition();

    /**
     * Tracks largest attained pool size. Accessed only under
     * mainLock.
     */
    private int largestPoolSize;

    /**
     * Counter for completed tasks. Updated only on termination of
     * worker threads. Accessed only under mainLock.
     */
    private long completedTaskCount;

    /*
     * All user control parameters are declared as volatiles so that
     * ongoing actions are based on freshest values, but without need
     * for locking, since no internal invariants depend on them
     * changing synchronously with respect to other actions.
     */

    /**
     * Factory for new threads. All threads are created using this
     * factory (via method addWorker).  All callers must be prepared
     * for addWorker to fail, which may reflect a system or user's
     * policy limiting the number of threads.  Even though it is not
     * treated as an error, failure to create threads may result in
     * new tasks being rejected or existing ones remaining stuck in
     * the queue.
     *
     * We go further and preserve pool invariants even in the face of
     * errors such as OutOfMemoryError, that might be thrown while
     * trying to create threads.  Such errors are rather common due to
     * the need to allocate a native stack in Thread.start, and users
     * will want to perform clean pool shutdown to clean up.  There
     * will likely be enough memory available for the cleanup code to
     * complete without encountering yet another OutOfMemoryError.
     */
    private volatile ThreadFactory threadFactory;

    /**
     * Handler called when saturated or shutdown in execute.
     */
    private volatile RejectedExecutionHandler handler;

    /**
     * Timeout in nanoseconds for idle threads waiting for work.
     * Threads use this timeout when there are more than corePoolSize
     * present or if allowCoreThreadTimeOut. Otherwise they wait
     * forever for new work.
     */
    private volatile long keepAliveTime;

    /**
     * If false (default), core threads stay alive even when idle.
     * If true, core threads use keepAliveTime to time out waiting
     * for work.
     */
    private volatile boolean allowCoreThreadTimeOut;

    /**
     * Core pool size is the minimum number of workers to keep alive
     * (and not allow to time out etc) unless allowCoreThreadTimeOut
     * is set, in which case the minimum is zero.
     *
     * Since the worker count is actually stored in COUNT_BITS bits,
     * the effective limit is {@code corePoolSize & COUNT_MASK}.
     */
    private volatile int corePoolSize;

    /**
     * Maximum pool size.
     *
     * Since the worker count is actually stored in COUNT_BITS bits,
     * the effective limit is {@code maximumPoolSize & COUNT_MASK}.
     */
    private volatile int maximumPoolSize;

    /**
     * The default rejected execution handler.
     */
    private static final RejectedExecutionHandler defaultHandler =
        new AbortPolicy();

    /**
     * Permission required for callers of shutdown and shutdownNow.
     * We additionally require (see checkShutdownAccess) that callers
     * have permission to actually interrupt threads in the worker set
     * (as governed by Thread.interrupt, which relies on
     * ThreadGroup.checkAccess, which in turn relies on
     * SecurityManager.checkAccess). Shutdowns are attempted only if
     * these checks pass.
     *
     * All actual invocations of Thread.interrupt (see
     * interruptIdleWorkers and interruptWorkers) ignore
     * SecurityExceptions, meaning that the attempted interrupts
     * silently fail. In the case of shutdown, they should not fail
     * unless the SecurityManager has inconsistent policies, sometimes
     * allowing access to a thread and sometimes not. In such cases,
     * failure to actually interrupt threads may disable or delay full
     * termination. Other uses of interruptIdleWorkers are advisory,
     * and failure to actually interrupt will merely delay response to
     * configuration changes so is not handled exceptionally.
     */
    private static final RuntimePermission shutdownPerm =
        new RuntimePermission("modifyThread");

    /**
     * Class Worker mainly maintains interrupt control state for
     * threads running tasks, along with other minor bookkeeping.
     * This class opportunistically extends AbstractQueuedSynchronizer
     * to simplify acquiring and releasing a lock surrounding each
     * task execution.  This protects against interrupts that are
     * intended to wake up a worker thread waiting for a task from
     * instead interrupting a task being run.  We implement a simple
     * non-reentrant mutual exclusion lock rather than use
     * ReentrantLock because we do not want worker tasks to be able to
     * reacquire the lock when they invoke pool control methods like
     * setCorePoolSize.  Additionally, to suppress interrupts until
     * the thread actually starts running tasks, we initialize lock
     * state to a negative value, and clear it upon start (in
     * runWorker).
     */
    private final class Worker
        extends AbstractQueuedSynchronizer
        implements Runnable
    {
        /**
         * This class will never be serialized, but we provide a
         * serialVersionUID to suppress a javac warning.
         */
        private static final long serialVersionUID = 6138294804551838833L;

        /** Thread this worker is running in.  Null if factory fails. */
        @SuppressWarnings("serial") // Unlikely to be serializable
        final Thread thread;
        /** Initial task to run.  Possibly null. */
        @SuppressWarnings("serial") // Not statically typed as Serializable
        Runnable firstTask;
        /** Per-thread task counter */
        volatile long completedTasks;

        // TODO: switch to AbstractQueuedLongSynchronizer and move
        // completedTasks into the lock word.

        /**
         * Creates with given first task and thread from ThreadFactory.
         * @param firstTask the first task (null if none)
         */
        Worker(Runnable firstTask) {
            setState(-1); // inhibit interrupts until runWorker
            this.firstTask = firstTask;
            this.thread = getThreadFactory().newThread(this);
        }

        /** Delegates main run loop to outer runWorker. */
        public void run() {
            runWorker(this);
        }

        // Lock methods
        //
        // The value 0 represents the unlocked state.
        // The value 1 represents the locked state.

        protected boolean isHeldExclusively() {
            return getState() != 0;
        }

        protected boolean tryAcquire(int unused) {
            if (compareAndSetState(0, 1)) {
                setExclusiveOwnerThread(Thread.currentThread());
                return true;
            }
            return false;
        }

        protected boolean tryRelease(int unused) {
            setExclusiveOwnerThread(null);
            setState(0);
            return true;
        }

        public void lock()        { acquire(1); }
        public boolean tryLock()  { return tryAcquire(1); }
        public void unlock()      { release(1); }
        public boolean isLocked() { return isHeldExclusively(); }

        void interruptIfStarted() {
            Thread t;
            if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {
                try {
                    t.interrupt();
                } catch (SecurityException ignore) {
                }
            }
        }
    }

    /*
     * Methods for setting control state
     */

    /**
     * Transitions runState to given target, or leaves it alone if
     * already at least the given target.
     *
     * @param targetState the desired state, either SHUTDOWN or STOP
     *        (but not TIDYING or TERMINATED -- use tryTerminate for that)
     */
    private void advanceRunState(int targetState) {
        // assert targetState == SHUTDOWN || targetState == STOP;
        for (;;) {
            int c = ctl.get();
            if (runStateAtLeast(c, targetState) ||
                ctl.compareAndSet(c, ctlOf(targetState, workerCountOf(c))))
                break;
        }
    }

    /**
     * Transitions to TERMINATED state if either (SHUTDOWN and pool
     * and queue empty) or (STOP and pool empty).  If otherwise
     * eligible to terminate but workerCount is nonzero, interrupts an
     * idle worker to ensure that shutdown signals propagate. This
     * method must be called following any action that might make
     * termination possible -- reducing worker count or removing tasks
     * from the queue during shutdown. The method is non-private to
     * allow access from ScheduledThreadPoolExecutor.
     */
    final void tryTerminate() {
        for (;;) {
            int c = ctl.get();
            if (isRunning(c) ||
                runStateAtLeast(c, TIDYING) ||
                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))
                return;
            if (workerCountOf(c) != 0) { // Eligible to terminate
                interruptIdleWorkers(ONLY_ONE);
                return;
            }

            final ReentrantLock mainLock = this.mainLock;
            mainLock.lock();
            try {
                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {
                    try {
                        terminated();
                    } finally {
                        ctl.set(ctlOf(TERMINATED, 0));
                        termination.signalAll();
                    }
                    return;
                }
            } finally {
                mainLock.unlock();
            }
            // else retry on failed CAS
        }
    }

    /*
     * Methods for controlling interrupts to worker threads.
     */

    /**
     * If there is a security manager, makes sure caller has
     * permission to shut down threads in general (see shutdownPerm).
     * If this passes, additionally makes sure the caller is allowed
     * to interrupt each worker thread. This might not be true even if
     * first check passed, if the SecurityManager treats some threads
     * specially.
     */
    private void checkShutdownAccess() {
        // assert mainLock.isHeldByCurrentThread();
        @SuppressWarnings("removal")
        SecurityManager security = System.getSecurityManager();
        if (security != null) {
            security.checkPermission(shutdownPerm);
            for (Worker w : workers)
                security.checkAccess(w.thread);
        }
    }

    /**
     * Interrupts all threads, even if active. Ignores SecurityExceptions
     * (in which case some threads may remain uninterrupted).
     */
    private void interruptWorkers() {
        // assert mainLock.isHeldByCurrentThread();
        for (Worker w : workers)
            w.interruptIfStarted();
    }

    /**
     * Interrupts threads that might be waiting for tasks (as
     * indicated by not being locked) so they can check for
     * termination or configuration changes. Ignores
     * SecurityExceptions (in which case some threads may remain
     * uninterrupted).
     *
     * @param onlyOne If true, interrupt at most one worker. This is
     * called only from tryTerminate when termination is otherwise
     * enabled but there are still other workers.  In this case, at
     * most one waiting worker is interrupted to propagate shutdown
     * signals in case all threads are currently waiting.
     * Interrupting any arbitrary thread ensures that newly arriving
     * workers since shutdown began will also eventually exit.
     * To guarantee eventual termination, it suffices to always
     * interrupt only one idle worker, but shutdown() interrupts all
     * idle workers so that redundant workers exit promptly, not
     * waiting for a straggler task to finish.
     */
    private void interruptIdleWorkers(boolean onlyOne) {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            for (Worker w : workers) {
                Thread t = w.thread;
                if (!t.isInterrupted() && w.tryLock()) {
                    try {
                        t.interrupt();
                    } catch (SecurityException ignore) {
                    } finally {
                        w.unlock();
                    }
                }
                if (onlyOne)
                    break;
            }
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Common form of interruptIdleWorkers, to avoid having to
     * remember what the boolean argument means.
     */
    private void interruptIdleWorkers() {
        interruptIdleWorkers(false);
    }

    private static final boolean ONLY_ONE = true;

    /*
     * Misc utilities, most of which are also exported to
     * ScheduledThreadPoolExecutor
     */

    /**
     * Invokes the rejected execution handler for the given command.
     * Package-protected for use by ScheduledThreadPoolExecutor.
     */
    final void reject(Runnable command) {
        handler.rejectedExecution(command, this);
    }

    /**
     * Performs any further cleanup following run state transition on
     * invocation of shutdown.  A no-op here, but used by
     * ScheduledThreadPoolExecutor to cancel delayed tasks.
     */
    void onShutdown() {
    }

    /**
     * Drains the task queue into a new list, normally using
     * drainTo. But if the queue is a DelayQueue or any other kind of
     * queue for which poll or drainTo may fail to remove some
     * elements, it deletes them one by one.
     */
    private List<Runnable> drainQueue() {
        BlockingQueue<Runnable> q = workQueue;
        ArrayList<Runnable> taskList = new ArrayList<>();
        q.drainTo(taskList);
        if (!q.isEmpty()) {
            for (Runnable r : q.toArray(new Runnable[0])) {
                if (q.remove(r))
                    taskList.add(r);
            }
        }
        return taskList;
    }

    /*
     * Methods for creating, running and cleaning up after workers
     */

    /**
     * Checks if a new worker can be added with respect to current
     * pool state and the given bound (either core or maximum). If so,
     * the worker count is adjusted accordingly, and, if possible, a
     * new worker is created and started, running firstTask as its
     * first task. This method returns false if the pool is stopped or
     * eligible to shut down. It also returns false if the thread
     * factory fails to create a thread when asked.  If the thread
     * creation fails, either due to the thread factory returning
     * null, or due to an exception (typically OutOfMemoryError in
     * Thread.start()), we roll back cleanly.
     *
     * @param firstTask the task the new thread should run first (or
     * null if none). Workers are created with an initial first task
     * (in method execute()) to bypass queuing when there are fewer
     * than corePoolSize threads (in which case we always start one),
     * or when the queue is full (in which case we must bypass queue).
     * Initially idle threads are usually created via
     * prestartCoreThread or to replace other dying workers.
     *
     * @param core if true use corePoolSize as bound, else
     * maximumPoolSize. (A boolean indicator is used here rather than a
     * value to ensure reads of fresh values after checking other pool
     * state).
     * @return true if successful
     */
    private boolean addWorker(Runnable firstTask, boolean core) {
        retry:
        for (int c = ctl.get();;) {
            // Check if queue empty only if necessary.
            if (runStateAtLeast(c, SHUTDOWN)
                && (runStateAtLeast(c, STOP)
                    || firstTask != null
                    || workQueue.isEmpty()))
                return false;

            for (;;) {
                if (workerCountOf(c)
                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))
                    return false;
                if (compareAndIncrementWorkerCount(c))
                    break retry;
                c = ctl.get();  // Re-read ctl
                if (runStateAtLeast(c, SHUTDOWN))
                    continue retry;
                // else CAS failed due to workerCount change; retry inner loop
            }
        }

        boolean workerStarted = false;
        boolean workerAdded = false;
        Worker w = null;
        try {
            w = new Worker(firstTask);
            final Thread t = w.thread;
            if (t != null) {
                final ReentrantLock mainLock = this.mainLock;
                mainLock.lock();
                try {
                    // Recheck while holding lock.
                    // Back out on ThreadFactory failure or if
                    // shut down before lock acquired.
                    int c = ctl.get();

                    if (isRunning(c) ||
                        (runStateLessThan(c, STOP) && firstTask == null)) {
                        if (t.getState() != Thread.State.NEW)
                            throw new IllegalThreadStateException();
                        workers.add(w);
                        workerAdded = true;
                        int s = workers.size();
                        if (s > largestPoolSize)
                            largestPoolSize = s;
                    }
                } finally {
                    mainLock.unlock();
                }
                if (workerAdded) {
                    t.start();
                    workerStarted = true;
                }
            }
        } finally {
            if (! workerStarted)
                addWorkerFailed(w);
        }
        return workerStarted;
    }

    /**
     * Rolls back the worker thread creation.
     * - removes worker from workers, if present
     * - decrements worker count
     * - rechecks for termination, in case the existence of this
     *   worker was holding up termination
     */
    private void addWorkerFailed(Worker w) {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            if (w != null)
                workers.remove(w);
            decrementWorkerCount();
            tryTerminate();
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Performs cleanup and bookkeeping for a dying worker. Called
     * only from worker threads. Unless completedAbruptly is set,
     * assumes that workerCount has already been adjusted to account
     * for exit.  This method removes thread from worker set, and
     * possibly terminates the pool or replaces the worker if either
     * it exited due to user task exception or if fewer than
     * corePoolSize workers are running or queue is non-empty but
     * there are no workers.
     *
     * @param w the worker
     * @param completedAbruptly if the worker died due to user exception
     */
    private void processWorkerExit(Worker w, boolean completedAbruptly) {
        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted
            decrementWorkerCount();

        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            completedTaskCount += w.completedTasks;
            workers.remove(w);
        } finally {
            mainLock.unlock();
        }

        tryTerminate();

        int c = ctl.get();
        if (runStateLessThan(c, STOP)) {
            if (!completedAbruptly) {
                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;
                if (min == 0 && ! workQueue.isEmpty())
                    min = 1;
                if (workerCountOf(c) >= min)
                    return; // replacement not needed
            }
            addWorker(null, false);
        }
    }

    /**
     * Performs blocking or timed wait for a task, depending on
     * current configuration settings, or returns null if this worker
     * must exit because of any of:
     * 1. There are more than maximumPoolSize workers (due to
     *    a call to setMaximumPoolSize).
     * 2. The pool is stopped.
     * 3. The pool is shutdown and the queue is empty.
     * 4. This worker timed out waiting for a task, and timed-out
     *    workers are subject to termination (that is,
     *    {@code allowCoreThreadTimeOut || workerCount > corePoolSize})
     *    both before and after the timed wait, and if the queue is
     *    non-empty, this worker is not the last thread in the pool.
     *
     * @return task, or null if the worker must exit, in which case
     *         workerCount is decremented
     */
    private Runnable getTask() {
        boolean timedOut = false; // Did the last poll() time out?

        for (;;) {
            int c = ctl.get();

            // Check if queue empty only if necessary.
            if (runStateAtLeast(c, SHUTDOWN)
                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {
                decrementWorkerCount();
                return null;
            }

            int wc = workerCountOf(c);

            // Are workers subject to culling?
            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;

            if ((wc > maximumPoolSize || (timed && timedOut))
                && (wc > 1 || workQueue.isEmpty())) {
                if (compareAndDecrementWorkerCount(c))
                    return null;
                continue;
            }

            try {
                Runnable r = timed ?
                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :
                    workQueue.take();
                if (r != null)
                    return r;
                timedOut = true;
            } catch (InterruptedException retry) {
                timedOut = false;
            }
        }
    }

    /**
     * Main worker run loop.  Repeatedly gets tasks from queue and
     * executes them, while coping with a number of issues:
     *
     * 1. We may start out with an initial task, in which case we
     * don't need to get the first one. Otherwise, as long as pool is
     * running, we get tasks from getTask. If it returns null then the
     * worker exits due to changed pool state or configuration
     * parameters.  Other exits result from exception throws in
     * external code, in which case completedAbruptly holds, which
     * usually leads processWorkerExit to replace this thread.
     *
     * 2. Before running any task, the lock is acquired to prevent
     * other pool interrupts while the task is executing, and then we
     * ensure that unless pool is stopping, this thread does not have
     * its interrupt set.
     *
     * 3. Each task run is preceded by a call to beforeExecute, which
     * might throw an exception, in which case we cause thread to die
     * (breaking loop with completedAbruptly true) without processing
     * the task.
     *
     * 4. Assuming beforeExecute completes normally, we run the task,
     * gathering any of its thrown exceptions to send to afterExecute.
     * We separately handle RuntimeException, Error (both of which the
     * specs guarantee that we trap) and arbitrary Throwables.
     * Because we cannot rethrow Throwables within Runnable.run, we
     * wrap them within Errors on the way out (to the thread's
     * UncaughtExceptionHandler).  Any thrown exception also
     * conservatively causes thread to die.
     *
     * 5. After task.run completes, we call afterExecute, which may
     * also throw an exception, which will also cause thread to
     * die. According to JLS Sec 14.20, this exception is the one that
     * will be in effect even if task.run throws.
     *
     * The net effect of the exception mechanics is that afterExecute
     * and the thread's UncaughtExceptionHandler have as accurate
     * information as we can provide about any problems encountered by
     * user code.
     *
     * @param w the worker
     */
    final void runWorker(Worker w) {
        Thread wt = Thread.currentThread();
        Runnable task = w.firstTask;
        w.firstTask = null;
        w.unlock(); // allow interrupts
        boolean completedAbruptly = true;
        try {
            while (task != null || (task = getTask()) != null) {
                w.lock();
                // If pool is stopping, ensure thread is interrupted;
                // if not, ensure thread is not interrupted.  This
                // requires a recheck in second case to deal with
                // shutdownNow race while clearing interrupt
                if ((runStateAtLeast(ctl.get(), STOP) ||
                     (Thread.interrupted() &&
                      runStateAtLeast(ctl.get(), STOP))) &&
                    !wt.isInterrupted())
                    wt.interrupt();
                try {
                    beforeExecute(wt, task);
                    try {
                        task.run();
                        afterExecute(task, null);
                    } catch (Throwable ex) {
                        afterExecute(task, ex);
                        throw ex;
                    }
                } finally {
                    task = null;
                    w.completedTasks++;
                    w.unlock();
                }
            }
            completedAbruptly = false;
        } finally {
            processWorkerExit(w, completedAbruptly);
        }
    }

    // Public constructors and methods

    /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters, the
     * {@linkplain Executors#defaultThreadFactory default thread factory}
     * and the {@linkplain ThreadPoolExecutor.AbortPolicy
     * default rejected execution handler}.
     *
     * <p>It may be more convenient to use one of the {@link Executors}
     * factory methods instead of this general purpose constructor.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
    }

    /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters and the {@linkplain ThreadPoolExecutor.AbortPolicy
     * default rejected execution handler}.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             threadFactory, defaultHandler);
    }

    /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters and the
     * {@linkplain Executors#defaultThreadFactory default thread factory}.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              RejectedExecutionHandler handler) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), handler);
    }

    /**
     * Creates a new {@code ThreadPoolExecutor} with the given initial
     * parameters.
     *
     * @param corePoolSize the number of threads to keep in the pool, even
     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set
     * @param maximumPoolSize the maximum number of threads to allow in the
     *        pool
     * @param keepAliveTime when the number of threads is greater than
     *        the core, this is the maximum time that excess idle threads
     *        will wait for new tasks before terminating.
     * @param unit the time unit for the {@code keepAliveTime} argument
     * @param workQueue the queue to use for holding tasks before they are
     *        executed.  This queue will hold only the {@code Runnable}
     *        tasks submitted by the {@code execute} method.
     * @param threadFactory the factory to use when the executor
     *        creates a new thread
     * @param handler the handler to use when execution is blocked
     *        because the thread bounds and queue capacities are reached
     * @throws IllegalArgumentException if one of the following holds:<br>
     *         {@code corePoolSize < 0}<br>
     *         {@code keepAliveTime < 0}<br>
     *         {@code maximumPoolSize <= 0}<br>
     *         {@code maximumPoolSize < corePoolSize}
     * @throws NullPointerException if {@code workQueue}
     *         or {@code threadFactory} or {@code handler} is null
     */
    public ThreadPoolExecutor(int corePoolSize,
                              int maximumPoolSize,
                              long keepAliveTime,
                              TimeUnit unit,
                              BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory,
                              RejectedExecutionHandler handler) {
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }

    /**
     * Executes the given task sometime in the future.  The task
     * may execute in a new thread or in an existing pooled thread.
     *
     * If the task cannot be submitted for execution, either because this
     * executor has been shutdown or because its capacity has been reached,
     * the task is handled by the current {@link RejectedExecutionHandler}.
     *
     * @param command the task to execute
     * @throws RejectedExecutionException at discretion of
     *         {@code RejectedExecutionHandler}, if the task
     *         cannot be accepted for execution
     * @throws NullPointerException if {@code command} is null
     */
    public void execute(Runnable command) {
        if (command == null)
            throw new NullPointerException();
        /*
         * Proceed in 3 steps:
         *
         * 1. If fewer than corePoolSize threads are running, try to
         * start a new thread with the given command as its first
         * task.  The call to addWorker atomically checks runState and
         * workerCount, and so prevents false alarms that would add
         * threads when it shouldn't, by returning false.
         *
         * 2. If a task can be successfully queued, then we still need
         * to double-check whether we should have added a thread
         * (because existing ones died since last checking) or that
         * the pool shut down since entry into this method. So we
         * recheck state and if necessary roll back the enqueuing if
         * stopped, or start a new thread if there are none.
         *
         * 3. If we cannot queue task, then we try to add a new
         * thread.  If it fails, we know we are shut down or saturated
         * and so reject the task.
         */
        int c = ctl.get();
        if (workerCountOf(c) < corePoolSize) {
            if (addWorker(command, true))
                return;
            c = ctl.get();
        }
        if (isRunning(c) && workQueue.offer(command)) {
            int recheck = ctl.get();
            if (! isRunning(recheck) && remove(command))
                reject(command);
            else if (workerCountOf(recheck) == 0)
                addWorker(null, false);
        }
        else if (!addWorker(command, false))
            reject(command);
    }

    /**
     * Initiates an orderly shutdown in which previously submitted
     * tasks are executed, but no new tasks will be accepted.
     * Invocation has no additional effect if already shut down.
     *
     * <p>This method does not wait for previously submitted tasks to
     * complete execution.  Use {@link #awaitTermination awaitTermination}
     * to do that.
     *
     * @throws SecurityException {@inheritDoc}
     */
    public void shutdown() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            checkShutdownAccess();
            advanceRunState(SHUTDOWN);
            interruptIdleWorkers();
            onShutdown(); // hook for ScheduledThreadPoolExecutor
        } finally {
            mainLock.unlock();
        }
        tryTerminate();
    }

    /**
     * Attempts to stop all actively executing tasks, halts the
     * processing of waiting tasks, and returns a list of the tasks
     * that were awaiting execution. These tasks are drained (removed)
     * from the task queue upon return from this method.
     *
     * <p>This method does not wait for actively executing tasks to
     * terminate.  Use {@link #awaitTermination awaitTermination} to
     * do that.
     *
     * <p>There are no guarantees beyond best-effort attempts to stop
     * processing actively executing tasks.  This implementation
     * interrupts tasks via {@link Thread#interrupt}; any task that
     * fails to respond to interrupts may never terminate.
     *
     * @throws SecurityException {@inheritDoc}
     */
    public List<Runnable> shutdownNow() {
        List<Runnable> tasks;
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            checkShutdownAccess();
            advanceRunState(STOP);
            interruptWorkers();
            tasks = drainQueue();
        } finally {
            mainLock.unlock();
        }
        tryTerminate();
        return tasks;
    }

    public boolean isShutdown() {
        return runStateAtLeast(ctl.get(), SHUTDOWN);
    }

    /** Used by ScheduledThreadPoolExecutor. */
    boolean isStopped() {
        return runStateAtLeast(ctl.get(), STOP);
    }

    /**
     * Returns true if this executor is in the process of terminating
     * after {@link #shutdown} or {@link #shutdownNow} but has not
     * completely terminated.  This method may be useful for
     * debugging. A return of {@code true} reported a sufficient
     * period after shutdown may indicate that submitted tasks have
     * ignored or suppressed interruption, causing this executor not
     * to properly terminate.
     *
     * @return {@code true} if terminating but not yet terminated
     */
    public boolean isTerminating() {
        int c = ctl.get();
        return runStateAtLeast(c, SHUTDOWN) && runStateLessThan(c, TERMINATED);
    }

    public boolean isTerminated() {
        return runStateAtLeast(ctl.get(), TERMINATED);
    }

    public boolean awaitTermination(long timeout, TimeUnit unit)
        throws InterruptedException {
        long nanos = unit.toNanos(timeout);
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            while (runStateLessThan(ctl.get(), TERMINATED)) {
                if (nanos <= 0L)
                    return false;
                nanos = termination.awaitNanos(nanos);
            }
            return true;
        } finally {
            mainLock.unlock();
        }
    }

    // Override without "throws Throwable" for compatibility with subclasses
    // whose finalize method invokes super.finalize() (as is recommended).
    // Before JDK 11, finalize() had a non-empty method body.

    /**
     * @implNote Previous versions of this class had a finalize method
     * that shut down this executor, but in this version, finalize
     * does nothing.
     */
    @Deprecated(since="9")
    protected void finalize() {}

    /**
     * Sets the thread factory used to create new threads.
     *
     * @param threadFactory the new thread factory
     * @throws NullPointerException if threadFactory is null
     * @see #getThreadFactory
     */
    public void setThreadFactory(ThreadFactory threadFactory) {
        if (threadFactory == null)
            throw new NullPointerException();
        this.threadFactory = threadFactory;
    }

    /**
     * Returns the thread factory used to create new threads.
     *
     * @return the current thread factory
     * @see #setThreadFactory(ThreadFactory)
     */
    public ThreadFactory getThreadFactory() {
        return threadFactory;
    }

    /**
     * Sets a new handler for unexecutable tasks.
     *
     * @param handler the new handler
     * @throws NullPointerException if handler is null
     * @see #getRejectedExecutionHandler
     */
    public void setRejectedExecutionHandler(RejectedExecutionHandler handler) {
        if (handler == null)
            throw new NullPointerException();
        this.handler = handler;
    }

    /**
     * Returns the current handler for unexecutable tasks.
     *
     * @return the current handler
     * @see #setRejectedExecutionHandler(RejectedExecutionHandler)
     */
    public RejectedExecutionHandler getRejectedExecutionHandler() {
        return handler;
    }

    /**
     * Sets the core number of threads.  This overrides any value set
     * in the constructor.  If the new value is smaller than the
     * current value, excess existing threads will be terminated when
     * they next become idle.  If larger, new threads will, if needed,
     * be started to execute any queued tasks.
     *
     * @param corePoolSize the new core size
     * @throws IllegalArgumentException if {@code corePoolSize < 0}
     *         or {@code corePoolSize} is greater than the {@linkplain
     *         #getMaximumPoolSize() maximum pool size}
     * @see #getCorePoolSize
     */
    public void setCorePoolSize(int corePoolSize) {
        if (corePoolSize < 0 || maximumPoolSize < corePoolSize)
            throw new IllegalArgumentException();
        int delta = corePoolSize - this.corePoolSize;
        this.corePoolSize = corePoolSize;
        if (workerCountOf(ctl.get()) > corePoolSize)
            interruptIdleWorkers();
        else if (delta > 0) {
            // We don't really know how many new threads are "needed".
            // As a heuristic, prestart enough new workers (up to new
            // core size) to handle the current number of tasks in
            // queue, but stop if queue becomes empty while doing so.
            int k = Math.min(delta, workQueue.size());
            while (k-- > 0 && addWorker(null, true)) {
                if (workQueue.isEmpty())
                    break;
            }
        }
    }

    /**
     * Returns the core number of threads.
     *
     * @return the core number of threads
     * @see #setCorePoolSize
     */
    public int getCorePoolSize() {
        return corePoolSize;
    }

    /**
     * Starts a core thread, causing it to idly wait for work. This
     * overrides the default policy of starting core threads only when
     * new tasks are executed. This method will return {@code false}
     * if all core threads have already been started.
     *
     * @return {@code true} if a thread was started
     */
    public boolean prestartCoreThread() {
        return workerCountOf(ctl.get()) < corePoolSize &&
            addWorker(null, true);
    }

    /**
     * Same as prestartCoreThread except arranges that at least one
     * thread is started even if corePoolSize is 0.
     */
    void ensurePrestart() {
        int wc = workerCountOf(ctl.get());
        if (wc < corePoolSize)
            addWorker(null, true);
        else if (wc == 0)
            addWorker(null, false);
    }

    /**
     * Starts all core threads, causing them to idly wait for work. This
     * overrides the default policy of starting core threads only when
     * new tasks are executed.
     *
     * @return the number of threads started
     */
    public int prestartAllCoreThreads() {
        int n = 0;
        while (addWorker(null, true))
            ++n;
        return n;
    }

    /**
     * Returns true if this pool allows core threads to time out and
     * terminate if no tasks arrive within the keepAlive time, being
     * replaced if needed when new tasks arrive. When true, the same
     * keep-alive policy applying to non-core threads applies also to
     * core threads. When false (the default), core threads are never
     * terminated due to lack of incoming tasks.
     *
     * @return {@code true} if core threads are allowed to time out,
     *         else {@code false}
     *
     * @since 1.6
     */
    public boolean allowsCoreThreadTimeOut() {
        return allowCoreThreadTimeOut;
    }

    /**
     * Sets the policy governing whether core threads may time out and
     * terminate if no tasks arrive within the keep-alive time, being
     * replaced if needed when new tasks arrive. When false, core
     * threads are never terminated due to lack of incoming
     * tasks. When true, the same keep-alive policy applying to
     * non-core threads applies also to core threads. To avoid
     * continual thread replacement, the keep-alive time must be
     * greater than zero when setting {@code true}. This method
     * should in general be called before the pool is actively used.
     *
     * @param value {@code true} if should time out, else {@code false}
     * @throws IllegalArgumentException if value is {@code true}
     *         and the current keep-alive time is not greater than zero
     *
     * @since 1.6
     */
    public void allowCoreThreadTimeOut(boolean value) {
        if (value && keepAliveTime <= 0)
            throw new IllegalArgumentException("Core threads must have nonzero keep alive times");
        if (value != allowCoreThreadTimeOut) {
            allowCoreThreadTimeOut = value;
            if (value)
                interruptIdleWorkers();
        }
    }

    /**
     * Sets the maximum allowed number of threads. This overrides any
     * value set in the constructor. If the new value is smaller than
     * the current value, excess existing threads will be
     * terminated when they next become idle.
     *
     * @param maximumPoolSize the new maximum
     * @throws IllegalArgumentException if the new maximum is
     *         less than or equal to zero, or
     *         less than the {@linkplain #getCorePoolSize core pool size}
     * @see #getMaximumPoolSize
     */
    public void setMaximumPoolSize(int maximumPoolSize) {
        if (maximumPoolSize <= 0 || maximumPoolSize < corePoolSize)
            throw new IllegalArgumentException();
        this.maximumPoolSize = maximumPoolSize;
        if (workerCountOf(ctl.get()) > maximumPoolSize)
            interruptIdleWorkers();
    }

    /**
     * Returns the maximum allowed number of threads.
     *
     * @return the maximum allowed number of threads
     * @see #setMaximumPoolSize
     */
    public int getMaximumPoolSize() {
        return maximumPoolSize;
    }

    /**
     * Sets the thread keep-alive time, which is the amount of time
     * that threads may remain idle before being terminated.
     * Threads that wait this amount of time without processing a
     * task will be terminated if there are more than the core
     * number of threads currently in the pool, or if this pool
     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.
     * This overrides any value set in the constructor.
     *
     * @param time the time to wait.  A time value of zero will cause
     *        excess threads to terminate immediately after executing tasks.
     * @param unit the time unit of the {@code time} argument
     * @throws IllegalArgumentException if {@code time} less than zero or
     *         if {@code time} is zero and {@code allowsCoreThreadTimeOut}
     * @see #getKeepAliveTime(TimeUnit)
     */
    public void setKeepAliveTime(long time, TimeUnit unit) {
        if (time < 0)
            throw new IllegalArgumentException();
        if (time == 0 && allowsCoreThreadTimeOut())
            throw new IllegalArgumentException("Core threads must have nonzero keep alive times");
        long keepAliveTime = unit.toNanos(time);
        long delta = keepAliveTime - this.keepAliveTime;
        this.keepAliveTime = keepAliveTime;
        if (delta < 0)
            interruptIdleWorkers();
    }

    /**
     * Returns the thread keep-alive time, which is the amount of time
     * that threads may remain idle before being terminated.
     * Threads that wait this amount of time without processing a
     * task will be terminated if there are more than the core
     * number of threads currently in the pool, or if this pool
     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.
     *
     * @param unit the desired time unit of the result
     * @return the time limit
     * @see #setKeepAliveTime(long, TimeUnit)
     */
    public long getKeepAliveTime(TimeUnit unit) {
        return unit.convert(keepAliveTime, TimeUnit.NANOSECONDS);
    }

    /* User-level queue utilities */

    /**
     * Returns the task queue used by this executor. Access to the
     * task queue is intended primarily for debugging and monitoring.
     * This queue may be in active use.  Retrieving the task queue
     * does not prevent queued tasks from executing.
     *
     * @return the task queue
     */
    public BlockingQueue<Runnable> getQueue() {
        return workQueue;
    }

    /**
     * Removes this task from the executor's internal queue if it is
     * present, thus causing it not to be run if it has not already
     * started.
     *
     * <p>This method may be useful as one part of a cancellation
     * scheme.  It may fail to remove tasks that have been converted
     * into other forms before being placed on the internal queue.
     * For example, a task entered using {@code submit} might be
     * converted into a form that maintains {@code Future} status.
     * However, in such cases, method {@link #purge} may be used to
     * remove those Futures that have been cancelled.
     *
     * @param task the task to remove
     * @return {@code true} if the task was removed
     */
    public boolean remove(Runnable task) {
        boolean removed = workQueue.remove(task);
        tryTerminate(); // In case SHUTDOWN and now empty
        return removed;
    }

    /**
     * Tries to remove from the work queue all {@link Future}
     * tasks that have been cancelled. This method can be useful as a
     * storage reclamation operation, that has no other impact on
     * functionality. Cancelled tasks are never executed, but may
     * accumulate in work queues until worker threads can actively
     * remove them. Invoking this method instead tries to remove them now.
     * However, this method may fail to remove tasks in
     * the presence of interference by other threads.
     */
    public void purge() {
        final BlockingQueue<Runnable> q = workQueue;
        try {
            Iterator<Runnable> it = q.iterator();
            while (it.hasNext()) {
                Runnable r = it.next();
                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())
                    it.remove();
            }
        } catch (ConcurrentModificationException fallThrough) {
            // Take slow path if we encounter interference during traversal.
            // Make copy for traversal and call remove for cancelled entries.
            // The slow path is more likely to be O(N*N).
            for (Object r : q.toArray())
                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())
                    q.remove(r);
        }

        tryTerminate(); // In case SHUTDOWN and now empty
    }

    /* Statistics */

    /**
     * Returns the current number of threads in the pool.
     *
     * @return the number of threads
     */
    public int getPoolSize() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            // Remove rare and surprising possibility of
            // isTerminated() && getPoolSize() > 0
            return runStateAtLeast(ctl.get(), TIDYING) ? 0
                : workers.size();
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Returns the approximate number of threads that are actively
     * executing tasks.
     *
     * @return the number of threads
     */
    public int getActiveCount() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            int n = 0;
            for (Worker w : workers)
                if (w.isLocked())
                    ++n;
            return n;
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Returns the largest number of threads that have ever
     * simultaneously been in the pool.
     *
     * @return the number of threads
     */
    public int getLargestPoolSize() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            return largestPoolSize;
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Returns the approximate total number of tasks that have ever been
     * scheduled for execution. Because the states of tasks and
     * threads may change dynamically during computation, the returned
     * value is only an approximation.
     *
     * @return the number of tasks
     */
    public long getTaskCount() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            long n = completedTaskCount;
            for (Worker w : workers) {
                n += w.completedTasks;
                if (w.isLocked())
                    ++n;
            }
            return n + workQueue.size();
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Returns the approximate total number of tasks that have
     * completed execution. Because the states of tasks and threads
     * may change dynamically during computation, the returned value
     * is only an approximation, but one that does not ever decrease
     * across successive calls.
     *
     * @return the number of tasks
     */
    public long getCompletedTaskCount() {
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            long n = completedTaskCount;
            for (Worker w : workers)
                n += w.completedTasks;
            return n;
        } finally {
            mainLock.unlock();
        }
    }

    /**
     * Returns a string identifying this pool, as well as its state,
     * including indications of run state and estimated worker and
     * task counts.
     *
     * @return a string identifying this pool, as well as its state
     */
    public String toString() {
        long ncompleted;
        int nworkers, nactive;
        final ReentrantLock mainLock = this.mainLock;
        mainLock.lock();
        try {
            ncompleted = completedTaskCount;
            nactive = 0;
            nworkers = workers.size();
            for (Worker w : workers) {
                ncompleted += w.completedTasks;
                if (w.isLocked())
                    ++nactive;
            }
        } finally {
            mainLock.unlock();
        }
        int c = ctl.get();
        String runState =
            isRunning(c) ? "Running" :
            runStateAtLeast(c, TERMINATED) ? "Terminated" :
            "Shutting down";
        return super.toString() +
            "[" + runState +
            ", pool size = " + nworkers +
            ", active threads = " + nactive +
            ", queued tasks = " + workQueue.size() +
            ", completed tasks = " + ncompleted +
            "]";
    }

    /* Extension hooks */

    /**
     * Method invoked prior to executing the given Runnable in the
     * given thread.  This method is invoked by thread {@code t} that
     * will execute task {@code r}, and may be used to re-initialize
     * ThreadLocals, or to perform logging.
     *
     * <p>This implementation does nothing, but may be customized in
     * subclasses. Note: To properly nest multiple overridings, subclasses
     * should generally invoke {@code super.beforeExecute} at the end of
     * this method.
     *
     * @param t the thread that will run task {@code r}
     * @param r the task that will be executed
     */
    protected void beforeExecute(Thread t, Runnable r) { }

    /**
     * Method invoked upon completion of execution of the given Runnable.
     * This method is invoked by the thread that executed the task. If
     * non-null, the Throwable is the uncaught {@code RuntimeException}
     * or {@code Error} that caused execution to terminate abruptly.
     *
     * <p>This implementation does nothing, but may be customized in
     * subclasses. Note: To properly nest multiple overridings, subclasses
     * should generally invoke {@code super.afterExecute} at the
     * beginning of this method.
     *
     * <p><b>Note:</b> When actions are enclosed in tasks (such as
     * {@link FutureTask}) either explicitly or via methods such as
     * {@code submit}, these task objects catch and maintain
     * computational exceptions, and so they do not cause abrupt
     * termination, and the internal exceptions are <em>not</em>
     * passed to this method. If you would like to trap both kinds of
     * failures in this method, you can further probe for such cases,
     * as in this sample subclass that prints either the direct cause
     * or the underlying exception if a task has been aborted:
     *
     * <pre> {@code
     * class ExtendedExecutor extends ThreadPoolExecutor {
     *   // ...
     *   protected void afterExecute(Runnable r, Throwable t) {
     *     super.afterExecute(r, t);
     *     if (t == null
     *         && r instanceof Future<?>
     *         && ((Future<?>)r).isDone()) {
     *       try {
     *         Object result = ((Future<?>) r).get();
     *       } catch (CancellationException ce) {
     *         t = ce;
     *       } catch (ExecutionException ee) {
     *         t = ee.getCause();
     *       } catch (InterruptedException ie) {
     *         // ignore/reset
     *         Thread.currentThread().interrupt();
     *       }
     *     }
     *     if (t != null)
     *       System.out.println(t);
     *   }
     * }}</pre>
     *
     * @param r the runnable that has completed
     * @param t the exception that caused termination, or null if
     * execution completed normally
     */
    protected void afterExecute(Runnable r, Throwable t) { }

    /**
     * Method invoked when the Executor has terminated.  Default
     * implementation does nothing. Note: To properly nest multiple
     * overridings, subclasses should generally invoke
     * {@code super.terminated} within this method.
     */
    protected void terminated() { }

    /* Predefined RejectedExecutionHandlers */

    /**
     * A handler for rejected tasks that runs the rejected task
     * directly in the calling thread of the {@code execute} method,
     * unless the executor has been shut down, in which case the task
     * is discarded.
     */
    public static class CallerRunsPolicy implements RejectedExecutionHandler {
        /**
         * Creates a {@code CallerRunsPolicy}.
         */
        public CallerRunsPolicy() { }

        /**
         * Executes task r in the caller's thread, unless the executor
         * has been shut down, in which case the task is discarded.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                r.run();
            }
        }
    }

    /**
     * A handler for rejected tasks that throws a
     * {@link RejectedExecutionException}.
     *
     * This is the default handler for {@link ThreadPoolExecutor} and
     * {@link ScheduledThreadPoolExecutor}.
     */
    public static class AbortPolicy implements RejectedExecutionHandler {
        /**
         * Creates an {@code AbortPolicy}.
         */
        public AbortPolicy() { }

        /**
         * Always throws RejectedExecutionException.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         * @throws RejectedExecutionException always
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            throw new RejectedExecutionException("Task " + r.toString() +
                                                 " rejected from " +
                                                 e.toString());
        }
    }

    /**
     * A handler for rejected tasks that silently discards the
     * rejected task.
     */
    public static class DiscardPolicy implements RejectedExecutionHandler {
        /**
         * Creates a {@code DiscardPolicy}.
         */
        public DiscardPolicy() { }

        /**
         * Does nothing, which has the effect of discarding task r.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        }
    }

    /**
     * A handler for rejected tasks that discards the oldest unhandled
     * request and then retries {@code execute}, unless the executor
     * is shut down, in which case the task is discarded. This policy is
     * rarely useful in cases where other threads may be waiting for
     * tasks to terminate, or failures must be recorded. Instead consider
     * using a handler of the form:
     * <pre> {@code
     * new RejectedExecutionHandler() {
     *   public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
     *     Runnable dropped = e.getQueue().poll();
     *     if (dropped instanceof Future<?>) {
     *       ((Future<?>)dropped).cancel(false);
     *       // also consider logging the failure
     *     }
     *     e.execute(r);  // retry
     * }}}</pre>
     */
    public static class DiscardOldestPolicy implements RejectedExecutionHandler {
        /**
         * Creates a {@code DiscardOldestPolicy} for the given executor.
         */
        public DiscardOldestPolicy() { }

        /**
         * Obtains and ignores the next task that the executor
         * would otherwise execute, if one is immediately available,
         * and then retries execution of task r, unless the executor
         * is shut down, in which case task r is instead discarded.
         *
         * @param r the runnable task requested to be executed
         * @param e the executor attempting to execute this task
         */
        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
            if (!e.isShutdown()) {
                e.getQueue().poll();
                e.execute(r);
            }
        }
    }
}
 ^NPcqCzcP

Integer.SIZE=32
COUNT_BITS=29 ^r8Jy8cRC

0000 0000 0000 0000 0000 0000 0000 0001 ^9kmxIQru

1<< COUNT_BITS ^ofOIy6n7

0010 0000 0000 0000 0000 0000 0000 0000

-1

0001 1111 1111 1111 1111 1111 1111 1111

=536870911 ^7h9LiOwn

0 ^1OPjqjHo

-536870912 ^XCFyHt02

536870912 ^3wqx3EmY

1073741824 ^qYpWM769

1610612736 ^l1Md3CGB

or 操作，操作符“|” ^C1v7WYVX

定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。 ^PI9c484y

1110 0000 0000 0000 0000 0000 0000 0000 ^hdo43w4V

1000 0000 0000 0000 0000 0000 0000 0000 ^sqVkHS59

and 操作，操作符“&” ^QNnuUB83

定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。 ^daZJgN0g

100 0000 0000 0000 0000 0000 0000 0000 ^Jn4zVZDH

10 0000 0000 0000 0000 0000 0000 0000 ^orrySlph

not操作，操作符“~” ^61pEFIUS

定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。 ^1J0eQdto

相当于不是running时候 ^r7sfONdW

不是running或shutdown状态 ^jryd9p9A

%%
# Drawing
```json
{
	"type": "excalidraw",
	"version": 2,
	"source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.0.13",
	"elements": [
		{
			"type": "rectangle",
			"version": 157,
			"versionNonce": 886554968,
			"isDeleted": false,
			"id": "T_IfEJgzej3u9o0aMojVk",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -245.7827258142723,
			"y": -362.79287305902545,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 96.78399658203125,
			"height": 35,
			"seed": 192240300,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "4n420RxD"
				},
				{
					"id": "gM9uDHGxHDbmNZx2fb7Y9",
					"type": "arrow"
				}
			],
			"updated": 1704364401182,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 145,
			"versionNonce": 1988206632,
			"isDeleted": false,
			"id": "4n420RxD",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -237.3907275232567,
			"y": -357.79287305902545,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 80,
			"height": 25,
			"seed": 821648300,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401182,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 1,
			"text": "提交任务",
			"rawText": "提交任务",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "T_IfEJgzej3u9o0aMojVk",
			"originalText": "提交任务",
			"lineHeight": 1.25,
			"baseline": 18
		},
		{
			"type": "rectangle",
			"version": 225,
			"versionNonce": 1678104152,
			"isDeleted": false,
			"id": "cSCQqxhqorGcisT6jgZ8W",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -266.0594336522896,
			"y": 444.91513651258674,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 123.06976608716718,
			"height": 53.93990030030926,
			"seed": 1856685484,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "HPDOSMbb"
				},
				{
					"id": "nNmQBNr2-Tz2uYIY4uJhD",
					"type": "arrow"
				},
				{
					"id": "XwG1KmlJC-7BuU5kMnnk-",
					"type": "arrow"
				}
			],
			"updated": 1704364401182,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 201,
			"versionNonce": 682318632,
			"isDeleted": false,
			"id": "HPDOSMbb",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -244.52455060870602,
			"y": 459.8850866627414,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 80,
			"height": 24,
			"seed": 772007084,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "拒绝策略",
			"rawText": "拒绝策略",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "cSCQqxhqorGcisT6jgZ8W",
			"originalText": "拒绝策略",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 1226,
			"versionNonce": 245975896,
			"isDeleted": false,
			"id": "gM9uDHGxHDbmNZx2fb7Y9",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -197.77753035988297,
			"y": -324.94020042385114,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 0.6783166560129814,
			"height": 38.14668804182338,
			"seed": 701722388,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "T_IfEJgzej3u9o0aMojVk",
				"gap": 2.852672635174315,
				"focus": 0.0005111729572233496
			},
			"endBinding": {
				"elementId": "nt7nr4Gu1S748ZfR_-6Xk",
				"gap": 1,
				"focus": -0.049239621610345126
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-0.6783166560129814,
					38.14668804182338
				]
			]
		},
		{
			"type": "arrow",
			"version": 3214,
			"versionNonce": 65386024,
			"isDeleted": false,
			"id": "C_Rb9IbOW7akGqW5d9orl",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -199.89746854752175,
			"y": -129.02177999476774,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 3.5189546220184127,
			"height": 89.81137164918775,
			"seed": 329105068,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "nSVnUTBr"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "nt7nr4Gu1S748ZfR_-6Xk",
				"gap": 1,
				"focus": 0.005150854670764651
			},
			"endBinding": {
				"elementId": "HUxSIj4LqHtmZjeeB5ARv",
				"gap": 1,
				"focus": -0.0246650599871322
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					-3.5189546220184127,
					89.81137164918775
				]
			]
		},
		{
			"type": "text",
			"version": 46,
			"versionNonce": 1595285592,
			"isDeleted": false,
			"id": "nSVnUTBr",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -139.6391386577669,
			"y": -154.66984700073368,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 1265043116,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "是",
			"rawText": "是",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "C_Rb9IbOW7akGqW5d9orl",
			"originalText": "是",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 3121,
			"versionNonce": 1562161448,
			"isDeleted": false,
			"id": "yZBF-raaQCqh61VvxiCnC",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -206.43541242914645,
			"y": 85.26782110988472,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 3.8217236408809185,
			"height": 129.94889866411938,
			"seed": 1173999508,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "aH6hUxEf"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "HUxSIj4LqHtmZjeeB5ARv",
				"focus": -0.05155573481371115,
				"gap": 3.6145849357471747
			},
			"endBinding": {
				"elementId": "WEVCdVaLjtz4Fcffub1u0",
				"focus": 0.030003558344254236,
				"gap": 2.018778129519845
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					3.8217236408809185,
					129.94889866411938
				]
			]
		},
		{
			"type": "text",
			"version": 44,
			"versionNonce": 354440536,
			"isDeleted": false,
			"id": "aH6hUxEf",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 31.01899833956397,
			"y": -157.28915547656146,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 968286124,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "是",
			"rawText": "是",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "yZBF-raaQCqh61VvxiCnC",
			"originalText": "是",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 971,
			"versionNonce": 196889640,
			"isDeleted": false,
			"id": "nNmQBNr2-Tz2uYIY4uJhD",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -204.74096397613587,
			"y": 335.3449721483234,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 0.7998726757485315,
			"height": 105.12965099720145,
			"seed": 463015212,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"type": "text",
					"id": "ZKfTL8XU"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": null,
			"endBinding": {
				"elementId": "cSCQqxhqorGcisT6jgZ8W",
				"gap": 4.440513367061925,
				"focus": 0.013321069795486494
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					0.7998726757485315,
					105.12965099720145
				]
			]
		},
		{
			"type": "text",
			"version": 41,
			"versionNonce": 1444670040,
			"isDeleted": false,
			"id": "ZKfTL8XU",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -213.28930419786712,
			"y": 382.7576139357677,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 80557716,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "是",
			"rawText": "是",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "nNmQBNr2-Tz2uYIY4uJhD",
			"originalText": "是",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "ellipse",
			"version": 446,
			"versionNonce": 430999336,
			"isDeleted": false,
			"id": "z7hRMv-_l1mhRi6DggYVQ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -221.28488670607743,
			"y": 628.1794743874673,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#1e1e1e",
			"width": 34.630054079908234,
			"height": 30.990820833119585,
			"seed": 1099783596,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "ZPEXTIlR7mhLNM11sy4Xy",
					"type": "arrow"
				},
				{
					"id": "XwG1KmlJC-7BuU5kMnnk-",
					"type": "arrow"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false
		},
		{
			"type": "rectangle",
			"version": 631,
			"versionNonce": 2094849880,
			"isDeleted": false,
			"id": "gvJG81DLcbyQvfgp3qFDY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 161.00507094683132,
			"y": -236.06448083592576,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 137.74783740489278,
			"height": 58,
			"seed": 793576620,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "NsXRwofq"
				},
				{
					"id": "e8QVRqdayhgBvPYKkvCK6",
					"type": "arrow"
				},
				{
					"id": "ZPEXTIlR7mhLNM11sy4Xy",
					"type": "arrow"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 843,
			"versionNonce": 1936024104,
			"isDeleted": false,
			"id": "NsXRwofq",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 169.8789896492777,
			"y": -231.06448083592576,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 120,
			"height": 48,
			"seed": 387944236,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "创建线程执行\n任务",
			"rawText": "创建线程执行任务",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "gvJG81DLcbyQvfgp3qFDY",
			"originalText": "创建线程执行任务",
			"lineHeight": 1.2,
			"baseline": 44
		},
		{
			"type": "rectangle",
			"version": 896,
			"versionNonce": 1773614168,
			"isDeleted": false,
			"id": "KFlCeFOaKunOIV71uAJVZ",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -48.421909365827304,
			"y": 5.0862391349695315,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 114.04829334890923,
			"height": 35.906669435699314,
			"seed": 719212180,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "kOE4EpYm"
				},
				{
					"id": "KZLdTHeJqOywDihRKDcZ2",
					"type": "arrow"
				},
				{
					"id": "_v-BtdCvnsMkNaksOGJ7l",
					"type": "arrow"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 1320,
			"versionNonce": 398908712,
			"isDeleted": false,
			"id": "kOE4EpYm",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -31.39776269137269,
			"y": 11.039573852819188,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 80,
			"height": 24,
			"seed": 1693342508,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "放入队列",
			"rawText": "放入队列",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "KFlCeFOaKunOIV71uAJVZ",
			"originalText": "放入队列",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "rectangle",
			"version": 509,
			"versionNonce": 1384048984,
			"isDeleted": false,
			"id": "Yxh_94PFSuKFPbsKze27r",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -36.1613309210621,
			"y": 263.8327201124986,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 137.74783740489278,
			"height": 58,
			"seed": 991477012,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "vryEFD3S"
				},
				{
					"id": "k1cC5DU5JGpSPfd19KAcA",
					"type": "arrow"
				},
				{
					"id": "gHnC0qdj-7I5CbrTI6wYi",
					"type": "arrow"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 722,
			"versionNonce": 1212217384,
			"isDeleted": false,
			"id": "vryEFD3S",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -27.287412218615714,
			"y": 268.8327201124986,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 120,
			"height": 48,
			"seed": 212335252,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "创建线程执行\n任务",
			"rawText": "创建线程执行任务",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "Yxh_94PFSuKFPbsKze27r",
			"originalText": "创建线程执行任务",
			"lineHeight": 1.2,
			"baseline": 44
		},
		{
			"type": "arrow",
			"version": 1459,
			"versionNonce": 310902360,
			"isDeleted": false,
			"id": "e8QVRqdayhgBvPYKkvCK6",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -119.35628435136533,
			"y": -204.3829701191549,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 279.164746855704,
			"height": 1.7395987047809456,
			"seed": 1385470892,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "XQfTqQvv"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "nt7nr4Gu1S748ZfR_-6Xk",
				"gap": 8.736214369183095,
				"focus": 0.027341816281219546
			},
			"endBinding": {
				"elementId": "gvJG81DLcbyQvfgp3qFDY",
				"gap": 1.1966084424926748,
				"focus": -0.1650657329960843
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					279.164746855704,
					1.7395987047809456
				]
			]
		},
		{
			"type": "text",
			"version": 42,
			"versionNonce": 952087336,
			"isDeleted": false,
			"id": "XQfTqQvv",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -29.945031784679173,
			"y": -201.1983421370964,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 2057939884,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "否",
			"rawText": "否",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "e8QVRqdayhgBvPYKkvCK6",
			"originalText": "否",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 3151,
			"versionNonce": 2057728856,
			"isDeleted": false,
			"id": "KZLdTHeJqOywDihRKDcZ2",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -146.44517995116453,
			"y": 22.44544880734223,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 94.56913089869352,
			"height": 0.6680540444326226,
			"seed": 1277249812,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "G5aan4TS"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "HUxSIj4LqHtmZjeeB5ARv",
				"gap": 4.161962435354603,
				"focus": -0.002965193256785967
			},
			"endBinding": {
				"elementId": "KFlCeFOaKunOIV71uAJVZ",
				"gap": 3.454139686643714,
				"focus": 0.09203497145585661
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					94.56913089869352,
					-0.6680540444326226
				]
			]
		},
		{
			"type": "text",
			"version": 43,
			"versionNonce": 1068437032,
			"isDeleted": false,
			"id": "G5aan4TS",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -52.57364677731573,
			"y": -56.47923259865294,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 650264492,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "否",
			"rawText": "否",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "KZLdTHeJqOywDihRKDcZ2",
			"originalText": "否",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 636,
			"versionNonce": 556681304,
			"isDeleted": false,
			"id": "k1cC5DU5JGpSPfd19KAcA",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -152.23224409566174,
			"y": 285.00418638105714,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 114.59892250285682,
			"height": 0,
			"seed": 132295212,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"type": "text",
					"id": "YqCmve2i"
				}
			],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "WEVCdVaLjtz4Fcffub1u0",
				"gap": 4.890009494436406,
				"focus": 0.09332140098467608
			},
			"endBinding": {
				"elementId": "Yxh_94PFSuKFPbsKze27r",
				"gap": 1.471990671742816,
				"focus": 0.2699494390152221
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					114.59892250285682,
					0
				]
			]
		},
		{
			"type": "text",
			"version": 45,
			"versionNonce": 100337960,
			"isDeleted": false,
			"id": "YqCmve2i",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 109.0665381253637,
			"y": -60.244340619173045,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 20,
			"height": 24,
			"seed": 1695471788,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"fontSize": 20,
			"fontFamily": 3,
			"text": "否",
			"rawText": "否",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "k1cC5DU5JGpSPfd19KAcA",
			"originalText": "否",
			"lineHeight": 1.2,
			"baseline": 20
		},
		{
			"type": "arrow",
			"version": 988,
			"versionNonce": 838040920,
			"isDeleted": false,
			"id": "ZPEXTIlR7mhLNM11sy4Xy",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 220.78812149458588,
			"y": -174.09729326437835,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 415.5535556177231,
			"height": 817.7721780684057,
			"seed": 1624804140,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "gvJG81DLcbyQvfgp3qFDY",
				"gap": 3.967187571547356,
				"focus": 0.13667424352016774
			},
			"endBinding": {
				"elementId": "z7hRMv-_l1mhRi6DggYVQ",
				"focus": -0.25787012909214907,
				"gap": 1.518167983666448
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					9.090868154691833,
					817.7721780684057
				],
				[
					-406.4626874630312,
					817.7721780684057
				]
			]
		},
		{
			"type": "arrow",
			"version": 416,
			"versionNonce": 923771944,
			"isDeleted": false,
			"id": "gHnC0qdj-7I5CbrTI6wYi",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 109.7786244961936,
			"y": 292.7713429336391,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 102.52026129102379,
			"height": 0,
			"seed": 1785979564,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401183,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "Yxh_94PFSuKFPbsKze27r",
				"gap": 8.192118012362926,
				"focus": -0.002116454443429622
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					102.52026129102379,
					0
				]
			]
		},
		{
			"type": "diamond",
			"version": 2365,
			"versionNonce": 1045041752,
			"isDeleted": false,
			"id": "HUxSIj4LqHtmZjeeB5ARv",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -258.4845831920238,
			"y": -38.960425664208685,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 107.92006516663574,
			"height": 124,
			"seed": 1352084,
			"groupIds": [
				"pmtSAxMA6JB0BZEHcXqA8",
				"1Hbe0Xu0LRZU428IqmoTY"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "C_Rb9IbOW7akGqW5d9orl",
					"type": "arrow"
				},
				{
					"id": "yZBF-raaQCqh61VvxiCnC",
					"type": "arrow"
				},
				{
					"id": "KZLdTHeJqOywDihRKDcZ2",
					"type": "arrow"
				}
			],
			"updated": 1704364401184,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 285,
			"versionNonce": 819519272,
			"isDeleted": false,
			"id": "LFRrU0lX",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -240.22423935421583,
			"y": 5.488133581334949,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 69.75994873046875,
			"height": 41.8797512678286,
			"seed": 149163924,
			"groupIds": [
				"1Hbe0Xu0LRZU428IqmoTY"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 17.44989636159525,
			"fontFamily": 3,
			"text": "阻塞队列\n已满",
			"rawText": "阻塞队列\n已满",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "阻塞队列\n已满",
			"lineHeight": 1.2,
			"baseline": 37
		},
		{
			"type": "diamond",
			"version": 2103,
			"versionNonce": 218498904,
			"isDeleted": false,
			"id": "WEVCdVaLjtz4Fcffub1u0",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -261.5993354799764,
			"y": 217.21825952000725,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 107.92006516663574,
			"height": 124,
			"seed": 150513684,
			"groupIds": [
				"s63kQINA-FctafzG99eBZ",
				"LdSeX7eA4d_79dH8V9Eat"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "nNmQBNr2-Tz2uYIY4uJhD",
					"type": "arrow"
				},
				{
					"id": "yZBF-raaQCqh61VvxiCnC",
					"type": "arrow"
				},
				{
					"id": "k1cC5DU5JGpSPfd19KAcA",
					"type": "arrow"
				}
			],
			"updated": 1704364401184,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 763,
			"versionNonce": 868399656,
			"isDeleted": false,
			"id": "wJT1xs79",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -237.29969554700588,
			"y": 259.85869628133656,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 89.84992980957031,
			"height": 43.135524470202164,
			"seed": 1957595180,
			"groupIds": [
				"LdSeX7eA4d_79dH8V9Eat"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 17.973135195917568,
			"fontFamily": 3,
			"text": "已到\n最大线程数",
			"rawText": "已到\n最大线程数",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "已到\n最大线程数",
			"lineHeight": 1.2,
			"baseline": 39
		},
		{
			"type": "diamond",
			"version": 2145,
			"versionNonce": 1786469464,
			"isDeleted": false,
			"id": "nt7nr4Gu1S748ZfR_-6Xk",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": -265.29956962475615,
			"y": -287.5597633608785,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "transparent",
			"width": 137.62878358887204,
			"height": 160.99056373784742,
			"seed": 1028425388,
			"groupIds": [
				"MBbtgEOVPc4odAZM0JsLS",
				"EVdva4IYyKcIrxN-YFKpM"
			],
			"frameId": null,
			"roundness": {
				"type": 2
			},
			"boundElements": [
				{
					"id": "gM9uDHGxHDbmNZx2fb7Y9",
					"type": "arrow"
				},
				{
					"id": "C_Rb9IbOW7akGqW5d9orl",
					"type": "arrow"
				},
				{
					"type": "text",
					"id": "doY0TAMK"
				},
				{
					"id": "e8QVRqdayhgBvPYKkvCK6",
					"type": "arrow"
				}
			],
			"updated": 1704364401184,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 743,
			"versionNonce": 314127656,
			"isDeleted": false,
			"id": "doY0TAMK",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -225.34235541699127,
			"y": -242.0618494931311,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 57.89996337890625,
			"height": 69.49945413342893,
			"seed": 1528217370,
			"groupIds": [
				"MBbtgEOVPc4odAZM0JsLS",
				"EVdva4IYyKcIrxN-YFKpM"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 19.30540392595248,
			"fontFamily": 3,
			"text": "到达核\n心线程\n数",
			"rawText": "到达核心线程数",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "nt7nr4Gu1S748ZfR_-6Xk",
			"originalText": "到达核心线程数",
			"lineHeight": 1.2,
			"baseline": 65
		},
		{
			"type": "arrow",
			"version": 104,
			"versionNonce": 2021527896,
			"isDeleted": false,
			"id": "XwG1KmlJC-7BuU5kMnnk-",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -208.313100940347,
			"y": 503.6235320015189,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 1.4636605322276353,
			"height": 120.90750754602118,
			"seed": 519278228,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cSCQqxhqorGcisT6jgZ8W",
				"gap": 4.768495188622893,
				"focus": 0.06745346682864312
			},
			"endBinding": {
				"elementId": "z7hRMv-_l1mhRi6DggYVQ",
				"gap": 3.8284864182292733,
				"focus": -0.15291210481841808
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					1.4636605322276353,
					120.90750754602118
				]
			]
		},
		{
			"type": "rectangle",
			"version": 194,
			"versionNonce": 1663586344,
			"isDeleted": false,
			"id": "hzEQ0ow4Zc4vtwJxATPeM",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 469.15444173029687,
			"y": 9173.470519116494,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 217.6749093191964,
			"height": 49,
			"seed": 1064450392,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "text",
					"id": "r8Jy8cRC"
				}
			],
			"updated": 1704364401184,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 175,
			"versionNonce": 2120361560,
			"isDeleted": false,
			"id": "r8Jy8cRC",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 507.67939638989503,
			"y": 9178.770519116493,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 140.625,
			"height": 38.4,
			"seed": 1129824088,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "Integer.SIZE=32\nCOUNT_BITS=29",
			"rawText": "Integer.SIZE=32\nCOUNT_BITS=29",
			"textAlign": "center",
			"verticalAlign": "middle",
			"containerId": "hzEQ0ow4Zc4vtwJxATPeM",
			"originalText": "Integer.SIZE=32\nCOUNT_BITS=29",
			"lineHeight": 1.2,
			"baseline": 34
		},
		{
			"type": "arrow",
			"version": 531,
			"versionNonce": 754715432,
			"isDeleted": false,
			"id": "_v-BtdCvnsMkNaksOGJ7l",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 68.75269986176839,
			"y": 23.48762570133219,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 150.23633659509164,
			"height": 0,
			"seed": 2121501972,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "KFlCeFOaKunOIV71uAJVZ",
				"gap": 3.1263158786864693,
				"focus": 0.024956469399945818
			},
			"endBinding": null,
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "arrow",
			"points": [
				[
					0,
					0
				],
				[
					150.23633659509164,
					0
				]
			]
		},
		{
			"type": "rectangle",
			"version": 71,
			"versionNonce": 956013400,
			"isDeleted": false,
			"id": "cO2ShusooszR-hj-UXi7k",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 62.13407471380225,
			"y": 9215.432276988211,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 341.2237180955964,
			"height": 24.974438820436262,
			"seed": 588102488,
			"groupIds": [],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "arrow",
					"id": "YHCkOjSZ"
				}
			],
			"updated": 1704364401184,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 36,
			"versionNonce": 446719528,
			"isDeleted": false,
			"id": "NPcqCzcP",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": -203.81221563577384,
			"y": 1819.004663926763,
			"strokeColor": "#1e1e1e",
			"backgroundColor": "#ffffff",
			"width": 918.75,
			"height": 40896,
			"seed": 1591035480,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "/*\n * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.\n *\n * This code is free software; you can redistribute it and/or modify it\n * under the terms of the GNU General Public License version 2 only, as\n * published by the Free Software Foundation.  Oracle designates this\n * particular file as subject to the \"Classpath\" exception as provided\n * by Oracle in the LICENSE file that accompanied this code.\n *\n * This code is distributed in the hope that it will be useful, but WITHOUT\n * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or\n * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License\n * version 2 for more details (a copy is included in the LICENSE file that\n * accompanied this code).\n *\n * You should have received a copy of the GNU General Public License version\n * 2 along with this work; if not, write to the Free Software Foundation,\n * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.\n *\n * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA\n * or visit www.oracle.com if you need additional information or have any\n * questions.\n */\n\n/*\n * This file is available under and governed by the GNU General Public\n * License version 2 only, as published by the Free Software Foundation.\n * However, the following notice accompanied the original version of this\n * file:\n *\n * Written by Doug Lea with assistance from members of JCP JSR-166\n * Expert Group and released to the public domain, as explained at\n * http://creativecommons.org/publicdomain/zero/1.0/\n */\n\npackage java.util.concurrent;\n\nimport java.util.ArrayList;\nimport java.util.ConcurrentModificationException;\nimport java.util.HashSet;\nimport java.util.Iterator;\nimport java.util.List;\nimport java.util.concurrent.atomic.AtomicInteger;\nimport java.util.concurrent.locks.AbstractQueuedSynchronizer;\nimport java.util.concurrent.locks.Condition;\nimport java.util.concurrent.locks.ReentrantLock;\n\n/**\n * An {@link ExecutorService} that executes each submitted task using\n * one of possibly several pooled threads, normally configured\n * using {@link Executors} factory methods.\n *\n * <p>Thread pools address two different problems: they usually\n * provide improved performance when executing large numbers of\n * asynchronous tasks, due to reduced per-task invocation overhead,\n * and they provide a means of bounding and managing the resources,\n * including threads, consumed when executing a collection of tasks.\n * Each {@code ThreadPoolExecutor} also maintains some basic\n * statistics, such as the number of completed tasks.\n *\n * <p>To be useful across a wide range of contexts, this class\n * provides many adjustable parameters and extensibility\n * hooks. However, programmers are urged to use the more convenient\n * {@link Executors} factory methods {@link\n * Executors#newCachedThreadPool} (unbounded thread pool, with\n * automatic thread reclamation), {@link Executors#newFixedThreadPool}\n * (fixed size thread pool) and {@link\n * Executors#newSingleThreadExecutor} (single background thread), that\n * preconfigure settings for the most common usage\n * scenarios. Otherwise, use the following guide when manually\n * configuring and tuning this class:\n *\n * <dl>\n *\n * <dt>Core and maximum pool sizes</dt>\n *\n * <dd>A {@code ThreadPoolExecutor} will automatically adjust the\n * pool size (see {@link #getPoolSize})\n * according to the bounds set by\n * corePoolSize (see {@link #getCorePoolSize}) and\n * maximumPoolSize (see {@link #getMaximumPoolSize}).\n *\n * When a new task is submitted in method {@link #execute(Runnable)},\n * if fewer than corePoolSize threads are running, a new thread is\n * created to handle the request, even if other worker threads are\n * idle.  Else if fewer than maximumPoolSize threads are running, a\n * new thread will be created to handle the request only if the queue\n * is full.  By setting corePoolSize and maximumPoolSize the same, you\n * create a fixed-size thread pool. By setting maximumPoolSize to an\n * essentially unbounded value such as {@code Integer.MAX_VALUE}, you\n * allow the pool to accommodate an arbitrary number of concurrent\n * tasks. Most typically, core and maximum pool sizes are set only\n * upon construction, but they may also be changed dynamically using\n * {@link #setCorePoolSize} and {@link #setMaximumPoolSize}. </dd>\n *\n * <dt>On-demand construction</dt>\n *\n * <dd>By default, even core threads are initially created and\n * started only when new tasks arrive, but this can be overridden\n * dynamically using method {@link #prestartCoreThread} or {@link\n * #prestartAllCoreThreads}.  You probably want to prestart threads if\n * you construct the pool with a non-empty queue. </dd>\n *\n * <dt>Creating new threads</dt>\n *\n * <dd>New threads are created using a {@link ThreadFactory}.  If not\n * otherwise specified, a {@link Executors#defaultThreadFactory} is\n * used, that creates threads to all be in the same {@link\n * ThreadGroup} and with the same {@code NORM_PRIORITY} priority and\n * non-daemon status. By supplying a different ThreadFactory, you can\n * alter the thread's name, thread group, priority, daemon status,\n * etc. If a {@code ThreadFactory} fails to create a thread when asked\n * by returning null from {@code newThread}, the executor will\n * continue, but might not be able to execute any tasks. Threads\n * should possess the \"modifyThread\" {@code RuntimePermission}. If\n * worker threads or other threads using the pool do not possess this\n * permission, service may be degraded: configuration changes may not\n * take effect in a timely manner, and a shutdown pool may remain in a\n * state in which termination is possible but not completed.</dd>\n *\n * <dt>Keep-alive times</dt>\n *\n * <dd>If the pool currently has more than corePoolSize threads,\n * excess threads will be terminated if they have been idle for more\n * than the keepAliveTime (see {@link #getKeepAliveTime(TimeUnit)}).\n * This provides a means of reducing resource consumption when the\n * pool is not being actively used. If the pool becomes more active\n * later, new threads will be constructed. This parameter can also be\n * changed dynamically using method {@link #setKeepAliveTime(long,\n * TimeUnit)}.  Using a value of {@code Long.MAX_VALUE} {@link\n * TimeUnit#NANOSECONDS} effectively disables idle threads from ever\n * terminating prior to shut down. By default, the keep-alive policy\n * applies only when there are more than corePoolSize threads, but\n * method {@link #allowCoreThreadTimeOut(boolean)} can be used to\n * apply this time-out policy to core threads as well, so long as the\n * keepAliveTime value is non-zero. </dd>\n *\n * <dt>Queuing</dt>\n *\n * <dd>Any {@link BlockingQueue} may be used to transfer and hold\n * submitted tasks.  The use of this queue interacts with pool sizing:\n *\n * <ul>\n *\n * <li>If fewer than corePoolSize threads are running, the Executor\n * always prefers adding a new thread\n * rather than queuing.\n *\n * <li>If corePoolSize or more threads are running, the Executor\n * always prefers queuing a request rather than adding a new\n * thread.\n *\n * <li>If a request cannot be queued, a new thread is created unless\n * this would exceed maximumPoolSize, in which case, the task will be\n * rejected.\n *\n * </ul>\n *\n * There are three general strategies for queuing:\n * <ol>\n *\n * <li><em> Direct handoffs.</em> A good default choice for a work\n * queue is a {@link SynchronousQueue} that hands off tasks to threads\n * without otherwise holding them. Here, an attempt to queue a task\n * will fail if no threads are immediately available to run it, so a\n * new thread will be constructed. This policy avoids lockups when\n * handling sets of requests that might have internal dependencies.\n * Direct handoffs generally require unbounded maximumPoolSizes to\n * avoid rejection of new submitted tasks. This in turn admits the\n * possibility of unbounded thread growth when commands continue to\n * arrive on average faster than they can be processed.\n *\n * <li><em> Unbounded queues.</em> Using an unbounded queue (for\n * example a {@link LinkedBlockingQueue} without a predefined\n * capacity) will cause new tasks to wait in the queue when all\n * corePoolSize threads are busy. Thus, no more than corePoolSize\n * threads will ever be created. (And the value of the maximumPoolSize\n * therefore doesn't have any effect.)  This may be appropriate when\n * each task is completely independent of others, so tasks cannot\n * affect each others execution; for example, in a web page server.\n * While this style of queuing can be useful in smoothing out\n * transient bursts of requests, it admits the possibility of\n * unbounded work queue growth when commands continue to arrive on\n * average faster than they can be processed.\n *\n * <li><em>Bounded queues.</em> A bounded queue (for example, an\n * {@link ArrayBlockingQueue}) helps prevent resource exhaustion when\n * used with finite maximumPoolSizes, but can be more difficult to\n * tune and control.  Queue sizes and maximum pool sizes may be traded\n * off for each other: Using large queues and small pools minimizes\n * CPU usage, OS resources, and context-switching overhead, but can\n * lead to artificially low throughput.  If tasks frequently block (for\n * example if they are I/O bound), a system may be able to schedule\n * time for more threads than you otherwise allow. Use of small queues\n * generally requires larger pool sizes, which keeps CPUs busier but\n * may encounter unacceptable scheduling overhead, which also\n * decreases throughput.\n *\n * </ol>\n *\n * </dd>\n *\n * <dt>Rejected tasks</dt>\n *\n * <dd>New tasks submitted in method {@link #execute(Runnable)} will be\n * <em>rejected</em> when the Executor has been shut down, and also when\n * the Executor uses finite bounds for both maximum threads and work queue\n * capacity, and is saturated.  In either case, the {@code execute} method\n * invokes the {@link\n * RejectedExecutionHandler#rejectedExecution(Runnable, ThreadPoolExecutor)}\n * method of its {@link RejectedExecutionHandler}.  Four predefined handler\n * policies are provided:\n *\n * <ol>\n *\n * <li>In the default {@link ThreadPoolExecutor.AbortPolicy}, the handler\n * throws a runtime {@link RejectedExecutionException} upon rejection.\n *\n * <li>In {@link ThreadPoolExecutor.CallerRunsPolicy}, the thread\n * that invokes {@code execute} itself runs the task. This provides a\n * simple feedback control mechanism that will slow down the rate that\n * new tasks are submitted.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardPolicy}, a task that cannot\n * be executed is simply dropped. This policy is designed only for\n * those rare cases in which task completion is never relied upon.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardOldestPolicy}, if the\n * executor is not shut down, the task at the head of the work queue\n * is dropped, and then execution is retried (which can fail again,\n * causing this to be repeated.) This policy is rarely acceptable.  In\n * nearly all cases, you should also cancel the task to cause an\n * exception in any component waiting for its completion, and/or log\n * the failure, as illustrated in {@link\n * ThreadPoolExecutor.DiscardOldestPolicy} documentation.\n *\n * </ol>\n *\n * It is possible to define and use other kinds of {@link\n * RejectedExecutionHandler} classes. Doing so requires some care\n * especially when policies are designed to work only under particular\n * capacity or queuing policies. </dd>\n *\n * <dt>Hook methods</dt>\n *\n * <dd>This class provides {@code protected} overridable\n * {@link #beforeExecute(Thread, Runnable)} and\n * {@link #afterExecute(Runnable, Throwable)} methods that are called\n * before and after execution of each task.  These can be used to\n * manipulate the execution environment; for example, reinitializing\n * ThreadLocals, gathering statistics, or adding log entries.\n * Additionally, method {@link #terminated} can be overridden to perform\n * any special processing that needs to be done once the Executor has\n * fully terminated.\n *\n * <p>If hook, callback, or BlockingQueue methods throw exceptions,\n * internal worker threads may in turn fail, abruptly terminate, and\n * possibly be replaced.</dd>\n *\n * <dt>Queue maintenance</dt>\n *\n * <dd>Method {@link #getQueue()} allows access to the work queue\n * for purposes of monitoring and debugging.  Use of this method for\n * any other purpose is strongly discouraged.  Two supplied methods,\n * {@link #remove(Runnable)} and {@link #purge} are available to\n * assist in storage reclamation when large numbers of queued tasks\n * become cancelled.</dd>\n *\n * <dt>Reclamation</dt>\n *\n * <dd>A pool that is no longer referenced in a program <em>AND</em>\n * has no remaining threads may be reclaimed (garbage collected)\n * without being explicitly shutdown. You can configure a pool to\n * allow all unused threads to eventually die by setting appropriate\n * keep-alive times, using a lower bound of zero core threads and/or\n * setting {@link #allowCoreThreadTimeOut(boolean)}.  </dd>\n *\n * </dl>\n *\n * <p><b>Extension example.</b> Most extensions of this class\n * override one or more of the protected hook methods. For example,\n * here is a subclass that adds a simple pause/resume feature:\n *\n * <pre> {@code\n * class PausableThreadPoolExecutor extends ThreadPoolExecutor {\n *   private boolean isPaused;\n *   private ReentrantLock pauseLock = new ReentrantLock();\n *   private Condition unpaused = pauseLock.newCondition();\n *\n *   public PausableThreadPoolExecutor(...) { super(...); }\n *\n *   protected void beforeExecute(Thread t, Runnable r) {\n *     super.beforeExecute(t, r);\n *     pauseLock.lock();\n *     try {\n *       while (isPaused) unpaused.await();\n *     } catch (InterruptedException ie) {\n *       t.interrupt();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void pause() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = true;\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void resume() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = false;\n *       unpaused.signalAll();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n * }}</pre>\n *\n * @since 1.5\n * @author Doug Lea\n */\npublic class ThreadPoolExecutor extends AbstractExecutorService {\n    /**\n     * The main pool control state, ctl, is an atomic integer packing\n     * two conceptual fields\n     *   workerCount, indicating the effective number of threads\n     *   runState,    indicating whether running, shutting down etc\n     *\n     * In order to pack them into one int, we limit workerCount to\n     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2\n     * billion) otherwise representable. If this is ever an issue in\n     * the future, the variable can be changed to be an AtomicLong,\n     * and the shift/mask constants below adjusted. But until the need\n     * arises, this code is a bit faster and simpler using an int.\n     *\n     * The workerCount is the number of workers that have been\n     * permitted to start and not permitted to stop.  The value may be\n     * transiently different from the actual number of live threads,\n     * for example when a ThreadFactory fails to create a thread when\n     * asked, and when exiting threads are still performing\n     * bookkeeping before terminating. The user-visible pool size is\n     * reported as the current size of the workers set.\n     *\n     * The runState provides the main lifecycle control, taking on values:\n     *\n     *   RUNNING:  Accept new tasks and process queued tasks\n     *   SHUTDOWN: Don't accept new tasks, but process queued tasks\n     *   STOP:     Don't accept new tasks, don't process queued tasks,\n     *             and interrupt in-progress tasks\n     *   TIDYING:  All tasks have terminated, workerCount is zero,\n     *             the thread transitioning to state TIDYING\n     *             will run the terminated() hook method\n     *   TERMINATED: terminated() has completed\n     *\n     * The numerical order among these values matters, to allow\n     * ordered comparisons. The runState monotonically increases over\n     * time, but need not hit each state. The transitions are:\n     *\n     * RUNNING -> SHUTDOWN\n     *    On invocation of shutdown()\n     * (RUNNING or SHUTDOWN) -> STOP\n     *    On invocation of shutdownNow()\n     * SHUTDOWN -> TIDYING\n     *    When both queue and pool are empty\n     * STOP -> TIDYING\n     *    When pool is empty\n     * TIDYING -> TERMINATED\n     *    When the terminated() hook method has completed\n     *\n     * Threads waiting in awaitTermination() will return when the\n     * state reaches TERMINATED.\n     *\n     * Detecting the transition from SHUTDOWN to TIDYING is less\n     * straightforward than you'd like because the queue may become222\n     * empty after non-empty and vice versa during SHUTDOWN state, but\n     * we can only terminate if, after seeing that it is empty, we see\n     * that workerCount is 0 (which sometimes entails a recheck -- see\n     * below).\n     */\n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n    /*\n     * Bit field accessors that don't require unpacking ctl.\n     * These depend on the bit layout and on workerCount being never negative.\n     */\n\n    private static boolean runStateLessThan(int c, int s) {\n        return c < s;\n    }\n\n    private static boolean runStateAtLeast(int c, int s) {\n        return c >= s;\n    }\n\n    private static boolean isRunning(int c) {\n        return c < SHUTDOWN;\n    }\n\n    /**\n     * Attempts to CAS-increment the workerCount field of ctl.\n     */\n    private boolean compareAndIncrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect + 1);\n    }\n\n    /**\n     * Attempts to CAS-decrement the workerCount field of ctl.\n     */\n    private boolean compareAndDecrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect - 1);\n    }\n\n    /**\n     * Decrements the workerCount field of ctl. This is called only on\n     * abrupt termination of a thread (see processWorkerExit). Other\n     * decrements are performed within getTask.\n     */\n    private void decrementWorkerCount() {\n        ctl.addAndGet(-1);\n    }\n\n    /**\n     * The queue used for holding tasks and handing off to worker\n     * threads.  We do not require that workQueue.poll() returning\n     * null necessarily means that workQueue.isEmpty(), so rely\n     * solely on isEmpty to see if the queue is empty (which we must\n     * do for example when deciding whether to transition from\n     * SHUTDOWN to TIDYING).  This accommodates special-purpose\n     * queues such as DelayQueues for which poll() is allowed to\n     * return null even if it may later return non-null when delays\n     * expire.\n     */\n    private final BlockingQueue<Runnable> workQueue;\n\n    /**\n     * Lock held on access to workers set and related bookkeeping.\n     * While we could use a concurrent set of some sort, it turns out\n     * to be generally preferable to use a lock. Among the reasons is\n     * that this serializes interruptIdleWorkers, which avoids\n     * unnecessary interrupt storms, especially during shutdown.\n     * Otherwise exiting threads would concurrently interrupt those\n     * that have not yet interrupted. It also simplifies some of the\n     * associated statistics bookkeeping of largestPoolSize etc. We\n     * also hold mainLock on shutdown and shutdownNow, for the sake of\n     * ensuring workers set is stable while separately checking\n     * permission to interrupt and actually interrupting.\n     */\n    private final ReentrantLock mainLock = new ReentrantLock();\n\n    /**\n     * Set containing all worker threads in pool. Accessed only when\n     * holding mainLock.\n     */\n    private final HashSet<Worker> workers = new HashSet<>();\n\n    /**\n     * Wait condition to support awaitTermination.\n     */\n    private final Condition termination = mainLock.newCondition();\n\n    /**\n     * Tracks largest attained pool size. Accessed only under\n     * mainLock.\n     */\n    private int largestPoolSize;\n\n    /**\n     * Counter for completed tasks. Updated only on termination of\n     * worker threads. Accessed only under mainLock.\n     */\n    private long completedTaskCount;\n\n    /*\n     * All user control parameters are declared as volatiles so that\n     * ongoing actions are based on freshest values, but without need\n     * for locking, since no internal invariants depend on them\n     * changing synchronously with respect to other actions.\n     */\n\n    /**\n     * Factory for new threads. All threads are created using this\n     * factory (via method addWorker).  All callers must be prepared\n     * for addWorker to fail, which may reflect a system or user's\n     * policy limiting the number of threads.  Even though it is not\n     * treated as an error, failure to create threads may result in\n     * new tasks being rejected or existing ones remaining stuck in\n     * the queue.\n     *\n     * We go further and preserve pool invariants even in the face of\n     * errors such as OutOfMemoryError, that might be thrown while\n     * trying to create threads.  Such errors are rather common due to\n     * the need to allocate a native stack in Thread.start, and users\n     * will want to perform clean pool shutdown to clean up.  There\n     * will likely be enough memory available for the cleanup code to\n     * complete without encountering yet another OutOfMemoryError.\n     */\n    private volatile ThreadFactory threadFactory;\n\n    /**\n     * Handler called when saturated or shutdown in execute.\n     */\n    private volatile RejectedExecutionHandler handler;\n\n    /**\n     * Timeout in nanoseconds for idle threads waiting for work.\n     * Threads use this timeout when there are more than corePoolSize\n     * present or if allowCoreThreadTimeOut. Otherwise they wait\n     * forever for new work.\n     */\n    private volatile long keepAliveTime;\n\n    /**\n     * If false (default), core threads stay alive even when idle.\n     * If true, core threads use keepAliveTime to time out waiting\n     * for work.\n     */\n    private volatile boolean allowCoreThreadTimeOut;\n\n    /**\n     * Core pool size is the minimum number of workers to keep alive\n     * (and not allow to time out etc) unless allowCoreThreadTimeOut\n     * is set, in which case the minimum is zero.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code corePoolSize & COUNT_MASK}.\n     */\n    private volatile int corePoolSize;\n\n    /**\n     * Maximum pool size.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code maximumPoolSize & COUNT_MASK}.\n     */\n    private volatile int maximumPoolSize;\n\n    /**\n     * The default rejected execution handler.\n     */\n    private static final RejectedExecutionHandler defaultHandler =\n        new AbortPolicy();\n\n    /**\n     * Permission required for callers of shutdown and shutdownNow.\n     * We additionally require (see checkShutdownAccess) that callers\n     * have permission to actually interrupt threads in the worker set\n     * (as governed by Thread.interrupt, which relies on\n     * ThreadGroup.checkAccess, which in turn relies on\n     * SecurityManager.checkAccess). Shutdowns are attempted only if\n     * these checks pass.\n     *\n     * All actual invocations of Thread.interrupt (see\n     * interruptIdleWorkers and interruptWorkers) ignore\n     * SecurityExceptions, meaning that the attempted interrupts\n     * silently fail. In the case of shutdown, they should not fail\n     * unless the SecurityManager has inconsistent policies, sometimes\n     * allowing access to a thread and sometimes not. In such cases,\n     * failure to actually interrupt threads may disable or delay full\n     * termination. Other uses of interruptIdleWorkers are advisory,\n     * and failure to actually interrupt will merely delay response to\n     * configuration changes so is not handled exceptionally.\n     */\n    private static final RuntimePermission shutdownPerm =\n        new RuntimePermission(\"modifyThread\");\n\n    /**\n     * Class Worker mainly maintains interrupt control state for\n     * threads running tasks, along with other minor bookkeeping.\n     * This class opportunistically extends AbstractQueuedSynchronizer\n     * to simplify acquiring and releasing a lock surrounding each\n     * task execution.  This protects against interrupts that are\n     * intended to wake up a worker thread waiting for a task from\n     * instead interrupting a task being run.  We implement a simple\n     * non-reentrant mutual exclusion lock rather than use\n     * ReentrantLock because we do not want worker tasks to be able to\n     * reacquire the lock when they invoke pool control methods like\n     * setCorePoolSize.  Additionally, to suppress interrupts until\n     * the thread actually starts running tasks, we initialize lock\n     * state to a negative value, and clear it upon start (in\n     * runWorker).\n     */\n    private final class Worker\n        extends AbstractQueuedSynchronizer\n        implements Runnable\n    {\n        /**\n         * This class will never be serialized, but we provide a\n         * serialVersionUID to suppress a javac warning.\n         */\n        private static final long serialVersionUID = 6138294804551838833L;\n\n        /** Thread this worker is running in.  Null if factory fails. */\n        @SuppressWarnings(\"serial\") // Unlikely to be serializable\n        final Thread thread;\n        /** Initial task to run.  Possibly null. */\n        @SuppressWarnings(\"serial\") // Not statically typed as Serializable\n        Runnable firstTask;\n        /** Per-thread task counter */\n        volatile long completedTasks;\n\n        // TODO: switch to AbstractQueuedLongSynchronizer and move\n        // completedTasks into the lock word.\n\n        /**\n         * Creates with given first task and thread from ThreadFactory.\n         * @param firstTask the first task (null if none)\n         */\n        Worker(Runnable firstTask) {\n            setState(-1); // inhibit interrupts until runWorker\n            this.firstTask = firstTask;\n            this.thread = getThreadFactory().newThread(this);\n        }\n\n        /** Delegates main run loop to outer runWorker. */\n        public void run() {\n            runWorker(this);\n        }\n\n        // Lock methods\n        //\n        // The value 0 represents the unlocked state.\n        // The value 1 represents the locked state.\n\n        protected boolean isHeldExclusively() {\n            return getState() != 0;\n        }\n\n        protected boolean tryAcquire(int unused) {\n            if (compareAndSetState(0, 1)) {\n                setExclusiveOwnerThread(Thread.currentThread());\n                return true;\n            }\n            return false;\n        }\n\n        protected boolean tryRelease(int unused) {\n            setExclusiveOwnerThread(null);\n            setState(0);\n            return true;\n        }\n\n        public void lock()        { acquire(1); }\n        public boolean tryLock()  { return tryAcquire(1); }\n        public void unlock()      { release(1); }\n        public boolean isLocked() { return isHeldExclusively(); }\n\n        void interruptIfStarted() {\n            Thread t;\n            if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {\n                try {\n                    t.interrupt();\n                } catch (SecurityException ignore) {\n                }\n            }\n        }\n    }\n\n    /*\n     * Methods for setting control state\n     */\n\n    /**\n     * Transitions runState to given target, or leaves it alone if\n     * already at least the given target.\n     *\n     * @param targetState the desired state, either SHUTDOWN or STOP\n     *        (but not TIDYING or TERMINATED -- use tryTerminate for that)\n     */\n    private void advanceRunState(int targetState) {\n        // assert targetState == SHUTDOWN || targetState == STOP;\n        for (;;) {\n            int c = ctl.get();\n            if (runStateAtLeast(c, targetState) ||\n                ctl.compareAndSet(c, ctlOf(targetState, workerCountOf(c))))\n                break;\n        }\n    }\n\n    /**\n     * Transitions to TERMINATED state if either (SHUTDOWN and pool\n     * and queue empty) or (STOP and pool empty).  If otherwise\n     * eligible to terminate but workerCount is nonzero, interrupts an\n     * idle worker to ensure that shutdown signals propagate. This\n     * method must be called following any action that might make\n     * termination possible -- reducing worker count or removing tasks\n     * from the queue during shutdown. The method is non-private to\n     * allow access from ScheduledThreadPoolExecutor.\n     */\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n    /*\n     * Methods for controlling interrupts to worker threads.\n     */\n\n    /**\n     * If there is a security manager, makes sure caller has\n     * permission to shut down threads in general (see shutdownPerm).\n     * If this passes, additionally makes sure the caller is allowed\n     * to interrupt each worker thread. This might not be true even if\n     * first check passed, if the SecurityManager treats some threads\n     * specially.\n     */\n    private void checkShutdownAccess() {\n        // assert mainLock.isHeldByCurrentThread();\n        @SuppressWarnings(\"removal\")\n        SecurityManager security = System.getSecurityManager();\n        if (security != null) {\n            security.checkPermission(shutdownPerm);\n            for (Worker w : workers)\n                security.checkAccess(w.thread);\n        }\n    }\n\n    /**\n     * Interrupts all threads, even if active. Ignores SecurityExceptions\n     * (in which case some threads may remain uninterrupted).\n     */\n    private void interruptWorkers() {\n        // assert mainLock.isHeldByCurrentThread();\n        for (Worker w : workers)\n            w.interruptIfStarted();\n    }\n\n    /**\n     * Interrupts threads that might be waiting for tasks (as\n     * indicated by not being locked) so they can check for\n     * termination or configuration changes. Ignores\n     * SecurityExceptions (in which case some threads may remain\n     * uninterrupted).\n     *\n     * @param onlyOne If true, interrupt at most one worker. This is\n     * called only from tryTerminate when termination is otherwise\n     * enabled but there are still other workers.  In this case, at\n     * most one waiting worker is interrupted to propagate shutdown\n     * signals in case all threads are currently waiting.\n     * Interrupting any arbitrary thread ensures that newly arriving\n     * workers since shutdown began will also eventually exit.\n     * To guarantee eventual termination, it suffices to always\n     * interrupt only one idle worker, but shutdown() interrupts all\n     * idle workers so that redundant workers exit promptly, not\n     * waiting for a straggler task to finish.\n     */\n    private void interruptIdleWorkers(boolean onlyOne) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            for (Worker w : workers) {\n                Thread t = w.thread;\n                if (!t.isInterrupted() && w.tryLock()) {\n                    try {\n                        t.interrupt();\n                    } catch (SecurityException ignore) {\n                    } finally {\n                        w.unlock();\n                    }\n                }\n                if (onlyOne)\n                    break;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Common form of interruptIdleWorkers, to avoid having to\n     * remember what the boolean argument means.\n     */\n    private void interruptIdleWorkers() {\n        interruptIdleWorkers(false);\n    }\n\n    private static final boolean ONLY_ONE = true;\n\n    /*\n     * Misc utilities, most of which are also exported to\n     * ScheduledThreadPoolExecutor\n     */\n\n    /**\n     * Invokes the rejected execution handler for the given command.\n     * Package-protected for use by ScheduledThreadPoolExecutor.\n     */\n    final void reject(Runnable command) {\n        handler.rejectedExecution(command, this);\n    }\n\n    /**\n     * Performs any further cleanup following run state transition on\n     * invocation of shutdown.  A no-op here, but used by\n     * ScheduledThreadPoolExecutor to cancel delayed tasks.\n     */\n    void onShutdown() {\n    }\n\n    /**\n     * Drains the task queue into a new list, normally using\n     * drainTo. But if the queue is a DelayQueue or any other kind of\n     * queue for which poll or drainTo may fail to remove some\n     * elements, it deletes them one by one.\n     */\n    private List<Runnable> drainQueue() {\n        BlockingQueue<Runnable> q = workQueue;\n        ArrayList<Runnable> taskList = new ArrayList<>();\n        q.drainTo(taskList);\n        if (!q.isEmpty()) {\n            for (Runnable r : q.toArray(new Runnable[0])) {\n                if (q.remove(r))\n                    taskList.add(r);\n            }\n        }\n        return taskList;\n    }\n\n    /*\n     * Methods for creating, running and cleaning up after workers\n     */\n\n    /**\n     * Checks if a new worker can be added with respect to current\n     * pool state and the given bound (either core or maximum). If so,\n     * the worker count is adjusted accordingly, and, if possible, a\n     * new worker is created and started, running firstTask as its\n     * first task. This method returns false if the pool is stopped or\n     * eligible to shut down. It also returns false if the thread\n     * factory fails to create a thread when asked.  If the thread\n     * creation fails, either due to the thread factory returning\n     * null, or due to an exception (typically OutOfMemoryError in\n     * Thread.start()), we roll back cleanly.\n     *\n     * @param firstTask the task the new thread should run first (or\n     * null if none). Workers are created with an initial first task\n     * (in method execute()) to bypass queuing when there are fewer\n     * than corePoolSize threads (in which case we always start one),\n     * or when the queue is full (in which case we must bypass queue).\n     * Initially idle threads are usually created via\n     * prestartCoreThread or to replace other dying workers.\n     *\n     * @param core if true use corePoolSize as bound, else\n     * maximumPoolSize. (A boolean indicator is used here rather than a\n     * value to ensure reads of fresh values after checking other pool\n     * state).\n     * @return true if successful\n     */\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n    /**\n     * Rolls back the worker thread creation.\n     * - removes worker from workers, if present\n     * - decrements worker count\n     * - rechecks for termination, in case the existence of this\n     *   worker was holding up termination\n     */\n    private void addWorkerFailed(Worker w) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            if (w != null)\n                workers.remove(w);\n            decrementWorkerCount();\n            tryTerminate();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Performs cleanup and bookkeeping for a dying worker. Called\n     * only from worker threads. Unless completedAbruptly is set,\n     * assumes that workerCount has already been adjusted to account\n     * for exit.  This method removes thread from worker set, and\n     * possibly terminates the pool or replaces the worker if either\n     * it exited due to user task exception or if fewer than\n     * corePoolSize workers are running or queue is non-empty but\n     * there are no workers.\n     *\n     * @param w the worker\n     * @param completedAbruptly if the worker died due to user exception\n     */\n    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    /**\n     * Performs blocking or timed wait for a task, depending on\n     * current configuration settings, or returns null if this worker\n     * must exit because of any of:\n     * 1. There are more than maximumPoolSize workers (due to\n     *    a call to setMaximumPoolSize).\n     * 2. The pool is stopped.\n     * 3. The pool is shutdown and the queue is empty.\n     * 4. This worker timed out waiting for a task, and timed-out\n     *    workers are subject to termination (that is,\n     *    {@code allowCoreThreadTimeOut || workerCount > corePoolSize})\n     *    both before and after the timed wait, and if the queue is\n     *    non-empty, this worker is not the last thread in the pool.\n     *\n     * @return task, or null if the worker must exit, in which case\n     *         workerCount is decremented\n     */\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n    /**\n     * Main worker run loop.  Repeatedly gets tasks from queue and\n     * executes them, while coping with a number of issues:\n     *\n     * 1. We may start out with an initial task, in which case we\n     * don't need to get the first one. Otherwise, as long as pool is\n     * running, we get tasks from getTask. If it returns null then the\n     * worker exits due to changed pool state or configuration\n     * parameters.  Other exits result from exception throws in\n     * external code, in which case completedAbruptly holds, which\n     * usually leads processWorkerExit to replace this thread.\n     *\n     * 2. Before running any task, the lock is acquired to prevent\n     * other pool interrupts while the task is executing, and then we\n     * ensure that unless pool is stopping, this thread does not have\n     * its interrupt set.\n     *\n     * 3. Each task run is preceded by a call to beforeExecute, which\n     * might throw an exception, in which case we cause thread to die\n     * (breaking loop with completedAbruptly true) without processing\n     * the task.\n     *\n     * 4. Assuming beforeExecute completes normally, we run the task,\n     * gathering any of its thrown exceptions to send to afterExecute.\n     * We separately handle RuntimeException, Error (both of which the\n     * specs guarantee that we trap) and arbitrary Throwables.\n     * Because we cannot rethrow Throwables within Runnable.run, we\n     * wrap them within Errors on the way out (to the thread's\n     * UncaughtExceptionHandler).  Any thrown exception also\n     * conservatively causes thread to die.\n     *\n     * 5. After task.run completes, we call afterExecute, which may\n     * also throw an exception, which will also cause thread to\n     * die. According to JLS Sec 14.20, this exception is the one that\n     * will be in effect even if task.run throws.\n     *\n     * The net effect of the exception mechanics is that afterExecute\n     * and the thread's UncaughtExceptionHandler have as accurate\n     * information as we can provide about any problems encountered by\n     * user code.\n     *\n     * @param w the worker\n     */\n    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    // Public constructors and methods\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters, the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}\n     * and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * <p>It may be more convenient to use one of the {@link Executors}\n     * factory methods instead of this general purpose constructor.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             threadFactory, defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              RejectedExecutionHandler handler) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), handler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory,\n                              RejectedExecutionHandler handler) {\n        if (corePoolSize < 0 ||\n            maximumPoolSize <= 0 ||\n            maximumPoolSize < corePoolSize ||\n            keepAliveTime < 0)\n            throw new IllegalArgumentException();\n        if (workQueue == null || threadFactory == null || handler == null)\n            throw new NullPointerException();\n        this.corePoolSize = corePoolSize;\n        this.maximumPoolSize = maximumPoolSize;\n        this.workQueue = workQueue;\n        this.keepAliveTime = unit.toNanos(keepAliveTime);\n        this.threadFactory = threadFactory;\n        this.handler = handler;\n    }\n\n    /**\n     * Executes the given task sometime in the future.  The task\n     * may execute in a new thread or in an existing pooled thread.\n     *\n     * If the task cannot be submitted for execution, either because this\n     * executor has been shutdown or because its capacity has been reached,\n     * the task is handled by the current {@link RejectedExecutionHandler}.\n     *\n     * @param command the task to execute\n     * @throws RejectedExecutionException at discretion of\n     *         {@code RejectedExecutionHandler}, if the task\n     *         cannot be accepted for execution\n     * @throws NullPointerException if {@code command} is null\n     */\n    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n\n    /**\n     * Initiates an orderly shutdown in which previously submitted\n     * tasks are executed, but no new tasks will be accepted.\n     * Invocation has no additional effect if already shut down.\n     *\n     * <p>This method does not wait for previously submitted tasks to\n     * complete execution.  Use {@link #awaitTermination awaitTermination}\n     * to do that.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public void shutdown() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(SHUTDOWN);\n            interruptIdleWorkers();\n            onShutdown(); // hook for ScheduledThreadPoolExecutor\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n    }\n\n    /**\n     * Attempts to stop all actively executing tasks, halts the\n     * processing of waiting tasks, and returns a list of the tasks\n     * that were awaiting execution. These tasks are drained (removed)\n     * from the task queue upon return from this method.\n     *\n     * <p>This method does not wait for actively executing tasks to\n     * terminate.  Use {@link #awaitTermination awaitTermination} to\n     * do that.\n     *\n     * <p>There are no guarantees beyond best-effort attempts to stop\n     * processing actively executing tasks.  This implementation\n     * interrupts tasks via {@link Thread#interrupt}; any task that\n     * fails to respond to interrupts may never terminate.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public List<Runnable> shutdownNow() {\n        List<Runnable> tasks;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(STOP);\n            interruptWorkers();\n            tasks = drainQueue();\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n        return tasks;\n    }\n\n    public boolean isShutdown() {\n        return runStateAtLeast(ctl.get(), SHUTDOWN);\n    }\n\n    /** Used by ScheduledThreadPoolExecutor. */\n    boolean isStopped() {\n        return runStateAtLeast(ctl.get(), STOP);\n    }\n\n    /**\n     * Returns true if this executor is in the process of terminating\n     * after {@link #shutdown} or {@link #shutdownNow} but has not\n     * completely terminated.  This method may be useful for\n     * debugging. A return of {@code true} reported a sufficient\n     * period after shutdown may indicate that submitted tasks have\n     * ignored or suppressed interruption, causing this executor not\n     * to properly terminate.\n     *\n     * @return {@code true} if terminating but not yet terminated\n     */\n    public boolean isTerminating() {\n        int c = ctl.get();\n        return runStateAtLeast(c, SHUTDOWN) && runStateLessThan(c, TERMINATED);\n    }\n\n    public boolean isTerminated() {\n        return runStateAtLeast(ctl.get(), TERMINATED);\n    }\n\n    public boolean awaitTermination(long timeout, TimeUnit unit)\n        throws InterruptedException {\n        long nanos = unit.toNanos(timeout);\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            while (runStateLessThan(ctl.get(), TERMINATED)) {\n                if (nanos <= 0L)\n                    return false;\n                nanos = termination.awaitNanos(nanos);\n            }\n            return true;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    // Override without \"throws Throwable\" for compatibility with subclasses\n    // whose finalize method invokes super.finalize() (as is recommended).\n    // Before JDK 11, finalize() had a non-empty method body.\n\n    /**\n     * @implNote Previous versions of this class had a finalize method\n     * that shut down this executor, but in this version, finalize\n     * does nothing.\n     */\n    @Deprecated(since=\"9\")\n    protected void finalize() {}\n\n    /**\n     * Sets the thread factory used to create new threads.\n     *\n     * @param threadFactory the new thread factory\n     * @throws NullPointerException if threadFactory is null\n     * @see #getThreadFactory\n     */\n    public void setThreadFactory(ThreadFactory threadFactory) {\n        if (threadFactory == null)\n            throw new NullPointerException();\n        this.threadFactory = threadFactory;\n    }\n\n    /**\n     * Returns the thread factory used to create new threads.\n     *\n     * @return the current thread factory\n     * @see #setThreadFactory(ThreadFactory)\n     */\n    public ThreadFactory getThreadFactory() {\n        return threadFactory;\n    }\n\n    /**\n     * Sets a new handler for unexecutable tasks.\n     *\n     * @param handler the new handler\n     * @throws NullPointerException if handler is null\n     * @see #getRejectedExecutionHandler\n     */\n    public void setRejectedExecutionHandler(RejectedExecutionHandler handler) {\n        if (handler == null)\n            throw new NullPointerException();\n        this.handler = handler;\n    }\n\n    /**\n     * Returns the current handler for unexecutable tasks.\n     *\n     * @return the current handler\n     * @see #setRejectedExecutionHandler(RejectedExecutionHandler)\n     */\n    public RejectedExecutionHandler getRejectedExecutionHandler() {\n        return handler;\n    }\n\n    /**\n     * Sets the core number of threads.  This overrides any value set\n     * in the constructor.  If the new value is smaller than the\n     * current value, excess existing threads will be terminated when\n     * they next become idle.  If larger, new threads will, if needed,\n     * be started to execute any queued tasks.\n     *\n     * @param corePoolSize the new core size\n     * @throws IllegalArgumentException if {@code corePoolSize < 0}\n     *         or {@code corePoolSize} is greater than the {@linkplain\n     *         #getMaximumPoolSize() maximum pool size}\n     * @see #getCorePoolSize\n     */\n    public void setCorePoolSize(int corePoolSize) {\n        if (corePoolSize < 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        int delta = corePoolSize - this.corePoolSize;\n        this.corePoolSize = corePoolSize;\n        if (workerCountOf(ctl.get()) > corePoolSize)\n            interruptIdleWorkers();\n        else if (delta > 0) {\n            // We don't really know how many new threads are \"needed\".\n            // As a heuristic, prestart enough new workers (up to new\n            // core size) to handle the current number of tasks in\n            // queue, but stop if queue becomes empty while doing so.\n            int k = Math.min(delta, workQueue.size());\n            while (k-- > 0 && addWorker(null, true)) {\n                if (workQueue.isEmpty())\n                    break;\n            }\n        }\n    }\n\n    /**\n     * Returns the core number of threads.\n     *\n     * @return the core number of threads\n     * @see #setCorePoolSize\n     */\n    public int getCorePoolSize() {\n        return corePoolSize;\n    }\n\n    /**\n     * Starts a core thread, causing it to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed. This method will return {@code false}\n     * if all core threads have already been started.\n     *\n     * @return {@code true} if a thread was started\n     */\n    public boolean prestartCoreThread() {\n        return workerCountOf(ctl.get()) < corePoolSize &&\n            addWorker(null, true);\n    }\n\n    /**\n     * Same as prestartCoreThread except arranges that at least one\n     * thread is started even if corePoolSize is 0.\n     */\n    void ensurePrestart() {\n        int wc = workerCountOf(ctl.get());\n        if (wc < corePoolSize)\n            addWorker(null, true);\n        else if (wc == 0)\n            addWorker(null, false);\n    }\n\n    /**\n     * Starts all core threads, causing them to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed.\n     *\n     * @return the number of threads started\n     */\n    public int prestartAllCoreThreads() {\n        int n = 0;\n        while (addWorker(null, true))\n            ++n;\n        return n;\n    }\n\n    /**\n     * Returns true if this pool allows core threads to time out and\n     * terminate if no tasks arrive within the keepAlive time, being\n     * replaced if needed when new tasks arrive. When true, the same\n     * keep-alive policy applying to non-core threads applies also to\n     * core threads. When false (the default), core threads are never\n     * terminated due to lack of incoming tasks.\n     *\n     * @return {@code true} if core threads are allowed to time out,\n     *         else {@code false}\n     *\n     * @since 1.6\n     */\n    public boolean allowsCoreThreadTimeOut() {\n        return allowCoreThreadTimeOut;\n    }\n\n    /**\n     * Sets the policy governing whether core threads may time out and\n     * terminate if no tasks arrive within the keep-alive time, being\n     * replaced if needed when new tasks arrive. When false, core\n     * threads are never terminated due to lack of incoming\n     * tasks. When true, the same keep-alive policy applying to\n     * non-core threads applies also to core threads. To avoid\n     * continual thread replacement, the keep-alive time must be\n     * greater than zero when setting {@code true}. This method\n     * should in general be called before the pool is actively used.\n     *\n     * @param value {@code true} if should time out, else {@code false}\n     * @throws IllegalArgumentException if value is {@code true}\n     *         and the current keep-alive time is not greater than zero\n     *\n     * @since 1.6\n     */\n    public void allowCoreThreadTimeOut(boolean value) {\n        if (value && keepAliveTime <= 0)\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        if (value != allowCoreThreadTimeOut) {\n            allowCoreThreadTimeOut = value;\n            if (value)\n                interruptIdleWorkers();\n        }\n    }\n\n    /**\n     * Sets the maximum allowed number of threads. This overrides any\n     * value set in the constructor. If the new value is smaller than\n     * the current value, excess existing threads will be\n     * terminated when they next become idle.\n     *\n     * @param maximumPoolSize the new maximum\n     * @throws IllegalArgumentException if the new maximum is\n     *         less than or equal to zero, or\n     *         less than the {@linkplain #getCorePoolSize core pool size}\n     * @see #getMaximumPoolSize\n     */\n    public void setMaximumPoolSize(int maximumPoolSize) {\n        if (maximumPoolSize <= 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        this.maximumPoolSize = maximumPoolSize;\n        if (workerCountOf(ctl.get()) > maximumPoolSize)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the maximum allowed number of threads.\n     *\n     * @return the maximum allowed number of threads\n     * @see #setMaximumPoolSize\n     */\n    public int getMaximumPoolSize() {\n        return maximumPoolSize;\n    }\n\n    /**\n     * Sets the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     * This overrides any value set in the constructor.\n     *\n     * @param time the time to wait.  A time value of zero will cause\n     *        excess threads to terminate immediately after executing tasks.\n     * @param unit the time unit of the {@code time} argument\n     * @throws IllegalArgumentException if {@code time} less than zero or\n     *         if {@code time} is zero and {@code allowsCoreThreadTimeOut}\n     * @see #getKeepAliveTime(TimeUnit)\n     */\n    public void setKeepAliveTime(long time, TimeUnit unit) {\n        if (time < 0)\n            throw new IllegalArgumentException();\n        if (time == 0 && allowsCoreThreadTimeOut())\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        long keepAliveTime = unit.toNanos(time);\n        long delta = keepAliveTime - this.keepAliveTime;\n        this.keepAliveTime = keepAliveTime;\n        if (delta < 0)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     *\n     * @param unit the desired time unit of the result\n     * @return the time limit\n     * @see #setKeepAliveTime(long, TimeUnit)\n     */\n    public long getKeepAliveTime(TimeUnit unit) {\n        return unit.convert(keepAliveTime, TimeUnit.NANOSECONDS);\n    }\n\n    /* User-level queue utilities */\n\n    /**\n     * Returns the task queue used by this executor. Access to the\n     * task queue is intended primarily for debugging and monitoring.\n     * This queue may be in active use.  Retrieving the task queue\n     * does not prevent queued tasks from executing.\n     *\n     * @return the task queue\n     */\n    public BlockingQueue<Runnable> getQueue() {\n        return workQueue;\n    }\n\n    /**\n     * Removes this task from the executor's internal queue if it is\n     * present, thus causing it not to be run if it has not already\n     * started.\n     *\n     * <p>This method may be useful as one part of a cancellation\n     * scheme.  It may fail to remove tasks that have been converted\n     * into other forms before being placed on the internal queue.\n     * For example, a task entered using {@code submit} might be\n     * converted into a form that maintains {@code Future} status.\n     * However, in such cases, method {@link #purge} may be used to\n     * remove those Futures that have been cancelled.\n     *\n     * @param task the task to remove\n     * @return {@code true} if the task was removed\n     */\n    public boolean remove(Runnable task) {\n        boolean removed = workQueue.remove(task);\n        tryTerminate(); // In case SHUTDOWN and now empty\n        return removed;\n    }\n\n    /**\n     * Tries to remove from the work queue all {@link Future}\n     * tasks that have been cancelled. This method can be useful as a\n     * storage reclamation operation, that has no other impact on\n     * functionality. Cancelled tasks are never executed, but may\n     * accumulate in work queues until worker threads can actively\n     * remove them. Invoking this method instead tries to remove them now.\n     * However, this method may fail to remove tasks in\n     * the presence of interference by other threads.\n     */\n    public void purge() {\n        final BlockingQueue<Runnable> q = workQueue;\n        try {\n            Iterator<Runnable> it = q.iterator();\n            while (it.hasNext()) {\n                Runnable r = it.next();\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    it.remove();\n            }\n        } catch (ConcurrentModificationException fallThrough) {\n            // Take slow path if we encounter interference during traversal.\n            // Make copy for traversal and call remove for cancelled entries.\n            // The slow path is more likely to be O(N*N).\n            for (Object r : q.toArray())\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    q.remove(r);\n        }\n\n        tryTerminate(); // In case SHUTDOWN and now empty\n    }\n\n    /* Statistics */\n\n    /**\n     * Returns the current number of threads in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            // Remove rare and surprising possibility of\n            // isTerminated() && getPoolSize() > 0\n            return runStateAtLeast(ctl.get(), TIDYING) ? 0\n                : workers.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate number of threads that are actively\n     * executing tasks.\n     *\n     * @return the number of threads\n     */\n    public int getActiveCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            int n = 0;\n            for (Worker w : workers)\n                if (w.isLocked())\n                    ++n;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the largest number of threads that have ever\n     * simultaneously been in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getLargestPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            return largestPoolSize;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have ever been\n     * scheduled for execution. Because the states of tasks and\n     * threads may change dynamically during computation, the returned\n     * value is only an approximation.\n     *\n     * @return the number of tasks\n     */\n    public long getTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers) {\n                n += w.completedTasks;\n                if (w.isLocked())\n                    ++n;\n            }\n            return n + workQueue.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have\n     * completed execution. Because the states of tasks and threads\n     * may change dynamically during computation, the returned value\n     * is only an approximation, but one that does not ever decrease\n     * across successive calls.\n     *\n     * @return the number of tasks\n     */\n    public long getCompletedTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers)\n                n += w.completedTasks;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns a string identifying this pool, as well as its state,\n     * including indications of run state and estimated worker and\n     * task counts.\n     *\n     * @return a string identifying this pool, as well as its state\n     */\n    public String toString() {\n        long ncompleted;\n        int nworkers, nactive;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            ncompleted = completedTaskCount;\n            nactive = 0;\n            nworkers = workers.size();\n            for (Worker w : workers) {\n                ncompleted += w.completedTasks;\n                if (w.isLocked())\n                    ++nactive;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n        int c = ctl.get();\n        String runState =\n            isRunning(c) ? \"Running\" :\n            runStateAtLeast(c, TERMINATED) ? \"Terminated\" :\n            \"Shutting down\";\n        return super.toString() +\n            \"[\" + runState +\n            \", pool size = \" + nworkers +\n            \", active threads = \" + nactive +\n            \", queued tasks = \" + workQueue.size() +\n            \", completed tasks = \" + ncompleted +\n            \"]\";\n    }\n\n    /* Extension hooks */\n\n    /**\n     * Method invoked prior to executing the given Runnable in the\n     * given thread.  This method is invoked by thread {@code t} that\n     * will execute task {@code r}, and may be used to re-initialize\n     * ThreadLocals, or to perform logging.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.beforeExecute} at the end of\n     * this method.\n     *\n     * @param t the thread that will run task {@code r}\n     * @param r the task that will be executed\n     */\n    protected void beforeExecute(Thread t, Runnable r) { }\n\n    /**\n     * Method invoked upon completion of execution of the given Runnable.\n     * This method is invoked by the thread that executed the task. If\n     * non-null, the Throwable is the uncaught {@code RuntimeException}\n     * or {@code Error} that caused execution to terminate abruptly.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.afterExecute} at the\n     * beginning of this method.\n     *\n     * <p><b>Note:</b> When actions are enclosed in tasks (such as\n     * {@link FutureTask}) either explicitly or via methods such as\n     * {@code submit}, these task objects catch and maintain\n     * computational exceptions, and so they do not cause abrupt\n     * termination, and the internal exceptions are <em>not</em>\n     * passed to this method. If you would like to trap both kinds of\n     * failures in this method, you can further probe for such cases,\n     * as in this sample subclass that prints either the direct cause\n     * or the underlying exception if a task has been aborted:\n     *\n     * <pre> {@code\n     * class ExtendedExecutor extends ThreadPoolExecutor {\n     *   // ...\n     *   protected void afterExecute(Runnable r, Throwable t) {\n     *     super.afterExecute(r, t);\n     *     if (t == null\n     *         && r instanceof Future<?>\n     *         && ((Future<?>)r).isDone()) {\n     *       try {\n     *         Object result = ((Future<?>) r).get();\n     *       } catch (CancellationException ce) {\n     *         t = ce;\n     *       } catch (ExecutionException ee) {\n     *         t = ee.getCause();\n     *       } catch (InterruptedException ie) {\n     *         // ignore/reset\n     *         Thread.currentThread().interrupt();\n     *       }\n     *     }\n     *     if (t != null)\n     *       System.out.println(t);\n     *   }\n     * }}</pre>\n     *\n     * @param r the runnable that has completed\n     * @param t the exception that caused termination, or null if\n     * execution completed normally\n     */\n    protected void afterExecute(Runnable r, Throwable t) { }\n\n    /**\n     * Method invoked when the Executor has terminated.  Default\n     * implementation does nothing. Note: To properly nest multiple\n     * overridings, subclasses should generally invoke\n     * {@code super.terminated} within this method.\n     */\n    protected void terminated() { }\n\n    /* Predefined RejectedExecutionHandlers */\n\n    /**\n     * A handler for rejected tasks that runs the rejected task\n     * directly in the calling thread of the {@code execute} method,\n     * unless the executor has been shut down, in which case the task\n     * is discarded.\n     */\n    public static class CallerRunsPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code CallerRunsPolicy}.\n         */\n        public CallerRunsPolicy() { }\n\n        /**\n         * Executes task r in the caller's thread, unless the executor\n         * has been shut down, in which case the task is discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                r.run();\n            }\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that throws a\n     * {@link RejectedExecutionException}.\n     *\n     * This is the default handler for {@link ThreadPoolExecutor} and\n     * {@link ScheduledThreadPoolExecutor}.\n     */\n    public static class AbortPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates an {@code AbortPolicy}.\n         */\n        public AbortPolicy() { }\n\n        /**\n         * Always throws RejectedExecutionException.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         * @throws RejectedExecutionException always\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            throw new RejectedExecutionException(\"Task \" + r.toString() +\n                                                 \" rejected from \" +\n                                                 e.toString());\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that silently discards the\n     * rejected task.\n     */\n    public static class DiscardPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardPolicy}.\n         */\n        public DiscardPolicy() { }\n\n        /**\n         * Does nothing, which has the effect of discarding task r.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that discards the oldest unhandled\n     * request and then retries {@code execute}, unless the executor\n     * is shut down, in which case the task is discarded. This policy is\n     * rarely useful in cases where other threads may be waiting for\n     * tasks to terminate, or failures must be recorded. Instead consider\n     * using a handler of the form:\n     * <pre> {@code\n     * new RejectedExecutionHandler() {\n     *   public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n     *     Runnable dropped = e.getQueue().poll();\n     *     if (dropped instanceof Future<?>) {\n     *       ((Future<?>)dropped).cancel(false);\n     *       // also consider logging the failure\n     *     }\n     *     e.execute(r);  // retry\n     * }}}</pre>\n     */\n    public static class DiscardOldestPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardOldestPolicy} for the given executor.\n         */\n        public DiscardOldestPolicy() { }\n\n        /**\n         * Obtains and ignores the next task that the executor\n         * would otherwise execute, if one is immediately available,\n         * and then retries execution of task r, unless the executor\n         * is shut down, in which case task r is instead discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                e.getQueue().poll();\n                e.execute(r);\n            }\n        }\n    }\n}\n",
			"rawText": "/*\n * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.\n *\n * This code is free software; you can redistribute it and/or modify it\n * under the terms of the GNU General Public License version 2 only, as\n * published by the Free Software Foundation.  Oracle designates this\n * particular file as subject to the \"Classpath\" exception as provided\n * by Oracle in the LICENSE file that accompanied this code.\n *\n * This code is distributed in the hope that it will be useful, but WITHOUT\n * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or\n * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License\n * version 2 for more details (a copy is included in the LICENSE file that\n * accompanied this code).\n *\n * You should have received a copy of the GNU General Public License version\n * 2 along with this work; if not, write to the Free Software Foundation,\n * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.\n *\n * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA\n * or visit www.oracle.com if you need additional information or have any\n * questions.\n */\n\n/*\n * This file is available under and governed by the GNU General Public\n * License version 2 only, as published by the Free Software Foundation.\n * However, the following notice accompanied the original version of this\n * file:\n *\n * Written by Doug Lea with assistance from members of JCP JSR-166\n * Expert Group and released to the public domain, as explained at\n * http://creativecommons.org/publicdomain/zero/1.0/\n */\n\npackage java.util.concurrent;\n\nimport java.util.ArrayList;\nimport java.util.ConcurrentModificationException;\nimport java.util.HashSet;\nimport java.util.Iterator;\nimport java.util.List;\nimport java.util.concurrent.atomic.AtomicInteger;\nimport java.util.concurrent.locks.AbstractQueuedSynchronizer;\nimport java.util.concurrent.locks.Condition;\nimport java.util.concurrent.locks.ReentrantLock;\n\n/**\n * An {@link ExecutorService} that executes each submitted task using\n * one of possibly several pooled threads, normally configured\n * using {@link Executors} factory methods.\n *\n * <p>Thread pools address two different problems: they usually\n * provide improved performance when executing large numbers of\n * asynchronous tasks, due to reduced per-task invocation overhead,\n * and they provide a means of bounding and managing the resources,\n * including threads, consumed when executing a collection of tasks.\n * Each {@code ThreadPoolExecutor} also maintains some basic\n * statistics, such as the number of completed tasks.\n *\n * <p>To be useful across a wide range of contexts, this class\n * provides many adjustable parameters and extensibility\n * hooks. However, programmers are urged to use the more convenient\n * {@link Executors} factory methods {@link\n * Executors#newCachedThreadPool} (unbounded thread pool, with\n * automatic thread reclamation), {@link Executors#newFixedThreadPool}\n * (fixed size thread pool) and {@link\n * Executors#newSingleThreadExecutor} (single background thread), that\n * preconfigure settings for the most common usage\n * scenarios. Otherwise, use the following guide when manually\n * configuring and tuning this class:\n *\n * <dl>\n *\n * <dt>Core and maximum pool sizes</dt>\n *\n * <dd>A {@code ThreadPoolExecutor} will automatically adjust the\n * pool size (see {@link #getPoolSize})\n * according to the bounds set by\n * corePoolSize (see {@link #getCorePoolSize}) and\n * maximumPoolSize (see {@link #getMaximumPoolSize}).\n *\n * When a new task is submitted in method {@link #execute(Runnable)},\n * if fewer than corePoolSize threads are running, a new thread is\n * created to handle the request, even if other worker threads are\n * idle.  Else if fewer than maximumPoolSize threads are running, a\n * new thread will be created to handle the request only if the queue\n * is full.  By setting corePoolSize and maximumPoolSize the same, you\n * create a fixed-size thread pool. By setting maximumPoolSize to an\n * essentially unbounded value such as {@code Integer.MAX_VALUE}, you\n * allow the pool to accommodate an arbitrary number of concurrent\n * tasks. Most typically, core and maximum pool sizes are set only\n * upon construction, but they may also be changed dynamically using\n * {@link #setCorePoolSize} and {@link #setMaximumPoolSize}. </dd>\n *\n * <dt>On-demand construction</dt>\n *\n * <dd>By default, even core threads are initially created and\n * started only when new tasks arrive, but this can be overridden\n * dynamically using method {@link #prestartCoreThread} or {@link\n * #prestartAllCoreThreads}.  You probably want to prestart threads if\n * you construct the pool with a non-empty queue. </dd>\n *\n * <dt>Creating new threads</dt>\n *\n * <dd>New threads are created using a {@link ThreadFactory}.  If not\n * otherwise specified, a {@link Executors#defaultThreadFactory} is\n * used, that creates threads to all be in the same {@link\n * ThreadGroup} and with the same {@code NORM_PRIORITY} priority and\n * non-daemon status. By supplying a different ThreadFactory, you can\n * alter the thread's name, thread group, priority, daemon status,\n * etc. If a {@code ThreadFactory} fails to create a thread when asked\n * by returning null from {@code newThread}, the executor will\n * continue, but might not be able to execute any tasks. Threads\n * should possess the \"modifyThread\" {@code RuntimePermission}. If\n * worker threads or other threads using the pool do not possess this\n * permission, service may be degraded: configuration changes may not\n * take effect in a timely manner, and a shutdown pool may remain in a\n * state in which termination is possible but not completed.</dd>\n *\n * <dt>Keep-alive times</dt>\n *\n * <dd>If the pool currently has more than corePoolSize threads,\n * excess threads will be terminated if they have been idle for more\n * than the keepAliveTime (see {@link #getKeepAliveTime(TimeUnit)}).\n * This provides a means of reducing resource consumption when the\n * pool is not being actively used. If the pool becomes more active\n * later, new threads will be constructed. This parameter can also be\n * changed dynamically using method {@link #setKeepAliveTime(long,\n * TimeUnit)}.  Using a value of {@code Long.MAX_VALUE} {@link\n * TimeUnit#NANOSECONDS} effectively disables idle threads from ever\n * terminating prior to shut down. By default, the keep-alive policy\n * applies only when there are more than corePoolSize threads, but\n * method {@link #allowCoreThreadTimeOut(boolean)} can be used to\n * apply this time-out policy to core threads as well, so long as the\n * keepAliveTime value is non-zero. </dd>\n *\n * <dt>Queuing</dt>\n *\n * <dd>Any {@link BlockingQueue} may be used to transfer and hold\n * submitted tasks.  The use of this queue interacts with pool sizing:\n *\n * <ul>\n *\n * <li>If fewer than corePoolSize threads are running, the Executor\n * always prefers adding a new thread\n * rather than queuing.\n *\n * <li>If corePoolSize or more threads are running, the Executor\n * always prefers queuing a request rather than adding a new\n * thread.\n *\n * <li>If a request cannot be queued, a new thread is created unless\n * this would exceed maximumPoolSize, in which case, the task will be\n * rejected.\n *\n * </ul>\n *\n * There are three general strategies for queuing:\n * <ol>\n *\n * <li><em> Direct handoffs.</em> A good default choice for a work\n * queue is a {@link SynchronousQueue} that hands off tasks to threads\n * without otherwise holding them. Here, an attempt to queue a task\n * will fail if no threads are immediately available to run it, so a\n * new thread will be constructed. This policy avoids lockups when\n * handling sets of requests that might have internal dependencies.\n * Direct handoffs generally require unbounded maximumPoolSizes to\n * avoid rejection of new submitted tasks. This in turn admits the\n * possibility of unbounded thread growth when commands continue to\n * arrive on average faster than they can be processed.\n *\n * <li><em> Unbounded queues.</em> Using an unbounded queue (for\n * example a {@link LinkedBlockingQueue} without a predefined\n * capacity) will cause new tasks to wait in the queue when all\n * corePoolSize threads are busy. Thus, no more than corePoolSize\n * threads will ever be created. (And the value of the maximumPoolSize\n * therefore doesn't have any effect.)  This may be appropriate when\n * each task is completely independent of others, so tasks cannot\n * affect each others execution; for example, in a web page server.\n * While this style of queuing can be useful in smoothing out\n * transient bursts of requests, it admits the possibility of\n * unbounded work queue growth when commands continue to arrive on\n * average faster than they can be processed.\n *\n * <li><em>Bounded queues.</em> A bounded queue (for example, an\n * {@link ArrayBlockingQueue}) helps prevent resource exhaustion when\n * used with finite maximumPoolSizes, but can be more difficult to\n * tune and control.  Queue sizes and maximum pool sizes may be traded\n * off for each other: Using large queues and small pools minimizes\n * CPU usage, OS resources, and context-switching overhead, but can\n * lead to artificially low throughput.  If tasks frequently block (for\n * example if they are I/O bound), a system may be able to schedule\n * time for more threads than you otherwise allow. Use of small queues\n * generally requires larger pool sizes, which keeps CPUs busier but\n * may encounter unacceptable scheduling overhead, which also\n * decreases throughput.\n *\n * </ol>\n *\n * </dd>\n *\n * <dt>Rejected tasks</dt>\n *\n * <dd>New tasks submitted in method {@link #execute(Runnable)} will be\n * <em>rejected</em> when the Executor has been shut down, and also when\n * the Executor uses finite bounds for both maximum threads and work queue\n * capacity, and is saturated.  In either case, the {@code execute} method\n * invokes the {@link\n * RejectedExecutionHandler#rejectedExecution(Runnable, ThreadPoolExecutor)}\n * method of its {@link RejectedExecutionHandler}.  Four predefined handler\n * policies are provided:\n *\n * <ol>\n *\n * <li>In the default {@link ThreadPoolExecutor.AbortPolicy}, the handler\n * throws a runtime {@link RejectedExecutionException} upon rejection.\n *\n * <li>In {@link ThreadPoolExecutor.CallerRunsPolicy}, the thread\n * that invokes {@code execute} itself runs the task. This provides a\n * simple feedback control mechanism that will slow down the rate that\n * new tasks are submitted.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardPolicy}, a task that cannot\n * be executed is simply dropped. This policy is designed only for\n * those rare cases in which task completion is never relied upon.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardOldestPolicy}, if the\n * executor is not shut down, the task at the head of the work queue\n * is dropped, and then execution is retried (which can fail again,\n * causing this to be repeated.) This policy is rarely acceptable.  In\n * nearly all cases, you should also cancel the task to cause an\n * exception in any component waiting for its completion, and/or log\n * the failure, as illustrated in {@link\n * ThreadPoolExecutor.DiscardOldestPolicy} documentation.\n *\n * </ol>\n *\n * It is possible to define and use other kinds of {@link\n * RejectedExecutionHandler} classes. Doing so requires some care\n * especially when policies are designed to work only under particular\n * capacity or queuing policies. </dd>\n *\n * <dt>Hook methods</dt>\n *\n * <dd>This class provides {@code protected} overridable\n * {@link #beforeExecute(Thread, Runnable)} and\n * {@link #afterExecute(Runnable, Throwable)} methods that are called\n * before and after execution of each task.  These can be used to\n * manipulate the execution environment; for example, reinitializing\n * ThreadLocals, gathering statistics, or adding log entries.\n * Additionally, method {@link #terminated} can be overridden to perform\n * any special processing that needs to be done once the Executor has\n * fully terminated.\n *\n * <p>If hook, callback, or BlockingQueue methods throw exceptions,\n * internal worker threads may in turn fail, abruptly terminate, and\n * possibly be replaced.</dd>\n *\n * <dt>Queue maintenance</dt>\n *\n * <dd>Method {@link #getQueue()} allows access to the work queue\n * for purposes of monitoring and debugging.  Use of this method for\n * any other purpose is strongly discouraged.  Two supplied methods,\n * {@link #remove(Runnable)} and {@link #purge} are available to\n * assist in storage reclamation when large numbers of queued tasks\n * become cancelled.</dd>\n *\n * <dt>Reclamation</dt>\n *\n * <dd>A pool that is no longer referenced in a program <em>AND</em>\n * has no remaining threads may be reclaimed (garbage collected)\n * without being explicitly shutdown. You can configure a pool to\n * allow all unused threads to eventually die by setting appropriate\n * keep-alive times, using a lower bound of zero core threads and/or\n * setting {@link #allowCoreThreadTimeOut(boolean)}.  </dd>\n *\n * </dl>\n *\n * <p><b>Extension example.</b> Most extensions of this class\n * override one or more of the protected hook methods. For example,\n * here is a subclass that adds a simple pause/resume feature:\n *\n * <pre> {@code\n * class PausableThreadPoolExecutor extends ThreadPoolExecutor {\n *   private boolean isPaused;\n *   private ReentrantLock pauseLock = new ReentrantLock();\n *   private Condition unpaused = pauseLock.newCondition();\n *\n *   public PausableThreadPoolExecutor(...) { super(...); }\n *\n *   protected void beforeExecute(Thread t, Runnable r) {\n *     super.beforeExecute(t, r);\n *     pauseLock.lock();\n *     try {\n *       while (isPaused) unpaused.await();\n *     } catch (InterruptedException ie) {\n *       t.interrupt();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void pause() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = true;\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void resume() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = false;\n *       unpaused.signalAll();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n * }}</pre>\n *\n * @since 1.5\n * @author Doug Lea\n */\npublic class ThreadPoolExecutor extends AbstractExecutorService {\n    /**\n     * The main pool control state, ctl, is an atomic integer packing\n     * two conceptual fields\n     *   workerCount, indicating the effective number of threads\n     *   runState,    indicating whether running, shutting down etc\n     *\n     * In order to pack them into one int, we limit workerCount to\n     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2\n     * billion) otherwise representable. If this is ever an issue in\n     * the future, the variable can be changed to be an AtomicLong,\n     * and the shift/mask constants below adjusted. But until the need\n     * arises, this code is a bit faster and simpler using an int.\n     *\n     * The workerCount is the number of workers that have been\n     * permitted to start and not permitted to stop.  The value may be\n     * transiently different from the actual number of live threads,\n     * for example when a ThreadFactory fails to create a thread when\n     * asked, and when exiting threads are still performing\n     * bookkeeping before terminating. The user-visible pool size is\n     * reported as the current size of the workers set.\n     *\n     * The runState provides the main lifecycle control, taking on values:\n     *\n     *   RUNNING:  Accept new tasks and process queued tasks\n     *   SHUTDOWN: Don't accept new tasks, but process queued tasks\n     *   STOP:     Don't accept new tasks, don't process queued tasks,\n     *             and interrupt in-progress tasks\n     *   TIDYING:  All tasks have terminated, workerCount is zero,\n     *             the thread transitioning to state TIDYING\n     *             will run the terminated() hook method\n     *   TERMINATED: terminated() has completed\n     *\n     * The numerical order among these values matters, to allow\n     * ordered comparisons. The runState monotonically increases over\n     * time, but need not hit each state. The transitions are:\n     *\n     * RUNNING -> SHUTDOWN\n     *    On invocation of shutdown()\n     * (RUNNING or SHUTDOWN) -> STOP\n     *    On invocation of shutdownNow()\n     * SHUTDOWN -> TIDYING\n     *    When both queue and pool are empty\n     * STOP -> TIDYING\n     *    When pool is empty\n     * TIDYING -> TERMINATED\n     *    When the terminated() hook method has completed\n     *\n     * Threads waiting in awaitTermination() will return when the\n     * state reaches TERMINATED.\n     *\n     * Detecting the transition from SHUTDOWN to TIDYING is less\n     * straightforward than you'd like because the queue may become222\n     * empty after non-empty and vice versa during SHUTDOWN state, but\n     * we can only terminate if, after seeing that it is empty, we see\n     * that workerCount is 0 (which sometimes entails a recheck -- see\n     * below).\n     */\n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n    /*\n     * Bit field accessors that don't require unpacking ctl.\n     * These depend on the bit layout and on workerCount being never negative.\n     */\n\n    private static boolean runStateLessThan(int c, int s) {\n        return c < s;\n    }\n\n    private static boolean runStateAtLeast(int c, int s) {\n        return c >= s;\n    }\n\n    private static boolean isRunning(int c) {\n        return c < SHUTDOWN;\n    }\n\n    /**\n     * Attempts to CAS-increment the workerCount field of ctl.\n     */\n    private boolean compareAndIncrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect + 1);\n    }\n\n    /**\n     * Attempts to CAS-decrement the workerCount field of ctl.\n     */\n    private boolean compareAndDecrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect - 1);\n    }\n\n    /**\n     * Decrements the workerCount field of ctl. This is called only on\n     * abrupt termination of a thread (see processWorkerExit). Other\n     * decrements are performed within getTask.\n     */\n    private void decrementWorkerCount() {\n        ctl.addAndGet(-1);\n    }\n\n    /**\n     * The queue used for holding tasks and handing off to worker\n     * threads.  We do not require that workQueue.poll() returning\n     * null necessarily means that workQueue.isEmpty(), so rely\n     * solely on isEmpty to see if the queue is empty (which we must\n     * do for example when deciding whether to transition from\n     * SHUTDOWN to TIDYING).  This accommodates special-purpose\n     * queues such as DelayQueues for which poll() is allowed to\n     * return null even if it may later return non-null when delays\n     * expire.\n     */\n    private final BlockingQueue<Runnable> workQueue;\n\n    /**\n     * Lock held on access to workers set and related bookkeeping.\n     * While we could use a concurrent set of some sort, it turns out\n     * to be generally preferable to use a lock. Among the reasons is\n     * that this serializes interruptIdleWorkers, which avoids\n     * unnecessary interrupt storms, especially during shutdown.\n     * Otherwise exiting threads would concurrently interrupt those\n     * that have not yet interrupted. It also simplifies some of the\n     * associated statistics bookkeeping of largestPoolSize etc. We\n     * also hold mainLock on shutdown and shutdownNow, for the sake of\n     * ensuring workers set is stable while separately checking\n     * permission to interrupt and actually interrupting.\n     */\n    private final ReentrantLock mainLock = new ReentrantLock();\n\n    /**\n     * Set containing all worker threads in pool. Accessed only when\n     * holding mainLock.\n     */\n    private final HashSet<Worker> workers = new HashSet<>();\n\n    /**\n     * Wait condition to support awaitTermination.\n     */\n    private final Condition termination = mainLock.newCondition();\n\n    /**\n     * Tracks largest attained pool size. Accessed only under\n     * mainLock.\n     */\n    private int largestPoolSize;\n\n    /**\n     * Counter for completed tasks. Updated only on termination of\n     * worker threads. Accessed only under mainLock.\n     */\n    private long completedTaskCount;\n\n    /*\n     * All user control parameters are declared as volatiles so that\n     * ongoing actions are based on freshest values, but without need\n     * for locking, since no internal invariants depend on them\n     * changing synchronously with respect to other actions.\n     */\n\n    /**\n     * Factory for new threads. All threads are created using this\n     * factory (via method addWorker).  All callers must be prepared\n     * for addWorker to fail, which may reflect a system or user's\n     * policy limiting the number of threads.  Even though it is not\n     * treated as an error, failure to create threads may result in\n     * new tasks being rejected or existing ones remaining stuck in\n     * the queue.\n     *\n     * We go further and preserve pool invariants even in the face of\n     * errors such as OutOfMemoryError, that might be thrown while\n     * trying to create threads.  Such errors are rather common due to\n     * the need to allocate a native stack in Thread.start, and users\n     * will want to perform clean pool shutdown to clean up.  There\n     * will likely be enough memory available for the cleanup code to\n     * complete without encountering yet another OutOfMemoryError.\n     */\n    private volatile ThreadFactory threadFactory;\n\n    /**\n     * Handler called when saturated or shutdown in execute.\n     */\n    private volatile RejectedExecutionHandler handler;\n\n    /**\n     * Timeout in nanoseconds for idle threads waiting for work.\n     * Threads use this timeout when there are more than corePoolSize\n     * present or if allowCoreThreadTimeOut. Otherwise they wait\n     * forever for new work.\n     */\n    private volatile long keepAliveTime;\n\n    /**\n     * If false (default), core threads stay alive even when idle.\n     * If true, core threads use keepAliveTime to time out waiting\n     * for work.\n     */\n    private volatile boolean allowCoreThreadTimeOut;\n\n    /**\n     * Core pool size is the minimum number of workers to keep alive\n     * (and not allow to time out etc) unless allowCoreThreadTimeOut\n     * is set, in which case the minimum is zero.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code corePoolSize & COUNT_MASK}.\n     */\n    private volatile int corePoolSize;\n\n    /**\n     * Maximum pool size.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code maximumPoolSize & COUNT_MASK}.\n     */\n    private volatile int maximumPoolSize;\n\n    /**\n     * The default rejected execution handler.\n     */\n    private static final RejectedExecutionHandler defaultHandler =\n        new AbortPolicy();\n\n    /**\n     * Permission required for callers of shutdown and shutdownNow.\n     * We additionally require (see checkShutdownAccess) that callers\n     * have permission to actually interrupt threads in the worker set\n     * (as governed by Thread.interrupt, which relies on\n     * ThreadGroup.checkAccess, which in turn relies on\n     * SecurityManager.checkAccess). Shutdowns are attempted only if\n     * these checks pass.\n     *\n     * All actual invocations of Thread.interrupt (see\n     * interruptIdleWorkers and interruptWorkers) ignore\n     * SecurityExceptions, meaning that the attempted interrupts\n     * silently fail. In the case of shutdown, they should not fail\n     * unless the SecurityManager has inconsistent policies, sometimes\n     * allowing access to a thread and sometimes not. In such cases,\n     * failure to actually interrupt threads may disable or delay full\n     * termination. Other uses of interruptIdleWorkers are advisory,\n     * and failure to actually interrupt will merely delay response to\n     * configuration changes so is not handled exceptionally.\n     */\n    private static final RuntimePermission shutdownPerm =\n        new RuntimePermission(\"modifyThread\");\n\n    /**\n     * Class Worker mainly maintains interrupt control state for\n     * threads running tasks, along with other minor bookkeeping.\n     * This class opportunistically extends AbstractQueuedSynchronizer\n     * to simplify acquiring and releasing a lock surrounding each\n     * task execution.  This protects against interrupts that are\n     * intended to wake up a worker thread waiting for a task from\n     * instead interrupting a task being run.  We implement a simple\n     * non-reentrant mutual exclusion lock rather than use\n     * ReentrantLock because we do not want worker tasks to be able to\n     * reacquire the lock when they invoke pool control methods like\n     * setCorePoolSize.  Additionally, to suppress interrupts until\n     * the thread actually starts running tasks, we initialize lock\n     * state to a negative value, and clear it upon start (in\n     * runWorker).\n     */\n    private final class Worker\n        extends AbstractQueuedSynchronizer\n        implements Runnable\n    {\n        /**\n         * This class will never be serialized, but we provide a\n         * serialVersionUID to suppress a javac warning.\n         */\n        private static final long serialVersionUID = 6138294804551838833L;\n\n        /** Thread this worker is running in.  Null if factory fails. */\n        @SuppressWarnings(\"serial\") // Unlikely to be serializable\n        final Thread thread;\n        /** Initial task to run.  Possibly null. */\n        @SuppressWarnings(\"serial\") // Not statically typed as Serializable\n        Runnable firstTask;\n        /** Per-thread task counter */\n        volatile long completedTasks;\n\n        // TODO: switch to AbstractQueuedLongSynchronizer and move\n        // completedTasks into the lock word.\n\n        /**\n         * Creates with given first task and thread from ThreadFactory.\n         * @param firstTask the first task (null if none)\n         */\n        Worker(Runnable firstTask) {\n            setState(-1); // inhibit interrupts until runWorker\n            this.firstTask = firstTask;\n            this.thread = getThreadFactory().newThread(this);\n        }\n\n        /** Delegates main run loop to outer runWorker. */\n        public void run() {\n            runWorker(this);\n        }\n\n        // Lock methods\n        //\n        // The value 0 represents the unlocked state.\n        // The value 1 represents the locked state.\n\n        protected boolean isHeldExclusively() {\n            return getState() != 0;\n        }\n\n        protected boolean tryAcquire(int unused) {\n            if (compareAndSetState(0, 1)) {\n                setExclusiveOwnerThread(Thread.currentThread());\n                return true;\n            }\n            return false;\n        }\n\n        protected boolean tryRelease(int unused) {\n            setExclusiveOwnerThread(null);\n            setState(0);\n            return true;\n        }\n\n        public void lock()        { acquire(1); }\n        public boolean tryLock()  { return tryAcquire(1); }\n        public void unlock()      { release(1); }\n        public boolean isLocked() { return isHeldExclusively(); }\n\n        void interruptIfStarted() {\n            Thread t;\n            if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {\n                try {\n                    t.interrupt();\n                } catch (SecurityException ignore) {\n                }\n            }\n        }\n    }\n\n    /*\n     * Methods for setting control state\n     */\n\n    /**\n     * Transitions runState to given target, or leaves it alone if\n     * already at least the given target.\n     *\n     * @param targetState the desired state, either SHUTDOWN or STOP\n     *        (but not TIDYING or TERMINATED -- use tryTerminate for that)\n     */\n    private void advanceRunState(int targetState) {\n        // assert targetState == SHUTDOWN || targetState == STOP;\n        for (;;) {\n            int c = ctl.get();\n            if (runStateAtLeast(c, targetState) ||\n                ctl.compareAndSet(c, ctlOf(targetState, workerCountOf(c))))\n                break;\n        }\n    }\n\n    /**\n     * Transitions to TERMINATED state if either (SHUTDOWN and pool\n     * and queue empty) or (STOP and pool empty).  If otherwise\n     * eligible to terminate but workerCount is nonzero, interrupts an\n     * idle worker to ensure that shutdown signals propagate. This\n     * method must be called following any action that might make\n     * termination possible -- reducing worker count or removing tasks\n     * from the queue during shutdown. The method is non-private to\n     * allow access from ScheduledThreadPoolExecutor.\n     */\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n    /*\n     * Methods for controlling interrupts to worker threads.\n     */\n\n    /**\n     * If there is a security manager, makes sure caller has\n     * permission to shut down threads in general (see shutdownPerm).\n     * If this passes, additionally makes sure the caller is allowed\n     * to interrupt each worker thread. This might not be true even if\n     * first check passed, if the SecurityManager treats some threads\n     * specially.\n     */\n    private void checkShutdownAccess() {\n        // assert mainLock.isHeldByCurrentThread();\n        @SuppressWarnings(\"removal\")\n        SecurityManager security = System.getSecurityManager();\n        if (security != null) {\n            security.checkPermission(shutdownPerm);\n            for (Worker w : workers)\n                security.checkAccess(w.thread);\n        }\n    }\n\n    /**\n     * Interrupts all threads, even if active. Ignores SecurityExceptions\n     * (in which case some threads may remain uninterrupted).\n     */\n    private void interruptWorkers() {\n        // assert mainLock.isHeldByCurrentThread();\n        for (Worker w : workers)\n            w.interruptIfStarted();\n    }\n\n    /**\n     * Interrupts threads that might be waiting for tasks (as\n     * indicated by not being locked) so they can check for\n     * termination or configuration changes. Ignores\n     * SecurityExceptions (in which case some threads may remain\n     * uninterrupted).\n     *\n     * @param onlyOne If true, interrupt at most one worker. This is\n     * called only from tryTerminate when termination is otherwise\n     * enabled but there are still other workers.  In this case, at\n     * most one waiting worker is interrupted to propagate shutdown\n     * signals in case all threads are currently waiting.\n     * Interrupting any arbitrary thread ensures that newly arriving\n     * workers since shutdown began will also eventually exit.\n     * To guarantee eventual termination, it suffices to always\n     * interrupt only one idle worker, but shutdown() interrupts all\n     * idle workers so that redundant workers exit promptly, not\n     * waiting for a straggler task to finish.\n     */\n    private void interruptIdleWorkers(boolean onlyOne) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            for (Worker w : workers) {\n                Thread t = w.thread;\n                if (!t.isInterrupted() && w.tryLock()) {\n                    try {\n                        t.interrupt();\n                    } catch (SecurityException ignore) {\n                    } finally {\n                        w.unlock();\n                    }\n                }\n                if (onlyOne)\n                    break;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Common form of interruptIdleWorkers, to avoid having to\n     * remember what the boolean argument means.\n     */\n    private void interruptIdleWorkers() {\n        interruptIdleWorkers(false);\n    }\n\n    private static final boolean ONLY_ONE = true;\n\n    /*\n     * Misc utilities, most of which are also exported to\n     * ScheduledThreadPoolExecutor\n     */\n\n    /**\n     * Invokes the rejected execution handler for the given command.\n     * Package-protected for use by ScheduledThreadPoolExecutor.\n     */\n    final void reject(Runnable command) {\n        handler.rejectedExecution(command, this);\n    }\n\n    /**\n     * Performs any further cleanup following run state transition on\n     * invocation of shutdown.  A no-op here, but used by\n     * ScheduledThreadPoolExecutor to cancel delayed tasks.\n     */\n    void onShutdown() {\n    }\n\n    /**\n     * Drains the task queue into a new list, normally using\n     * drainTo. But if the queue is a DelayQueue or any other kind of\n     * queue for which poll or drainTo may fail to remove some\n     * elements, it deletes them one by one.\n     */\n    private List<Runnable> drainQueue() {\n        BlockingQueue<Runnable> q = workQueue;\n        ArrayList<Runnable> taskList = new ArrayList<>();\n        q.drainTo(taskList);\n        if (!q.isEmpty()) {\n            for (Runnable r : q.toArray(new Runnable[0])) {\n                if (q.remove(r))\n                    taskList.add(r);\n            }\n        }\n        return taskList;\n    }\n\n    /*\n     * Methods for creating, running and cleaning up after workers\n     */\n\n    /**\n     * Checks if a new worker can be added with respect to current\n     * pool state and the given bound (either core or maximum). If so,\n     * the worker count is adjusted accordingly, and, if possible, a\n     * new worker is created and started, running firstTask as its\n     * first task. This method returns false if the pool is stopped or\n     * eligible to shut down. It also returns false if the thread\n     * factory fails to create a thread when asked.  If the thread\n     * creation fails, either due to the thread factory returning\n     * null, or due to an exception (typically OutOfMemoryError in\n     * Thread.start()), we roll back cleanly.\n     *\n     * @param firstTask the task the new thread should run first (or\n     * null if none). Workers are created with an initial first task\n     * (in method execute()) to bypass queuing when there are fewer\n     * than corePoolSize threads (in which case we always start one),\n     * or when the queue is full (in which case we must bypass queue).\n     * Initially idle threads are usually created via\n     * prestartCoreThread or to replace other dying workers.\n     *\n     * @param core if true use corePoolSize as bound, else\n     * maximumPoolSize. (A boolean indicator is used here rather than a\n     * value to ensure reads of fresh values after checking other pool\n     * state).\n     * @return true if successful\n     */\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n    /**\n     * Rolls back the worker thread creation.\n     * - removes worker from workers, if present\n     * - decrements worker count\n     * - rechecks for termination, in case the existence of this\n     *   worker was holding up termination\n     */\n    private void addWorkerFailed(Worker w) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            if (w != null)\n                workers.remove(w);\n            decrementWorkerCount();\n            tryTerminate();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Performs cleanup and bookkeeping for a dying worker. Called\n     * only from worker threads. Unless completedAbruptly is set,\n     * assumes that workerCount has already been adjusted to account\n     * for exit.  This method removes thread from worker set, and\n     * possibly terminates the pool or replaces the worker if either\n     * it exited due to user task exception or if fewer than\n     * corePoolSize workers are running or queue is non-empty but\n     * there are no workers.\n     *\n     * @param w the worker\n     * @param completedAbruptly if the worker died due to user exception\n     */\n    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    /**\n     * Performs blocking or timed wait for a task, depending on\n     * current configuration settings, or returns null if this worker\n     * must exit because of any of:\n     * 1. There are more than maximumPoolSize workers (due to\n     *    a call to setMaximumPoolSize).\n     * 2. The pool is stopped.\n     * 3. The pool is shutdown and the queue is empty.\n     * 4. This worker timed out waiting for a task, and timed-out\n     *    workers are subject to termination (that is,\n     *    {@code allowCoreThreadTimeOut || workerCount > corePoolSize})\n     *    both before and after the timed wait, and if the queue is\n     *    non-empty, this worker is not the last thread in the pool.\n     *\n     * @return task, or null if the worker must exit, in which case\n     *         workerCount is decremented\n     */\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n    /**\n     * Main worker run loop.  Repeatedly gets tasks from queue and\n     * executes them, while coping with a number of issues:\n     *\n     * 1. We may start out with an initial task, in which case we\n     * don't need to get the first one. Otherwise, as long as pool is\n     * running, we get tasks from getTask. If it returns null then the\n     * worker exits due to changed pool state or configuration\n     * parameters.  Other exits result from exception throws in\n     * external code, in which case completedAbruptly holds, which\n     * usually leads processWorkerExit to replace this thread.\n     *\n     * 2. Before running any task, the lock is acquired to prevent\n     * other pool interrupts while the task is executing, and then we\n     * ensure that unless pool is stopping, this thread does not have\n     * its interrupt set.\n     *\n     * 3. Each task run is preceded by a call to beforeExecute, which\n     * might throw an exception, in which case we cause thread to die\n     * (breaking loop with completedAbruptly true) without processing\n     * the task.\n     *\n     * 4. Assuming beforeExecute completes normally, we run the task,\n     * gathering any of its thrown exceptions to send to afterExecute.\n     * We separately handle RuntimeException, Error (both of which the\n     * specs guarantee that we trap) and arbitrary Throwables.\n     * Because we cannot rethrow Throwables within Runnable.run, we\n     * wrap them within Errors on the way out (to the thread's\n     * UncaughtExceptionHandler).  Any thrown exception also\n     * conservatively causes thread to die.\n     *\n     * 5. After task.run completes, we call afterExecute, which may\n     * also throw an exception, which will also cause thread to\n     * die. According to JLS Sec 14.20, this exception is the one that\n     * will be in effect even if task.run throws.\n     *\n     * The net effect of the exception mechanics is that afterExecute\n     * and the thread's UncaughtExceptionHandler have as accurate\n     * information as we can provide about any problems encountered by\n     * user code.\n     *\n     * @param w the worker\n     */\n    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    // Public constructors and methods\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters, the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}\n     * and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * <p>It may be more convenient to use one of the {@link Executors}\n     * factory methods instead of this general purpose constructor.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             threadFactory, defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              RejectedExecutionHandler handler) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), handler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory,\n                              RejectedExecutionHandler handler) {\n        if (corePoolSize < 0 ||\n            maximumPoolSize <= 0 ||\n            maximumPoolSize < corePoolSize ||\n            keepAliveTime < 0)\n            throw new IllegalArgumentException();\n        if (workQueue == null || threadFactory == null || handler == null)\n            throw new NullPointerException();\n        this.corePoolSize = corePoolSize;\n        this.maximumPoolSize = maximumPoolSize;\n        this.workQueue = workQueue;\n        this.keepAliveTime = unit.toNanos(keepAliveTime);\n        this.threadFactory = threadFactory;\n        this.handler = handler;\n    }\n\n    /**\n     * Executes the given task sometime in the future.  The task\n     * may execute in a new thread or in an existing pooled thread.\n     *\n     * If the task cannot be submitted for execution, either because this\n     * executor has been shutdown or because its capacity has been reached,\n     * the task is handled by the current {@link RejectedExecutionHandler}.\n     *\n     * @param command the task to execute\n     * @throws RejectedExecutionException at discretion of\n     *         {@code RejectedExecutionHandler}, if the task\n     *         cannot be accepted for execution\n     * @throws NullPointerException if {@code command} is null\n     */\n    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n\n    /**\n     * Initiates an orderly shutdown in which previously submitted\n     * tasks are executed, but no new tasks will be accepted.\n     * Invocation has no additional effect if already shut down.\n     *\n     * <p>This method does not wait for previously submitted tasks to\n     * complete execution.  Use {@link #awaitTermination awaitTermination}\n     * to do that.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public void shutdown() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(SHUTDOWN);\n            interruptIdleWorkers();\n            onShutdown(); // hook for ScheduledThreadPoolExecutor\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n    }\n\n    /**\n     * Attempts to stop all actively executing tasks, halts the\n     * processing of waiting tasks, and returns a list of the tasks\n     * that were awaiting execution. These tasks are drained (removed)\n     * from the task queue upon return from this method.\n     *\n     * <p>This method does not wait for actively executing tasks to\n     * terminate.  Use {@link #awaitTermination awaitTermination} to\n     * do that.\n     *\n     * <p>There are no guarantees beyond best-effort attempts to stop\n     * processing actively executing tasks.  This implementation\n     * interrupts tasks via {@link Thread#interrupt}; any task that\n     * fails to respond to interrupts may never terminate.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public List<Runnable> shutdownNow() {\n        List<Runnable> tasks;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(STOP);\n            interruptWorkers();\n            tasks = drainQueue();\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n        return tasks;\n    }\n\n    public boolean isShutdown() {\n        return runStateAtLeast(ctl.get(), SHUTDOWN);\n    }\n\n    /** Used by ScheduledThreadPoolExecutor. */\n    boolean isStopped() {\n        return runStateAtLeast(ctl.get(), STOP);\n    }\n\n    /**\n     * Returns true if this executor is in the process of terminating\n     * after {@link #shutdown} or {@link #shutdownNow} but has not\n     * completely terminated.  This method may be useful for\n     * debugging. A return of {@code true} reported a sufficient\n     * period after shutdown may indicate that submitted tasks have\n     * ignored or suppressed interruption, causing this executor not\n     * to properly terminate.\n     *\n     * @return {@code true} if terminating but not yet terminated\n     */\n    public boolean isTerminating() {\n        int c = ctl.get();\n        return runStateAtLeast(c, SHUTDOWN) && runStateLessThan(c, TERMINATED);\n    }\n\n    public boolean isTerminated() {\n        return runStateAtLeast(ctl.get(), TERMINATED);\n    }\n\n    public boolean awaitTermination(long timeout, TimeUnit unit)\n        throws InterruptedException {\n        long nanos = unit.toNanos(timeout);\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            while (runStateLessThan(ctl.get(), TERMINATED)) {\n                if (nanos <= 0L)\n                    return false;\n                nanos = termination.awaitNanos(nanos);\n            }\n            return true;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    // Override without \"throws Throwable\" for compatibility with subclasses\n    // whose finalize method invokes super.finalize() (as is recommended).\n    // Before JDK 11, finalize() had a non-empty method body.\n\n    /**\n     * @implNote Previous versions of this class had a finalize method\n     * that shut down this executor, but in this version, finalize\n     * does nothing.\n     */\n    @Deprecated(since=\"9\")\n    protected void finalize() {}\n\n    /**\n     * Sets the thread factory used to create new threads.\n     *\n     * @param threadFactory the new thread factory\n     * @throws NullPointerException if threadFactory is null\n     * @see #getThreadFactory\n     */\n    public void setThreadFactory(ThreadFactory threadFactory) {\n        if (threadFactory == null)\n            throw new NullPointerException();\n        this.threadFactory = threadFactory;\n    }\n\n    /**\n     * Returns the thread factory used to create new threads.\n     *\n     * @return the current thread factory\n     * @see #setThreadFactory(ThreadFactory)\n     */\n    public ThreadFactory getThreadFactory() {\n        return threadFactory;\n    }\n\n    /**\n     * Sets a new handler for unexecutable tasks.\n     *\n     * @param handler the new handler\n     * @throws NullPointerException if handler is null\n     * @see #getRejectedExecutionHandler\n     */\n    public void setRejectedExecutionHandler(RejectedExecutionHandler handler) {\n        if (handler == null)\n            throw new NullPointerException();\n        this.handler = handler;\n    }\n\n    /**\n     * Returns the current handler for unexecutable tasks.\n     *\n     * @return the current handler\n     * @see #setRejectedExecutionHandler(RejectedExecutionHandler)\n     */\n    public RejectedExecutionHandler getRejectedExecutionHandler() {\n        return handler;\n    }\n\n    /**\n     * Sets the core number of threads.  This overrides any value set\n     * in the constructor.  If the new value is smaller than the\n     * current value, excess existing threads will be terminated when\n     * they next become idle.  If larger, new threads will, if needed,\n     * be started to execute any queued tasks.\n     *\n     * @param corePoolSize the new core size\n     * @throws IllegalArgumentException if {@code corePoolSize < 0}\n     *         or {@code corePoolSize} is greater than the {@linkplain\n     *         #getMaximumPoolSize() maximum pool size}\n     * @see #getCorePoolSize\n     */\n    public void setCorePoolSize(int corePoolSize) {\n        if (corePoolSize < 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        int delta = corePoolSize - this.corePoolSize;\n        this.corePoolSize = corePoolSize;\n        if (workerCountOf(ctl.get()) > corePoolSize)\n            interruptIdleWorkers();\n        else if (delta > 0) {\n            // We don't really know how many new threads are \"needed\".\n            // As a heuristic, prestart enough new workers (up to new\n            // core size) to handle the current number of tasks in\n            // queue, but stop if queue becomes empty while doing so.\n            int k = Math.min(delta, workQueue.size());\n            while (k-- > 0 && addWorker(null, true)) {\n                if (workQueue.isEmpty())\n                    break;\n            }\n        }\n    }\n\n    /**\n     * Returns the core number of threads.\n     *\n     * @return the core number of threads\n     * @see #setCorePoolSize\n     */\n    public int getCorePoolSize() {\n        return corePoolSize;\n    }\n\n    /**\n     * Starts a core thread, causing it to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed. This method will return {@code false}\n     * if all core threads have already been started.\n     *\n     * @return {@code true} if a thread was started\n     */\n    public boolean prestartCoreThread() {\n        return workerCountOf(ctl.get()) < corePoolSize &&\n            addWorker(null, true);\n    }\n\n    /**\n     * Same as prestartCoreThread except arranges that at least one\n     * thread is started even if corePoolSize is 0.\n     */\n    void ensurePrestart() {\n        int wc = workerCountOf(ctl.get());\n        if (wc < corePoolSize)\n            addWorker(null, true);\n        else if (wc == 0)\n            addWorker(null, false);\n    }\n\n    /**\n     * Starts all core threads, causing them to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed.\n     *\n     * @return the number of threads started\n     */\n    public int prestartAllCoreThreads() {\n        int n = 0;\n        while (addWorker(null, true))\n            ++n;\n        return n;\n    }\n\n    /**\n     * Returns true if this pool allows core threads to time out and\n     * terminate if no tasks arrive within the keepAlive time, being\n     * replaced if needed when new tasks arrive. When true, the same\n     * keep-alive policy applying to non-core threads applies also to\n     * core threads. When false (the default), core threads are never\n     * terminated due to lack of incoming tasks.\n     *\n     * @return {@code true} if core threads are allowed to time out,\n     *         else {@code false}\n     *\n     * @since 1.6\n     */\n    public boolean allowsCoreThreadTimeOut() {\n        return allowCoreThreadTimeOut;\n    }\n\n    /**\n     * Sets the policy governing whether core threads may time out and\n     * terminate if no tasks arrive within the keep-alive time, being\n     * replaced if needed when new tasks arrive. When false, core\n     * threads are never terminated due to lack of incoming\n     * tasks. When true, the same keep-alive policy applying to\n     * non-core threads applies also to core threads. To avoid\n     * continual thread replacement, the keep-alive time must be\n     * greater than zero when setting {@code true}. This method\n     * should in general be called before the pool is actively used.\n     *\n     * @param value {@code true} if should time out, else {@code false}\n     * @throws IllegalArgumentException if value is {@code true}\n     *         and the current keep-alive time is not greater than zero\n     *\n     * @since 1.6\n     */\n    public void allowCoreThreadTimeOut(boolean value) {\n        if (value && keepAliveTime <= 0)\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        if (value != allowCoreThreadTimeOut) {\n            allowCoreThreadTimeOut = value;\n            if (value)\n                interruptIdleWorkers();\n        }\n    }\n\n    /**\n     * Sets the maximum allowed number of threads. This overrides any\n     * value set in the constructor. If the new value is smaller than\n     * the current value, excess existing threads will be\n     * terminated when they next become idle.\n     *\n     * @param maximumPoolSize the new maximum\n     * @throws IllegalArgumentException if the new maximum is\n     *         less than or equal to zero, or\n     *         less than the {@linkplain #getCorePoolSize core pool size}\n     * @see #getMaximumPoolSize\n     */\n    public void setMaximumPoolSize(int maximumPoolSize) {\n        if (maximumPoolSize <= 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        this.maximumPoolSize = maximumPoolSize;\n        if (workerCountOf(ctl.get()) > maximumPoolSize)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the maximum allowed number of threads.\n     *\n     * @return the maximum allowed number of threads\n     * @see #setMaximumPoolSize\n     */\n    public int getMaximumPoolSize() {\n        return maximumPoolSize;\n    }\n\n    /**\n     * Sets the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     * This overrides any value set in the constructor.\n     *\n     * @param time the time to wait.  A time value of zero will cause\n     *        excess threads to terminate immediately after executing tasks.\n     * @param unit the time unit of the {@code time} argument\n     * @throws IllegalArgumentException if {@code time} less than zero or\n     *         if {@code time} is zero and {@code allowsCoreThreadTimeOut}\n     * @see #getKeepAliveTime(TimeUnit)\n     */\n    public void setKeepAliveTime(long time, TimeUnit unit) {\n        if (time < 0)\n            throw new IllegalArgumentException();\n        if (time == 0 && allowsCoreThreadTimeOut())\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        long keepAliveTime = unit.toNanos(time);\n        long delta = keepAliveTime - this.keepAliveTime;\n        this.keepAliveTime = keepAliveTime;\n        if (delta < 0)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     *\n     * @param unit the desired time unit of the result\n     * @return the time limit\n     * @see #setKeepAliveTime(long, TimeUnit)\n     */\n    public long getKeepAliveTime(TimeUnit unit) {\n        return unit.convert(keepAliveTime, TimeUnit.NANOSECONDS);\n    }\n\n    /* User-level queue utilities */\n\n    /**\n     * Returns the task queue used by this executor. Access to the\n     * task queue is intended primarily for debugging and monitoring.\n     * This queue may be in active use.  Retrieving the task queue\n     * does not prevent queued tasks from executing.\n     *\n     * @return the task queue\n     */\n    public BlockingQueue<Runnable> getQueue() {\n        return workQueue;\n    }\n\n    /**\n     * Removes this task from the executor's internal queue if it is\n     * present, thus causing it not to be run if it has not already\n     * started.\n     *\n     * <p>This method may be useful as one part of a cancellation\n     * scheme.  It may fail to remove tasks that have been converted\n     * into other forms before being placed on the internal queue.\n     * For example, a task entered using {@code submit} might be\n     * converted into a form that maintains {@code Future} status.\n     * However, in such cases, method {@link #purge} may be used to\n     * remove those Futures that have been cancelled.\n     *\n     * @param task the task to remove\n     * @return {@code true} if the task was removed\n     */\n    public boolean remove(Runnable task) {\n        boolean removed = workQueue.remove(task);\n        tryTerminate(); // In case SHUTDOWN and now empty\n        return removed;\n    }\n\n    /**\n     * Tries to remove from the work queue all {@link Future}\n     * tasks that have been cancelled. This method can be useful as a\n     * storage reclamation operation, that has no other impact on\n     * functionality. Cancelled tasks are never executed, but may\n     * accumulate in work queues until worker threads can actively\n     * remove them. Invoking this method instead tries to remove them now.\n     * However, this method may fail to remove tasks in\n     * the presence of interference by other threads.\n     */\n    public void purge() {\n        final BlockingQueue<Runnable> q = workQueue;\n        try {\n            Iterator<Runnable> it = q.iterator();\n            while (it.hasNext()) {\n                Runnable r = it.next();\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    it.remove();\n            }\n        } catch (ConcurrentModificationException fallThrough) {\n            // Take slow path if we encounter interference during traversal.\n            // Make copy for traversal and call remove for cancelled entries.\n            // The slow path is more likely to be O(N*N).\n            for (Object r : q.toArray())\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    q.remove(r);\n        }\n\n        tryTerminate(); // In case SHUTDOWN and now empty\n    }\n\n    /* Statistics */\n\n    /**\n     * Returns the current number of threads in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            // Remove rare and surprising possibility of\n            // isTerminated() && getPoolSize() > 0\n            return runStateAtLeast(ctl.get(), TIDYING) ? 0\n                : workers.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate number of threads that are actively\n     * executing tasks.\n     *\n     * @return the number of threads\n     */\n    public int getActiveCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            int n = 0;\n            for (Worker w : workers)\n                if (w.isLocked())\n                    ++n;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the largest number of threads that have ever\n     * simultaneously been in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getLargestPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            return largestPoolSize;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have ever been\n     * scheduled for execution. Because the states of tasks and\n     * threads may change dynamically during computation, the returned\n     * value is only an approximation.\n     *\n     * @return the number of tasks\n     */\n    public long getTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers) {\n                n += w.completedTasks;\n                if (w.isLocked())\n                    ++n;\n            }\n            return n + workQueue.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have\n     * completed execution. Because the states of tasks and threads\n     * may change dynamically during computation, the returned value\n     * is only an approximation, but one that does not ever decrease\n     * across successive calls.\n     *\n     * @return the number of tasks\n     */\n    public long getCompletedTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers)\n                n += w.completedTasks;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns a string identifying this pool, as well as its state,\n     * including indications of run state and estimated worker and\n     * task counts.\n     *\n     * @return a string identifying this pool, as well as its state\n     */\n    public String toString() {\n        long ncompleted;\n        int nworkers, nactive;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            ncompleted = completedTaskCount;\n            nactive = 0;\n            nworkers = workers.size();\n            for (Worker w : workers) {\n                ncompleted += w.completedTasks;\n                if (w.isLocked())\n                    ++nactive;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n        int c = ctl.get();\n        String runState =\n            isRunning(c) ? \"Running\" :\n            runStateAtLeast(c, TERMINATED) ? \"Terminated\" :\n            \"Shutting down\";\n        return super.toString() +\n            \"[\" + runState +\n            \", pool size = \" + nworkers +\n            \", active threads = \" + nactive +\n            \", queued tasks = \" + workQueue.size() +\n            \", completed tasks = \" + ncompleted +\n            \"]\";\n    }\n\n    /* Extension hooks */\n\n    /**\n     * Method invoked prior to executing the given Runnable in the\n     * given thread.  This method is invoked by thread {@code t} that\n     * will execute task {@code r}, and may be used to re-initialize\n     * ThreadLocals, or to perform logging.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.beforeExecute} at the end of\n     * this method.\n     *\n     * @param t the thread that will run task {@code r}\n     * @param r the task that will be executed\n     */\n    protected void beforeExecute(Thread t, Runnable r) { }\n\n    /**\n     * Method invoked upon completion of execution of the given Runnable.\n     * This method is invoked by the thread that executed the task. If\n     * non-null, the Throwable is the uncaught {@code RuntimeException}\n     * or {@code Error} that caused execution to terminate abruptly.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.afterExecute} at the\n     * beginning of this method.\n     *\n     * <p><b>Note:</b> When actions are enclosed in tasks (such as\n     * {@link FutureTask}) either explicitly or via methods such as\n     * {@code submit}, these task objects catch and maintain\n     * computational exceptions, and so they do not cause abrupt\n     * termination, and the internal exceptions are <em>not</em>\n     * passed to this method. If you would like to trap both kinds of\n     * failures in this method, you can further probe for such cases,\n     * as in this sample subclass that prints either the direct cause\n     * or the underlying exception if a task has been aborted:\n     *\n     * <pre> {@code\n     * class ExtendedExecutor extends ThreadPoolExecutor {\n     *   // ...\n     *   protected void afterExecute(Runnable r, Throwable t) {\n     *     super.afterExecute(r, t);\n     *     if (t == null\n     *         && r instanceof Future<?>\n     *         && ((Future<?>)r).isDone()) {\n     *       try {\n     *         Object result = ((Future<?>) r).get();\n     *       } catch (CancellationException ce) {\n     *         t = ce;\n     *       } catch (ExecutionException ee) {\n     *         t = ee.getCause();\n     *       } catch (InterruptedException ie) {\n     *         // ignore/reset\n     *         Thread.currentThread().interrupt();\n     *       }\n     *     }\n     *     if (t != null)\n     *       System.out.println(t);\n     *   }\n     * }}</pre>\n     *\n     * @param r the runnable that has completed\n     * @param t the exception that caused termination, or null if\n     * execution completed normally\n     */\n    protected void afterExecute(Runnable r, Throwable t) { }\n\n    /**\n     * Method invoked when the Executor has terminated.  Default\n     * implementation does nothing. Note: To properly nest multiple\n     * overridings, subclasses should generally invoke\n     * {@code super.terminated} within this method.\n     */\n    protected void terminated() { }\n\n    /* Predefined RejectedExecutionHandlers */\n\n    /**\n     * A handler for rejected tasks that runs the rejected task\n     * directly in the calling thread of the {@code execute} method,\n     * unless the executor has been shut down, in which case the task\n     * is discarded.\n     */\n    public static class CallerRunsPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code CallerRunsPolicy}.\n         */\n        public CallerRunsPolicy() { }\n\n        /**\n         * Executes task r in the caller's thread, unless the executor\n         * has been shut down, in which case the task is discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                r.run();\n            }\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that throws a\n     * {@link RejectedExecutionException}.\n     *\n     * This is the default handler for {@link ThreadPoolExecutor} and\n     * {@link ScheduledThreadPoolExecutor}.\n     */\n    public static class AbortPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates an {@code AbortPolicy}.\n         */\n        public AbortPolicy() { }\n\n        /**\n         * Always throws RejectedExecutionException.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         * @throws RejectedExecutionException always\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            throw new RejectedExecutionException(\"Task \" + r.toString() +\n                                                 \" rejected from \" +\n                                                 e.toString());\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that silently discards the\n     * rejected task.\n     */\n    public static class DiscardPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardPolicy}.\n         */\n        public DiscardPolicy() { }\n\n        /**\n         * Does nothing, which has the effect of discarding task r.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that discards the oldest unhandled\n     * request and then retries {@code execute}, unless the executor\n     * is shut down, in which case the task is discarded. This policy is\n     * rarely useful in cases where other threads may be waiting for\n     * tasks to terminate, or failures must be recorded. Instead consider\n     * using a handler of the form:\n     * <pre> {@code\n     * new RejectedExecutionHandler() {\n     *   public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n     *     Runnable dropped = e.getQueue().poll();\n     *     if (dropped instanceof Future<?>) {\n     *       ((Future<?>)dropped).cancel(false);\n     *       // also consider logging the failure\n     *     }\n     *     e.execute(r);  // retry\n     * }}}</pre>\n     */\n    public static class DiscardOldestPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardOldestPolicy} for the given executor.\n         */\n        public DiscardOldestPolicy() { }\n\n        /**\n         * Obtains and ignores the next task that the executor\n         * would otherwise execute, if one is immediately available,\n         * and then retries execution of task r, unless the executor\n         * is shut down, in which case task r is instead discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                e.getQueue().poll();\n                e.execute(r);\n            }\n        }\n    }\n}\n",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "/*\n * DO NOT ALTER OR REMOVE COPYRIGHT NOTICES OR THIS FILE HEADER.\n *\n * This code is free software; you can redistribute it and/or modify it\n * under the terms of the GNU General Public License version 2 only, as\n * published by the Free Software Foundation.  Oracle designates this\n * particular file as subject to the \"Classpath\" exception as provided\n * by Oracle in the LICENSE file that accompanied this code.\n *\n * This code is distributed in the hope that it will be useful, but WITHOUT\n * ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or\n * FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License\n * version 2 for more details (a copy is included in the LICENSE file that\n * accompanied this code).\n *\n * You should have received a copy of the GNU General Public License version\n * 2 along with this work; if not, write to the Free Software Foundation,\n * Inc., 51 Franklin St, Fifth Floor, Boston, MA 02110-1301 USA.\n *\n * Please contact Oracle, 500 Oracle Parkway, Redwood Shores, CA 94065 USA\n * or visit www.oracle.com if you need additional information or have any\n * questions.\n */\n\n/*\n * This file is available under and governed by the GNU General Public\n * License version 2 only, as published by the Free Software Foundation.\n * However, the following notice accompanied the original version of this\n * file:\n *\n * Written by Doug Lea with assistance from members of JCP JSR-166\n * Expert Group and released to the public domain, as explained at\n * http://creativecommons.org/publicdomain/zero/1.0/\n */\n\npackage java.util.concurrent;\n\nimport java.util.ArrayList;\nimport java.util.ConcurrentModificationException;\nimport java.util.HashSet;\nimport java.util.Iterator;\nimport java.util.List;\nimport java.util.concurrent.atomic.AtomicInteger;\nimport java.util.concurrent.locks.AbstractQueuedSynchronizer;\nimport java.util.concurrent.locks.Condition;\nimport java.util.concurrent.locks.ReentrantLock;\n\n/**\n * An {@link ExecutorService} that executes each submitted task using\n * one of possibly several pooled threads, normally configured\n * using {@link Executors} factory methods.\n *\n * <p>Thread pools address two different problems: they usually\n * provide improved performance when executing large numbers of\n * asynchronous tasks, due to reduced per-task invocation overhead,\n * and they provide a means of bounding and managing the resources,\n * including threads, consumed when executing a collection of tasks.\n * Each {@code ThreadPoolExecutor} also maintains some basic\n * statistics, such as the number of completed tasks.\n *\n * <p>To be useful across a wide range of contexts, this class\n * provides many adjustable parameters and extensibility\n * hooks. However, programmers are urged to use the more convenient\n * {@link Executors} factory methods {@link\n * Executors#newCachedThreadPool} (unbounded thread pool, with\n * automatic thread reclamation), {@link Executors#newFixedThreadPool}\n * (fixed size thread pool) and {@link\n * Executors#newSingleThreadExecutor} (single background thread), that\n * preconfigure settings for the most common usage\n * scenarios. Otherwise, use the following guide when manually\n * configuring and tuning this class:\n *\n * <dl>\n *\n * <dt>Core and maximum pool sizes</dt>\n *\n * <dd>A {@code ThreadPoolExecutor} will automatically adjust the\n * pool size (see {@link #getPoolSize})\n * according to the bounds set by\n * corePoolSize (see {@link #getCorePoolSize}) and\n * maximumPoolSize (see {@link #getMaximumPoolSize}).\n *\n * When a new task is submitted in method {@link #execute(Runnable)},\n * if fewer than corePoolSize threads are running, a new thread is\n * created to handle the request, even if other worker threads are\n * idle.  Else if fewer than maximumPoolSize threads are running, a\n * new thread will be created to handle the request only if the queue\n * is full.  By setting corePoolSize and maximumPoolSize the same, you\n * create a fixed-size thread pool. By setting maximumPoolSize to an\n * essentially unbounded value such as {@code Integer.MAX_VALUE}, you\n * allow the pool to accommodate an arbitrary number of concurrent\n * tasks. Most typically, core and maximum pool sizes are set only\n * upon construction, but they may also be changed dynamically using\n * {@link #setCorePoolSize} and {@link #setMaximumPoolSize}. </dd>\n *\n * <dt>On-demand construction</dt>\n *\n * <dd>By default, even core threads are initially created and\n * started only when new tasks arrive, but this can be overridden\n * dynamically using method {@link #prestartCoreThread} or {@link\n * #prestartAllCoreThreads}.  You probably want to prestart threads if\n * you construct the pool with a non-empty queue. </dd>\n *\n * <dt>Creating new threads</dt>\n *\n * <dd>New threads are created using a {@link ThreadFactory}.  If not\n * otherwise specified, a {@link Executors#defaultThreadFactory} is\n * used, that creates threads to all be in the same {@link\n * ThreadGroup} and with the same {@code NORM_PRIORITY} priority and\n * non-daemon status. By supplying a different ThreadFactory, you can\n * alter the thread's name, thread group, priority, daemon status,\n * etc. If a {@code ThreadFactory} fails to create a thread when asked\n * by returning null from {@code newThread}, the executor will\n * continue, but might not be able to execute any tasks. Threads\n * should possess the \"modifyThread\" {@code RuntimePermission}. If\n * worker threads or other threads using the pool do not possess this\n * permission, service may be degraded: configuration changes may not\n * take effect in a timely manner, and a shutdown pool may remain in a\n * state in which termination is possible but not completed.</dd>\n *\n * <dt>Keep-alive times</dt>\n *\n * <dd>If the pool currently has more than corePoolSize threads,\n * excess threads will be terminated if they have been idle for more\n * than the keepAliveTime (see {@link #getKeepAliveTime(TimeUnit)}).\n * This provides a means of reducing resource consumption when the\n * pool is not being actively used. If the pool becomes more active\n * later, new threads will be constructed. This parameter can also be\n * changed dynamically using method {@link #setKeepAliveTime(long,\n * TimeUnit)}.  Using a value of {@code Long.MAX_VALUE} {@link\n * TimeUnit#NANOSECONDS} effectively disables idle threads from ever\n * terminating prior to shut down. By default, the keep-alive policy\n * applies only when there are more than corePoolSize threads, but\n * method {@link #allowCoreThreadTimeOut(boolean)} can be used to\n * apply this time-out policy to core threads as well, so long as the\n * keepAliveTime value is non-zero. </dd>\n *\n * <dt>Queuing</dt>\n *\n * <dd>Any {@link BlockingQueue} may be used to transfer and hold\n * submitted tasks.  The use of this queue interacts with pool sizing:\n *\n * <ul>\n *\n * <li>If fewer than corePoolSize threads are running, the Executor\n * always prefers adding a new thread\n * rather than queuing.\n *\n * <li>If corePoolSize or more threads are running, the Executor\n * always prefers queuing a request rather than adding a new\n * thread.\n *\n * <li>If a request cannot be queued, a new thread is created unless\n * this would exceed maximumPoolSize, in which case, the task will be\n * rejected.\n *\n * </ul>\n *\n * There are three general strategies for queuing:\n * <ol>\n *\n * <li><em> Direct handoffs.</em> A good default choice for a work\n * queue is a {@link SynchronousQueue} that hands off tasks to threads\n * without otherwise holding them. Here, an attempt to queue a task\n * will fail if no threads are immediately available to run it, so a\n * new thread will be constructed. This policy avoids lockups when\n * handling sets of requests that might have internal dependencies.\n * Direct handoffs generally require unbounded maximumPoolSizes to\n * avoid rejection of new submitted tasks. This in turn admits the\n * possibility of unbounded thread growth when commands continue to\n * arrive on average faster than they can be processed.\n *\n * <li><em> Unbounded queues.</em> Using an unbounded queue (for\n * example a {@link LinkedBlockingQueue} without a predefined\n * capacity) will cause new tasks to wait in the queue when all\n * corePoolSize threads are busy. Thus, no more than corePoolSize\n * threads will ever be created. (And the value of the maximumPoolSize\n * therefore doesn't have any effect.)  This may be appropriate when\n * each task is completely independent of others, so tasks cannot\n * affect each others execution; for example, in a web page server.\n * While this style of queuing can be useful in smoothing out\n * transient bursts of requests, it admits the possibility of\n * unbounded work queue growth when commands continue to arrive on\n * average faster than they can be processed.\n *\n * <li><em>Bounded queues.</em> A bounded queue (for example, an\n * {@link ArrayBlockingQueue}) helps prevent resource exhaustion when\n * used with finite maximumPoolSizes, but can be more difficult to\n * tune and control.  Queue sizes and maximum pool sizes may be traded\n * off for each other: Using large queues and small pools minimizes\n * CPU usage, OS resources, and context-switching overhead, but can\n * lead to artificially low throughput.  If tasks frequently block (for\n * example if they are I/O bound), a system may be able to schedule\n * time for more threads than you otherwise allow. Use of small queues\n * generally requires larger pool sizes, which keeps CPUs busier but\n * may encounter unacceptable scheduling overhead, which also\n * decreases throughput.\n *\n * </ol>\n *\n * </dd>\n *\n * <dt>Rejected tasks</dt>\n *\n * <dd>New tasks submitted in method {@link #execute(Runnable)} will be\n * <em>rejected</em> when the Executor has been shut down, and also when\n * the Executor uses finite bounds for both maximum threads and work queue\n * capacity, and is saturated.  In either case, the {@code execute} method\n * invokes the {@link\n * RejectedExecutionHandler#rejectedExecution(Runnable, ThreadPoolExecutor)}\n * method of its {@link RejectedExecutionHandler}.  Four predefined handler\n * policies are provided:\n *\n * <ol>\n *\n * <li>In the default {@link ThreadPoolExecutor.AbortPolicy}, the handler\n * throws a runtime {@link RejectedExecutionException} upon rejection.\n *\n * <li>In {@link ThreadPoolExecutor.CallerRunsPolicy}, the thread\n * that invokes {@code execute} itself runs the task. This provides a\n * simple feedback control mechanism that will slow down the rate that\n * new tasks are submitted.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardPolicy}, a task that cannot\n * be executed is simply dropped. This policy is designed only for\n * those rare cases in which task completion is never relied upon.\n *\n * <li>In {@link ThreadPoolExecutor.DiscardOldestPolicy}, if the\n * executor is not shut down, the task at the head of the work queue\n * is dropped, and then execution is retried (which can fail again,\n * causing this to be repeated.) This policy is rarely acceptable.  In\n * nearly all cases, you should also cancel the task to cause an\n * exception in any component waiting for its completion, and/or log\n * the failure, as illustrated in {@link\n * ThreadPoolExecutor.DiscardOldestPolicy} documentation.\n *\n * </ol>\n *\n * It is possible to define and use other kinds of {@link\n * RejectedExecutionHandler} classes. Doing so requires some care\n * especially when policies are designed to work only under particular\n * capacity or queuing policies. </dd>\n *\n * <dt>Hook methods</dt>\n *\n * <dd>This class provides {@code protected} overridable\n * {@link #beforeExecute(Thread, Runnable)} and\n * {@link #afterExecute(Runnable, Throwable)} methods that are called\n * before and after execution of each task.  These can be used to\n * manipulate the execution environment; for example, reinitializing\n * ThreadLocals, gathering statistics, or adding log entries.\n * Additionally, method {@link #terminated} can be overridden to perform\n * any special processing that needs to be done once the Executor has\n * fully terminated.\n *\n * <p>If hook, callback, or BlockingQueue methods throw exceptions,\n * internal worker threads may in turn fail, abruptly terminate, and\n * possibly be replaced.</dd>\n *\n * <dt>Queue maintenance</dt>\n *\n * <dd>Method {@link #getQueue()} allows access to the work queue\n * for purposes of monitoring and debugging.  Use of this method for\n * any other purpose is strongly discouraged.  Two supplied methods,\n * {@link #remove(Runnable)} and {@link #purge} are available to\n * assist in storage reclamation when large numbers of queued tasks\n * become cancelled.</dd>\n *\n * <dt>Reclamation</dt>\n *\n * <dd>A pool that is no longer referenced in a program <em>AND</em>\n * has no remaining threads may be reclaimed (garbage collected)\n * without being explicitly shutdown. You can configure a pool to\n * allow all unused threads to eventually die by setting appropriate\n * keep-alive times, using a lower bound of zero core threads and/or\n * setting {@link #allowCoreThreadTimeOut(boolean)}.  </dd>\n *\n * </dl>\n *\n * <p><b>Extension example.</b> Most extensions of this class\n * override one or more of the protected hook methods. For example,\n * here is a subclass that adds a simple pause/resume feature:\n *\n * <pre> {@code\n * class PausableThreadPoolExecutor extends ThreadPoolExecutor {\n *   private boolean isPaused;\n *   private ReentrantLock pauseLock = new ReentrantLock();\n *   private Condition unpaused = pauseLock.newCondition();\n *\n *   public PausableThreadPoolExecutor(...) { super(...); }\n *\n *   protected void beforeExecute(Thread t, Runnable r) {\n *     super.beforeExecute(t, r);\n *     pauseLock.lock();\n *     try {\n *       while (isPaused) unpaused.await();\n *     } catch (InterruptedException ie) {\n *       t.interrupt();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void pause() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = true;\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n *\n *   public void resume() {\n *     pauseLock.lock();\n *     try {\n *       isPaused = false;\n *       unpaused.signalAll();\n *     } finally {\n *       pauseLock.unlock();\n *     }\n *   }\n * }}</pre>\n *\n * @since 1.5\n * @author Doug Lea\n */\npublic class ThreadPoolExecutor extends AbstractExecutorService {\n    /**\n     * The main pool control state, ctl, is an atomic integer packing\n     * two conceptual fields\n     *   workerCount, indicating the effective number of threads\n     *   runState,    indicating whether running, shutting down etc\n     *\n     * In order to pack them into one int, we limit workerCount to\n     * (2^29)-1 (about 500 million) threads rather than (2^31)-1 (2\n     * billion) otherwise representable. If this is ever an issue in\n     * the future, the variable can be changed to be an AtomicLong,\n     * and the shift/mask constants below adjusted. But until the need\n     * arises, this code is a bit faster and simpler using an int.\n     *\n     * The workerCount is the number of workers that have been\n     * permitted to start and not permitted to stop.  The value may be\n     * transiently different from the actual number of live threads,\n     * for example when a ThreadFactory fails to create a thread when\n     * asked, and when exiting threads are still performing\n     * bookkeeping before terminating. The user-visible pool size is\n     * reported as the current size of the workers set.\n     *\n     * The runState provides the main lifecycle control, taking on values:\n     *\n     *   RUNNING:  Accept new tasks and process queued tasks\n     *   SHUTDOWN: Don't accept new tasks, but process queued tasks\n     *   STOP:     Don't accept new tasks, don't process queued tasks,\n     *             and interrupt in-progress tasks\n     *   TIDYING:  All tasks have terminated, workerCount is zero,\n     *             the thread transitioning to state TIDYING\n     *             will run the terminated() hook method\n     *   TERMINATED: terminated() has completed\n     *\n     * The numerical order among these values matters, to allow\n     * ordered comparisons. The runState monotonically increases over\n     * time, but need not hit each state. The transitions are:\n     *\n     * RUNNING -> SHUTDOWN\n     *    On invocation of shutdown()\n     * (RUNNING or SHUTDOWN) -> STOP\n     *    On invocation of shutdownNow()\n     * SHUTDOWN -> TIDYING\n     *    When both queue and pool are empty\n     * STOP -> TIDYING\n     *    When pool is empty\n     * TIDYING -> TERMINATED\n     *    When the terminated() hook method has completed\n     *\n     * Threads waiting in awaitTermination() will return when the\n     * state reaches TERMINATED.\n     *\n     * Detecting the transition from SHUTDOWN to TIDYING is less\n     * straightforward than you'd like because the queue may become222\n     * empty after non-empty and vice versa during SHUTDOWN state, but\n     * we can only terminate if, after seeing that it is empty, we see\n     * that workerCount is 0 (which sometimes entails a recheck -- see\n     * below).\n     */\n    private final AtomicInteger ctl = new AtomicInteger(ctlOf(RUNNING, 0));\n    private static final int COUNT_BITS = Integer.SIZE - 3;\n    private static final int COUNT_MASK = (1 << COUNT_BITS) - 1;\n\n    // runState is stored in the high-order bits\n    private static final int RUNNING    = -1 << COUNT_BITS;\n    private static final int SHUTDOWN   =  0 << COUNT_BITS;\n    private static final int STOP       =  1 << COUNT_BITS;\n    private static final int TIDYING    =  2 << COUNT_BITS;\n    private static final int TERMINATED =  3 << COUNT_BITS;\n\n    // Packing and unpacking ctl\n    private static int runStateOf(int c)     { return c & ~COUNT_MASK; }\n    private static int workerCountOf(int c)  { return c & COUNT_MASK; }\n    private static int ctlOf(int rs, int wc) { return rs | wc; }\n\n    /*\n     * Bit field accessors that don't require unpacking ctl.\n     * These depend on the bit layout and on workerCount being never negative.\n     */\n\n    private static boolean runStateLessThan(int c, int s) {\n        return c < s;\n    }\n\n    private static boolean runStateAtLeast(int c, int s) {\n        return c >= s;\n    }\n\n    private static boolean isRunning(int c) {\n        return c < SHUTDOWN;\n    }\n\n    /**\n     * Attempts to CAS-increment the workerCount field of ctl.\n     */\n    private boolean compareAndIncrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect + 1);\n    }\n\n    /**\n     * Attempts to CAS-decrement the workerCount field of ctl.\n     */\n    private boolean compareAndDecrementWorkerCount(int expect) {\n        return ctl.compareAndSet(expect, expect - 1);\n    }\n\n    /**\n     * Decrements the workerCount field of ctl. This is called only on\n     * abrupt termination of a thread (see processWorkerExit). Other\n     * decrements are performed within getTask.\n     */\n    private void decrementWorkerCount() {\n        ctl.addAndGet(-1);\n    }\n\n    /**\n     * The queue used for holding tasks and handing off to worker\n     * threads.  We do not require that workQueue.poll() returning\n     * null necessarily means that workQueue.isEmpty(), so rely\n     * solely on isEmpty to see if the queue is empty (which we must\n     * do for example when deciding whether to transition from\n     * SHUTDOWN to TIDYING).  This accommodates special-purpose\n     * queues such as DelayQueues for which poll() is allowed to\n     * return null even if it may later return non-null when delays\n     * expire.\n     */\n    private final BlockingQueue<Runnable> workQueue;\n\n    /**\n     * Lock held on access to workers set and related bookkeeping.\n     * While we could use a concurrent set of some sort, it turns out\n     * to be generally preferable to use a lock. Among the reasons is\n     * that this serializes interruptIdleWorkers, which avoids\n     * unnecessary interrupt storms, especially during shutdown.\n     * Otherwise exiting threads would concurrently interrupt those\n     * that have not yet interrupted. It also simplifies some of the\n     * associated statistics bookkeeping of largestPoolSize etc. We\n     * also hold mainLock on shutdown and shutdownNow, for the sake of\n     * ensuring workers set is stable while separately checking\n     * permission to interrupt and actually interrupting.\n     */\n    private final ReentrantLock mainLock = new ReentrantLock();\n\n    /**\n     * Set containing all worker threads in pool. Accessed only when\n     * holding mainLock.\n     */\n    private final HashSet<Worker> workers = new HashSet<>();\n\n    /**\n     * Wait condition to support awaitTermination.\n     */\n    private final Condition termination = mainLock.newCondition();\n\n    /**\n     * Tracks largest attained pool size. Accessed only under\n     * mainLock.\n     */\n    private int largestPoolSize;\n\n    /**\n     * Counter for completed tasks. Updated only on termination of\n     * worker threads. Accessed only under mainLock.\n     */\n    private long completedTaskCount;\n\n    /*\n     * All user control parameters are declared as volatiles so that\n     * ongoing actions are based on freshest values, but without need\n     * for locking, since no internal invariants depend on them\n     * changing synchronously with respect to other actions.\n     */\n\n    /**\n     * Factory for new threads. All threads are created using this\n     * factory (via method addWorker).  All callers must be prepared\n     * for addWorker to fail, which may reflect a system or user's\n     * policy limiting the number of threads.  Even though it is not\n     * treated as an error, failure to create threads may result in\n     * new tasks being rejected or existing ones remaining stuck in\n     * the queue.\n     *\n     * We go further and preserve pool invariants even in the face of\n     * errors such as OutOfMemoryError, that might be thrown while\n     * trying to create threads.  Such errors are rather common due to\n     * the need to allocate a native stack in Thread.start, and users\n     * will want to perform clean pool shutdown to clean up.  There\n     * will likely be enough memory available for the cleanup code to\n     * complete without encountering yet another OutOfMemoryError.\n     */\n    private volatile ThreadFactory threadFactory;\n\n    /**\n     * Handler called when saturated or shutdown in execute.\n     */\n    private volatile RejectedExecutionHandler handler;\n\n    /**\n     * Timeout in nanoseconds for idle threads waiting for work.\n     * Threads use this timeout when there are more than corePoolSize\n     * present or if allowCoreThreadTimeOut. Otherwise they wait\n     * forever for new work.\n     */\n    private volatile long keepAliveTime;\n\n    /**\n     * If false (default), core threads stay alive even when idle.\n     * If true, core threads use keepAliveTime to time out waiting\n     * for work.\n     */\n    private volatile boolean allowCoreThreadTimeOut;\n\n    /**\n     * Core pool size is the minimum number of workers to keep alive\n     * (and not allow to time out etc) unless allowCoreThreadTimeOut\n     * is set, in which case the minimum is zero.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code corePoolSize & COUNT_MASK}.\n     */\n    private volatile int corePoolSize;\n\n    /**\n     * Maximum pool size.\n     *\n     * Since the worker count is actually stored in COUNT_BITS bits,\n     * the effective limit is {@code maximumPoolSize & COUNT_MASK}.\n     */\n    private volatile int maximumPoolSize;\n\n    /**\n     * The default rejected execution handler.\n     */\n    private static final RejectedExecutionHandler defaultHandler =\n        new AbortPolicy();\n\n    /**\n     * Permission required for callers of shutdown and shutdownNow.\n     * We additionally require (see checkShutdownAccess) that callers\n     * have permission to actually interrupt threads in the worker set\n     * (as governed by Thread.interrupt, which relies on\n     * ThreadGroup.checkAccess, which in turn relies on\n     * SecurityManager.checkAccess). Shutdowns are attempted only if\n     * these checks pass.\n     *\n     * All actual invocations of Thread.interrupt (see\n     * interruptIdleWorkers and interruptWorkers) ignore\n     * SecurityExceptions, meaning that the attempted interrupts\n     * silently fail. In the case of shutdown, they should not fail\n     * unless the SecurityManager has inconsistent policies, sometimes\n     * allowing access to a thread and sometimes not. In such cases,\n     * failure to actually interrupt threads may disable or delay full\n     * termination. Other uses of interruptIdleWorkers are advisory,\n     * and failure to actually interrupt will merely delay response to\n     * configuration changes so is not handled exceptionally.\n     */\n    private static final RuntimePermission shutdownPerm =\n        new RuntimePermission(\"modifyThread\");\n\n    /**\n     * Class Worker mainly maintains interrupt control state for\n     * threads running tasks, along with other minor bookkeeping.\n     * This class opportunistically extends AbstractQueuedSynchronizer\n     * to simplify acquiring and releasing a lock surrounding each\n     * task execution.  This protects against interrupts that are\n     * intended to wake up a worker thread waiting for a task from\n     * instead interrupting a task being run.  We implement a simple\n     * non-reentrant mutual exclusion lock rather than use\n     * ReentrantLock because we do not want worker tasks to be able to\n     * reacquire the lock when they invoke pool control methods like\n     * setCorePoolSize.  Additionally, to suppress interrupts until\n     * the thread actually starts running tasks, we initialize lock\n     * state to a negative value, and clear it upon start (in\n     * runWorker).\n     */\n    private final class Worker\n        extends AbstractQueuedSynchronizer\n        implements Runnable\n    {\n        /**\n         * This class will never be serialized, but we provide a\n         * serialVersionUID to suppress a javac warning.\n         */\n        private static final long serialVersionUID = 6138294804551838833L;\n\n        /** Thread this worker is running in.  Null if factory fails. */\n        @SuppressWarnings(\"serial\") // Unlikely to be serializable\n        final Thread thread;\n        /** Initial task to run.  Possibly null. */\n        @SuppressWarnings(\"serial\") // Not statically typed as Serializable\n        Runnable firstTask;\n        /** Per-thread task counter */\n        volatile long completedTasks;\n\n        // TODO: switch to AbstractQueuedLongSynchronizer and move\n        // completedTasks into the lock word.\n\n        /**\n         * Creates with given first task and thread from ThreadFactory.\n         * @param firstTask the first task (null if none)\n         */\n        Worker(Runnable firstTask) {\n            setState(-1); // inhibit interrupts until runWorker\n            this.firstTask = firstTask;\n            this.thread = getThreadFactory().newThread(this);\n        }\n\n        /** Delegates main run loop to outer runWorker. */\n        public void run() {\n            runWorker(this);\n        }\n\n        // Lock methods\n        //\n        // The value 0 represents the unlocked state.\n        // The value 1 represents the locked state.\n\n        protected boolean isHeldExclusively() {\n            return getState() != 0;\n        }\n\n        protected boolean tryAcquire(int unused) {\n            if (compareAndSetState(0, 1)) {\n                setExclusiveOwnerThread(Thread.currentThread());\n                return true;\n            }\n            return false;\n        }\n\n        protected boolean tryRelease(int unused) {\n            setExclusiveOwnerThread(null);\n            setState(0);\n            return true;\n        }\n\n        public void lock()        { acquire(1); }\n        public boolean tryLock()  { return tryAcquire(1); }\n        public void unlock()      { release(1); }\n        public boolean isLocked() { return isHeldExclusively(); }\n\n        void interruptIfStarted() {\n            Thread t;\n            if (getState() >= 0 && (t = thread) != null && !t.isInterrupted()) {\n                try {\n                    t.interrupt();\n                } catch (SecurityException ignore) {\n                }\n            }\n        }\n    }\n\n    /*\n     * Methods for setting control state\n     */\n\n    /**\n     * Transitions runState to given target, or leaves it alone if\n     * already at least the given target.\n     *\n     * @param targetState the desired state, either SHUTDOWN or STOP\n     *        (but not TIDYING or TERMINATED -- use tryTerminate for that)\n     */\n    private void advanceRunState(int targetState) {\n        // assert targetState == SHUTDOWN || targetState == STOP;\n        for (;;) {\n            int c = ctl.get();\n            if (runStateAtLeast(c, targetState) ||\n                ctl.compareAndSet(c, ctlOf(targetState, workerCountOf(c))))\n                break;\n        }\n    }\n\n    /**\n     * Transitions to TERMINATED state if either (SHUTDOWN and pool\n     * and queue empty) or (STOP and pool empty).  If otherwise\n     * eligible to terminate but workerCount is nonzero, interrupts an\n     * idle worker to ensure that shutdown signals propagate. This\n     * method must be called following any action that might make\n     * termination possible -- reducing worker count or removing tasks\n     * from the queue during shutdown. The method is non-private to\n     * allow access from ScheduledThreadPoolExecutor.\n     */\n    final void tryTerminate() {\n        for (;;) {\n            int c = ctl.get();\n            if (isRunning(c) ||\n                runStateAtLeast(c, TIDYING) ||\n                (runStateLessThan(c, STOP) && ! workQueue.isEmpty()))\n                return;\n            if (workerCountOf(c) != 0) { // Eligible to terminate\n                interruptIdleWorkers(ONLY_ONE);\n                return;\n            }\n\n            final ReentrantLock mainLock = this.mainLock;\n            mainLock.lock();\n            try {\n                if (ctl.compareAndSet(c, ctlOf(TIDYING, 0))) {\n                    try {\n                        terminated();\n                    } finally {\n                        ctl.set(ctlOf(TERMINATED, 0));\n                        termination.signalAll();\n                    }\n                    return;\n                }\n            } finally {\n                mainLock.unlock();\n            }\n            // else retry on failed CAS\n        }\n    }\n\n    /*\n     * Methods for controlling interrupts to worker threads.\n     */\n\n    /**\n     * If there is a security manager, makes sure caller has\n     * permission to shut down threads in general (see shutdownPerm).\n     * If this passes, additionally makes sure the caller is allowed\n     * to interrupt each worker thread. This might not be true even if\n     * first check passed, if the SecurityManager treats some threads\n     * specially.\n     */\n    private void checkShutdownAccess() {\n        // assert mainLock.isHeldByCurrentThread();\n        @SuppressWarnings(\"removal\")\n        SecurityManager security = System.getSecurityManager();\n        if (security != null) {\n            security.checkPermission(shutdownPerm);\n            for (Worker w : workers)\n                security.checkAccess(w.thread);\n        }\n    }\n\n    /**\n     * Interrupts all threads, even if active. Ignores SecurityExceptions\n     * (in which case some threads may remain uninterrupted).\n     */\n    private void interruptWorkers() {\n        // assert mainLock.isHeldByCurrentThread();\n        for (Worker w : workers)\n            w.interruptIfStarted();\n    }\n\n    /**\n     * Interrupts threads that might be waiting for tasks (as\n     * indicated by not being locked) so they can check for\n     * termination or configuration changes. Ignores\n     * SecurityExceptions (in which case some threads may remain\n     * uninterrupted).\n     *\n     * @param onlyOne If true, interrupt at most one worker. This is\n     * called only from tryTerminate when termination is otherwise\n     * enabled but there are still other workers.  In this case, at\n     * most one waiting worker is interrupted to propagate shutdown\n     * signals in case all threads are currently waiting.\n     * Interrupting any arbitrary thread ensures that newly arriving\n     * workers since shutdown began will also eventually exit.\n     * To guarantee eventual termination, it suffices to always\n     * interrupt only one idle worker, but shutdown() interrupts all\n     * idle workers so that redundant workers exit promptly, not\n     * waiting for a straggler task to finish.\n     */\n    private void interruptIdleWorkers(boolean onlyOne) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            for (Worker w : workers) {\n                Thread t = w.thread;\n                if (!t.isInterrupted() && w.tryLock()) {\n                    try {\n                        t.interrupt();\n                    } catch (SecurityException ignore) {\n                    } finally {\n                        w.unlock();\n                    }\n                }\n                if (onlyOne)\n                    break;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Common form of interruptIdleWorkers, to avoid having to\n     * remember what the boolean argument means.\n     */\n    private void interruptIdleWorkers() {\n        interruptIdleWorkers(false);\n    }\n\n    private static final boolean ONLY_ONE = true;\n\n    /*\n     * Misc utilities, most of which are also exported to\n     * ScheduledThreadPoolExecutor\n     */\n\n    /**\n     * Invokes the rejected execution handler for the given command.\n     * Package-protected for use by ScheduledThreadPoolExecutor.\n     */\n    final void reject(Runnable command) {\n        handler.rejectedExecution(command, this);\n    }\n\n    /**\n     * Performs any further cleanup following run state transition on\n     * invocation of shutdown.  A no-op here, but used by\n     * ScheduledThreadPoolExecutor to cancel delayed tasks.\n     */\n    void onShutdown() {\n    }\n\n    /**\n     * Drains the task queue into a new list, normally using\n     * drainTo. But if the queue is a DelayQueue or any other kind of\n     * queue for which poll or drainTo may fail to remove some\n     * elements, it deletes them one by one.\n     */\n    private List<Runnable> drainQueue() {\n        BlockingQueue<Runnable> q = workQueue;\n        ArrayList<Runnable> taskList = new ArrayList<>();\n        q.drainTo(taskList);\n        if (!q.isEmpty()) {\n            for (Runnable r : q.toArray(new Runnable[0])) {\n                if (q.remove(r))\n                    taskList.add(r);\n            }\n        }\n        return taskList;\n    }\n\n    /*\n     * Methods for creating, running and cleaning up after workers\n     */\n\n    /**\n     * Checks if a new worker can be added with respect to current\n     * pool state and the given bound (either core or maximum). If so,\n     * the worker count is adjusted accordingly, and, if possible, a\n     * new worker is created and started, running firstTask as its\n     * first task. This method returns false if the pool is stopped or\n     * eligible to shut down. It also returns false if the thread\n     * factory fails to create a thread when asked.  If the thread\n     * creation fails, either due to the thread factory returning\n     * null, or due to an exception (typically OutOfMemoryError in\n     * Thread.start()), we roll back cleanly.\n     *\n     * @param firstTask the task the new thread should run first (or\n     * null if none). Workers are created with an initial first task\n     * (in method execute()) to bypass queuing when there are fewer\n     * than corePoolSize threads (in which case we always start one),\n     * or when the queue is full (in which case we must bypass queue).\n     * Initially idle threads are usually created via\n     * prestartCoreThread or to replace other dying workers.\n     *\n     * @param core if true use corePoolSize as bound, else\n     * maximumPoolSize. (A boolean indicator is used here rather than a\n     * value to ensure reads of fresh values after checking other pool\n     * state).\n     * @return true if successful\n     */\n    private boolean addWorker(Runnable firstTask, boolean core) {\n        retry:\n        for (int c = ctl.get();;) {\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP)\n                    || firstTask != null\n                    || workQueue.isEmpty()))\n                return false;\n\n            for (;;) {\n                if (workerCountOf(c)\n                    >= ((core ? corePoolSize : maximumPoolSize) & COUNT_MASK))\n                    return false;\n                if (compareAndIncrementWorkerCount(c))\n                    break retry;\n                c = ctl.get();  // Re-read ctl\n                if (runStateAtLeast(c, SHUTDOWN))\n                    continue retry;\n                // else CAS failed due to workerCount change; retry inner loop\n            }\n        }\n\n        boolean workerStarted = false;\n        boolean workerAdded = false;\n        Worker w = null;\n        try {\n            w = new Worker(firstTask);\n            final Thread t = w.thread;\n            if (t != null) {\n                final ReentrantLock mainLock = this.mainLock;\n                mainLock.lock();\n                try {\n                    // Recheck while holding lock.\n                    // Back out on ThreadFactory failure or if\n                    // shut down before lock acquired.\n                    int c = ctl.get();\n\n                    if (isRunning(c) ||\n                        (runStateLessThan(c, STOP) && firstTask == null)) {\n                        if (t.getState() != Thread.State.NEW)\n                            throw new IllegalThreadStateException();\n                        workers.add(w);\n                        workerAdded = true;\n                        int s = workers.size();\n                        if (s > largestPoolSize)\n                            largestPoolSize = s;\n                    }\n                } finally {\n                    mainLock.unlock();\n                }\n                if (workerAdded) {\n                    t.start();\n                    workerStarted = true;\n                }\n            }\n        } finally {\n            if (! workerStarted)\n                addWorkerFailed(w);\n        }\n        return workerStarted;\n    }\n\n    /**\n     * Rolls back the worker thread creation.\n     * - removes worker from workers, if present\n     * - decrements worker count\n     * - rechecks for termination, in case the existence of this\n     *   worker was holding up termination\n     */\n    private void addWorkerFailed(Worker w) {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            if (w != null)\n                workers.remove(w);\n            decrementWorkerCount();\n            tryTerminate();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Performs cleanup and bookkeeping for a dying worker. Called\n     * only from worker threads. Unless completedAbruptly is set,\n     * assumes that workerCount has already been adjusted to account\n     * for exit.  This method removes thread from worker set, and\n     * possibly terminates the pool or replaces the worker if either\n     * it exited due to user task exception or if fewer than\n     * corePoolSize workers are running or queue is non-empty but\n     * there are no workers.\n     *\n     * @param w the worker\n     * @param completedAbruptly if the worker died due to user exception\n     */\n    private void processWorkerExit(Worker w, boolean completedAbruptly) {\n        if (completedAbruptly) // If abrupt, then workerCount wasn't adjusted\n            decrementWorkerCount();\n\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            completedTaskCount += w.completedTasks;\n            workers.remove(w);\n        } finally {\n            mainLock.unlock();\n        }\n\n        tryTerminate();\n\n        int c = ctl.get();\n        if (runStateLessThan(c, STOP)) {\n            if (!completedAbruptly) {\n                int min = allowCoreThreadTimeOut ? 0 : corePoolSize;\n                if (min == 0 && ! workQueue.isEmpty())\n                    min = 1;\n                if (workerCountOf(c) >= min)\n                    return; // replacement not needed\n            }\n            addWorker(null, false);\n        }\n    }\n\n    /**\n     * Performs blocking or timed wait for a task, depending on\n     * current configuration settings, or returns null if this worker\n     * must exit because of any of:\n     * 1. There are more than maximumPoolSize workers (due to\n     *    a call to setMaximumPoolSize).\n     * 2. The pool is stopped.\n     * 3. The pool is shutdown and the queue is empty.\n     * 4. This worker timed out waiting for a task, and timed-out\n     *    workers are subject to termination (that is,\n     *    {@code allowCoreThreadTimeOut || workerCount > corePoolSize})\n     *    both before and after the timed wait, and if the queue is\n     *    non-empty, this worker is not the last thread in the pool.\n     *\n     * @return task, or null if the worker must exit, in which case\n     *         workerCount is decremented\n     */\n    private Runnable getTask() {\n        boolean timedOut = false; // Did the last poll() time out?\n\n        for (;;) {\n            int c = ctl.get();\n\n            // Check if queue empty only if necessary.\n            if (runStateAtLeast(c, SHUTDOWN)\n                && (runStateAtLeast(c, STOP) || workQueue.isEmpty())) {\n                decrementWorkerCount();\n                return null;\n            }\n\n            int wc = workerCountOf(c);\n\n            // Are workers subject to culling?\n            boolean timed = allowCoreThreadTimeOut || wc > corePoolSize;\n\n            if ((wc > maximumPoolSize || (timed && timedOut))\n                && (wc > 1 || workQueue.isEmpty())) {\n                if (compareAndDecrementWorkerCount(c))\n                    return null;\n                continue;\n            }\n\n            try {\n                Runnable r = timed ?\n                    workQueue.poll(keepAliveTime, TimeUnit.NANOSECONDS) :\n                    workQueue.take();\n                if (r != null)\n                    return r;\n                timedOut = true;\n            } catch (InterruptedException retry) {\n                timedOut = false;\n            }\n        }\n    }\n\n    /**\n     * Main worker run loop.  Repeatedly gets tasks from queue and\n     * executes them, while coping with a number of issues:\n     *\n     * 1. We may start out with an initial task, in which case we\n     * don't need to get the first one. Otherwise, as long as pool is\n     * running, we get tasks from getTask. If it returns null then the\n     * worker exits due to changed pool state or configuration\n     * parameters.  Other exits result from exception throws in\n     * external code, in which case completedAbruptly holds, which\n     * usually leads processWorkerExit to replace this thread.\n     *\n     * 2. Before running any task, the lock is acquired to prevent\n     * other pool interrupts while the task is executing, and then we\n     * ensure that unless pool is stopping, this thread does not have\n     * its interrupt set.\n     *\n     * 3. Each task run is preceded by a call to beforeExecute, which\n     * might throw an exception, in which case we cause thread to die\n     * (breaking loop with completedAbruptly true) without processing\n     * the task.\n     *\n     * 4. Assuming beforeExecute completes normally, we run the task,\n     * gathering any of its thrown exceptions to send to afterExecute.\n     * We separately handle RuntimeException, Error (both of which the\n     * specs guarantee that we trap) and arbitrary Throwables.\n     * Because we cannot rethrow Throwables within Runnable.run, we\n     * wrap them within Errors on the way out (to the thread's\n     * UncaughtExceptionHandler).  Any thrown exception also\n     * conservatively causes thread to die.\n     *\n     * 5. After task.run completes, we call afterExecute, which may\n     * also throw an exception, which will also cause thread to\n     * die. According to JLS Sec 14.20, this exception is the one that\n     * will be in effect even if task.run throws.\n     *\n     * The net effect of the exception mechanics is that afterExecute\n     * and the thread's UncaughtExceptionHandler have as accurate\n     * information as we can provide about any problems encountered by\n     * user code.\n     *\n     * @param w the worker\n     */\n    final void runWorker(Worker w) {\n        Thread wt = Thread.currentThread();\n        Runnable task = w.firstTask;\n        w.firstTask = null;\n        w.unlock(); // allow interrupts\n        boolean completedAbruptly = true;\n        try {\n            while (task != null || (task = getTask()) != null) {\n                w.lock();\n                // If pool is stopping, ensure thread is interrupted;\n                // if not, ensure thread is not interrupted.  This\n                // requires a recheck in second case to deal with\n                // shutdownNow race while clearing interrupt\n                if ((runStateAtLeast(ctl.get(), STOP) ||\n                     (Thread.interrupted() &&\n                      runStateAtLeast(ctl.get(), STOP))) &&\n                    !wt.isInterrupted())\n                    wt.interrupt();\n                try {\n                    beforeExecute(wt, task);\n                    try {\n                        task.run();\n                        afterExecute(task, null);\n                    } catch (Throwable ex) {\n                        afterExecute(task, ex);\n                        throw ex;\n                    }\n                } finally {\n                    task = null;\n                    w.completedTasks++;\n                    w.unlock();\n                }\n            }\n            completedAbruptly = false;\n        } finally {\n            processWorkerExit(w, completedAbruptly);\n        }\n    }\n\n    // Public constructors and methods\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters, the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}\n     * and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * <p>It may be more convenient to use one of the {@link Executors}\n     * factory methods instead of this general purpose constructor.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the {@linkplain ThreadPoolExecutor.AbortPolicy\n     * default rejected execution handler}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             threadFactory, defaultHandler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters and the\n     * {@linkplain Executors#defaultThreadFactory default thread factory}.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              RejectedExecutionHandler handler) {\n        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,\n             Executors.defaultThreadFactory(), handler);\n    }\n\n    /**\n     * Creates a new {@code ThreadPoolExecutor} with the given initial\n     * parameters.\n     *\n     * @param corePoolSize the number of threads to keep in the pool, even\n     *        if they are idle, unless {@code allowCoreThreadTimeOut} is set\n     * @param maximumPoolSize the maximum number of threads to allow in the\n     *        pool\n     * @param keepAliveTime when the number of threads is greater than\n     *        the core, this is the maximum time that excess idle threads\n     *        will wait for new tasks before terminating.\n     * @param unit the time unit for the {@code keepAliveTime} argument\n     * @param workQueue the queue to use for holding tasks before they are\n     *        executed.  This queue will hold only the {@code Runnable}\n     *        tasks submitted by the {@code execute} method.\n     * @param threadFactory the factory to use when the executor\n     *        creates a new thread\n     * @param handler the handler to use when execution is blocked\n     *        because the thread bounds and queue capacities are reached\n     * @throws IllegalArgumentException if one of the following holds:<br>\n     *         {@code corePoolSize < 0}<br>\n     *         {@code keepAliveTime < 0}<br>\n     *         {@code maximumPoolSize <= 0}<br>\n     *         {@code maximumPoolSize < corePoolSize}\n     * @throws NullPointerException if {@code workQueue}\n     *         or {@code threadFactory} or {@code handler} is null\n     */\n    public ThreadPoolExecutor(int corePoolSize,\n                              int maximumPoolSize,\n                              long keepAliveTime,\n                              TimeUnit unit,\n                              BlockingQueue<Runnable> workQueue,\n                              ThreadFactory threadFactory,\n                              RejectedExecutionHandler handler) {\n        if (corePoolSize < 0 ||\n            maximumPoolSize <= 0 ||\n            maximumPoolSize < corePoolSize ||\n            keepAliveTime < 0)\n            throw new IllegalArgumentException();\n        if (workQueue == null || threadFactory == null || handler == null)\n            throw new NullPointerException();\n        this.corePoolSize = corePoolSize;\n        this.maximumPoolSize = maximumPoolSize;\n        this.workQueue = workQueue;\n        this.keepAliveTime = unit.toNanos(keepAliveTime);\n        this.threadFactory = threadFactory;\n        this.handler = handler;\n    }\n\n    /**\n     * Executes the given task sometime in the future.  The task\n     * may execute in a new thread or in an existing pooled thread.\n     *\n     * If the task cannot be submitted for execution, either because this\n     * executor has been shutdown or because its capacity has been reached,\n     * the task is handled by the current {@link RejectedExecutionHandler}.\n     *\n     * @param command the task to execute\n     * @throws RejectedExecutionException at discretion of\n     *         {@code RejectedExecutionHandler}, if the task\n     *         cannot be accepted for execution\n     * @throws NullPointerException if {@code command} is null\n     */\n    public void execute(Runnable command) {\n        if (command == null)\n            throw new NullPointerException();\n        /*\n         * Proceed in 3 steps:\n         *\n         * 1. If fewer than corePoolSize threads are running, try to\n         * start a new thread with the given command as its first\n         * task.  The call to addWorker atomically checks runState and\n         * workerCount, and so prevents false alarms that would add\n         * threads when it shouldn't, by returning false.\n         *\n         * 2. If a task can be successfully queued, then we still need\n         * to double-check whether we should have added a thread\n         * (because existing ones died since last checking) or that\n         * the pool shut down since entry into this method. So we\n         * recheck state and if necessary roll back the enqueuing if\n         * stopped, or start a new thread if there are none.\n         *\n         * 3. If we cannot queue task, then we try to add a new\n         * thread.  If it fails, we know we are shut down or saturated\n         * and so reject the task.\n         */\n        int c = ctl.get();\n        if (workerCountOf(c) < corePoolSize) {\n            if (addWorker(command, true))\n                return;\n            c = ctl.get();\n        }\n        if (isRunning(c) && workQueue.offer(command)) {\n            int recheck = ctl.get();\n            if (! isRunning(recheck) && remove(command))\n                reject(command);\n            else if (workerCountOf(recheck) == 0)\n                addWorker(null, false);\n        }\n        else if (!addWorker(command, false))\n            reject(command);\n    }\n\n    /**\n     * Initiates an orderly shutdown in which previously submitted\n     * tasks are executed, but no new tasks will be accepted.\n     * Invocation has no additional effect if already shut down.\n     *\n     * <p>This method does not wait for previously submitted tasks to\n     * complete execution.  Use {@link #awaitTermination awaitTermination}\n     * to do that.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public void shutdown() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(SHUTDOWN);\n            interruptIdleWorkers();\n            onShutdown(); // hook for ScheduledThreadPoolExecutor\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n    }\n\n    /**\n     * Attempts to stop all actively executing tasks, halts the\n     * processing of waiting tasks, and returns a list of the tasks\n     * that were awaiting execution. These tasks are drained (removed)\n     * from the task queue upon return from this method.\n     *\n     * <p>This method does not wait for actively executing tasks to\n     * terminate.  Use {@link #awaitTermination awaitTermination} to\n     * do that.\n     *\n     * <p>There are no guarantees beyond best-effort attempts to stop\n     * processing actively executing tasks.  This implementation\n     * interrupts tasks via {@link Thread#interrupt}; any task that\n     * fails to respond to interrupts may never terminate.\n     *\n     * @throws SecurityException {@inheritDoc}\n     */\n    public List<Runnable> shutdownNow() {\n        List<Runnable> tasks;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            checkShutdownAccess();\n            advanceRunState(STOP);\n            interruptWorkers();\n            tasks = drainQueue();\n        } finally {\n            mainLock.unlock();\n        }\n        tryTerminate();\n        return tasks;\n    }\n\n    public boolean isShutdown() {\n        return runStateAtLeast(ctl.get(), SHUTDOWN);\n    }\n\n    /** Used by ScheduledThreadPoolExecutor. */\n    boolean isStopped() {\n        return runStateAtLeast(ctl.get(), STOP);\n    }\n\n    /**\n     * Returns true if this executor is in the process of terminating\n     * after {@link #shutdown} or {@link #shutdownNow} but has not\n     * completely terminated.  This method may be useful for\n     * debugging. A return of {@code true} reported a sufficient\n     * period after shutdown may indicate that submitted tasks have\n     * ignored or suppressed interruption, causing this executor not\n     * to properly terminate.\n     *\n     * @return {@code true} if terminating but not yet terminated\n     */\n    public boolean isTerminating() {\n        int c = ctl.get();\n        return runStateAtLeast(c, SHUTDOWN) && runStateLessThan(c, TERMINATED);\n    }\n\n    public boolean isTerminated() {\n        return runStateAtLeast(ctl.get(), TERMINATED);\n    }\n\n    public boolean awaitTermination(long timeout, TimeUnit unit)\n        throws InterruptedException {\n        long nanos = unit.toNanos(timeout);\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            while (runStateLessThan(ctl.get(), TERMINATED)) {\n                if (nanos <= 0L)\n                    return false;\n                nanos = termination.awaitNanos(nanos);\n            }\n            return true;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    // Override without \"throws Throwable\" for compatibility with subclasses\n    // whose finalize method invokes super.finalize() (as is recommended).\n    // Before JDK 11, finalize() had a non-empty method body.\n\n    /**\n     * @implNote Previous versions of this class had a finalize method\n     * that shut down this executor, but in this version, finalize\n     * does nothing.\n     */\n    @Deprecated(since=\"9\")\n    protected void finalize() {}\n\n    /**\n     * Sets the thread factory used to create new threads.\n     *\n     * @param threadFactory the new thread factory\n     * @throws NullPointerException if threadFactory is null\n     * @see #getThreadFactory\n     */\n    public void setThreadFactory(ThreadFactory threadFactory) {\n        if (threadFactory == null)\n            throw new NullPointerException();\n        this.threadFactory = threadFactory;\n    }\n\n    /**\n     * Returns the thread factory used to create new threads.\n     *\n     * @return the current thread factory\n     * @see #setThreadFactory(ThreadFactory)\n     */\n    public ThreadFactory getThreadFactory() {\n        return threadFactory;\n    }\n\n    /**\n     * Sets a new handler for unexecutable tasks.\n     *\n     * @param handler the new handler\n     * @throws NullPointerException if handler is null\n     * @see #getRejectedExecutionHandler\n     */\n    public void setRejectedExecutionHandler(RejectedExecutionHandler handler) {\n        if (handler == null)\n            throw new NullPointerException();\n        this.handler = handler;\n    }\n\n    /**\n     * Returns the current handler for unexecutable tasks.\n     *\n     * @return the current handler\n     * @see #setRejectedExecutionHandler(RejectedExecutionHandler)\n     */\n    public RejectedExecutionHandler getRejectedExecutionHandler() {\n        return handler;\n    }\n\n    /**\n     * Sets the core number of threads.  This overrides any value set\n     * in the constructor.  If the new value is smaller than the\n     * current value, excess existing threads will be terminated when\n     * they next become idle.  If larger, new threads will, if needed,\n     * be started to execute any queued tasks.\n     *\n     * @param corePoolSize the new core size\n     * @throws IllegalArgumentException if {@code corePoolSize < 0}\n     *         or {@code corePoolSize} is greater than the {@linkplain\n     *         #getMaximumPoolSize() maximum pool size}\n     * @see #getCorePoolSize\n     */\n    public void setCorePoolSize(int corePoolSize) {\n        if (corePoolSize < 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        int delta = corePoolSize - this.corePoolSize;\n        this.corePoolSize = corePoolSize;\n        if (workerCountOf(ctl.get()) > corePoolSize)\n            interruptIdleWorkers();\n        else if (delta > 0) {\n            // We don't really know how many new threads are \"needed\".\n            // As a heuristic, prestart enough new workers (up to new\n            // core size) to handle the current number of tasks in\n            // queue, but stop if queue becomes empty while doing so.\n            int k = Math.min(delta, workQueue.size());\n            while (k-- > 0 && addWorker(null, true)) {\n                if (workQueue.isEmpty())\n                    break;\n            }\n        }\n    }\n\n    /**\n     * Returns the core number of threads.\n     *\n     * @return the core number of threads\n     * @see #setCorePoolSize\n     */\n    public int getCorePoolSize() {\n        return corePoolSize;\n    }\n\n    /**\n     * Starts a core thread, causing it to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed. This method will return {@code false}\n     * if all core threads have already been started.\n     *\n     * @return {@code true} if a thread was started\n     */\n    public boolean prestartCoreThread() {\n        return workerCountOf(ctl.get()) < corePoolSize &&\n            addWorker(null, true);\n    }\n\n    /**\n     * Same as prestartCoreThread except arranges that at least one\n     * thread is started even if corePoolSize is 0.\n     */\n    void ensurePrestart() {\n        int wc = workerCountOf(ctl.get());\n        if (wc < corePoolSize)\n            addWorker(null, true);\n        else if (wc == 0)\n            addWorker(null, false);\n    }\n\n    /**\n     * Starts all core threads, causing them to idly wait for work. This\n     * overrides the default policy of starting core threads only when\n     * new tasks are executed.\n     *\n     * @return the number of threads started\n     */\n    public int prestartAllCoreThreads() {\n        int n = 0;\n        while (addWorker(null, true))\n            ++n;\n        return n;\n    }\n\n    /**\n     * Returns true if this pool allows core threads to time out and\n     * terminate if no tasks arrive within the keepAlive time, being\n     * replaced if needed when new tasks arrive. When true, the same\n     * keep-alive policy applying to non-core threads applies also to\n     * core threads. When false (the default), core threads are never\n     * terminated due to lack of incoming tasks.\n     *\n     * @return {@code true} if core threads are allowed to time out,\n     *         else {@code false}\n     *\n     * @since 1.6\n     */\n    public boolean allowsCoreThreadTimeOut() {\n        return allowCoreThreadTimeOut;\n    }\n\n    /**\n     * Sets the policy governing whether core threads may time out and\n     * terminate if no tasks arrive within the keep-alive time, being\n     * replaced if needed when new tasks arrive. When false, core\n     * threads are never terminated due to lack of incoming\n     * tasks. When true, the same keep-alive policy applying to\n     * non-core threads applies also to core threads. To avoid\n     * continual thread replacement, the keep-alive time must be\n     * greater than zero when setting {@code true}. This method\n     * should in general be called before the pool is actively used.\n     *\n     * @param value {@code true} if should time out, else {@code false}\n     * @throws IllegalArgumentException if value is {@code true}\n     *         and the current keep-alive time is not greater than zero\n     *\n     * @since 1.6\n     */\n    public void allowCoreThreadTimeOut(boolean value) {\n        if (value && keepAliveTime <= 0)\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        if (value != allowCoreThreadTimeOut) {\n            allowCoreThreadTimeOut = value;\n            if (value)\n                interruptIdleWorkers();\n        }\n    }\n\n    /**\n     * Sets the maximum allowed number of threads. This overrides any\n     * value set in the constructor. If the new value is smaller than\n     * the current value, excess existing threads will be\n     * terminated when they next become idle.\n     *\n     * @param maximumPoolSize the new maximum\n     * @throws IllegalArgumentException if the new maximum is\n     *         less than or equal to zero, or\n     *         less than the {@linkplain #getCorePoolSize core pool size}\n     * @see #getMaximumPoolSize\n     */\n    public void setMaximumPoolSize(int maximumPoolSize) {\n        if (maximumPoolSize <= 0 || maximumPoolSize < corePoolSize)\n            throw new IllegalArgumentException();\n        this.maximumPoolSize = maximumPoolSize;\n        if (workerCountOf(ctl.get()) > maximumPoolSize)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the maximum allowed number of threads.\n     *\n     * @return the maximum allowed number of threads\n     * @see #setMaximumPoolSize\n     */\n    public int getMaximumPoolSize() {\n        return maximumPoolSize;\n    }\n\n    /**\n     * Sets the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     * This overrides any value set in the constructor.\n     *\n     * @param time the time to wait.  A time value of zero will cause\n     *        excess threads to terminate immediately after executing tasks.\n     * @param unit the time unit of the {@code time} argument\n     * @throws IllegalArgumentException if {@code time} less than zero or\n     *         if {@code time} is zero and {@code allowsCoreThreadTimeOut}\n     * @see #getKeepAliveTime(TimeUnit)\n     */\n    public void setKeepAliveTime(long time, TimeUnit unit) {\n        if (time < 0)\n            throw new IllegalArgumentException();\n        if (time == 0 && allowsCoreThreadTimeOut())\n            throw new IllegalArgumentException(\"Core threads must have nonzero keep alive times\");\n        long keepAliveTime = unit.toNanos(time);\n        long delta = keepAliveTime - this.keepAliveTime;\n        this.keepAliveTime = keepAliveTime;\n        if (delta < 0)\n            interruptIdleWorkers();\n    }\n\n    /**\n     * Returns the thread keep-alive time, which is the amount of time\n     * that threads may remain idle before being terminated.\n     * Threads that wait this amount of time without processing a\n     * task will be terminated if there are more than the core\n     * number of threads currently in the pool, or if this pool\n     * {@linkplain #allowsCoreThreadTimeOut() allows core thread timeout}.\n     *\n     * @param unit the desired time unit of the result\n     * @return the time limit\n     * @see #setKeepAliveTime(long, TimeUnit)\n     */\n    public long getKeepAliveTime(TimeUnit unit) {\n        return unit.convert(keepAliveTime, TimeUnit.NANOSECONDS);\n    }\n\n    /* User-level queue utilities */\n\n    /**\n     * Returns the task queue used by this executor. Access to the\n     * task queue is intended primarily for debugging and monitoring.\n     * This queue may be in active use.  Retrieving the task queue\n     * does not prevent queued tasks from executing.\n     *\n     * @return the task queue\n     */\n    public BlockingQueue<Runnable> getQueue() {\n        return workQueue;\n    }\n\n    /**\n     * Removes this task from the executor's internal queue if it is\n     * present, thus causing it not to be run if it has not already\n     * started.\n     *\n     * <p>This method may be useful as one part of a cancellation\n     * scheme.  It may fail to remove tasks that have been converted\n     * into other forms before being placed on the internal queue.\n     * For example, a task entered using {@code submit} might be\n     * converted into a form that maintains {@code Future} status.\n     * However, in such cases, method {@link #purge} may be used to\n     * remove those Futures that have been cancelled.\n     *\n     * @param task the task to remove\n     * @return {@code true} if the task was removed\n     */\n    public boolean remove(Runnable task) {\n        boolean removed = workQueue.remove(task);\n        tryTerminate(); // In case SHUTDOWN and now empty\n        return removed;\n    }\n\n    /**\n     * Tries to remove from the work queue all {@link Future}\n     * tasks that have been cancelled. This method can be useful as a\n     * storage reclamation operation, that has no other impact on\n     * functionality. Cancelled tasks are never executed, but may\n     * accumulate in work queues until worker threads can actively\n     * remove them. Invoking this method instead tries to remove them now.\n     * However, this method may fail to remove tasks in\n     * the presence of interference by other threads.\n     */\n    public void purge() {\n        final BlockingQueue<Runnable> q = workQueue;\n        try {\n            Iterator<Runnable> it = q.iterator();\n            while (it.hasNext()) {\n                Runnable r = it.next();\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    it.remove();\n            }\n        } catch (ConcurrentModificationException fallThrough) {\n            // Take slow path if we encounter interference during traversal.\n            // Make copy for traversal and call remove for cancelled entries.\n            // The slow path is more likely to be O(N*N).\n            for (Object r : q.toArray())\n                if (r instanceof Future<?> && ((Future<?>)r).isCancelled())\n                    q.remove(r);\n        }\n\n        tryTerminate(); // In case SHUTDOWN and now empty\n    }\n\n    /* Statistics */\n\n    /**\n     * Returns the current number of threads in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            // Remove rare and surprising possibility of\n            // isTerminated() && getPoolSize() > 0\n            return runStateAtLeast(ctl.get(), TIDYING) ? 0\n                : workers.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate number of threads that are actively\n     * executing tasks.\n     *\n     * @return the number of threads\n     */\n    public int getActiveCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            int n = 0;\n            for (Worker w : workers)\n                if (w.isLocked())\n                    ++n;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the largest number of threads that have ever\n     * simultaneously been in the pool.\n     *\n     * @return the number of threads\n     */\n    public int getLargestPoolSize() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            return largestPoolSize;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have ever been\n     * scheduled for execution. Because the states of tasks and\n     * threads may change dynamically during computation, the returned\n     * value is only an approximation.\n     *\n     * @return the number of tasks\n     */\n    public long getTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers) {\n                n += w.completedTasks;\n                if (w.isLocked())\n                    ++n;\n            }\n            return n + workQueue.size();\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns the approximate total number of tasks that have\n     * completed execution. Because the states of tasks and threads\n     * may change dynamically during computation, the returned value\n     * is only an approximation, but one that does not ever decrease\n     * across successive calls.\n     *\n     * @return the number of tasks\n     */\n    public long getCompletedTaskCount() {\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            long n = completedTaskCount;\n            for (Worker w : workers)\n                n += w.completedTasks;\n            return n;\n        } finally {\n            mainLock.unlock();\n        }\n    }\n\n    /**\n     * Returns a string identifying this pool, as well as its state,\n     * including indications of run state and estimated worker and\n     * task counts.\n     *\n     * @return a string identifying this pool, as well as its state\n     */\n    public String toString() {\n        long ncompleted;\n        int nworkers, nactive;\n        final ReentrantLock mainLock = this.mainLock;\n        mainLock.lock();\n        try {\n            ncompleted = completedTaskCount;\n            nactive = 0;\n            nworkers = workers.size();\n            for (Worker w : workers) {\n                ncompleted += w.completedTasks;\n                if (w.isLocked())\n                    ++nactive;\n            }\n        } finally {\n            mainLock.unlock();\n        }\n        int c = ctl.get();\n        String runState =\n            isRunning(c) ? \"Running\" :\n            runStateAtLeast(c, TERMINATED) ? \"Terminated\" :\n            \"Shutting down\";\n        return super.toString() +\n            \"[\" + runState +\n            \", pool size = \" + nworkers +\n            \", active threads = \" + nactive +\n            \", queued tasks = \" + workQueue.size() +\n            \", completed tasks = \" + ncompleted +\n            \"]\";\n    }\n\n    /* Extension hooks */\n\n    /**\n     * Method invoked prior to executing the given Runnable in the\n     * given thread.  This method is invoked by thread {@code t} that\n     * will execute task {@code r}, and may be used to re-initialize\n     * ThreadLocals, or to perform logging.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.beforeExecute} at the end of\n     * this method.\n     *\n     * @param t the thread that will run task {@code r}\n     * @param r the task that will be executed\n     */\n    protected void beforeExecute(Thread t, Runnable r) { }\n\n    /**\n     * Method invoked upon completion of execution of the given Runnable.\n     * This method is invoked by the thread that executed the task. If\n     * non-null, the Throwable is the uncaught {@code RuntimeException}\n     * or {@code Error} that caused execution to terminate abruptly.\n     *\n     * <p>This implementation does nothing, but may be customized in\n     * subclasses. Note: To properly nest multiple overridings, subclasses\n     * should generally invoke {@code super.afterExecute} at the\n     * beginning of this method.\n     *\n     * <p><b>Note:</b> When actions are enclosed in tasks (such as\n     * {@link FutureTask}) either explicitly or via methods such as\n     * {@code submit}, these task objects catch and maintain\n     * computational exceptions, and so they do not cause abrupt\n     * termination, and the internal exceptions are <em>not</em>\n     * passed to this method. If you would like to trap both kinds of\n     * failures in this method, you can further probe for such cases,\n     * as in this sample subclass that prints either the direct cause\n     * or the underlying exception if a task has been aborted:\n     *\n     * <pre> {@code\n     * class ExtendedExecutor extends ThreadPoolExecutor {\n     *   // ...\n     *   protected void afterExecute(Runnable r, Throwable t) {\n     *     super.afterExecute(r, t);\n     *     if (t == null\n     *         && r instanceof Future<?>\n     *         && ((Future<?>)r).isDone()) {\n     *       try {\n     *         Object result = ((Future<?>) r).get();\n     *       } catch (CancellationException ce) {\n     *         t = ce;\n     *       } catch (ExecutionException ee) {\n     *         t = ee.getCause();\n     *       } catch (InterruptedException ie) {\n     *         // ignore/reset\n     *         Thread.currentThread().interrupt();\n     *       }\n     *     }\n     *     if (t != null)\n     *       System.out.println(t);\n     *   }\n     * }}</pre>\n     *\n     * @param r the runnable that has completed\n     * @param t the exception that caused termination, or null if\n     * execution completed normally\n     */\n    protected void afterExecute(Runnable r, Throwable t) { }\n\n    /**\n     * Method invoked when the Executor has terminated.  Default\n     * implementation does nothing. Note: To properly nest multiple\n     * overridings, subclasses should generally invoke\n     * {@code super.terminated} within this method.\n     */\n    protected void terminated() { }\n\n    /* Predefined RejectedExecutionHandlers */\n\n    /**\n     * A handler for rejected tasks that runs the rejected task\n     * directly in the calling thread of the {@code execute} method,\n     * unless the executor has been shut down, in which case the task\n     * is discarded.\n     */\n    public static class CallerRunsPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code CallerRunsPolicy}.\n         */\n        public CallerRunsPolicy() { }\n\n        /**\n         * Executes task r in the caller's thread, unless the executor\n         * has been shut down, in which case the task is discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                r.run();\n            }\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that throws a\n     * {@link RejectedExecutionException}.\n     *\n     * This is the default handler for {@link ThreadPoolExecutor} and\n     * {@link ScheduledThreadPoolExecutor}.\n     */\n    public static class AbortPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates an {@code AbortPolicy}.\n         */\n        public AbortPolicy() { }\n\n        /**\n         * Always throws RejectedExecutionException.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         * @throws RejectedExecutionException always\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            throw new RejectedExecutionException(\"Task \" + r.toString() +\n                                                 \" rejected from \" +\n                                                 e.toString());\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that silently discards the\n     * rejected task.\n     */\n    public static class DiscardPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardPolicy}.\n         */\n        public DiscardPolicy() { }\n\n        /**\n         * Does nothing, which has the effect of discarding task r.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n        }\n    }\n\n    /**\n     * A handler for rejected tasks that discards the oldest unhandled\n     * request and then retries {@code execute}, unless the executor\n     * is shut down, in which case the task is discarded. This policy is\n     * rarely useful in cases where other threads may be waiting for\n     * tasks to terminate, or failures must be recorded. Instead consider\n     * using a handler of the form:\n     * <pre> {@code\n     * new RejectedExecutionHandler() {\n     *   public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n     *     Runnable dropped = e.getQueue().poll();\n     *     if (dropped instanceof Future<?>) {\n     *       ((Future<?>)dropped).cancel(false);\n     *       // also consider logging the failure\n     *     }\n     *     e.execute(r);  // retry\n     * }}}</pre>\n     */\n    public static class DiscardOldestPolicy implements RejectedExecutionHandler {\n        /**\n         * Creates a {@code DiscardOldestPolicy} for the given executor.\n         */\n        public DiscardOldestPolicy() { }\n\n        /**\n         * Obtains and ignores the next task that the executor\n         * would otherwise execute, if one is immediately available,\n         * and then retries execution of task r, unless the executor\n         * is shut down, in which case task r is instead discarded.\n         *\n         * @param r the runnable task requested to be executed\n         * @param e the executor attempting to execute this task\n         */\n        public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {\n            if (!e.isShutdown()) {\n                e.getQueue().poll();\n                e.execute(r);\n            }\n        }\n    }\n}\n",
			"lineHeight": 1.2,
			"baseline": 40882
		},
		{
			"type": "rectangle",
			"version": 410,
			"versionNonce": 885266728,
			"isDeleted": false,
			"id": "O8q29OUubiYYe-bntwt9_",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 852.3212611043217,
			"y": 9084.182635495228,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 459.0881620697752,
			"height": 248.13747400355044,
			"seed": 1773817128,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": {
				"type": 3
			},
			"boundElements": [
				{
					"type": "arrow",
					"id": "YHCkOjSZ"
				}
			],
			"updated": 1704364587781,
			"link": null,
			"locked": false
		},
		{
			"type": "text",
			"version": 275,
			"versionNonce": 1245099048,
			"isDeleted": false,
			"id": "9kmxIQru",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 878.6991837573081,
			"y": 9115.563117907242,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 377.05078125,
			"height": 19.805592706359263,
			"seed": 1103613784,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364587781,
			"link": null,
			"locked": false,
			"fontSize": 16.50466058863272,
			"fontFamily": 3,
			"text": "0000 0000 0000 0000 0000 0000 0000 0001",
			"rawText": "0000 0000 0000 0000 0000 0000 0000 0001",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0000 0000 0000 0000 0000 0000 0000 0001",
			"lineHeight": 1.2,
			"baseline": 16
		},
		{
			"type": "text",
			"version": 275,
			"versionNonce": 1822289704,
			"isDeleted": false,
			"id": "ofOIy6n7",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 878.6991837573081,
			"y": 9145.684123481498,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 135.3515625,
			"height": 19.805592706359263,
			"seed": 2061595176,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364587781,
			"link": null,
			"locked": false,
			"fontSize": 16.50466058863272,
			"fontFamily": 3,
			"text": "1<< COUNT_BITS",
			"rawText": "1<< COUNT_BITS",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1<< COUNT_BITS",
			"lineHeight": 1.2,
			"baseline": 16
		},
		{
			"type": "text",
			"version": 353,
			"versionNonce": 1415957032,
			"isDeleted": false,
			"id": "7h9LiOwn",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 880.1416523239459,
			"y": 9180.536239329225,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 377.05078125,
			"height": 138.63914894451483,
			"seed": 1281202472,
			"groupIds": [
				"Yhfrf9v_rSqZbw8Qhjj6m"
			],
			"frameId": null,
			"roundness": null,
			"boundElements": [
				{
					"id": "YHCkOjSZ",
					"type": "arrow"
				}
			],
			"updated": 1704364587781,
			"link": null,
			"locked": false,
			"fontSize": 16.50466058863272,
			"fontFamily": 3,
			"text": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"rawText": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0010 0000 0000 0000 0000 0000 0000 0000\n\n-1\n\n0001 1111 1111 1111 1111 1111 1111 1111\n\n=536870911",
			"lineHeight": 1.2,
			"baseline": 134
		},
		{
			"type": "arrow",
			"version": 465,
			"versionNonce": 1283081512,
			"isDeleted": false,
			"id": "YHCkOjSZ",
			"fillStyle": "hachure",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 1,
			"opacity": 100,
			"angle": 0,
			"x": 408.60789244347706,
			"y": 9236.93344679747,
			"strokeColor": "#e03131",
			"backgroundColor": "transparent",
			"width": 439.71336866084476,
			"height": 4.776305274644983,
			"seed": 29075,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364587782,
			"link": null,
			"locked": false,
			"startBinding": {
				"elementId": "cO2ShusooszR-hj-UXi7k",
				"focus": -0.1391971606058405,
				"gap": 5.250099634078424
			},
			"endBinding": {
				"elementId": "7h9LiOwn",
				"focus": 0.1645206009121663,
				"gap": 31.82039121962407
			},
			"lastCommittedPoint": null,
			"startArrowhead": null,
			"endArrowhead": "triangle",
			"points": [
				[
					0,
					0
				],
				[
					67.5716371881328,
					4.776305274644983
				],
				[
					439.71336866084476,
					2.6166217018744646
				]
			]
		},
		{
			"type": "text",
			"version": 122,
			"versionNonce": 1084814120,
			"isDeleted": false,
			"id": "1OPjqjHo",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 360.0233328383608,
			"y": 9299.091251978036,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 9.375,
			"height": 19.2,
			"seed": 1763383640,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704366314751,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "0",
			"rawText": "0",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "0",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 77,
			"versionNonce": 59191128,
			"isDeleted": false,
			"id": "XCFyHt02",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 350.3774861086912,
			"y": 9275.60388496745,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 93.75,
			"height": 19.2,
			"seed": 376300120,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "-536870912",
			"rawText": "-536870912",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "-536870912",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 99,
			"versionNonce": 1061687848,
			"isDeleted": false,
			"id": "3wqx3EmY",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 348.5112176456528,
			"y": 9316.957549877821,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 84.375,
			"height": 19.2,
			"seed": 266906152,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "536870912",
			"rawText": "536870912",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "536870912",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 133,
			"versionNonce": 1184334936,
			"isDeleted": false,
			"id": "qYpWM769",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 349.11853912647666,
			"y": 9334.556819446552,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 93.75,
			"height": 19.2,
			"seed": 1742792792,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1073741824",
			"rawText": "1073741824",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1073741824",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 83,
			"versionNonce": 869502248,
			"isDeleted": false,
			"id": "l1Md3CGB",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 348.956883971714,
			"y": 9352.00620672158,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 93.75,
			"height": 19.2,
			"seed": 1801215784,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "1610612736",
			"rawText": "1610612736",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "1610612736",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 19,
			"versionNonce": 1322596696,
			"isDeleted": false,
			"id": "C1v7WYVX",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 955.4696955050911,
			"y": 9413.111265624279,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 152.25,
			"height": 19.2,
			"seed": 403328600,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "or 操作，操作符“|”",
			"rawText": "or 操作，操作符“|”",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "or 操作，操作符“|”",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"type": "text",
			"version": 99,
			"versionNonce": 523741224,
			"isDeleted": false,
			"id": "PI9c484y",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"angle": 0,
			"x": 956.8046043698942,
			"y": 9440.95225286945,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"width": 914.75,
			"height": 19.2,
			"seed": 1149996840,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"boundElements": [],
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"fontSize": 16,
			"fontFamily": 3,
			"text": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"rawText": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"textAlign": "left",
			"verticalAlign": "top",
			"containerId": null,
			"originalText": "定义：称为按位或运算符。它对整型参数的每一个二进制位进行布尔或操作，即两个对应的二进制位，任意一个为1时，就等于1。",
			"lineHeight": 1.2,
			"baseline": 15
		},
		{
			"id": "hdo43w4V",
			"type": "text",
			"x": 454.0478972597725,
			"y": 9275.48928704665,
			"width": 365.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 163512152,
			"version": 197,
			"versionNonce": 916579672,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364613445,
			"link": null,
			"locked": false,
			"text": "1110 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "1110 0000 0000 0000 0000 0000 0000 0000",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1110 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2
		},
		{
			"id": "orrySlph",
			"type": "text",
			"x": 473.7777422058409,
			"y": 9318.62555192286,
			"width": 346.875,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1705953064,
			"version": 260,
			"versionNonce": 652386136,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364746309,
			"link": null,
			"locked": false,
			"text": "10 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "10 0000 0000 0000 0000 0000 0000 0000",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "10 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2
		},
		{
			"id": "QNnuUB83",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9506.449669316824,
			"width": 161.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1177002328,
			"version": 1,
			"versionNonce": 0,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364687857,
			"link": null,
			"locked": false,
			"text": "and 操作，操作符“&”",
			"rawText": "and 操作，操作符“&”",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "and 操作，操作符“&”",
			"lineHeight": 1.2
		},
		{
			"id": "daZJgN0g",
			"type": "text",
			"x": 966.5381425630889,
			"y": 9541.489856518532,
			"width": 866.75,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 964676184,
			"version": 65,
			"versionNonce": 1755551320,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364706335,
			"link": null,
			"locked": false,
			"text": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"rawText": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "定义：称为按位与运算符。它对整型参数的每一个二进制位进行布尔与操作，即两个对应的二进制位同时为1时，才等于1。",
			"lineHeight": 1.2
		},
		{
			"id": "Jn4zVZDH",
			"type": "text",
			"x": 474.84048567934553,
			"y": 9337.338770682594,
			"width": 356.25,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1029211736,
			"version": 304,
			"versionNonce": 86766936,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364771226,
			"link": null,
			"locked": false,
			"text": "100 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "100 0000 0000 0000 0000 0000 0000 0000",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "100 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2
		},
		{
			"id": "sqVkHS59",
			"type": "text",
			"x": 473.9156678447381,
			"y": 9356.373744895987,
			"width": 365.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 179640104,
			"version": 339,
			"versionNonce": 1109044312,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364762842,
			"link": null,
			"locked": false,
			"text": "1000 0000 0000 0000 0000 0000 0000 0000",
			"rawText": "1000 0000 0000 0000 0000 0000 0000 0000",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1000 0000 0000 0000 0000 0000 0000 0000",
			"lineHeight": 1.2
		},
		{
			"id": "61pEFIUS",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9627.957894959016,
			"width": 152.25,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 233231448,
			"version": 1,
			"versionNonce": 0,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364812012,
			"link": null,
			"locked": false,
			"text": "not操作，操作符“~”",
			"rawText": "not操作，操作符“~”",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "not操作，操作符“~”",
			"lineHeight": 1.2
		},
		{
			"id": "1J0eQdto",
			"type": "text",
			"x": 950.4952937440763,
			"y": 9661.620701213538,
			"width": 624,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1619912792,
			"version": 101,
			"versionNonce": 232131160,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704364823464,
			"link": null,
			"locked": false,
			"text": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"rawText": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "定义：称为按位非运算符。它是一个单运算符，对运算数的所有二进制位进行取反操作。",
			"lineHeight": 1.2
		},
		{
			"id": "r7sfONdW",
			"type": "text",
			"x": 212.80299766114672,
			"y": 19012.339770068458,
			"width": 177.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 437132376,
			"version": 285,
			"versionNonce": 115508824,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704366573482,
			"link": null,
			"locked": false,
			"text": "相当于不是running时候",
			"rawText": "相当于不是running时候",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "相当于不是running时候",
			"lineHeight": 1.2
		},
		{
			"id": "jryd9p9A",
			"type": "text",
			"x": 208.51357848706493,
			"y": 19031.803311123356,
			"width": 220.625,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 2063271256,
			"version": 133,
			"versionNonce": 252751192,
			"isDeleted": false,
			"boundElements": null,
			"updated": 1704366615287,
			"link": null,
			"locked": false,
			"text": "不是running或shutdown状态",
			"rawText": "不是running或shutdown状态",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "不是running或shutdown状态",
			"lineHeight": 1.2
		},
		{
			"id": "qaWqv7Hl",
			"type": "text",
			"x": 843.8796273218377,
			"y": 9019.020120476309,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1700333912,
			"version": 5,
			"versionNonce": 1566597928,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364401184,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "CROEahXM",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9564.849669316825,
			"width": 37.5,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1251983400,
			"version": 2,
			"versionNonce": 508046936,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "1001",
			"rawText": "1001",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1001",
			"lineHeight": 1.2
		},
		{
			"id": "EYGoZMJB",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9594.049669316826,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 5879384,
			"version": 2,
			"versionNonce": 1817871144,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "2",
			"rawText": "2",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "2",
			"lineHeight": 1.2
		},
		{
			"id": "WC6RbFpk",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9623.249669316827,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 335903528,
			"version": 2,
			"versionNonce": 2128651096,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "mqa9dZuj",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9652.449669316828,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 897657688,
			"version": 2,
			"versionNonce": 1466949160,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "tTN1J18b",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9681.649669316828,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1056691752,
			"version": 2,
			"versionNonce": 2124576856,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "N0rNasp2",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9710.849669316829,
			"width": 37.5,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 535953496,
			"version": 2,
			"versionNonce": 220322088,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "1101",
			"rawText": "1101",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1101",
			"lineHeight": 1.2
		},
		{
			"id": "PELAz6xd",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9740.04966931683,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 275697960,
			"version": 2,
			"versionNonce": 828484952,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "2",
			"rawText": "2",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "2",
			"lineHeight": 1.2
		},
		{
			"id": "n5RQR2iY",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9769.24966931683,
			"width": 37.5,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 260060504,
			"version": 2,
			"versionNonce": 1356624936,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "1001",
			"rawText": "1001",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1001",
			"lineHeight": 1.2
		},
		{
			"id": "LXztZRXa",
			"type": "text",
			"x": 966.7844992357788,
			"y": 9798.449669316831,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1338666024,
			"version": 2,
			"versionNonce": 275923544,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364694573,
			"link": null,
			"locked": false,
			"text": "2",
			"rawText": "2",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "2",
			"lineHeight": 1.2
		},
		{
			"id": "c8E5QByO",
			"type": "text",
			"x": 983.5442544967707,
			"y": 9810.91855655818,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1839404120,
			"version": 2,
			"versionNonce": 71243816,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364692915,
			"link": null,
			"locked": false,
			"text": "",
			"rawText": "",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "",
			"lineHeight": 1.2
		},
		{
			"id": "RUI2sWkq",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9686.357894959017,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1205943592,
			"version": 2,
			"versionNonce": 1834621224,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "hSPaiDhI",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9715.557894959018,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 263883096,
			"version": 2,
			"versionNonce": 1070427480,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "fES5uxWH",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9744.757894959019,
			"width": 9.6328125,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1175410728,
			"version": 2,
			"versionNonce": 832552,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "�",
			"rawText": "�",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "�",
			"lineHeight": 1.2
		},
		{
			"id": "f6G3D7Dh",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9773.95789495902,
			"width": 37.5,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 1733271128,
			"version": 2,
			"versionNonce": 1533411928,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "1101",
			"rawText": "1101",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "1101",
			"lineHeight": 1.2
		},
		{
			"id": "6wR8s32s",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9803.15789495902,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 767693608,
			"version": 2,
			"versionNonce": 2008280872,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "2",
			"rawText": "2",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "2",
			"lineHeight": 1.2
		},
		{
			"id": "OQG3fyN0",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9832.357894959021,
			"width": 37.5,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 360321880,
			"version": 2,
			"versionNonce": 19134296,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364816262,
			"link": null,
			"locked": false,
			"text": "0010",
			"rawText": "0010",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "0010",
			"lineHeight": 1.2
		},
		{
			"id": "xZ9BYPOu",
			"type": "text",
			"x": 950.0247439747858,
			"y": 9861.557894959022,
			"width": 9.375,
			"height": 19.2,
			"angle": 0,
			"strokeColor": "#e03131",
			"backgroundColor": "#ffffff",
			"fillStyle": "solid",
			"strokeWidth": 2,
			"strokeStyle": "solid",
			"roughness": 0,
			"opacity": 100,
			"groupIds": [],
			"frameId": null,
			"roundness": null,
			"seed": 627404328,
			"version": 2,
			"versionNonce": 299893080,
			"isDeleted": true,
			"boundElements": null,
			"updated": 1704364818698,
			"link": null,
			"locked": false,
			"text": "2",
			"rawText": "2",
			"fontSize": 16,
			"fontFamily": 3,
			"textAlign": "left",
			"verticalAlign": "top",
			"baseline": 15,
			"containerId": null,
			"originalText": "2",
			"lineHeight": 1.2
		}
	],
	"appState": {
		"theme": "light",
		"viewBackgroundColor": "#ffffff",
		"currentItemStrokeColor": "#e03131",
		"currentItemBackgroundColor": "#ffffff",
		"currentItemFillStyle": "solid",
		"currentItemStrokeWidth": 2,
		"currentItemStrokeStyle": "solid",
		"currentItemRoughness": 0,
		"currentItemOpacity": 100,
		"currentItemFontFamily": 3,
		"currentItemFontSize": 16,
		"currentItemTextAlign": "left",
		"currentItemStartArrowhead": null,
		"currentItemEndArrowhead": "arrow",
		"scrollX": 401.23052394269445,
		"scrollY": -18362.278162107366,
		"zoom": {
			"value": 0.7160009089112283
		},
		"currentItemRoundness": "round",
		"gridSize": null,
		"gridColor": {
			"Bold": "#C9C9C9FF",
			"Regular": "#EDEDEDFF"
		},
		"currentStrokeOptions": null,
		"previousGridSize": null,
		"frameRendering": {
			"enabled": true,
			"clip": true,
			"name": true,
			"outline": true
		}
	},
	"files": {}
}
```
%%