#### AsyncTask

除了传统的thread,Android的线程形态还包括`AsyncTask`，`HandlerThread`，`IntentService`。这三者的底层实现也是线程。

`AsyncTask`是一种轻量级的异步任务类，它可以在线程池中执行后台任务，然后把结果传递给主线程并更新UI，`AsyncTask`底层封装了`Thread`、`Handler`。但是`AsyncTask`不适合做特别耗时的操作，耗时的操作还是通过线程池。

`AsyncTask`是一个抽象的泛型类，提供了`Params`、`Progress`、`Result`三个参数，`Params`为参数类型，`Progress`为后台执行任务的进度，`Result`后台任务返回的结果。

AsyncTask声明如下：
`public abstract class AsyncTask<Params,Progress,Result> `

`AsyncTask`提供四个核心方法：

 - `onPreExectue()`在主线程中执行，在异步任务执行之前，此方法被调用，一般用于做一些准备工作。
 - `doInBackground(Params...params)`在线程池中执行，此方法用于执行异步任务，params为异步任务的输入参数。在此方法中通过`publicProgress`方法来更新任务进度，`publicProgress`会调用`onProgressUpdate`方法，此方法需要返回计算结果给`onPostExecute`。
 - `onProgressUpdate(Progress..values)`，在主线程中执行，当后台任务执行进度发生改变时，该方法被调用。
 - `onPostExecute(Result..result)`在主线程中执行，在异步任务执行之后调用，参数result为`doInBackground`的返回值。
 
上面的方法，先调用`onPreExcute()`，然后是`doInBackground()`，最后是`onPostExecute()`。`AsyncTask`还提供`onCancelled()`，当异步任务被取消时，调用`onCancelled`，这时`onPostExecute()`不会被调用。

**注：无法在onInBackground中改变UI**
    **AsyncTask对象必须在主线程中创建，只能创建一次**
    **execute必须在UI线程中调用，只能调用一次**
