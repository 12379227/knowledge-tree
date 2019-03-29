## HTTP缓存机制简介

每个浏览器都自带了HTTP缓存的功能, 你只需要确保每个服务器响应都提供正确的HTTP标头指令, 以指示浏览器何时可以缓存响应以及可以缓存多久.  

服务器利用Cache-Control和ETag字段配合可以实现HTTP资源的缓存与刷新.

#### 主要流程

*browser端逻辑*

```c++
/*browser*/

if ((当前时间 - 上次获取资源的时间) > Cache-Control.max-age) 
    || Cache-Control.no-cache == true)//过期
{
    /*发送请求
    GET /file;
    If-None-Match: x123ddd; x123ddd 是ETag令牌
    */

    //server response

    if (retutn code == 304 Not Modified) 
    {
        /* 直接使用本地资源 */
    }

    if (return code == 200 OK)
    {
        /* 使用服务器返回的资源 */
    }
}
else 
{
    /* 直接使用本地资源 */
}

```

*server端逻辑*
``` c++
/* server*/

/*接收到一个来自客户端的Get请求
Get /file
If-None-Match: x123ddd
*/

if  (x123ddd == file.ETag)
{
    /*组装 response
    {
        304 Not Modified
        Cache-Control: max-age=xxx or nocache
        ETag: x123ddd
    } 
    */
    
    return reponse;
}
else
{
    /*组装 response
    {
        200 OK
        Centent-Length: 1024
        Cache-Control: max-age=xxx or nocache
        ETag: x23424fff ---new ETag
        (data)
    } 
    */

    return response;
}

```
####术语

- **ETag**: 验证令牌, 一个服务器资源的摘要. 资源内容改变, ETag值相应发生变更
  - 服务器使用ETag HTTP标头传递验证令牌
  - 资源为发生变化是不会传送任何数据
  <br>

- **Cache-Control** : 每个资源都可以通过Cache-Control HTTP标头定义其缓存策略
  - max-age: 指定从请求的时间开始, 允许提取的响应被重用的最长时间(单位: 秒).
  - no-cache: 标识需要先与服务器确认返回的响应是否发生了变化, 然后才能使用该响应来满足后续对同一网址的请求. 因此, 如果存在合适的验证令牌(ETag), no-cache会发起往返通信来验证缓存的响应, 但如果资源未发生变化, 则可避免下载. 
  - no-store: 它禁止浏览器以及所有的中间缓存存储任何版本的返回响应, 例如, 包含个人隐私数据或银行业务数据的响应. 每次用户请求该资产时, 都会向服务器发送请求, 并下载完整的响应. 
  - public: 明确表示响应是可以缓存的
  - private: 浏览器可以缓存"private"响应. 不过, 这些响应通常只为单个用户缓存, 因此不允许任何中间缓存(CDN)对其进行缓存. 例如，用户的浏览器可以缓存包含用户私人信息的 HTML 网页，但 CDN 却不能缓存。

#### 注意事项
- 使用一致的网址. 如果您在不同的网址上提供相同的内容，将会多次提取和存储这些内容. **网址区分大小写**
- 确保服务器提供验证令牌(ETag)


>[http缓存](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn)


