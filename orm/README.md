# beego orm

a powerful orm framework

now, beta, unstable, may be changing some api make your app build failed.

**Driver Support:**

* MySQL: [github.com/go-sql-driver/mysql](https://github.com/go-sql-driver/mysql)

**Features:**

...

**Install:**

	go get github.com/astaxie/beego/orm

## Quick Start

#### Simple Usage

```go
package main

import (
	"fmt"
	"github.com/astaxie/beego/orm"
	_ "github.com/go-sql-driver/mysql" // import your used driver
)

// Model Struct
type User struct {
	Id   int    `orm:"auto"`
	Name string `orm:"size(100)"`
}

func init() {
	// register model
	orm.RegisterModel(new(User))

	// set default database
	orm.RegisterDataBase("default", "mysql", "root:root@/my_db?charset=utf8", 30)
}

func main() {
	o := orm.NewOrm()

	user := User{Name: "slene"}

	// insert
	id, err := o.Insert(&user)

	// update
	user.Name = "astaxie"
	num, err := o.Update(&user)

	// read one
	u := User{Id: user.Id}
	err = o.Read(&u)

	// delete
	num, err = o.Delete(&u)	
}
```

#### Next with relation

```go
type Post struct {
	Id    int    `orm:"auto"`
	Title string `orm:"size(100)"`
	User  *User  `orm:"rel(fk)"`
}

var posts []*Post
qs := o.QueryTable("post")
num, err := qs.Filter("User__Name", "slene").All(&posts)
```

#### Use Raw sql

If you don't like ORM，use Raw SQL to query / mapping without ORM setting

```go
var maps []Params
num, err := o.Raw("SELECT id FROM user WHERE name = ?", "slene").Values(&maps)
if num > 0 {
	fmt.Println(maps[0]["id"])
}
```

#### Transaction

```go
o.Begin()
...
user := User{Name: "slene"}
id, err := o.Insert(&user)
if err != nil {
	o.Commit()
} else {
	o.Rollback()
}

```

#### Debug Log Queries

In development env, you can simple use

```go
func main() {
	orm.Debug = true
...
```

enable log queries.

output include all queries, such as exec / prepare / transaction.

like this:

```go
[ORM] - 2013-08-09 13:18:16 - [Queries/default] - [    db.Exec /     0.4ms] - [INSERT INTO `user` (`name`) VALUES (?)] - `slene`
...
```

note: not recommend use this in product env.

## Docs

more details and examples in docs and test

* [中文](docs/zh)
* English

## TODO
- some unrealized api
- examples
- docs
- support sqlite
- support postgres

## 
