# Ktor-client 学习笔记

## Ktor介绍

Ktor 包含一个多平台异步 HTTP 客户端，它允许您发出请求和处理响应，使用插件扩展其功能，例如身份验证、JSON 序列化等。在本教程中，我们将创建一个简单的客户端应用程序来发送请求和接收响应。

## 添加依赖项
1. 打开工程下的 ***build.gradle*** 文件并添加以下行来指定 Ktor 版本
    ``` kotlin
    ext.versions = [
      'ktor_version': '1.0.0'
    ]
    ```
1. 打开需要用到的module或者app目录的 ***build.gradle*** 添加依赖
    ```kotlin
    dependencies {

    // ktor 网络请求模块
    implementation "io.ktor:ktor-client-core:$ktor_version"
    implementation "io.ktor:ktor-client-cio:$ktor_version"
}
    ```
1. 创建一个


## 三 Ktor 服务端的使用
我们可以通过多种方式运行 Ktor 服务端程序：  
![](./ktor_server.png "ktor_server")
- 在 main() 中调用 embeddedServer 来启动 Ktor 应用
- 运行一个 EngineMain 的 main() 并使用 HOCON application.conf 配置文件
- 作为 Web 服务器中的 Servlet
- 在测试中使用 withTestApplication 来启动 Ktor 应用
### 3.1 Gradle 配置 Ktor
Kotlin 的版本需要 1.3.x，因为 Ktor 底层会依赖到 Kotlin Coroutines。
在需要使用 Ktor 的 module 中添加如下的依赖：
```kotlin
dependencies {
    ...
    implementation "io.ktor:ktor-server-core:${libs.ktor}"
    implementation "io.ktor:ktor-server-netty:${libs.ktor}"
}
```
后面的例子还会介绍 Ktor 其他的 artifact，例如：freemarker、gson 等。
### 3.2 embeddedServer




# 创建 client
```kotlin
val apiService = HttpClient(CIO) {
        defaultRequest {
            host = BASE_URL
            //port = BASE_PORT
            url { protocol = URLProtocol.HTTPS }
        }
        install(ContentNegotiation) { // 引入数据转换插件
            gson()
        }
        install(Logging)
        {
            // logger = Logger.SIMPLE // 用于显示本机请求信息
            level = LogLevel.ALL
        }
        install(HttpTimeout) {
            connectTimeoutMillis = 5000
            requestTimeoutMillis = 100000
            //socketTimeoutMillis = 100000
        }
    }
```
# 网络请求
```kotlin

```
# 下载，并显示进度
```kotlin
    override suspend fun downloadAndSave(
        path: String, file: File?, isStop: () -> Boolean, DLProgress: (Long, Long) -> Unit
    ) {
        if (path.isEmpty()) throw(java.lang.Exception("地址有误，下载失败！"))
        client.prepareGet(path).execute { httpResponse ->
            if (!httpResponse.status.isSuccess()) { // 如果网络请求失败，直接抛异常
                throw(java.lang.Exception(httpResponse.status.toString()))
            }
            val channel: ByteReadChannel = httpResponse.body()
            val contentLength = httpResponse.contentLength() // 网络请求数据的大小
            var currentLength = 0L
            var isStop = isStop()

            while (!channel.isClosedForRead && !isStop) {
                val packet = channel.readRemaining(DEFAULT_BUFFER_SIZE.toLong())
                while (!packet.isEmpty) {
                    val bytes = packet.readBytes()
                    currentLength += bytes.size
                    file?.appendBytes(bytes)
                    DLProgress(currentLength, contentLength!!) // 将下载进度回调
                }
                isStop = isStop()
            }
            if (isStop) httpResponse.cancel()
        }
    }

    override suspend fun getNetWorker(url: String): String {
       val response =  client.get(url)
       return response.bodyAsText()
    }

    override suspend fun breakpointDownloadAndSave(
        path: String,
        file: File?,
        total: Long,
        isStop: () -> Boolean,
        DLProgress: (Long, Long) -> Unit
    ) {
        if (path.isEmpty()) throw(java.lang.Exception("地址有误，下载失败！"))

        var currentLength = file?.let { if (total > 0) it.length() else 0L } ?: 0L // 文件的大小

        // 判断当前地址获取的大小跟之前下载的大小是否一致，如果不一致，直接重新下载
        client.prepareGet(path).execute {
            val length = it.contentLength() ?: 0L
            if (length != total) currentLength = 0L
        }

        client.prepareGet(path) {
            if (currentLength > 0) {
                this.header("Range", "bytes=$currentLength-${total}") // 设置获取内容位置
            }
        }.execute { httpResponse ->
            if (!httpResponse.status.isSuccess()) { // 如果网络请求失败，直接抛异常
                throw(java.lang.Exception(httpResponse.status.toString()))
            }
            val channel: ByteReadChannel = httpResponse.body()
            val contentLength =
                (httpResponse.contentLength() ?: 0L) + currentLength // 网络请求数据的大小，由于请求时扣减了。所以这边手动补上
            var isStop = isStop()

            while (!channel.isClosedForRead && !isStop) {
                val packet = channel.readRemaining(DEFAULT_BUFFER_SIZE.toLong())
                while (!packet.isEmpty) {
                    val bytes = packet.readBytes()
                    currentLength += bytes.size
                    file?.appendBytes(bytes)
                    DLProgress(currentLength, contentLength!!) // 将下载进度回调
                }
                isStop = isStop()
            }
            if (isStop) httpResponse.cancel()
        }
    }
```
# 搭建一个要求返回内容的架构
```kotlin
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// ApiResultData 文件
import io.ktor.client.call.*
import io.ktor.client.statement.*
import java.io.File

/*const val BASE_URL = "172.30.93.165"
const val BASE_PORT = 8080
const val BASE_URL_PATH = "http://$BASE_URL:$BASE_PORT/"*/
const val BASE_URL = "shop.plaoc.com"
const val BASE_PORT = 80
const val BASE_URL_PATH = "https://shop.plaoc.com/"
/*const val BASE_URL = "linge.plaoc.com"
const val BASE_PORT = 80
const val BASE_URL_PATH = "http://linge.plaoc.com/"*/


/*typealias _ERROR = suspend (Throwable) -> Unit
typealias _PROGRESS = suspend (downloadedSize: Long, length: Long, progress: Float) -> Unit
typealias _SUCCESS<T> = suspend (result: T) -> Unit*/

/*sealed class ApiResult<T>() {
    data class BizSuccess<T>(val errorCode: Int, val errorMsg: String, val data: T) : ApiResult<T>()
    data class BizError(val errorCode: Int, val errorMsg: String) : ApiResult<Nothing>()
    data class OtherError(val throwable: Throwable) : ApiResult<Nothing>()
}*/

interface IApiResult<T> {
    fun onPrepare() {}// 网络请求前
    fun onSuccess(errorCode: Int, errorMsg: String, data: T) {} // 网络请求成功，且返回数据
    fun onError(errorCode: Int, errorMsg: String, exception: Throwable? = null) {}
    fun downloadProgress(current: Long, total: Long, progress: Float) {} // 下载进度
    fun downloadSuccess(file: File) {} // 下载完成
}

//data class BaseData<T>(val errorCode: Int, val errorMsg: String, val data: T?)

class ApiResultData<out T> constructor(val value: Any?) {
    val isSuccess: Boolean get() = value !is Failure && value !is Progress && value !is Prepare
    val isFailure: Boolean get() = value is Failure
    val isLoading: Boolean get() = value is Progress
    val isPrepare: Boolean get() = value is Prepare

    fun exceptionOrNull(): Throwable? = when (value) {
        is Failure -> value.exception
        else -> null
    }

    companion object {
        fun <T> success(value: T): ApiResultData<T> = ApiResultData(value)

        fun <T> failure(exception: Throwable): ApiResultData<T> =
            ApiResultData(createFailure(exception))

        fun <T> prepare(exception: Throwable? = null): ApiResultData<T> =
            ApiResultData(createPrepare(exception))

        fun <T> progress(
            currentLength: Long = 0L, length: Long = 0L, progress: Float = 0f
        ): ApiResultData<T> = ApiResultData(createLoading(currentLength, length, progress))
    }

    data class Failure(val exception: Throwable)

    data class Progress(val currentLength: Long, val length: Long, val progress: Float)

    data class Prepare(val exception: Throwable?)

}

private fun createPrepare(exception: Throwable?): ApiResultData.Prepare =
    ApiResultData.Prepare(exception)

private fun createFailure(exception: Throwable): ApiResultData.Failure =
    ApiResultData.Failure(exception)


private fun createLoading(currentLength: Long, length: Long, progress: Float) =
    ApiResultData.Progress(currentLength, length, progress)


inline fun <R, T> ApiResultData<T>.fold(
    onSuccess: (value: T) -> R,
    onLoading: (loading: ApiResultData.Progress) -> R,
    onFailure: (exception: Throwable?) -> R,
    onPrepare: (exception: Throwable?) -> R
): R {
    return when {
        isFailure -> {
            onFailure(exceptionOrNull())
        }
        isLoading -> {
            onLoading(value as ApiResultData.Progress)
        }
        isPrepare -> {
            onPrepare(exceptionOrNull())
        }
        else -> {
            onSuccess(value as T)
        }
    }
}

inline fun <R> runCatching(block: () -> R): ApiResultData<R> {
    return try {
        ApiResultData.success(block())
    } catch (e: Throwable) {
        ApiResultData.failure(e)
    }
}

suspend inline fun <reified T> HttpResponse.checkAndBody(): T = if (this.status.value == 200) {
    this.body() // body有做bodyNullable判断，导致会有exception打印，这边做过滤
} else {
    BaseData(this.status.value, this.status.description, null) as T
}

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// BaseData 文件
import io.ktor.client.call.*
import io.ktor.client.statement.*

/**
 * 用于获取网络请求数据
 */
data class BaseData<T>(
    val errorCode: Int, // ==0 为成功
    val errorMsg: String,
    val data: T?,
    var isSuccess: Boolean = errorCode == 0 && data != null
)

/**
 * 直接返回最终值
 */
suspend inline fun <reified T> HttpResponse.bodyData(): T =
    if (this.status.value == 200) {
        this.body() // body有做bodyNullable判断，导致会有exception打印，这边做过滤
    } else {
        BaseData(this.status.value, this.status.description, null) as T
    }
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// ApiServer 文件
import info.bagen.libappmgr.entity.AppVersion
import info.bagen.libappmgr.network.base.ApiResultData
import info.bagen.libappmgr.network.base.BaseData
import io.ktor.client.statement.*
import java.io.File

interface ApiService {

    suspend fun getAppVersion(path: String): ApiResultData<BaseData<AppVersion>>
    suspend fun getAppVersion(): BaseData<AppVersion>

    suspend fun download(path: String, onDownload: (Long, Long) -> Unit): HttpResponse
    suspend fun downloadAndSave(
        path: String,
        file: File?,
        isStop: () -> Boolean,
        DLProgress: (Long, Long) -> Unit
    )

    suspend fun getNetWorker(url:String):String

    suspend fun breakpointDownloadAndSave(
        path: String,
        file: File?,
        total: Long,
        isStop: () -> Boolean,
        DLProgress: (Long, Long) -> Unit
    )

    companion object {
        val instance = ApiServiceImpl(KtorManager.apiService)
    }
}

//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// ApiServerImpl 文件
import info.bagen.libappmgr.entity.AppVersion
import info.bagen.libappmgr.network.base.ApiResultData
import info.bagen.libappmgr.network.base.BaseData
import info.bagen.libappmgr.network.base.bodyData
import info.bagen.libappmgr.network.base.checkAndBody
import io.ktor.client.*
import io.ktor.client.call.*
import io.ktor.client.plugins.*
import io.ktor.client.request.*
import io.ktor.client.statement.*
import io.ktor.http.*
import io.ktor.utils.io.*
import io.ktor.utils.io.core.*
import kotlinx.coroutines.cancel
import java.io.File

class ApiServiceImpl(private val client: HttpClient) : ApiService {


    override suspend fun getAppVersion(path: String): ApiResultData<BaseData<AppVersion>> =
        info.bagen.libappmgr.network.base.runCatching {
            client.get(path).checkAndBody()
        }

    override suspend fun getAppVersion(): BaseData<AppVersion> =
        client.get("KEJPMHLA/appversion.json").bodyData()

    override suspend fun download(path: String, DLProgress: (Long, Long) -> Unit): HttpResponse =
        client.get(path) {
            onDownload { bytesSentTotal, contentLength ->
                DLProgress(bytesSentTotal, contentLength)
            }
        }

    override suspend fun downloadAndSave(
        path: String, file: File?, isStop: () -> Boolean, DLProgress: (Long, Long) -> Unit
    ) {
        if (path.isEmpty()) throw(java.lang.Exception("地址有误，下载失败！"))
        client.prepareGet(path).execute { httpResponse ->
            if (!httpResponse.status.isSuccess()) { // 如果网络请求失败，直接抛异常
                throw(java.lang.Exception(httpResponse.status.toString()))
            }
            val channel: ByteReadChannel = httpResponse.body()
            val contentLength = httpResponse.contentLength() // 网络请求数据的大小
            var currentLength = 0L
            var isStop = isStop()

            while (!channel.isClosedForRead && !isStop) {
                val packet = channel.readRemaining(DEFAULT_BUFFER_SIZE.toLong())
                while (!packet.isEmpty) {
                    val bytes = packet.readBytes()
                    currentLength += bytes.size
                    file?.appendBytes(bytes)
                    DLProgress(currentLength, contentLength!!) // 将下载进度回调
                }
                isStop = isStop()
            }
            if (isStop) httpResponse.cancel()
        }
    }

    override suspend fun getNetWorker(url: String): String {
       val response =  client.get(url)
       return response.bodyAsText()
    }

    override suspend fun breakpointDownloadAndSave(
        path: String,
        file: File?,
        total: Long,
        isStop: () -> Boolean,
        DLProgress: (Long, Long) -> Unit
    ) {
        if (path.isEmpty()) throw(java.lang.Exception("地址有误，下载失败！"))

        var currentLength = file?.let { if (total > 0) it.length() else 0L } ?: 0L // 文件的大小

        // 判断当前地址获取的大小跟之前下载的大小是否一致，如果不一致，直接重新下载
        client.prepareGet(path).execute {
            val length = it.contentLength() ?: 0L
            if (length != total) currentLength = 0L
        }

        client.prepareGet(path) {
            if (currentLength > 0) {
                this.header("Range", "bytes=$currentLength-${total}") // 设置获取内容位置
            }
        }.execute { httpResponse ->
            if (!httpResponse.status.isSuccess()) { // 如果网络请求失败，直接抛异常
                throw(java.lang.Exception(httpResponse.status.toString()))
            }
            val channel: ByteReadChannel = httpResponse.body()
            val contentLength =
                (httpResponse.contentLength() ?: 0L) + currentLength // 网络请求数据的大小，由于请求时扣减了。所以这边手动补上
            var isStop = isStop()

            while (!channel.isClosedForRead && !isStop) {
                val packet = channel.readRemaining(DEFAULT_BUFFER_SIZE.toLong())
                while (!packet.isEmpty) {
                    val bytes = packet.readBytes()
                    currentLength += bytes.size
                    file?.appendBytes(bytes)
                    DLProgress(currentLength, contentLength!!) // 将下载进度回调
                }
                isStop = isStop()
            }
            if (isStop) httpResponse.cancel()
        }
    }
}
//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////// KtorManager 文件
import info.bagen.libappmgr.network.base.BASE_URL
import io.ktor.client.*
import io.ktor.client.engine.cio.*
import io.ktor.client.plugins.*
import io.ktor.client.plugins.contentnegotiation.*
import io.ktor.client.plugins.logging.*
import io.ktor.http.*
import io.ktor.serialization.gson.*

object KtorManager {

    val apiService = HttpClient(CIO) {
        defaultRequest {
            host = BASE_URL
            //port = BASE_PORT
            url { protocol = URLProtocol.HTTPS }
        }
        install(ContentNegotiation) { // 引入数据转换插件
            gson()
        }
        install(Logging)
        {
            // logger = Logger.SIMPLE // 用于显示本机请求信息
            level = LogLevel.ALL
        }
        install(HttpTimeout) {
            connectTimeoutMillis = 5000
            requestTimeoutMillis = 100000
            //socketTimeoutMillis = 100000
        }
    }
}
```