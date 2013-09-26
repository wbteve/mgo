mgo
===

labix.org/v2/mgo

==============
使用方式：
1、安装：
go get labix.org/v2/mgo

如果没有bzr . 在ubuntu下直接用apt-get install bzr

然后再安装；


2、运行testdb下的setup.sh

配置好mongodb的服务。
如果报错，安装supervisor，命令#easy_install supervisor

3、运行demo

//app.go

package main

import (
        "fmt"
        "labix.org/v2/mgo"
        "labix.org/v2/mgo/bson"
)

type Person struct {
        Name string
        Phone string
}

func main() {
        session, err := mgo.Dial("127.0.0.1:40201,127.0.0.1:40202")
        if err != nil {
                panic(err)
        }
        defer session.Close()
	
	fmt.Println("run:");

        // Optional. Switch the session to a monotonic behavior.
        session.SetMode(mgo.Monotonic, true)

        c := session.DB("test").C("people")
        err = c.Insert(&Person{"Ale", "+55 53 8116 9639"},
	               &Person{"eric", "+86 15612345678"})
        if err != nil {
                panic(err)
        }

        result := Person{}
        err = c.Find(bson.M{"name": "eric"}).One(&result)
        if err != nil {
                panic(err)
        }

        fmt.Println("Phone:", result.Phone)
}

前面洋鬼子没有说清楚，折腾了很久。现在折腾出来，供大家参考。
