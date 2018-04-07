# HTTP响应头信息

http请求头提供了关于请求，响应或者其他的发送实体的信息。

| 应答头           | 说明                                                         |
| ---------------- | ------------------------------------------------------------ |
| Allow            | 服务器支持哪些请求方法（如GET、POST等）                      |
| Content-Encoding | 文档的编码（Encode）方法。只有在解码之后才可以得到Content-Type头指定内容类型。利用gzip压缩文档能够显著地减少HTML文档的下载时间。 |
| Content-Length   | 表示内容的长度。只有当浏览器使用持久HTTP连接时才需要这个数据。 |
| Content-Type     | 表示后面的文档属于什么MIME类型。                             |
| Date             | 当前的GMT时间。                                              |
| Expires          | 存活时间。应该在什么时偶认为文档已经过期，从而不再缓存它     |
| Last-Modified    | 文档的最后改动时间。客户可以通过If-Modified-Since请求头提供一个日期，该请求将被视为一个条件GET，只有改动时间迟于指定时间的文档才会返回，否则返回304（Not Modified）状态。Last-Modified也可用setDateHeader方法（Java）来设置。 |
| Location         | 表示客户应当到哪里去提取文档。                               |
| Refresh          | 表示浏览器应该在多少时间之后刷新文档，以秒计。除了刷新当前文档之外，你还可以通过setHeader("Refresh", "5; URL=http://host/path")让浏览器读取指定的页面。注意这种功能通常是通过设置HTML页面HEAD区的`<META HTTP_EQUIV="Refresh" CONTENT="5;URL=http://host/path">`实现，这是因为，自动刷新或重定向对于那些不能使用CGI或Servlet的HTML编写者十分重要。但是，对于Servlet来说，直接设置Refresh头更加方便。 |
| Server           | 服务器名字。Servlet一般不设置这个值，而是由Web服务器自己设置。 |
| Set-Cookie       | 设置和页面关联的Cookie。                                     |
| WWW-Authenticate | 客户应该在Authorization头中提供什么类型的授权信息？在包含401（Unauthorized）状态行的应答中这个头是必须的。 |



