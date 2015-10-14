title: Go语言爬虫
tags:
  - go
  - goquery
  - 爬虫
id: 497
categories:
  - go
  - 编程语言
date: 2015-09-13 14:03:40
---

之前写爬虫都是用的python语言，最近发现go语言写起来也挺方便的，下面简单介绍一下。
这里说的爬虫并不是对网络中的很多资源进行不断的循环抓取，而只是抓通过程序的手段都某些网页实现特定的信息抓取。可以简单分成两个部分：抓取网页，对网页进行解析。
<!--more-->抓取网页。一般是向服务器发送一个http get/post请求，得到response。go提供的http包可以很好的实现。
[cce_js]
get方法：
resp, err := http.Get(“http://www.legendtkl.com")
post方法：
resp, err := http.Post(“http://example.com/upload”, “image/jpg”, &amp;buf)
resp, err := http.PostForm(“http://example.com/form”, url.Values{“key”:{“Value”}, “id”:{“123"}})
[/cce_js]
当然如果要更具体的设置HTTP client，可以建一个client。
[cce_js]
client := &amp;http.Client{
    CheckRedirect : redirectPolicyFunc,
}
[/cce_js]
client结构如下：
[cce_js]
type Client struct {
    Transport RoundTripper //Transport specifies the mechanism by which HTTP request are made
    CheckRedirect func(req *Request, via []*Request) error //if nil, use default, namely 10 consecutive request
    Jar CookieJar //specify the cookie jar
    Timeout time.Duration //request timeout
}
[/cce_js]
HTTP header，就是我们post时候需要提交的key-value，定义如下
type Header map[string][]string
提供了Add, Del, Set等操作。

HTTP request，我们前面直接用Get向服务器地址请求，为了更好的处理，可以使用Do()发送http request。
[cce_js]
func (c *client) Do(req *Request) (resp *Response, error)
    client := &amp;http.Client{
...
}
req, err := http.NewRequest(“GET”, “http://example.com”, nil)
req.Header.Add(“If-None-Match”, `W/“wyzzy"`)
resp, err := client.Do(req)
[/cce_js]
Request里面的数据结构属于http的内容，这里就不细说了。

上面这些只是得到网页内容，更重要的是网页内容解析。我们这里要使用的是goquery，类似jQuery，可以很方便的解析html内容。下面先看一个抓取糗事百科的例子。
[cce_js tab_size="4"]
package main

import (
    "fmt"
    "github.com/PuerkitoBio/goquery"
    "log"
)

func ExampleScrape() {
    doc, err := goquery.NewDocument("http://www.qiushibaike.com")
    if err != nil {
        log.Fatal(err)
    }
    doc.Find(".article").Each(func(i int, s *goquery.Selection) {
        if s.Find(".thumb").Nodes == nil &amp;&amp; s.Find(".video_holder").Nodes == nil {
            content := s.Find(".content").Text()
            fmt.Printf("%s", content)
        }
    })
}

func main() {
    ExampleScrape()
}
[/cce_js]
程序运行效果如下。

[![Screen Shot 2015-09-13 at 1.34.20 pm](http://www.legendtkl.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-13-at-1.34.20-pm-e1442123744676.png)](http://www.legendtkl.com/wp-content/uploads/2015/09/Screen-Shot-2015-09-13-at-1.34.20-pm.png)

goquery中最核心的就是find函数，原型如下
func (s *Selection) Find(selector string) *Selection
返回的Selection数据结构如下
[cce_js]
type Selection struct {
    Nodes []*html.Node
}
[/cce_js]
html包是golang.org/x/net/html，Node结构如下
[cce]
type Node struct {
    Parent, FirstChild, LastChild, PrevSibling, NextSibling *Node
    Type NodeType
    DataAtom atom.Atom
    Data string
    Namespace string
    Attr []Attribule
}
[/cce]
用来解析html网页，上面代码中的第二个if是用来过来糗事百科上面的视频和图片。
goquery内容很丰富，解析起来很方便，大家可以通过godoc查询。